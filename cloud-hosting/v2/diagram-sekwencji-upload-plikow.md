@startuml Sekwencja_Upload_Plikow
skinparam backgroundColor #FEFEFE
skinparam sequence {
    ArrowColor #0066CC
    LifeLineBorderColor #0066CC
    ParticipantBackgroundColor #E6F3FF
    ParticipantBorderColor #0066CC
}

title Diagram sekwencji: Upload plików na dysk wirtualny

actor "Klient" as Client
participant "React SPA\n(Panel klienta)" as Frontend
participant "Symfony API" as API
database "PostgreSQL" as DB
participant "MinIO\n(S3-compatible)" as MinIO

Client -> Frontend : Przeciągnięcie plików\n(drag & drop)
Frontend -> Frontend : Walidacja rozmiaru\ni typu plików

Frontend -> API : POST /api/storage/upload\nMultipart form data\n(Authorization: Bearer JWT)
activate API

API -> API : Weryfikacja tokenu JWT\ni uprawnień użytkownika
API -> DB : Sprawdzenie limitu\nprzestrzeni dyskowej
DB --> API : Dostępne: 4.2 GB

alt Przekroczony limit przestrzeni
    API --> Frontend : 413 Payload Too Large\n{error: "Brak miejsca"}
    Frontend --> Client : "Przekroczono limit\nprzestrzeni dyskowej"
else Wystarczająca przestrzeń
    API -> MinIO : PUT Object\n(bucket: user-{id}, key: path/file)
    MinIO --> API : 200 OK\n{etag, size}

    API -> DB : Zapisanie metadanych pliku\n(nazwa, rozmiar, typ, data)
    DB --> API : OK

    API --> Frontend : 201 Created\n{file_id, name, size, url}
    deactivate API

    Frontend --> Client : Wyświetlenie pliku\nna liście z podglądem
end

== Generowanie linku udostępniania ==

Client -> Frontend : Kliknięcie "Udostępnij"
Frontend -> API : POST /api/storage/files/{id}/share\n{expires_in: "7d"}
activate API

API -> MinIO : Generate presigned URL\n(expiry: 7 days)
MinIO --> API : Presigned URL

API -> DB : Zapisanie linku\n(token, wygaśnięcie)
API --> Frontend : {share_url, expires_at}
deactivate API

Frontend --> Client : Wyświetlenie linku\ndo skopiowania

@enduml
