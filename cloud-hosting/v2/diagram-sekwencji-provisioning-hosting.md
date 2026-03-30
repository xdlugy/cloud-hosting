@startuml Sekwencja_Provisioning_Hosting
skinparam backgroundColor #FEFEFE
skinparam sequence {
    ArrowColor #0066CC
    LifeLineBorderColor #0066CC
    ParticipantBackgroundColor #E6F3FF
    ParticipantBorderColor #0066CC
}

title Diagram sekwencji: Provisioning usługi hostingowej

actor "Klient" as Client
participant "React SPA\n(Panel klienta)" as Frontend
participant "Symfony API" as API
database "PostgreSQL" as DB
participant "RabbitMQ" as Queue
participant "Worker\n(Symfony Messenger)" as Worker
participant "Kubernetes\nAPI" as K8s
participant "Let's Encrypt" as LE
participant "PowerDNS" as DNS

Client -> Frontend : Wybór pakietu hostingowego\ni konfiguracji
Frontend -> API : POST /api/hosting/order\n{plan, domain, php_version}
activate API

API -> DB : Zapisanie zamówienia\n(status: pending)
API -> Queue : Publikacja zadania\nCreateHostingJob
API --> Frontend : 202 Accepted\n{order_id, status: provisioning}
deactivate API

Frontend --> Client : Wyświetlenie statusu:\n"Trwa tworzenie usługi..."

Queue -> Worker : Konsumpcja zadania\nCreateHostingJob
activate Worker

Worker -> DNS : Utworzenie rekordu DNS\n(A record -> IP serwera)
DNS --> Worker : OK

Worker -> K8s : Utworzenie namespace\nklienta
K8s --> Worker : Namespace created

Worker -> K8s : Deploy: nginx + php-fpm\n(Docker Compose stack)
K8s --> Worker : Pods running

Worker -> K8s : Deploy: baza danych\n(MySQL/PostgreSQL/MongoDB)
K8s --> Worker : Database pod running

Worker -> K8s : Konfiguracja Nginx\nIngress
K8s --> Worker : Ingress configured

Worker -> LE : Żądanie certyfikatu SSL\n(ACME challenge)
LE --> Worker : Certyfikat wydany

Worker -> DB : Aktualizacja statusu\n(status: active)
deactivate Worker

Frontend -> API : GET /api/hosting/{id}/status
API -> DB : Pobranie statusu
API --> Frontend : {status: active,\naccess_details: {...}}

Frontend --> Client : Wyświetlenie:\n"Usługa aktywna"\n+ dane dostępowe

@enduml
