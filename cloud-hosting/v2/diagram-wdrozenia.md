@startuml Diagram_Wdrozenia
skinparam backgroundColor #FEFEFE
skinparam defaultFontSize 11
skinparam node {
    BackgroundColor #E6F3FF
    BorderColor #0066CC
}

title Diagram wdrożenia systemu hostingowego

node "Serwer dedykowany Hetzner\n(Debian 13 + Proxmox VE)\n8GB RAM / 80GB Storage" as Server {

    node "Proxmox VE (Hypervisor)" as ProxmoxHost {

        node "VM: Klaster Kubernetes" as K8sVM {
            artifact "Nginx\n(reverse proxy)" as Nginx
            artifact "Kontenery klientów\n(nginx + php-fpm)" as WebContainers
            artifact "Let's Encrypt\n(certyfikaty SSL)" as SSL
        }

        node "VM: Serwer aplikacji" as AppVM {
            artifact "Symfony 7.4 API" as SymfonyAPI
            artifact "React SPA\n(panel klienta/admina)" as ReactApp
            artifact "RabbitMQ\n(kolejka zadań)" as RabbitMQ
            artifact "Worker Symfony Messenger\n(daemon uprzywilejowany)" as Worker
        }

        node "VM: Bazy danych" as DBVM {
            database "PostgreSQL\n(dane platformy)" as PGMain
            database "MySQL\n(bazy klientów + PowerDNS)" as MySQLCustomer
            database "PostgreSQL\n(bazy klientów)" as PGCustomer
            database "MongoDB\n(bazy klientów)" as MongoCustomer
        }

        node "VM: Poczta e-mail" as MailVM {
            artifact "Postfix (SMTP)" as Postfix
            artifact "Dovecot (IMAP/POP3)" as Dovecot
            artifact "Roundcube (webmail)" as Roundcube
        }

        node "VM: Storage i monitoring" as StorageVM {
            artifact "MinIO (S3)" as MinIO
            artifact "Prometheus" as Prometheus
            artifact "Grafana" as Grafana
            artifact "Restic (backup)" as Restic
        }

        node "VM: DNS" as DNSVM {
            artifact "PowerDNS" as PowerDNS
        }

        collections "VM: Serwery VPS klientów\n(KVM/QEMU)" as VPSInstances {
            artifact "Ubuntu / Debian / CentOS\n(cloud-init)" as VPSos
        }
    }
}

cloud "Internet" as Internet
actor "Klient" as Client
actor "Administrator" as Admin

Client --> Internet
Admin --> Internet
Internet --> Nginx : HTTPS (port 443)
Nginx --> ReactApp : routing panelu
Nginx --> SymfonyAPI : routing API
Nginx --> WebContainers : routing hostingu
Nginx --> Roundcube : routing webmail

SymfonyAPI --> PGMain : Doctrine ORM
SymfonyAPI --> RabbitMQ : Symfony Messenger
RabbitMQ --> Worker : konsumpcja zadań
Worker --> K8sVM : Docker/K8s API
Worker --> VPSInstances : Proxmox API
Worker --> MailVM : konfiguracja
Worker --> DNSVM : PowerDNS API

WebContainers --> MySQLCustomer
WebContainers --> PGCustomer
WebContainers --> MongoCustomer

Prometheus --> K8sVM : metryki
Prometheus --> AppVM : metryki
Prometheus --> DBVM : metryki
Restic --> MinIO : przechowywanie kopii

PowerDNS --> MySQLCustomer : backend DNS

@enduml
