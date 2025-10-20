# Cloud Hosting Panel - Architecture Design

## Project Overview

A modern, secure hosting panel for managing multiple service types:
- **Web Hosting**: Email, web server, database
- **VPS**: Virtual private servers
- **File Storage**: Cloud drive for storing files

**Learning Focus**: PHP, frontend frameworks, Linux, containerization, virtualization, networking, and cybersecurity

## High-Level Architecture

### Microservices + Containerization Design

```
┌─────────────────────────────────────────────────────────────┐
│                    User Interface Layer                      │
│            (React/Vue.js + TailwindCSS/Bootstrap)           │
└──────────────────────┬──────────────────────────────────────┘
                       │ HTTPS/REST API
┌──────────────────────┴──────────────────────────────────────┐
│              PHP API Gateway (Laravel/Symfony)               │
│  - Authentication (JWT tokens)                               │
│  - Authorization (RBAC)                                      │
│  - Request validation & rate limiting                        │
│  - API routing to backend services                           │
└──────┬───────────┬────────────┬──────────────┬─────────────┘
       │           │            │              │
   ┌───┴───┐   ┌──┴───┐   ┌────┴────┐   ┌────┴─────┐
   │ Web   │   │ VPS  │   │  File   │   │  DNS &   │
   │Hosting│   │Service│   │ Storage │   │  Email   │
   │Service│   │      │   │ Service │   │ Service  │
   └───┬───┘   └──┬───┘   └────┬────┘   └────┬─────┘
       │          │             │             │
┌──────┴──────────┴─────────────┴─────────────┴──────────────┐
│            Infrastructure Management Layer                   │
│  - Docker API (for web hosting containers)                  │
│  - libvirt API (for VPS management)                         │
│  - MinIO/S3 API (for file storage)                          │
│  - PowerDNS API + Postfix/Dovecot management                │
└─────────────────────────────────────────────────────────────┘
```

## Core Technology Stack

### 1. Frontend (Modern UI)
- **Framework**: React or Vue.js 3
- **State Management**: React Context/Redux or Pinia (Vue)
- **UI Library**: TailwindCSS or Material-UI
- **HTTP Client**: Axios
- **Real-time Updates**: WebSockets or Server-Sent Events

**Why**: Modern, reactive interface; great for CV; industry standard

### 2. Backend API (PHP-based)
- **Framework**: Laravel 11 (recommended) or Symfony 7
- **Authentication**: Laravel Sanctum (SPA) + JWT
- **Database**: MySQL 8.0 or PostgreSQL 16
- **Queue System**: Redis + Laravel Queue (for long-running tasks)
- **Cache**: Redis
- **API Documentation**: OpenAPI/Swagger

**Why**: Laravel is the most modern PHP framework, has excellent documentation, and teaches best practices

### 3. Service Architecture

#### A. Web Hosting Service (Docker-based)
Each customer gets isolated containers:

```
Customer Environment = Docker Compose Stack:
  ├── nginx-proxy (reverse proxy)
  ├── php-fpm-{customer_id} (PHP application)
  ├── mysql-{customer_id} (dedicated database)
  └── phpmyadmin-{customer_id} (DB management)
```

**Implementation**:
- PHP backend uses Docker SDK for PHP
- Resource limits via Docker (CPU, RAM, disk quotas)
- Network isolation via Docker networks
- Automated SSL via Let's Encrypt (Certbot)

**Security**:
- Each customer runs as different Unix user (mapped in container)
- No direct shell access from panel
- File operations through secure API
- `open_basedir` restrictions

#### B. VPS Service (KVM/QEMU + libvirt)
Virtual machines for full VPS:

**Implementation**:
- libvirt PHP bindings or REST API wrapper
- QEMU/KVM for virtualization
- Create VMs from templates (cloud-init)
- VNC/noVNC for console access in browser
- Snapshot management

**Technologies to learn**:
- KVM/QEMU virtualization
- libvirt management
- Virtual networking (bridges, VLANs)
- Storage pools (LVM, ZFS)

#### C. File Storage Service (S3-compatible)
Object storage for user files:

**Implementation**:
- MinIO (S3-compatible, easy to set up)
- Laravel Filesystem with S3 driver
- Per-user buckets with access policies
- Web-based file manager (FilePond or similar)
- WebDAV support (optional)

#### D. DNS & Email Service
**DNS**:
- PowerDNS with MySQL backend
- PowerDNS API for zone management
- Automatic record creation for new domains

**Email**:
- Postfix (SMTP) + Dovecot (IMAP/POP3)
- Virtual mailboxes (MySQL-based)
- Roundcube webmail (integrated)
- DKIM, SPF, DMARC configuration
- Spam filtering (SpamAssassin/Rspamd)

## Security Architecture

### Multi-Layer Security

#### Layer 1: Network Security
- Firewall (UFW/iptables)
- Fail2ban (brute force protection)
- VPN access for admin panel
- Network segmentation (DMZ for web, isolated backend)

#### Layer 2: Application Security
- OWASP Top 10 mitigations
- SQL injection prevention (PDO prepared statements)
- XSS prevention (Laravel's Blade auto-escaping)
- CSRF tokens
- Input validation & sanitization
- Rate limiting (per-user, per-IP)

#### Layer 3: Authentication & Authorization
- MFA/2FA (TOTP via Google Authenticator)
- Role-Based Access Control (RBAC)
- Audit logging (all admin actions)
- Session management
- Password hashing (bcrypt/Argon2)

#### Layer 4: Infrastructure Security
- Principle of least privilege
- Separate service accounts
- SELinux/AppArmor policies
- Container security (user namespaces, seccomp)
- VM isolation
- Encrypted storage (LUKS)

#### Layer 5: Monitoring & Intrusion Detection
- Log aggregation (ELK stack or Grafana Loki)
- Security monitoring (OSSEC or Wazuh)
- Intrusion detection (Suricata)
- Resource monitoring (Prometheus + Grafana)

## Command Execution Architecture

### Privileged API Daemon Pattern (Recommended)

```php
// Laravel API receives request
Route::post('/hosting/create-vhost', [HostingController::class, 'createVhost'])
    ->middleware(['auth', 'permission:create-vhost']);

// Controller
class HostingController {
    public function createVhost(Request $request) {
        $validated = $request->validate([
            'domain' => 'required|domain|unique:domains',
            'user_id' => 'required|exists:users,id'
        ]);

        // Queue job for privileged operations
        CreateVhostJob::dispatch($validated);

        return response()->json(['status' => 'queued']);
    }
}

// Queue Job (runs as privileged daemon)
class CreateVhostJob implements ShouldQueue {
    public function handle() {
        // Option 1: Use Docker API (recommended)
        $docker = new Docker\DockerClient();
        $container = $docker->createContainer([
            'Image' => 'custom-php-nginx',
            'Env' => [
                'VIRTUAL_HOST=' . $this->domain,
                'USER_ID=' . $this->userId
            ],
            'HostConfig' => [
                'Memory' => 512 * 1024 * 1024, // 512MB
                'CpuShares' => 512
            ]
        ]);

        // Option 2: Call privileged daemon via Unix socket
        $daemon = new UnixSocketClient('/var/run/hosting-daemon.sock');
        $daemon->send('create-vhost', $this->data);

        // Option 3: Execute whitelisted sudo command
        $command = sprintf(
            'sudo /usr/local/bin/hosting-create-vhost %s %s',
            escapeshellarg($this->domain),
            escapeshellarg($this->userId)
        );
        $process = Process::fromShellCommandline($command);
        $process->mustRun();
    }
}
```

**Why This Approach**:
- ✅ Web server (www-data) never runs privileged commands
- ✅ Queue isolation (commands run in background worker)
- ✅ Audit trail (all jobs logged)
- ✅ Retry logic (failed jobs can be retried)
- ✅ Rate limiting (queue throttling)

## PHP Shell Command Execution Methods

### 1. exec()
```php
exec(string $command, array &$output = null, int &$result_code = null): string|false
```
- Returns the last line of output
- Passes full output to `$output` array
- Returns exit code in `$result_code`
- Does NOT display output directly

```php
exec('ls -la /var/www', $output, $return_var);
foreach ($output as $line) {
    echo $line . "\n";
}
```

### 2. shell_exec()
```php
shell_exec(string $command): string|false|null
```
- Returns complete output as a string
- Shorthand: backticks `` `command` ``
- No access to exit code

```php
$result = shell_exec('whoami');
echo $result;
```

### 3. system()
```php
system(string $command, int &$result_code = null): string|false
```
- Displays output directly to browser/stdout
- Returns last line of output
- Useful for real-time output

### 4. passthru()
```php
passthru(string $command, int &$result_code = null): false|null
```
- Displays raw binary output
- Best for binary data (images, files)
- No return value (outputs directly)

### 5. proc_open() (Most Powerful)
```php
proc_open(
    array|string $command,
    array $descriptor_spec,
    array &$pipes,
    ?string $cwd = null,
    ?array $env_vars = null,
    ?array $options = null
): resource|false
```
- Full control over STDIN, STDOUT, STDERR
- Non-blocking execution possible
- Best for complex interactions

```php
$descriptorspec = [
    0 => ["pipe", "r"],  // stdin
    1 => ["pipe", "w"],  // stdout
    2 => ["pipe", "w"]   // stderr
];

$process = proc_open('some-command', $descriptorspec, $pipes);
$output = stream_get_contents($pipes[1]);
$errors = stream_get_contents($pipes[2]);
fclose($pipes[1]);
fclose($pipes[2]);
$return_value = proc_close($process);
```

### Critical Security Considerations

#### 1. Command Injection Prevention
NEVER pass unsanitized user input directly to shell commands:

```php
// DANGEROUS - vulnerable to injection
$file = $_GET['file'];
exec("cat /var/www/$file");

// SAFER - use escapeshellarg() and escapeshellcmd()
$file = escapeshellarg($_GET['file']);
exec("cat /var/www/$file");
```

#### 2. User Isolation
- Run PHP-FPM pools with different Unix users per customer
- Use `chroot` jails or containers to isolate environments
- Implement proper file permissions (use `open_basedir` in php.ini)

#### 3. Disable Dangerous Functions
In `php.ini` for shared hosting:
```ini
disable_functions = exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source
```

#### 4. Privilege Separation
- Use `sudo` with specific allowed commands
- Create dedicated service users with limited permissions
- Configure `/etc/sudoers` with NOPASSWD for specific commands only:
```
www-data ALL=(ALL) NOPASSWD: /usr/local/bin/create-vhost
www-data ALL=(ALL) NOPASSWD: /usr/local/bin/restart-service
```

#### 5. Alternative: Use APIs Instead
For hosting panels, consider:
- **Docker API** - Manage containers via HTTP API
- **systemd D-Bus API** - Control services without shell
- **Web panel daemons** - Background service with socket communication
- **Message queues** - Queue commands for processing by privileged daemon

## Development Roadmap

### Phase 1: Foundation (Weeks 1-2)
1. Set up development environment (Docker, Linux VM)
2. Install Laravel + React/Vue starter kit
3. Design database schema (users, services, resources)
4. Implement authentication system (login, registration, 2FA)

### Phase 2: Web Hosting Service (Weeks 3-5)
1. Docker container management
2. Nginx reverse proxy configuration
3. PHP-FPM pool creation
4. MySQL database provisioning
5. phpMyAdmin integration
6. File manager implementation

### Phase 3: VPS Service (Weeks 6-7)
1. libvirt/KVM setup
2. VM template creation
3. VM lifecycle management (create, start, stop, delete)
4. VNC console integration
5. Resource monitoring

### Phase 4: Storage & Email (Weeks 8-9)
1. MinIO setup and integration
2. S3 API implementation
3. Email server configuration (Postfix/Dovecot)
4. Webmail integration
5. DNS management (PowerDNS)

### Phase 5: Security Hardening (Week 10)
1. Implement security controls
2. Set up monitoring
3. Conduct security testing
4. Penetration testing (ethical)
5. Security audit documentation

### Phase 6: Testing & Documentation (Week 11-12)
1. Unit tests (PHPUnit)
2. Integration tests
3. Performance testing
4. User documentation
5. Thesis documentation

## Project Structure

```
praca_inzynierska/
├── backend/                    # Laravel API
│   ├── app/
│   │   ├── Http/Controllers/  # API endpoints
│   │   ├── Models/            # Database models
│   │   ├── Jobs/              # Queue jobs for privileged ops
│   │   ├── Services/          # Business logic
│   │   │   ├── Docker/        # Docker management
│   │   │   ├── Libvirt/       # VM management
│   │   │   ├── Storage/       # MinIO integration
│   │   │   └── DNS/           # PowerDNS integration
│   │   └── Security/          # Security utilities
│   ├── database/migrations/
│   ├── tests/
│   └── docker-compose.yml     # Development environment
├── frontend/                   # React/Vue SPA
│   ├── src/
│   │   ├── components/
│   │   ├── views/            # Page components
│   │   ├── store/            # State management
│   │   ├── api/              # API client
│   │   └── router/
│   └── package.json
├── infrastructure/             # Infrastructure as Code
│   ├── docker/                # Container configs
│   │   ├── php-nginx/        # Web hosting base image
│   │   └── monitoring/
│   ├── libvirt/               # VM templates
│   └── ansible/               # Automation scripts
├── docs/                       # Thesis documentation
│   ├── architecture/
│   ├── security/
│   └── api/                   # API documentation
└── scripts/                    # Helper scripts
    ├── setup.sh
    └── security-audit.sh
```

## Key Learning Outcomes

This architecture will teach:

✅ **PHP**: Modern framework (Laravel), API development, queue systems
✅ **Frontend**: React/Vue, SPA architecture, modern UI/UX
✅ **Linux**: System administration, service management, networking
✅ **Containerization**: Docker, orchestration, resource limits
✅ **Virtualization**: KVM/QEMU, libvirt, VM management
✅ **Networking**: Reverse proxies, DNS, VPNs, firewalls
✅ **Cybersecurity**: OWASP, intrusion detection, security monitoring, penetration testing
✅ **DevOps**: CI/CD, infrastructure as code, monitoring

## Recommended Resources

### Books
- "Laravel Up & Running" by Matt Stauffer
- "Docker Deep Dive" by Nigel Poulton
- "The Web Application Hacker's Handbook" by Dafydd Stuttard

### Online
- Laravel documentation
- Docker documentation
- OWASP Top 10
- Linux Academy courses on KVM/libvirt

## Popular Hosting Panel Approaches (Reference)

- **cPanel/WHM**: Uses Perl daemons + setuid wrappers
- **Plesk**: Uses privileged service (sw-cp-server)
- **ISPConfig**: PHP + sudo with restricted commands
- **Webmin**: Perl running as root (security concerns)
- **VestaCP/HestiaCP**: Bash scripts + sudo
