@startuml Diagram_Komponentow
!define RECTANGLE class
skinparam componentStyle uml2
skinparam backgroundColor #FEFEFE
skinparam defaultFontSize 12
skinparam component {
    BackgroundColor #E6F3FF
    BorderColor #0066CC
    ArrowColor #333333
}
skinparam package {
    BorderColor #666666
}

title Diagram komponentów systemu hostingowego

package "Warstwa prezentacji" #E6FFE6 {
    [WordPress\n+ WooCommerce] as WP <<Strona publiczna>>
    [Panel klienta\n(React + TailwindCSS)] as ClientPanel <<SPA>>
    [Panel administratora\n(React + TailwindCSS)] as AdminPanel <<SPA>>
}

package "Warstwa aplikacji" #FFE6E6 {
    [API Gateway\n(Symfony 7.4)] as API <<REST API>>
    [Moduł provisioningu] as Provisioning
    [Moduł monitoringu] as Monitoring
    [Moduł kopii zapasowych] as Backup
    [Moduł rozliczeniowy] as Billing
}

package "Warstwa komunikacji" #E6E6FF {
    [RabbitMQ] as Queue <<Message Broker>>
    [Nginx] as Proxy <<Reverse Proxy>>
}

package "Warstwa infrastruktury" #FFFFE6 {
    package "Hosting Web" {
        [Kubernetes] as K8s
        [Kontenery Docker\n(nginx + php-fpm)] as Docker
    }
    package "VPS" {
        [Proxmox VE] as Proxmox
        [KVM/QEMU] as KVM
    }
    package "Poczta" {
        [Postfix] as Postfix <<SMTP>>
        [Dovecot] as Dovecot <<IMAP/POP3>>
        [Roundcube] as Roundcube <<Webmail>>
    }
    [PowerDNS] as DNS
}

package "Warstwa danych" #FFF0E6 {
    database "PostgreSQL\n(dane platformy)" as PG
    database "MySQL / PostgreSQL / MongoDB\n(bazy klientów)" as CustomerDB
    storage "MinIO\n(S3-compatible)" as MinIO
}

package "Warstwa monitoringu i CI/CD" #F0F0F0 {
    [Prometheus] as Prom
    [Grafana] as Graf
    [Restic] as Restic <<Backup>>
    [GitLab CI/CD] as GitLab
    [Let's Encrypt] as LetsEncrypt
}

' Połączenia warstwy prezentacji -> API
WP --> API : HTTPS
ClientPanel --> API : HTTPS/REST
AdminPanel --> API : HTTPS/REST

' API -> warstwa komunikacji
API --> Queue : publikacja zadań
API --> Proxy : routing

' API -> warstwa danych
API --> PG : Doctrine ORM

' Warstwa komunikacji -> infrastruktura
Queue --> Provisioning : konsumpcja zadań
Provisioning --> K8s : Docker API
Provisioning --> Proxmox : Proxmox API
Provisioning --> DNS : PowerDNS API
Provisioning --> Postfix : konfiguracja

' Kubernetes -> Docker
K8s --> Docker : orkiestracja

' Proxmox -> KVM
Proxmox --> KVM : zarządzanie VM

' Poczta
Roundcube --> Dovecot : IMAP
Postfix --> Dovecot : dostarczanie

' Proxy -> kontenery
Proxy --> Docker : load balancing
Proxy --> LetsEncrypt : certyfikaty SSL (Certbot)

' Infrastruktura -> dane
Docker --> CustomerDB : połączenia DB
MinIO --> Backup : przechowywanie

' Monitoring
Monitoring --> Prom : zbieranie metryk
Prom --> Graf : wizualizacja
Backup --> Restic : wykonywanie kopii

@enduml
