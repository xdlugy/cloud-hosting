@startuml Diagram_Przypadkow_Uzycia
skinparam backgroundColor #FEFEFE
skinparam usecase {
    BackgroundColor #E6F3FF
    BorderColor #0066CC
}
skinparam actor {
    BorderColor #333333
}

title Diagram przypadków użycia systemu hostingowego

left to right direction

actor "Klient\n(niezalogowany)" as Guest
actor "Klient\n(zalogowany)" as User
actor "Administrator" as Admin

rectangle "System hostingowy" {

    package "Strona publiczna" {
        usecase "Przeglądanie oferty" as UC_Browse
        usecase "Porównywanie pakietów" as UC_Compare
        usecase "Rejestracja konta" as UC_Register
        usecase "Logowanie" as UC_Login
    }

    package "Zarządzanie kontem" {
        usecase "Zmiana hasła" as UC_Password
        usecase "Zarządzanie danymi profilu" as UC_Profile
        usecase "Przeglądanie dashboardu" as UC_Dashboard
    }

    package "Usługa: Hosting Web" {
        usecase "Zakup pakietu hostingowego" as UC_BuyHost
        usecase "Zarządzanie plikami (FTP/SFTP)" as UC_Files
        usecase "Zarządzanie bazą danych" as UC_DB
        usecase "Konfiguracja wersji PHP" as UC_PHP
        usecase "Przeglądanie logów" as UC_Logs
        usecase "Tworzenie kopii zapasowej" as UC_BackupHost
    }

    package "Usługa: VPS" {
        usecase "Zakup serwera VPS" as UC_BuyVPS
        usecase "Start / Stop / Restart VPS" as UC_VPSControl
        usecase "Dostęp do konsoli (noVNC)" as UC_Console
        usecase "Wybór systemu operacyjnego" as UC_OS
        usecase "Monitoring zasobów VPS" as UC_VPSMon
    }

    package "Usługa: Dysk wirtualny" {
        usecase "Zakup przestrzeni dyskowej" as UC_BuyDisk
        usecase "Upload / download plików" as UC_Upload
        usecase "Udostępnianie plików (linki)" as UC_Share
        usecase "Zarządzanie plikami" as UC_ManageFiles
    }

    package "Usługa: Poczta e-mail" {
        usecase "Tworzenie skrzynki e-mail" as UC_CreateMail
        usecase "Dostęp do webmail (Roundcube)" as UC_Webmail
    }

    package "Panel administracyjny" {
        usecase "Zarządzanie użytkownikami" as UC_ManageUsers
        usecase "Konfiguracja pakietów i cenników" as UC_Pricing
        usecase "Monitoring platformy" as UC_PlatformMon
        usecase "Przeglądanie logów systemowych" as UC_SysLogs
        usecase "Zarządzanie kopiami zapasowymi" as UC_AdminBackup
    }
}

' Klient niezalogowany
Guest --> UC_Browse
Guest --> UC_Compare
Guest --> UC_Register
Guest --> UC_Login

' Klient zalogowany
User --> UC_Dashboard
User --> UC_Password
User --> UC_Profile

User --> UC_BuyHost
User --> UC_Files
User --> UC_DB
User --> UC_PHP
User --> UC_Logs
User --> UC_BackupHost

User --> UC_BuyVPS
User --> UC_VPSControl
User --> UC_Console
User --> UC_OS
User --> UC_VPSMon

User --> UC_BuyDisk
User --> UC_Upload
User --> UC_Share
User --> UC_ManageFiles

User --> UC_CreateMail
User --> UC_Webmail

' Administrator
Admin --> UC_ManageUsers
Admin --> UC_Pricing
Admin --> UC_PlatformMon
Admin --> UC_SysLogs
Admin --> UC_AdminBackup

@enduml
