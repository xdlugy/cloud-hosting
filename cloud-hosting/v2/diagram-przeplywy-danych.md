@startuml Diagram_Przeplywu_Danych
skinparam backgroundColor #FEFEFE
skinparam defaultFontSize 11

title Diagram przepływu danych w systemie hostingowym

' Aktorzy zewnętrzni
rectangle "Klient\n(przeglądarka)" as ClientBrowser #E6FFE6
rectangle "Administrator\n(przeglądarka)" as AdminBrowser #E6FFE6
rectangle "Klient\n(FTP/SSH)" as ClientSSH #E6FFE6

' Warstwa wejściowa
rectangle "Nginx\n(reverse proxy + SSL)" as Nginx #E6E6FF

' Aplikacja
rectangle "WordPress\n+ WooCommerce" as WP #FFE6E6
rectangle "React SPA" as ReactSPA #FFE6E6
rectangle "Symfony 7.4 API" as SymfonyAPI #FFE6E6

' Kolejka
rectangle "RabbitMQ" as RabbitMQ #FFFFE6
rectangle "Worker\n(Symfony Messenger)" as Worker #FFFFE6

' Infrastruktura
rectangle "Kubernetes\n(hosting web)" as K8s #E6F3FF
rectangle "Proxmox VE\n(VPS)" as Proxmox #E6F3FF
rectangle "Postfix + Dovecot\n(email)" as Mail #E6F3FF
rectangle "PowerDNS" as DNS #E6F3FF

' Dane
database "PostgreSQL\n(platforma)" as PGMain #FFF0E6
database "Bazy klientów\n(MySQL/PG/Mongo)" as CustomerDB #FFF0E6
storage "MinIO\n(pliki)" as MinIO #FFF0E6

' Monitoring
rectangle "Prometheus\n+ Grafana" as Monitoring #F0F0F0
rectangle "Restic\n(backup)" as Backup #F0F0F0

' === Przepływy danych ===

' Wejście
ClientBrowser --> Nginx : HTTPS requests
AdminBrowser --> Nginx : HTTPS requests
ClientSSH --> K8s : FTP/SFTP
ClientSSH --> Proxmox : SSH

' Routing
Nginx --> WP : strona publiczna
Nginx --> ReactSPA : panel klienta/admina
Nginx --> SymfonyAPI : żądania API
Nginx --> Mail : webmail (Roundcube)

' Logika biznesowa
ReactSPA --> SymfonyAPI : REST API (JSON)
WP --> SymfonyAPI : webhook zamówienia
SymfonyAPI --> PGMain : odczyt/zapis danych
SymfonyAPI --> RabbitMQ : zlecenia operacji

' Wykonywanie zadań
RabbitMQ --> Worker : konsumpcja zadań
Worker --> K8s : provisioning hostingu
Worker --> Proxmox : provisioning VPS
Worker --> DNS : zarządzanie rekordami
Worker --> Mail : konfiguracja skrzynek

' Dane klientów
K8s --> CustomerDB : dostęp do baz
K8s --> MinIO : przechowywanie plików

' Monitoring i backup
K8s --> Monitoring : metryki
Proxmox --> Monitoring : metryki
SymfonyAPI --> Monitoring : metryki
Backup --> MinIO : kopie zapasowe

@enduml
