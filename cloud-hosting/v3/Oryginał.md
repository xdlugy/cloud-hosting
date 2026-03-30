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

### 4.2.1. Architektura komponentów

System składa się z pięciu warstw: prezentacji, aplikacji, komunikacji, infrastruktury oraz danych.

Warstwa prezentacji obejmuje publiczną stronę sprzedażową (WordPress + WooCommerce) oraz panel klienta i administratora (React SPA). Warstwa aplikacji zawiera API Gateway (Symfony 7.4) z modułami provisioningu, monitoringu, kopii zapasowych i rozliczeniowym. Warstwa komunikacji opiera się na RabbitMQ (broker wiadomości) i Nginx (reverse proxy). Warstwa infrastruktury realizuje faktyczne usługi: Kubernetes z kontenerami Docker (hosting web), Proxmox VE z KVM (VPS), Postfix + Dovecot + Roundcube (poczta) oraz PowerDNS (DNS). Warstwa danych obejmuje PostgreSQL (dane platformy), bazy klientów (MySQL/PostgreSQL/MongoDB) oraz MinIO (storage obiektowy).

### 4.2.2. Architektura wdrożenia

Cała platforma działa na jednym serwerze dedykowanym Hetzner z systemem Debian 13 i zainstalowanym Proxmox VE jako hypervisorem.

Proxmox VE zarządza następującymi maszynami wirtualnymi:

- **VM Klaster Kubernetes** — Nginx (reverse proxy), kontenery klientów (nginx + PHP-FPM), obsługa certyfikatów Let's Encrypt (Certbot).
- **VM Serwer aplikacji** — Symfony 7.4 API, React SPA, RabbitMQ, Worker Symfony Messenger.
- **VM Bazy danych** — PostgreSQL (dane platformy), MySQL/PostgreSQL/MongoDB (bazy klientów).
- **VM Poczta e-mail** — Postfix, Dovecot, Roundcube.
- **VM Storage i monitoring** — MinIO, Prometheus, Grafana, Restic.
- **VM DNS** — PowerDNS.
- **VM klientów VPS** — maszyny wirtualne KVM z systemami operacyjnymi klientów, konfigurowane przez cloud-init.

Cały ruch zewnętrzny trafia na port HTTPS (443) i jest kierowany przez Nginx do odpowiednich serwisów.

### 4.2.3. Aktorzy systemu i przypadki użycia

System identyfikuje trzech aktorów: klienta niezalogowanego, klienta zalogowanego oraz administratora.

**Klient niezalogowany** może przeglądać ofertę, porównywać pakiety, zarejestrować konto i zalogować się.

**Klient zalogowany** ma dostęp do zarządzania kontem (zmiana hasła, edycja profilu, dashboard), usługami hostingowymi (zakup, zarządzanie plikami, bazą danych, konfiguracją PHP, logami, kopiami zapasowymi), serwerami VPS (zakup, kontrola cyklu życia, konsola, monitoring), dyskiem wirtualnym (zakup, upload/download, udostępnianie) oraz pocztą e-mail (tworzenie skrzynek, webmail).

**Administrator** zarządza użytkownikami, konfiguracją pakietów i cenników, monitoruje platformę, przegląda logi systemowe oraz zarządza kopiami zapasowymi.

### 4.2.4. Przepływ danych

Cały ruch zewnętrzny (HTTP/HTTPS) wchodzi do systemu przez Nginx, który pełni rolę single point of entry. Nginx terminuje SSL i kieruje żądania do odpowiednich serwisów na podstawie nazwy domeny i ścieżki URL (konfiguracja server blocks z proxy_pass).

Żądania do panelu klienta są kierowane do React SPA, które komunikuje się z Symfony API przez REST. API wykonuje operacje CRUD na bazie PostgreSQL za pośrednictwem Doctrine ORM. Operacje infrastrukturalne (provisioning, konfiguracja) są kolejkowane w RabbitMQ i wykonywane asynchronicznie przez workera Symfony Messenger, który komunikuje się z odpowiednimi serwisami: Kubernetes API (hosting), Proxmox API (VPS), PowerDNS API (DNS), systemem pocztowym (e-mail).

### 4.2.5. Model danych

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

Generowanie linku udostępniania: klient klika „Udostępnij", API generuje URL w MinIO z datą wygaśnięcia i zapisuje token linku w PostgreSQL.
