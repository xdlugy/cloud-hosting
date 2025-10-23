@startuml Component_Diagram
!define RECTANGLE class
skinparam componentStyle uml2
skinparam backgroundColor #FEFEFE
skinparam component {
    BackgroundColor #E6F3FF
    BorderColor #0066CC
    ArrowColor #0066CC
}

package "Warstwa prezentacji" #E6FFE6 {
    [Strona serwisu] as WP
    [Panel użytkownika] as UserPanel
    [Panel administratora] as AdminPanel
    [Dysk wirtualny] as FileUI
}

package "Warstwa aplikacji" #FFE6E6 {
    [Bramka API] as API
    [Provisioning] as Provisioning
    [Monitoring] as Monitor
    [Kopia zapasowa] as Backup
    [Email] as EmailSvc
}

package "Warstwa orkiestracji" #E6E6FF {
    [Kubernetes] as K8s
    [RabbitMQ] as Queue
    [Traefik Proxy] as Traefik
}

package "Warstwa infrastruktury" #FFFFE6 {
    [KVM/Proxmox] as KVM
    [Docker] as Docker
    database "MySQL Cluster" as MySQL
    database "PostgreSQL" as PG
    database "MongoDB" as Mongo
    storage "MinIO Storage" as MinIO
}

package "Usługi zewnętrzne" #F0F0F0 {
    [Bramka płatności] as Payment
    [Let's Encrypt] as SSL
    [DNS] as DNS
    [SMTP] as SMTP
}

' Połączenia warstwy prezentacji
WP --> API
UserPanel --> API
AdminPanel --> API
FileUI --> API

' Połączenia API Gateway
API --> Provisioning
API --> Monitor
API --> Backup

' Połączenia do orkiestracji
Provisioning --> Queue
Provisioning --> K8s
Provisioning --> KVM
K8s --> Docker
Traefik --> Docker

' Połączenia do infrastruktury
UserPanel --> PG
FileUI --> MinIO
Monitor --> Mongo 

' Połączenia zewnętrzne
Traefik --> SSL
EmailSvc --> SMTP
Provisioning --> DNS

@enduml




