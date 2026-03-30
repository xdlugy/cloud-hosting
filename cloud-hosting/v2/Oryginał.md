# 1. Wstęp

### Wprowadzenie

Współczesne technologie hostingowe i chmurowe stanowią fundament infrastruktury IT wielu przedsiębiorstw oraz użytkowników indywidualnych na całym świecie. Transformacja cyfrowa, przyspieszona przez globalne wydarzenia ostatnich lat, fundamentalnie zmieniła sposób, w jaki organizacje podchodzą do zarządzania swoimi zasobami informatycznymi. Z rosnącym zapotrzebowaniem na elastyczność, skalowalność i dostępność danych, usługi chmurowe stały się jednym z najistotniejszych elementów współczesnej gospodarki cyfrowej, generując według prognoz Gartnera przychody przekraczające 600 miliardów dolarów w 2024 roku.

### Kontekst technologiczny i biznesowy

Dzięki rozwiązaniom chmurowym organizacje mogą optymalizować swoje zasoby, minimalizować koszty związane z infrastrukturą IT oraz uzyskać dostęp do zaawansowanych technologii bez potrzeby inwestowania w drogie sprzęty i oprogramowanie. Model pay-as-you-go pozwala przedsiębiorstwom na dynamiczne dostosowywanie wykorzystywanych zasobów do aktualnych potrzeb, co jest szczególnie istotne w kontekście nieprzewidywalności rynkowej i sezonowości biznesu.

Ewolucja technologii wirtualizacji, konteneryzacji oraz orkiestracji zasobów otworzyła nowe możliwości w zakresie projektowania systemów rozproszonych. Technologie takie jak Docker, Kubernetes czy KVM umożliwiają tworzenie wysoce skalowalnych i odpornych na awarie środowisk, które mogą obsługiwać wielu użytkowników jednocześnie przy zachowaniu wzajemnej izolacji. Jednocześnie rozwój modeli usługowych — Infrastructure as a Service (IaaS), Platform as a Service (PaaS) oraz Software as a Service (SaaS) — pozwala na dostosowanie oferty do specyficznych wymagań różnych segmentów rynku.

### Wyzwania współczesnego hostingu i rozwiązań chmurowych

Pomimo znaczących korzyści, implementacja rozwiązań chmurowych wiąże się z licznymi wyzwaniami. Kwestie bezpieczeństwa danych, zarządzania złożonością środowisk IT oraz optymalizacji kosztów wymagają zastosowania zaawansowanych narzędzi i metodologii. Dodatkowo, rosnące wymagania dotyczące wydajności i dostępności stawiają przed architektami systemów chmurowych coraz bardziej ambitne zadania.

Problem vendor lock-in, czyli uzależnienia od konkretnego dostawcy usług chmurowych, stał się istotnym czynnikiem wpływającym na decyzje architektoniczne. Organizacje coraz częściej poszukują rozwiązań opartych na otwartym oprogramowaniu, które pozwalają na zachowanie pełnej kontroli nad infrastrukturą i danymi.

### Cel i zakres pracy

Celem niniejszej pracy inżynierskiej jest zaprojektowanie i stworzenie kompleksowego systemu rozwiązań hostingowych i chmurowych, który umożliwia zdalne zarządzanie zasobami wirtualnymi w środowisku chmurowym. System ma na celu zapewnienie elastyczności, wydajności oraz bezpieczeństwa w kontekście hostingu aplikacji i przechowywania danych, przy jednoczesnym zachowaniu intuicyjności interfejsu użytkownika.

Platforma obejmuje trzy główne kategorie usług: hosting webowy z bazą danych i pocztą e-mail, wirtualne serwery prywatne (VPS) oparte na pełnej wirtualizacji sprzętowej oraz przestrzeń dyskową w chmurze. Całość opiera się wyłącznie na otwartym oprogramowaniu, co eliminuje koszty licencyjne i zapewnia pełną transparentność rozwiązania.

### Znaczenie i aktualność tematu

Podjęcie tematu pracy jest odpowiedzią na rosnące zapotrzebowanie rynku na nowoczesne rozwiązania hostingowe i chmurowe, które umożliwiają szybkie i bezpieczne przetwarzanie danych w dynamicznie zmieniającym się środowisku IT. Według raportu IDC, do 2025 roku ponad 85% przedsiębiorstw będzie wykorzystywać strategię cloud-first, co podkreśla kluczowe znaczenie rozwoju efektywnych i bezpiecznych platform chmurowych.

Praca wpisuje się w trend budowania samoobsługowych platform hostingowych opartych na technologiach open source, proponując rozwiązanie, które łączy konteneryzację (Docker, Kubernetes), pełną wirtualizację sprzętową (KVM/Proxmox VE), nowoczesne frameworki webowe (Symfony 7.4, React) oraz sprawdzone usługi infrastrukturalne (Postfix, PowerDNS, MinIO) w spójny, zintegrowany system.

# 2. Zakres funkcjonalny systemu

### Charakterystyka ogólna systemu

Projektowany system stanowi kompleksową platformę hostingową i chmurową, umożliwiającą użytkownikom zakup i zarządzanie różnorodnymi usługami IT poprzez interfejs webowy. System składa się z trzech głównych warstw: publicznej strony sprzedażowej opartej na WordPress z WooCommerce, panelu klienckiego do zarządzania usługami zbudowanego w Symfony 7.4 i React oraz panelu administracyjnego dla operatora platformy.

### Przepływ użytkownika w systemie

Potencjalny klient rozpoczyna swoją interakcję od publicznej strony sprzedażowej, gdzie może zapoznać się z ofertą usług podzielonych na trzy główne kategorie: hosting webowy z bazą danych i pocztą e-mail, wirtualne serwery prywatne (VPS) oraz przestrzeń dyskową w chmurze. Strona prezentuje porównanie dostępnych pakietów cenowych.

Proces zakupu rozpoczyna się od rejestracji konta — nowi użytkownicy wypełniają formularz rejestracyjny i weryfikują adres e-mail, natomiast istniejący klienci logują się do systemu. Po zalogowaniu użytkownik przechodzi przez proces konfiguracji wybranej usługi, gdzie określa parametry techniczne, takie jak wersja PHP dla hostingu, system operacyjny dla VPS czy wielkość przestrzeni dyskowej.

Po dokonaniu płatności system automatycznie uruchamia proces provisioningu — tworzenia i konfiguracji wybranej usługi. Zadanie jest kolejkowane w RabbitMQ i wykonywane asynchronicznie przez uprzywilejowanego workera. Klient jest informowany o postępie w panelu klienta.

### Panel zarządzania usługami

Po aktywacji usługi klient otrzymuje dostęp do dashboardu, który centralizuje zarządzanie wszystkimi produktami. Panel prezentuje status aktywnych usług, terminy płatności oraz umożliwia szybkie wykonywanie podstawowych operacji. Każda usługa posiada dedykowany widok zarządzania dostosowany do jej specyfiki:

- **Hosting webowy** — menedżer plików, panel administracyjny bazy danych, konfiguracja wersji PHP, przeglądanie logów, tworzenie kopii zapasowych.
- **VPS** — start, stop i restart maszyny wirtualnej, dostęp do konsoli przez noVNC w przeglądarce, monitoring zasobów (CPU, RAM, dysk).
- **Dysk wirtualny** — przeglądanie i zarządzanie plikami, upload przez drag & drop, generowanie publicznych linków udostępniania z opcją daty wygaśnięcia.

### Oferowane usługi

**Hosting Web** to rozwiązanie obejmujące przestrzeń na strony internetowe z obsługą PHP w wielu wersjach, automatycznie skonfigurowany serwer web nginx z PHP-FPM lub Apache, bazę danych do wyboru (MySQL, PostgreSQL lub MongoDB) z panelem administracyjnym oraz skrzynkę pocztową z dostępem przez webmail Roundcube. Każdy klient otrzymuje izolowany zestaw kontenerów Docker orkiestrowanych przez Kubernetes, co zapewnia separację zasobów i stabilność środowiska. System automatycznie generuje certyfikat SSL Let's Encrypt dla każdej domeny.

**Środowiska VPS** zapewniają użytkownikom dedykowane maszyny wirtualne z pełnym dostępem administracyjnym (root). Maszyny wirtualne są uruchamiane przy użyciu technologii KVM z wirtualizacją sprzętową, zarządzane przez Proxmox VE. Klienci mogą wybierać spośród dystrybucji systemu Linux (Ubuntu, Debian, CentOS), a konfiguracja początkowa jest automatyzowana za pomocą cloud-init. Dostęp do konsoli w przeglądarce zapewnia technologia noVNC.

**Dysk wirtualny** funkcjonuje jako przestrzeń w chmurze dostępna z dowolnego urządzenia przez przeglądarkę internetową. Backend oparty jest na MinIO — systemie obiektowym kompatybilnym z API Amazon S3. Usługa umożliwia przesyłanie plików metodą drag & drop, generowanie publicznych linków do udostępniania plików z opcjonalną datą wygaśnięcia oraz przywracanie usuniętych plików.

### Zarządzanie platformą

Administrator systemu dysponuje panelem kontrolnym, który pozwala na zarządzanie całą platformą. Interfejs administratora umożliwia przeglądanie i edycję kont użytkowników, konfigurację pakietów usług i cenników, monitorowanie zasobów infrastruktury (Prometheus + Grafana) oraz przeglądanie logów systemowych. System loguje wszystkie operacje użytkowników i administratorów w tabeli audit_logs, co zapewnia pełną rozliczalność działań.

# 3. Wykorzystywane narzędzia i technologie

## 3.1. Warstwa prezentacji i sprzedaży

Publiczna strona platformy hostingowej jest zbudowana w oparciu o system zarządzania treścią **WordPress** wraz z wtyczką e-commerce **WooCommerce**. Rozwiązanie to zapewnia szybkie wdrożenie funkcjonalności sprzedażowych, łatwą modyfikację treści marketingowych oraz gotowe integracje z systemami płatności. WordPress pełni rolę front-endu dla potencjalnych klientów — prezentuje ofertę, umożliwia porównywanie pakietów i finalizację zakupu. WooCommerce obsługuje koszyk, proces checkout oraz zarządzanie zamówieniami. Po dokonaniu płatności WooCommerce wysyła webhook do API Symfony, inicjując proces provisioningu wybranej usługi.

## 3.2. Panel zarządzania klienta

### Backend — Symfony 7.4

Główny panel użytkownika jest zaimplementowany jako aplikacja webowa w PHP wykorzystująca framework **Symfony 7.4**. Framework zapewnia:

- **Doctrine ORM** — mapowanie obiektowo-relacyjne do komunikacji z bazą danych PostgreSQL, automatyczne migracje schematu, repozytorium encji.
- **Symfony Security** — system uwierzytelniania (JWT) i autoryzacji (RBAC), ochrona endpointów API, zarządzanie sesjami.
- **Symfony Messenger** — komponent kolejkowania zadań, integracja z RabbitMQ jako transportem wiadomości, obsługa asynchronicznych operacji provisioningu.
- **API REST** — panel komunikuje się z frontendem poprzez interfejs REST API z dokumentacją OpenAPI/Swagger.

### Frontend — React + TailwindCSS

Interfejs użytkownika panelu klienta i administratora jest zrealizowany jako aplikacja jednostronicowa (SPA) w technologii **React**. Do stylowania komponentów wykorzystano framework CSS **TailwindCSS**, który zapewnia responsywność interfejsu i spójny system projektowy. Komunikacja z backendem odbywa się poprzez żądania HTTP do API REST (format JSON). Aktualizacje statusu usług (np. postęp provisioningu) mogą być realizowane za pomocą mechanizmu polling lub Server-Sent Events.

## 3.3. Baza danych platformy

Jako główną bazę danych platformy wybrano **PostgreSQL**. Przechowuje ona wszystkie dane operacyjne systemu: konta użytkowników, informacje o zamówieniach i usługach, metadane plików, konfigurację pakietów cenowych oraz logi audytowe. Doctrine ORM umożliwia definiowanie schematu bazy za pomocą encji PHP i automatyczne generowanie migracji.

## 3.4. Bazy danych klientów

Klienci usługi hostingowej mogą wybrać spośród trzech systemów bazodanowych:

- **MySQL** — relacyjna baza danych, najpopularniejszy wybór dla aplikacji PHP (WordPress, Joomla, Drupal).
- **PostgreSQL** — zaawansowana relacyjna baza z obsługą typów JSON, pełnotekstowego wyszukiwania i transakcji MVCC.
- **MongoDB** — dokumentowa baza danych NoSQL, odpowiednia dla aplikacji wymagających elastycznego schematu danych.

Każda instancja bazy klienta działa w izolowanym kontenerze Docker z dedykowanymi zasobami i wolumenami do trwałego przechowywania danych.

## 3.5. Infrastruktura hostingu webowego

Usługi hostingowe są realizowane poprzez konteneryzację z wykorzystaniem **Docker** oraz orkiestracji **Kubernetes**. Każdy klient otrzymuje izolowany zestaw kontenerów (namespace Kubernetes) składający się z:

- **nginx** — serwer webowy obsługujący żądania HTTP i przekazujący je do procesów PHP-FPM.
- **PHP-FPM** — interpreter PHP w wybranej wersji, uruchomiony jako dedykowany proces.
- **Baza danych** — kontener z wybranym systemem bazodanowym (MySQL, PostgreSQL lub MongoDB).

Limity zasobów (CPU, RAM, przestrzeń dyskowa) są egzekwowane na poziomie Kubernetes (ResourceQuota, LimitRange). Izolacja sieciowa jest realizowana za pomocą Network Policies w Kubernetes.

## 3.6. Wirtualizacja VPS

Środowiska VPS są uruchamiane przy użyciu technologii **KVM** (Kernel-based Virtual Machine) zarządzanej przez **Proxmox VE**. KVM zapewnia pełną wirtualizację sprzętową z wydajnością zbliżoną do natywnej, umożliwiając uruchamianie pełnych systemów operacyjnych. Proxmox VE udostępnia API REST do programowego zarządzania maszynami wirtualnymi.

Każdy VPS otrzymuje dedykowane zasoby CPU, RAM i dysku zgodnie z wybranym pakietem. Systemy operacyjne Ubuntu, Debian i CentOS są dostępne jako szablony z **cloud-init** do automatycznej konfiguracji początkowej (hostname, klucze SSH, konfiguracja sieciowa). Dostęp do konsoli w przeglądarce jest realizowany za pomocą **noVNC** — klienta VNC działającego w technologii HTML5.

## 3.7. System pocztowy

Infrastruktura e-mail jest oparta o trzy główne komponenty:

- **Postfix** — MTA (Mail Transfer Agent) odpowiedzialny za wysyłkę i odbieranie poczty elektronicznej za pomocą protokołu SMTP.
- **Dovecot** — serwer IMAP/POP3 zapewniający dostęp do skrzynek pocztowych z poziomu klientów pocztowych.
- **Roundcube** — interfejs webmail dostępny przez przeglądarkę, umożliwiający odczyt i wysyłkę poczty bez konieczności instalacji klienta pocztowego.

System pocztowy jest konfigurowany automatycznie podczas provisioningu usługi hostingowej. Skrzynki pocztowe są przechowywane na wolumenach z trwałym zapisem danych.

## 3.8. System DNS

Zarządzanie rekordami DNS jest realizowane przez **PowerDNS** z backendem MySQL. PowerDNS udostępnia API REST, które umożliwia automatyczne tworzenie i modyfikację stref DNS oraz rekordów (A, AAAA, CNAME, MX, TXT) podczas provisioningu nowych usług. Dla każdej nowo dodanej domeny system automatycznie tworzy podstawowe rekordy DNS wskazujące na odpowiednią infrastrukturę.

## 3.9. Przestrzeń dyskowa w chmurze

Usługa dysku wirtualnego wykorzystuje **MinIO** — system przechowywania obiektowego kompatybilny z API Amazon S3. MinIO zapewnia skalowalność, redundancję danych oraz integrację z bibliotekami S3 dostępnymi w PHP (AWS SDK for PHP lub Flysystem z adapterem S3). Każdy użytkownik otrzymuje dedykowany bucket z politykami dostępu ograniczającymi widoczność wyłącznie do własnych plików. Upload plików odbywa się przez API REST, a udostępnianie — za pomocą presigned URLs z opcjonalną datą wygaśnięcia.

## 3.10. Reverse proxy i terminacja SSL

**Nginx** pełni rolę reverse proxy i load balancera dla całej platformy. Konfiguracja wirtualnych hostów (server blocks) kieruje ruch do odpowiednich serwisów na podstawie nazwy domeny i ścieżki URL. Certyfikaty SSL od **Let's Encrypt** są pobierane i odnawiane automatycznie za pomocą narzędzia **Certbot**, które integruje się z Nginx i modyfikuje konfigurację serwera w celu obsługi HTTPS.

## 3.11. Kolejkowanie zadań

**RabbitMQ** pełni rolę brokera wiadomości dla asynchronicznych operacji systemowych. Symfony Messenger publikuje zadania (np. CreateHostingJob, CreateVPSJob) do kolejek RabbitMQ, skąd są konsumowane przez uprzywilejowanych workerów. Wzorzec ten zapewnia:

- Separację uprawnień — proces webowy (www-data) nigdy nie wykonuje operacji wymagających podwyższonych uprawnień.
- Niezawodność — nieudane zadania mogą być automatycznie ponawiane.
- Skalowalność — liczba workerów może być niezależnie zwiększana.
- Audytowalność — każde zadanie jest rejestrowane w logach.

## 3.12. Monitoring

Infrastruktura jest monitorowana przez stos **Prometheus** i **Grafana**. Prometheus zbiera metryki z poszczególnych komponentów systemu (kontenerów, maszyn wirtualnych, serwisów) za pomocą modelu pull. Grafana zapewnia wizualizację metryk w formie konfigurowalnych dashboardów oraz system alertów powiadamiających administratora o anomaliach.

## 3.13. Kopie zapasowe

Kopie zapasowe są realizowane przez **Restic** — narzędzie do tworzenia zaszyfrowanych, zdeduplikowanych backupów. Kopie są przechowywane w MinIO (object storage). Restic obsługuje przyrostowe kopie zapasowe, co minimalizuje zużycie przestrzeni dyskowej i czas wykonywania backupu.

## 3.14. CI/CD

Procesy ciągłej integracji i wdrażania są obsługiwane przez **GitLab CI/CD**. Pipeline obejmuje automatyczne uruchamianie testów, budowanie obrazów Docker oraz wdrażanie na środowisko produkcyjne.

## 3.15. Infrastruktura sprzętowa

System działa na serwerze dedykowanym Hetzner z 8 GB RAM i 80 GB przestrzeni dyskowej. Wybór serwera o ograniczonych zasobach jest celowy — platforma stanowi projekt inżynierski o charakterze demonstracyjnym, którego celem jest zaprojektowanie i weryfikacja architektury systemu, a nie obsługa ruchu produkcyjnego. Serwer o takich parametrach jest wystarczający do uruchomienia wszystkich komponentów platformy w minimalnej konfiguracji (po jednej instancji każdego serwisu) oraz obsługi kilku testowych usług klienckich, jednocześnie utrzymując koszty infrastruktury na niskim poziomie. System operacyjny hosta to **Debian 13**, na którym zainstalowano **Proxmox VE** jako hypervisor zarządzający zarówno maszynami wirtualnymi VPS klientów, jak i maszynami wirtualnymi wewnętrznymi platformy (serwer aplikacji, bazy danych, monitoring, poczta, DNS). Architektura została zaprojektowana tak, aby w przyszłości możliwe było skalowanie horyzontalne przez dodanie kolejnych węzłów do klastra Proxmox VE bez konieczności zmian w logice aplikacji.

# 4. Projekt architektury systemu

## 4.1. Wymagania

### Hosting Web

#### Wymagania funkcjonalne

- System musi umożliwiać wybór wersji PHP dla każdej usługi hostingowej.
- System musi automatycznie generować certyfikat SSL Let's Encrypt dla każdej domeny.
- System musi umożliwiać wybór silnika bazodanowego (MySQL, PostgreSQL lub MongoDB).
- System musi zapewniać dostęp FTP/SFTP do katalogu głównego hostingu.
- System musi umożliwiać tworzenie kopii zapasowych i przywracanie danych.

#### Wymagania niefunkcjonalne

- Każdy klient musi działać w izolowanym środowisku kontenerowym.
- Provisioning nowej usługi hostingowej nie powinien przekraczać 3 minut.

### Dysk wirtualny (Cloud Storage)

#### Wymagania funkcjonalne

- System musi umożliwiać upload plików przez przeglądarkę (w tym drag & drop).
- System musi generować publiczne linki do udostępniania plików z opcją daty wygaśnięcia.
- System musi umożliwiać przywrócenie usuniętych plików (soft delete).

#### Wymagania niefunkcjonalne

- Maksymalny rozmiar pojedynczego pliku uploadowanego przez przeglądarkę musi wynosić co najmniej 2 GB.

### VPS (Virtual Private Server)

#### Wymagania funkcjonalne

- System musi umożliwiać restart, zatrzymanie i uruchomienie VPS z poziomu panelu.
- System musi umożliwiać wybór systemu operacyjnego spośród dostępnych szablonów.
- System musi zapewniać dostęp do konsoli VPS przez przeglądarkę (noVNC).

#### Wymagania niefunkcjonalne

- Czas provisioningu nowego VPS nie powinien przekraczać 5 minut.
- Maszyny wirtualne muszą wykorzystywać pełną wirtualizację sprzętową (KVM).

### Poczta e-mail

#### Wymagania funkcjonalne

- System musi umożliwiać tworzenie skrzynek pocztowych powiązanych z domeną hostingową.
- System musi zapewniać dostęp do poczty przez interfejs webmail (Roundcube).

### Panel użytkownika

#### Wymagania funkcjonalne

- System musi wyświetlać status wszystkich aktywnych usług na dashboardzie.
- System musi umożliwiać zmianę hasła z walidacją siły hasła.
- System musi logować operacje użytkowników w celach audytowych.

#### Wymagania niefunkcjonalne

- Panel musi być responsywny i działać na urządzeniach mobilnych.
- Komunikacja z API musi być zabezpieczona tokenami JWT.

## 4.2. Architektura systemu

### 4.2.1. Diagram komponentów

Diagram komponentów przedstawia podział systemu na warstwy i moduły oraz zależności między nimi. System składa się z pięciu warstw: prezentacji, aplikacji, komunikacji, infrastruktury oraz danych.

Warstwa prezentacji obejmuje publiczną stronę sprzedażową (WordPress + WooCommerce) oraz panel klienta i administratora (React SPA). Warstwa aplikacji zawiera API Gateway (Symfony 7.4) z modułami provisioningu, monitoringu, kopii zapasowych i rozliczeniowym. Warstwa komunikacji opiera się na RabbitMQ (broker wiadomości) i Nginx (reverse proxy). Warstwa infrastruktury realizuje faktyczne usługi: Kubernetes z kontenerami Docker (hosting web), Proxmox VE z KVM (VPS), Postfix + Dovecot + Roundcube (poczta) oraz PowerDNS (DNS). Warstwa danych obejmuje PostgreSQL (dane platformy), bazy klientów (MySQL/PostgreSQL/MongoDB) oraz MinIO (storage obiektowy).

Pełna definicja diagramu PlantUML: [diagram-komponentow.md](diagram-komponentow.md)

### 4.2.2. Diagram wdrożenia

Diagram wdrożenia prezentuje fizyczny rozkład komponentów systemu na infrastrukturze sprzętowej. Cała platforma działa na jednym serwerze dedykowanym Hetzner z systemem Debian 13 i zainstalowanym Proxmox VE jako hypervisorem.

Proxmox VE zarządza następującymi maszynami wirtualnymi:

- **VM Klaster Kubernetes** — Nginx (reverse proxy), kontenery klientów (nginx + PHP-FPM), obsługa certyfikatów Let's Encrypt (Certbot).
- **VM Serwer aplikacji** — Symfony 7.4 API, React SPA, RabbitMQ, Worker Symfony Messenger.
- **VM Bazy danych** — PostgreSQL (dane platformy), MySQL/PostgreSQL/MongoDB (bazy klientów).
- **VM Poczta e-mail** — Postfix, Dovecot, Roundcube.
- **VM Storage i monitoring** — MinIO, Prometheus, Grafana, Restic.
- **VM DNS** — PowerDNS.
- **VM klientów VPS** — maszyny wirtualne KVM z systemami operacyjnymi klientów, konfigurowane przez cloud-init.

Cały ruch zewnętrzny trafia na port HTTPS (443) i jest kierowany przez Nginx do odpowiednich serwisów.

Pełna definicja diagramu PlantUML: [diagram-wdrozenia.md](diagram-wdrozenia.md)

### 4.2.3. Diagram przypadków użycia

Diagram przypadków użycia identyfikuje trzech aktorów systemu: klienta niezalogowanego, klienta zalogowanego oraz administratora.

**Klient niezalogowany** może przeglądać ofertę, porównywać pakiety, zarejestrować konto i zalogować się.

**Klient zalogowany** ma dostęp do zarządzania kontem (zmiana hasła, edycja profilu, dashboard), usługami hostingowymi (zakup, zarządzanie plikami, bazą danych, konfiguracją PHP, logami, kopiami zapasowymi), serwerami VPS (zakup, kontrola cyklu życia, konsola, monitoring), dyskiem wirtualnym (zakup, upload/download, udostępnianie) oraz pocztą e-mail (tworzenie skrzynek, webmail).

**Administrator** zarządza użytkownikami, konfiguracją pakietów i cenników, monitoruje platformę, przegląda logi systemowe oraz zarządza kopiami zapasowymi.

Pełna definicja diagramu PlantUML: [diagram-przypadkow-uzycia.md](diagram-przypadkow-uzycia.md)

### 4.2.4. Diagram przepływu danych

Diagram przepływu danych ilustruje kierunki przepływu informacji między komponentami systemu. Cały ruch zewnętrzny (HTTP/HTTPS) wchodzi do systemu przez Nginx, który pełni rolę single point of entry. Nginx terminuje SSL i kieruje żądania do odpowiednich serwisów na podstawie nazwy domeny i ścieżki URL (konfiguracja server blocks z proxy_pass).

Żądania do panelu klienta są kierowane do React SPA, które komunikuje się z Symfony API przez REST. API wykonuje operacje CRUD na bazie PostgreSQL za pośrednictwem Doctrine ORM. Operacje infrastrukturalne (provisioning, konfiguracja) są kolejkowane w RabbitMQ i wykonywane asynchronicznie przez workera Symfony Messenger, który komunikuje się z odpowiednimi serwisami: Kubernetes API (hosting), Proxmox API (VPS), PowerDNS API (DNS), systemem pocztowym (e-mail).

Pełna definicja diagramu PlantUML: [diagram-przeplywy-danych.md](diagram-przeplywy-danych.md)

### 4.2.5. Model danych (ERD)

Schemat bazy danych platformy (PostgreSQL) obejmuje następujące główne encje:

- **users** — dane kont użytkowników (e-mail, hasło, imię, nazwisko, rola).
- **hosting_services** — usługi hostingowe (domena, pakiet, wersja PHP, typ bazy, status, namespace Kubernetes).
- **vps_services** — usługi VPS (pakiet, szablon OS, zasoby CPU/RAM/dysk, adres IP, ID maszyny wirtualnej w Proxmox, port VNC, status).
- **storage_services** — usługi dysku wirtualnego (pakiet, limit i wykorzystanie przestrzeni, bucket MinIO, status).
- **storage_files** — pliki przechowywane na dysku wirtualnym (nazwa, ścieżka, rozmiar, typ MIME, soft delete).
- **file_shares** — linki udostępniania plików (token, URL, data wygaśnięcia).
- **email_accounts** — skrzynki pocztowe powiązane z usługą hostingową (adres, hasło, quota).
- **orders** — zamówienia (typ usługi, pakiet, kwota, status płatności, referencja płatności).
- **audit_logs** — logi audytowe (użytkownik, akcja, zasób, adres IP, szczegóły w formacie JSONB).

Relacje: użytkownik posiada wiele usług hostingowych, VPS i dyskowych; użytkownik składa wiele zamówień; usługa hostingowa zawiera wiele skrzynek e-mail; usługa dyskowa przechowuje wiele plików; plik może mieć wiele linków udostępniania.

Pełna definicja diagramu PlantUML: [diagram-erd.md](diagram-erd.md)

## 4.3. Procesy systemowe

### 4.3.1. Provisioning usługi hostingowej

Proces tworzenia usługi hostingowej przebiega następująco:

1. Klient wybiera pakiet hostingowy i konfigurację (domena, wersja PHP, typ bazy danych) w panelu React.
2. Frontend wysyła żądanie POST do API Symfony.
3. API waliduje dane, zapisuje zamówienie w PostgreSQL ze statusem „pending" i publikuje zadanie CreateHostingJob do kolejki RabbitMQ.
4. API zwraca odpowiedź 202 Accepted z identyfikatorem zamówienia.
5. Worker Symfony Messenger konsumuje zadanie i wykonuje sekwencję operacji:
   - Tworzenie rekordu DNS w PowerDNS (A record wskazujący na IP serwera).
   - Tworzenie namespace w Kubernetes.
   - Deploy kontenerów: nginx + PHP-FPM (Docker Compose stack).
   - Deploy kontenera bazy danych (MySQL/PostgreSQL/MongoDB).
   - Konfiguracja Nginx Ingress (server block z proxy_pass do serwisu klienta).
   - Żądanie certyfikatu SSL od Let's Encrypt za pomocą Certbot.
6. Worker aktualizuje status usługi na „active" w PostgreSQL.
7. Klient widzi aktywną usługę z danymi dostępowymi w panelu.

Pełna definicja diagramu sekwencji PlantUML: [diagram-sekwencji-provisioning-hosting.md](diagram-sekwencji-provisioning-hosting.md)

### 4.3.2. Provisioning serwera VPS

Proces tworzenia maszyny wirtualnej VPS:

1. Klient wybiera pakiet VPS, system operacyjny i parametry (CPU, RAM, dysk) w panelu React.
2. Frontend wysyła żądanie POST do API Symfony.
3. API waliduje dostępność zasobów, zapisuje zamówienie w PostgreSQL i publikuje zadanie CreateVPSJob do kolejki RabbitMQ.
4. API zwraca odpowiedź 202 Accepted.
5. Worker konsumuje zadanie i wykonuje sekwencję operacji:
   - Klonowanie szablonu VM z repozytorium Proxmox (cloud-init template).
   - Konfiguracja zasobów (CPU, RAM, dysk) zgodnie z wybranym pakietem.
   - Ustawienie cloud-init (hostname, klucze SSH, konfiguracja sieciowa).
   - Uruchomienie maszyny wirtualnej.
   - Tworzenie rekordu DNS (A record wskazujący na przydzielony adres IP VPS).
6. Worker zapisuje dane VPS (adres IP, port VNC, status) w PostgreSQL.
7. Klient widzi aktywny VPS z danymi dostępowymi (IP, SSH, link do konsoli noVNC).

Pełna definicja diagramu sekwencji PlantUML: [diagram-sekwencji-provisioning-vps.md](diagram-sekwencji-provisioning-vps.md)

### 4.3.3. Upload plików na dysk wirtualny

Proces przesyłania plików:

1. Klient przeciąga pliki do obszaru drag & drop w panelu React.
2. Frontend waliduje rozmiar i typ plików po stronie klienta.
3. Frontend wysyła żądanie POST (multipart form data) do API Symfony z tokenem JWT.
4. API weryfikuje token i uprawnienia użytkownika.
5. API sprawdza dostępny limit przestrzeni dyskowej w PostgreSQL.
6. Jeśli limit jest przekroczony — zwraca błąd 413.
7. Jeśli przestrzeń jest dostępna — API przesyła plik do MinIO (PUT Object do bucketu użytkownika).
8. API zapisuje metadane pliku w PostgreSQL (nazwa, rozmiar, typ MIME, data utworzenia).
9. API zwraca odpowiedź 201 Created z danymi pliku.
10. Klient widzi plik na liście.

Generowanie linku udostępniania: klient klika „Udostępnij", API generuje presigned URL w MinIO z datą wygaśnięcia i zapisuje token linku w PostgreSQL.

Pełna definicja diagramu sekwencji PlantUML: [diagram-sekwencji-upload-plikow.md](diagram-sekwencji-upload-plikow.md)

# 5. Implementacja systemu

## 5.1. Środowisko deweloperskie i infrastruktura

### Przygotowanie serwera

Implementacja rozpoczyna się od konfiguracji serwera dedykowanego Hetzner. Na serwerze instalowany jest system operacyjny Debian 13, a następnie Proxmox VE jako warstwa zarządzania wirtualizacją. Proxmox VE udostępnia interfejs webowy oraz API REST do tworzenia i zarządzania maszynami wirtualnymi, na których działają poszczególne komponenty platformy.

Maszyny wirtualne platformy są tworzone z określonymi zasobami:

- VM serwera aplikacji — Symfony API, React SPA, RabbitMQ, Worker.
- VM baz danych — PostgreSQL (platforma), instancje klientów (MySQL, PostgreSQL, MongoDB).
- VM Kubernetes — klaster z Nginx (reverse proxy) i kontenerami hostingowymi.
- VM poczty — Postfix, Dovecot, Roundcube.
- VM storage/monitoring — MinIO, Prometheus, Grafana, Restic.
- VM DNS — PowerDNS.

### Konfiguracja sieci

Maszyny wirtualne platformy komunikują się przez wewnętrzną sieć wirtualną zarządzaną przez Proxmox VE. Nginx stanowi jedyny punkt wejścia dla ruchu zewnętrznego (port 443 HTTPS). Komunikacja wewnętrzna między komponentami odbywa się w sieci prywatnej, niedostępnej z Internetu.

## 5.2. Backend — Symfony 7.4

### Struktura projektu

Projekt Symfony jest zorganizowany zgodnie z konwencjami frameworka:

- `src/Controller/` — kontrolery API REST (HostingController, VpsController, StorageController, UserController, AdminController).
- `src/Entity/` — encje Doctrine odwzorowujące tabele PostgreSQL (User, HostingService, VpsService, StorageService, StorageFile, FileShare, EmailAccount, Order, AuditLog).
- `src/Repository/` — repozytoria Doctrine z zapytaniami do bazy danych.
- `src/Service/` — serwisy logiki biznesowej (HostingProvisioner, VpsProvisioner, StorageManager, DnsManager, EmailManager).
- `src/Message/` — klasy wiadomości Symfony Messenger (CreateHostingJob, CreateVPSJob, DeleteHostingJob itp.).
- `src/MessageHandler/` — handlery konsumujące wiadomości z RabbitMQ i wykonujące operacje infrastrukturalne.
- `config/packages/` — konfiguracja frameworka (doctrine.yaml, messenger.yaml, security.yaml).

### Uwierzytelnianie i autoryzacja

System uwierzytelniania oparty jest na tokenach JWT (JSON Web Tokens). Po zalogowaniu użytkownik otrzymuje token JWT, który jest przesyłany w nagłówku Authorization każdego żądania do API. Symfony Security weryfikuje token i ustala tożsamość oraz rolę użytkownika (client lub admin). Endpointy administracyjne są chronione za pomocą mechanizmu RBAC (Role-Based Access Control).

### Wzorzec uprzywilejowanego demona

Kluczowym wzorcem architektonicznym jest separacja uprawnień. Proces webowy Symfony (działający jako użytkownik www-data) nigdy nie wykonuje bezpośrednio operacji wymagających uprawnień administratora systemu. Zamiast tego:

1. Kontroler API waliduje żądanie i publikuje wiadomość do kolejki RabbitMQ za pomocą Symfony Messenger.
2. Worker Symfony Messenger — uruchomiony jako osobny proces z odpowiednimi uprawnieniami — konsumuje wiadomość i wykonuje operację infrastrukturalną (tworzenie kontenerów, maszyn wirtualnych, rekordów DNS itp.).
3. Worker aktualizuje status operacji w bazie danych.

Wzorzec ten zapewnia bezpieczeństwo (webowy proces nie ma uprawnień do operacji systemowych), niezawodność (nieudane zadania są automatycznie ponawiane przez Messenger) oraz audytowalność (każde zadanie jest rejestrowane).

### Komunikacja z infrastrukturą

Worker Symfony Messenger komunikuje się z poszczególnymi komponentami infrastruktury za pośrednictwem ich API:

- **Kubernetes API** — tworzenie namespace, deployment kontenerów, konfiguracja IngressRoute (biblioteka PHP kubernetes-client).
- **Proxmox VE API** — klonowanie szablonów VM, konfiguracja zasobów, start/stop maszyn (REST API z tokenem uwierzytelniającym).
- **PowerDNS API** — tworzenie i modyfikacja stref DNS i rekordów (REST API).
- **MinIO API** — zarządzanie bucketami i politykami dostępu (AWS SDK for PHP z endpointem MinIO).

## 5.3. Frontend — React + TailwindCSS

### Struktura projektu

Aplikacja React jest zorganizowana funkcjonalnie:

- `src/components/` — współdzielone komponenty UI (nagłówek, nawigacja, tabele, formularze).
- `src/pages/` — komponenty stron (Dashboard, HostingManage, VpsManage, StorageManage, AdminPanel).
- `src/api/` — moduły komunikacji z API REST (konfiguracja Axios, definicje endpointów).
- `src/hooks/` — niestandardowe hooki React (useAuth, usePolling, useFileUpload).
- `src/context/` — konteksty React do zarządzania stanem globalnym (AuthContext).

### Interfejs użytkownika

Dashboard prezentuje listę aktywnych usług z ich statusem, szybkimi akcjami i datami wygaśnięcia. Każda usługa posiada dedykowany widok zarządzania:

- **Hosting** — widok menedżera plików, panel bazy danych, konfiguracja PHP, logi.
- **VPS** — panel sterowania (start/stop/restart), terminal noVNC osadzony w iframe, wykresy monitoringu zasobów (dane z Prometheus/Grafana).
- **Dysk wirtualny** — przeglądarka plików z funkcją drag & drop upload, lista udostępnionych linków.

TailwindCSS zapewnia responsywność interfejsu (media queries na poziomie klas CSS), a komponenty są projektowane mobile-first.

## 5.4. Provisioning hostingu — Docker + Kubernetes

### Tworzenie środowiska klienta

Provisioning nowej usługi hostingowej składa się z następujących kroków technicznych:

1. **Tworzenie namespace Kubernetes** — każdy klient otrzymuje izolowany namespace (np. `hosting-{user_id}-{service_id}`), w którym działają jego kontenery.
2. **Deploy kontenerów** — worker tworzy obiekty Kubernetes (Deployment, Service, PersistentVolumeClaim) definiujące:
   - Kontener nginx — serwer webowy z konfiguracją reverse proxy do PHP-FPM.
   - Kontener PHP-FPM — interpreter PHP w wybranej wersji z zamontowanym wolumenem plików klienta.
   - Kontener bazy danych — MySQL, PostgreSQL lub MongoDB z dedykowanym wolumenem danych.
3. **Konfiguracja Nginx Ingress** — server block z dyrektywą proxy_pass kierującą ruch z domeny klienta do odpowiedniego serwisu w namespace.
4. **Certyfikat SSL** — Certbot automatycznie żąda certyfikatu Let's Encrypt dla domeny klienta i konfiguruje Nginx do obsługi HTTPS.
5. **Konfiguracja DNS** — worker tworzy rekord A w PowerDNS wskazujący domenę na publiczny adres IP serwera.

### Zarządzanie zasobami

Limity zasobów są definiowane na poziomie Kubernetes:

- **ResourceQuota** — ogranicza łączne zasoby namespace (CPU, RAM, liczbę podów).
- **LimitRange** — określa domyślne i maksymalne zasoby dla pojedynczego kontenera.
- **PersistentVolumeClaim** — alokuje przestrzeń dyskową zgodnie z wykupionym pakietem.

## 5.5. Provisioning VPS — KVM + Proxmox VE

### Tworzenie maszyny wirtualnej

1. **Klonowanie szablonu** — worker wywołuje Proxmox API w celu sklonowania przygotowanego szablonu VM (np. ubuntu-22.04-cloudinit).
2. **Konfiguracja zasobów** — ustawienie liczby rdzeni CPU, ilości RAM i rozmiaru dysku zgodnie z wybranym pakietem.
3. **Cloud-init** — automatyczna konfiguracja systemu operacyjnego: ustawienie hostname, wstrzyknięcie kluczy SSH użytkownika, konfiguracja interfejsu sieciowego (adres IP, brama, DNS).
4. **Uruchomienie VM** — start maszyny wirtualnej przez Proxmox API.
5. **Rekord DNS** — tworzenie rekordu A w PowerDNS wskazującego na przydzielony adres IP VPS.

### Zarządzanie cyklem życia

Panel klienta umożliwia podstawowe operacje zarządzania VPS za pośrednictwem Proxmox API:

- **Start** — uruchomienie zatrzymanej maszyny.
- **Stop** — bezpieczne zatrzymanie (ACPI shutdown) lub wymuszenie zatrzymania (hard stop).
- **Restart** — restart maszyny (reboot).
- **Konsola** — dostęp do konsoli VNC przez przeglądarkę za pomocą noVNC. Proxmox generuje jednorazowy ticket VNC, który jest przekazywany do klienta noVNC osadzonego w panelu.

## 5.6. Dysk wirtualny — MinIO

### Architektura storage

MinIO jest skonfigurowany z jednym lub wieloma dyskami, zapewniając trwałość danych. Każdy użytkownik posiada dedykowany bucket (np. `user-{user_id}`), a polityki dostępu IAM ograniczają operacje wyłącznie do własnego bucketu.

### Operacje na plikach

- **Upload** — plik jest przesyłany z przeglądarki do API Symfony (multipart form data), które po walidacji przekazuje go do MinIO (PUT Object). Metadane pliku są zapisywane w PostgreSQL.
- **Download** — API generuje presigned URL do MinIO, który jest przekazywany do przeglądarki klienta.
- **Udostępnianie** — API tworzy presigned URL z określonym czasem wygaśnięcia i zapisuje token udostępniania w PostgreSQL. Link jest dostępny publicznie bez uwierzytelniania.
- **Usuwanie** — soft delete w PostgreSQL (ustawienie flagi is_deleted i daty deleted_at). Plik pozostaje w MinIO przez okres retencji, po którym jest trwale usuwany przez zadanie cron.

## 5.7. System pocztowy

### Konfiguracja komponentów

Podczas provisioningu usługi hostingowej worker konfiguruje system pocztowy:

1. **Postfix** — tworzenie wirtualnej domeny pocztowej i mapowań adresów w konfiguracji Postfix.
2. **Dovecot** — konfiguracja skrzynki pocztowej z katalogiem maildir na wolumenie z trwałym zapisem.
3. **Roundcube** — webmail jest dostępny pod adresem powiązanym z domeną klienta, kierowanym przez Nginx.

Tworzenie nowych skrzynek e-mail odbywa się z poziomu panelu klienta — API Symfony aktualizuje konfigurację Postfix i Dovecot za pośrednictwem workera.

## 5.8. System DNS — PowerDNS

PowerDNS jest skonfigurowany z backendem MySQL i udostępnia API REST na porcie wewnętrznym. Worker Symfony Messenger używa API PowerDNS do:

- Tworzenia nowych stref DNS dla domen klientów.
- Dodawania rekordów A, AAAA, CNAME, MX i TXT.
- Automatycznej konfiguracji rekordów MX dla usługi pocztowej.

Rekordy DNS są tworzone automatycznie podczas provisioningu usług i usuwane podczas ich dezaktywacji.

# 6. Testowanie i zabezpieczenia

## 6.1. Strategia testowania

### Testy jednostkowe

Testy jednostkowe weryfikują poprawność izolowanych komponentów logiki biznesowej. W projekcie Symfony testy są realizowane przy użyciu PHPUnit. Testowane są przede wszystkim:

- Walidacja danych wejściowych (formaty domen, siła haseł, limity zasobów).
- Logika serwisów biznesowych (obliczanie limitów, sprawdzanie uprawnień, generowanie konfiguracji).
- Transformacja danych między warstwami (encje Doctrine, obiekty DTO, odpowiedzi API).

### Testy integracyjne

Testy integracyjne weryfikują poprawność współpracy między komponentami. Obejmują:

- Testy API REST — wysyłanie żądań HTTP do endpointów Symfony i weryfikacja odpowiedzi (kody statusu, struktura JSON, nagłówki).
- Testy bazy danych — weryfikacja poprawności migracji Doctrine, operacji CRUD na encjach, relacji między tabelami.
- Testy kolejkowania — weryfikacja poprawności publikacji i konsumpcji wiadomości Symfony Messenger z RabbitMQ.

### Testy manualne

Testy manualne obejmują weryfikację pełnych scenariuszy użytkownika:

- Rejestracja konta i logowanie.
- Zakup usługi hostingowej — od wyboru pakietu po aktywację i dostęp do panelu zarządzania.
- Zakup VPS — od konfiguracji parametrów po uruchomienie i dostęp do konsoli noVNC.
- Upload plików na dysk wirtualny, generowanie linku udostępniania, pobieranie pliku.
- Tworzenie skrzynki e-mail, wysyłka i odbiór poczty przez Roundcube.

## 6.2. Zabezpieczenia systemu

### Warstwa sieciowa

- **Nginx jako single point of entry** — cały ruch zewnętrzny wchodzi do systemu wyłącznie przez Nginx na porcie 443 (HTTPS). Pozostałe porty maszyn wirtualnych nie są dostępne z Internetu.
- **Szyfrowanie TLS** — Certbot automatycznie pobiera i odnawia certyfikaty Let's Encrypt dla Nginx. Cała komunikacja zewnętrzna jest szyfrowana.
- **Izolacja sieciowa** — maszyny wirtualne platformy komunikują się przez sieć wewnętrzną Proxmox. Kontenery klientów są izolowane za pomocą Network Policies w Kubernetes.
- **Firewall** — konfiguracja iptables/nftables na hoście Proxmox ogranicza dostęp wyłącznie do wymaganych portów (443 HTTPS, 22 SSH dla administracji).

### Warstwa aplikacji

- **Ochrona przed SQL Injection** — Doctrine ORM używa parametryzowanych zapytań (prepared statements), eliminując możliwość wstrzyknięcia kodu SQL.
- **Ochrona przed XSS** — React automatycznie escapuje zawartość renderowaną w komponentach. Dane zwracane przez API są w formacie JSON, a nie HTML.
- **Ochrona przed CSRF** — komunikacja SPA z API odbywa się za pomocą tokenów JWT w nagłówku Authorization, co eliminuje podatność na CSRF.
- **Walidacja danych wejściowych** — Symfony Validator weryfikuje poprawność wszystkich danych wejściowych na poziomie kontrolerów.
- **Rate limiting** — ograniczenie liczby żądań na IP i na użytkownika, szczególnie dla endpointów logowania i rejestracji.

### Warstwa uwierzytelniania i autoryzacji

- **JWT (JSON Web Tokens)** — tokeny uwierzytelniające z określonym czasem ważności. Po wygaśnięciu tokenu użytkownik musi zalogować się ponownie.
- **Hashowanie haseł** — hasła użytkowników są przechowywane jako hashe bcrypt lub Argon2id (konfigurowalny algorytm w Symfony Security).
- **RBAC** — kontrola dostępu oparta na rolach (client, admin). Endpointy administracyjne są niedostępne dla użytkowników z rolą client.
- **Walidacja siły hasła** — rejestracja i zmiana hasła wymagają spełnienia minimalnych kryteriów (długość, różnorodność znaków).

### Warstwa infrastruktury

- **Separacja uprawnień** — proces webowy (www-data) nie posiada uprawnień do operacji systemowych. Operacje infrastrukturalne są wykonywane przez wydzielonego workera.
- **Izolacja kontenerów** — każdy klient hostingowy działa w izolowanym namespace Kubernetes z osobnymi limitami zasobów.
- **Izolacja maszyn wirtualnych** — serwery VPS klientów są pełnymi maszynami wirtualnymi KVM, zapewniającymi izolację na poziomie sprzętowym.
- **Audyt operacji** — wszystkie operacje użytkowników i administratorów są logowane w tabeli audit_logs z informacjami o wykonanej akcji, zasobie, adresie IP i szczegółach.

### Monitoring bezpieczeństwa

- **Prometheus** — zbieranie metryk z komponentów systemu (wykorzystanie CPU, RAM, dysku, liczba żądań, błędy).
- **Grafana** — wizualizacja metryk i konfiguracja alertów (np. alert o nietypowo wysokim wykorzystaniu zasobów, dużej liczbie nieudanych logowań, anomaliach w ruchu sieciowym).
- **Logi Symfony** — rejestrowanie błędów aplikacji, nieudanych prób uwierzytelniania i podejrzanych żądań.

## 6.3. Kopie zapasowe i odzyskiwanie danych

### Strategia backupu

Restic wykonuje kopie zapasowe następujących danych:

- **Baza danych PostgreSQL** — codzienny dump bazy platformy (pg_dump).
- **Bazy danych klientów** — kopie instancji MySQL (mysqldump), PostgreSQL (pg_dump) i MongoDB (mongodump).
- **Pliki hostingowe** — kopie wolumenów Kubernetes z plikami stron klientów.
- **Pliki MinIO** — kopie obiektów z bucketów użytkowników.
- **Konfiguracja systemu** — kopie plików konfiguracyjnych Postfix, Dovecot, PowerDNS, Nginx.

Kopie zapasowe są przechowywane w MinIO z szyfrowaniem i deduplikacją (funkcje wbudowane w Restic). Retencja kopii jest konfigurowana przez administratora.

### Odzyskiwanie danych

W przypadku awarii administrator może przywrócić dane z kopii zapasowej Restic. Procedura obejmuje:

1. Identyfikacja ostatniej poprawnej kopii (restic snapshots).
2. Przywrócenie danych do tymczasowej lokalizacji (restic restore).
3. Import danych do odpowiednich serwisów (bazy danych, wolumeny, pliki).

# 7. Wnioski

## 7.1. Realizacja celów pracy

Głównym celem pracy inżynierskiej było zaprojektowanie i stworzenie systemu rozwiązań hostingowych i chmurowych umożliwiającego zdalne zarządzanie zasobami wirtualnymi. Cel ten został zrealizowany poprzez opracowanie kompleksowej architektury platformy obejmującej trzy kategorie usług: hosting webowy, wirtualne serwery prywatne (VPS) oraz przestrzeń dyskową w chmurze.

System został zaprojektowany z wykorzystaniem wyłącznie oprogramowania open source, co eliminuje koszty licencyjne i zapewnia pełną kontrolę nad każdym komponentem infrastruktury. Zastosowanie wzorca uprzywilejowanego demona API (Symfony Messenger + RabbitMQ) zapewniło bezpieczną separację uprawnień między warstwą webową a operacjami systemowymi.

## 7.2. Zastosowane rozwiązania technologiczne

Architektura systemu opiera się na kilku kluczowych decyzjach technologicznych:

- **Konteneryzacja z orkiestracją** (Docker + Kubernetes) dla usług hostingowych zapewnia izolację klientów, powtarzalność środowisk i elastyczne zarządzanie zasobami.
- **Pełna wirtualizacja sprzętowa** (KVM + Proxmox VE) dla usług VPS zapewnia najwyższy poziom izolacji i umożliwia uruchamianie pełnych systemów operacyjnych z wydajnością zbliżoną do natywnej.
- **Object storage** (MinIO) dla usługi dyskowej zapewnia skalowalność i kompatybilność z interfejsem S3, co ułatwia integrację z istniejącymi narzędziami i bibliotekami.
- **Asynchroniczne przetwarzanie zadań** (RabbitMQ + Symfony Messenger) zapewnia niezawodność, powtarzalność i audytowalność operacji infrastrukturalnych.

## 7.3. Wyzwania i ograniczenia

Głównym ograniczeniem projektu jest infrastruktura sprzętowa — serwer dedykowany z 8 GB RAM i 80 GB przestrzeni dyskowej ogranicza liczbę jednocześnie obsługiwanych usług. W środowisku produkcyjnym wymagane byłoby skalowanie horyzontalne (dodanie kolejnych serwerów) oraz zastosowanie rozproszonego systemu plików.

Wyzwaniem architektonicznym było pogodzenie trzech fundamentalnie różnych modeli usług (kontenery, maszyny wirtualne, object storage) w jednym spójnym interfejsie użytkownika i zunifikowanym procesie provisioningu.

## 7.4. Możliwości dalszego rozwoju

System oferuje szerokie możliwości rozszerzenia:

- **Skalowanie horyzontalne** — dodanie kolejnych serwerów fizycznych do klastra Proxmox VE, umożliwiające migrację maszyn wirtualnych między węzłami i zwiększenie dostępnych zasobów.
- **Automatyczne skalowanie** — implementacja autoscalingu w Kubernetes dla usług hostingowych, dynamicznie dostosowującego zasoby do aktualnego obciążenia.
- **Rozbudowa systemu pocztowego** — dodanie SpamAssassin (filtrowanie spamu), ClamAV (skanowanie antywirusowe), OpenDKIM (uwierzytelnianie poczty) oraz konfiguracja SPF i DMARC.
- **Rozbudowa monitoringu** — dodanie ELK Stack (Elasticsearch, Logstash, Kibana) do centralizacji i analizy logów.
- **Aplikacja mobilna** — natywna aplikacja do zarządzania usługami z powiadomieniami push o statusie usług i alertach.
- **System automatycznego skalowania cen** — dynamiczne dostosowywanie cennika w zależności od wykorzystania zasobów infrastruktury.

## 7.5. Podsumowanie

Zrealizowany projekt demonstruje, że możliwe jest zbudowanie kompletnej platformy hostingowej i chmurowej w oparciu o otwarte technologie. Połączenie konteneryzacji, wirtualizacji sprzętowej i nowoczesnych frameworków webowych pozwala na stworzenie systemu, który jest jednocześnie funkcjonalny, bezpieczny i rozszerzalny. Praca stanowi solidną podstawę, na której można budować komercyjną platformę hostingową po rozwiązaniu ograniczeń infrastrukturalnych i dodaniu funkcji opisanych w sekcji dotyczącej dalszego rozwoju.

# 8. Bibliografia

1. Symfony SAS, „Symfony 7.4 Documentation", https://symfony.com/doc/7.4/index.html

2. Doctrine Project, „Doctrine ORM Documentation", https://www.doctrine-project.org/projects/orm.html

3. Meta Platforms, Inc., „React Documentation", https://react.dev/

4. Tailwind Labs, „TailwindCSS Documentation", https://tailwindcss.com/docs

5. The PostgreSQL Global Development Group, „PostgreSQL 16 Documentation", https://www.postgresql.org/docs/16/

6. Docker, Inc., „Docker Documentation", https://docs.docker.com/

7. The Kubernetes Authors, „Kubernetes Documentation", https://kubernetes.io/docs/

8. Proxmox Server Solutions GmbH, „Proxmox VE Documentation", https://pve.proxmox.com/pve-docs/

9. The QEMU Project, „QEMU Documentation", https://www.qemu.org/docs/master/

10. MinIO, Inc., „MinIO Documentation", https://min.io/docs/minio/linux/

11. Nginx, Inc., „Nginx Documentation", https://nginx.org/en/docs/

12. Electronic Frontier Foundation, „Certbot Documentation", https://eff-certbot.readthedocs.io/

13. VMware, Inc. (Pivotal), „RabbitMQ Documentation", https://www.rabbitmq.com/docs

14. The Postfix Authors, „Postfix Documentation", http://www.postfix.org/documentation.html

15. Dovecot Oy, „Dovecot Documentation", https://doc.dovecot.org/

16. The Roundcube Development Team, „Roundcube Webmail", https://roundcube.net/

17. PowerDNS.COM BV, „PowerDNS Authoritative Server Documentation", https://doc.powerdns.com/authoritative/

18. The Prometheus Authors, „Prometheus Documentation", https://prometheus.io/docs/

19. Grafana Labs, „Grafana Documentation", https://grafana.com/docs/grafana/latest/

20. Alexander Neumann et al., „Restic Documentation", https://restic.readthedocs.io/

21. GitLab B.V., „GitLab CI/CD Documentation", https://docs.gitlab.com/ee/ci/

22. Internet Security Research Group (ISRG), „Let's Encrypt Documentation", https://letsencrypt.org/docs/

23. The WordPress Foundation, „WordPress Documentation", https://developer.wordpress.org/

24. Automattic, „WooCommerce Documentation", https://woocommerce.com/documentation/

25. Oracle Corporation, „MySQL 8.0 Reference Manual", https://dev.mysql.com/doc/refman/8.0/en/

26. MongoDB, Inc., „MongoDB Documentation", https://www.mongodb.com/docs/

27. noVNC Contributors, „noVNC — HTML5 VNC Client", https://novnc.com/

28. Gartner, Inc., „Forecast: Public Cloud Services, Worldwide, 2022-2028", 2024.

29. International Data Corporation (IDC), „FutureScape: Worldwide Cloud 2025 Predictions", 2024.
