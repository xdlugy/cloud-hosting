@startuml Sekwencja_Provisioning_VPS
skinparam backgroundColor #FEFEFE
skinparam sequence {
    ArrowColor #0066CC
    LifeLineBorderColor #0066CC
    ParticipantBackgroundColor #E6F3FF
    ParticipantBorderColor #0066CC
}

title Diagram sekwencji: Provisioning serwera VPS

actor "Klient" as Client
participant "React SPA\n(Panel klienta)" as Frontend
participant "Symfony API" as API
database "PostgreSQL" as DB
participant "RabbitMQ" as Queue
participant "Worker\n(Symfony Messenger)" as Worker
participant "Proxmox VE\nAPI" as Proxmox
participant "PowerDNS" as DNS

Client -> Frontend : Wybór pakietu VPS,\nsystemu operacyjnego\ni parametrów
Frontend -> API : POST /api/vps/order\n{plan, os, cpu, ram, disk}
activate API

API -> API : Walidacja dostępności\nzasobów
API -> DB : Zapisanie zamówienia\n(status: pending)
API -> Queue : Publikacja zadania\nCreateVPSJob
API --> Frontend : 202 Accepted\n{order_id, status: provisioning}
deactivate API

Frontend --> Client : "Trwa tworzenie serwera VPS..."

Queue -> Worker : Konsumpcja zadania\nCreateVPSJob
activate Worker

Worker -> Proxmox : Klonowanie szablonu VM\n(cloud-init template)
Proxmox --> Worker : VM ID assigned

Worker -> Proxmox : Konfiguracja zasobów\n(CPU, RAM, dysk)
Proxmox --> Worker : Resources configured

Worker -> Proxmox : Ustawienie cloud-init\n(hostname, SSH key, sieć)
Proxmox --> Worker : Cloud-init configured

Worker -> Proxmox : Start maszyny wirtualnej
Proxmox --> Worker : VM started

Worker -> DNS : Utworzenie rekordu DNS\n(A record -> IP VPS)
DNS --> Worker : OK

Worker -> DB : Zapisanie danych VPS\n(IP, port VNC, status: active)
deactivate Worker

Frontend -> API : GET /api/vps/{id}/status
API -> DB : Pobranie statusu VPS
API --> Frontend : {status: active,\nip: "x.x.x.x",\nvnc_url: "...",\nssh_port: 22}

Frontend --> Client : Wyświetlenie:\n"VPS aktywny"\n+ dane dostępowe (IP, SSH)

@enduml
