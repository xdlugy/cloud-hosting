# 1. Wstęp
### Wprowadzenie

Współczesne technologie hostingowe i chmurowe stanowią fundament infrastruktury IT wielu przedsiębiorstw oraz użytkowników indywidualnych na całym świecie. Transformacja cyfrowa, przyspieszona przez globalne wydarzenia ostatnich lat, fundamentalnie zmieniła sposób, w jaki organizacje podchodzą do zarządzania swoimi zasobami informatycznymi. Z rosnącym zapotrzebowaniem na elastyczność, skalowalność i dostępność danych, usługi chmurowe stały się jednym z najistotniejszych elementów współczesnej gospodarki cyfrowej, generując według prognoz Gartnera przychody przekraczające 600 miliardów dolarów w 2024 roku.

### Kontekst technologiczny i biznesowy

Dzięki rozwiązaniom chmurowym organizacje mogą optymalizować swoje zasoby, minimalizować koszty związane z infrastrukturą IT oraz uzyskać dostęp do zaawansowanych technologii bez potrzeby inwestowania w drogie sprzęty i oprogramowanie. Model pay-as-you-go pozwala przedsiębiorstwom na dynamiczne dostosowywanie wykorzystywanych zasobów do aktualnych potrzeb, co jest szczególnie istotne w kontekście nieprzewidywalności rynkowej i sezonowości biznesu.

Ewolucja technologii wirtualizacji, konteneryzacji oraz orkiestracji zasobów otworzyła nowe możliwości w zakresie projektowania systemów rozproszonych. Technologie takie jak Docker, Kubernetes, OpenStack czy VMware vSphere umożliwiają tworzenie wysoce skalowalnych i odpornych na awarie środowisk, które mogą obsługiwać miliony użytkowników jednocześnie. Jednocześnie rozwój modeli usługowych - Infrastructure as a Service (IaaS), Platform as a Service (PaaS) oraz Software as a Service (SaaS) - pozwala na dostosowanie oferty do specyficznych wymagań różnych segmentów rynku.

### Wyzwania współczesnego hostingu i rozwiązań chmurowych

Pomimo znaczących korzyści, implementacja rozwiązań chmurowych wiąże się z licznymi wyzwaniami. Kwestie bezpieczeństwa danych, zgodności z regulacjami prawnymi (takimi jak RODO czy CCPA), zarządzania złożonością hybrydowych środowisk IT oraz optymalizacji kosztów wymagają zastosowania zaawansowanych narzędzi i metodologii. Dodatkowo, rosnące wymagania dotyczące wydajności, dostępności na poziomie 99,99% oraz możliwości disaster recovery stawiają przed architektami systemów chmurowych coraz bardziej ambitne zadania.

Problem vendor lock-in, czyli uzależnienia od konkretnego dostawcy usług chmurowych, stał się istotnym czynnikiem wpływającym na decyzje architektoniczne. Organizacje coraz częściej poszukują rozwiązań multi-cloud lub hybrid-cloud, które pozwalają na dywersyfikację ryzyka i optymalizację wykorzystania różnych platform chmurowych.

### Cel i zakres pracy

Celem niniejszej pracy inżynierskiej jest zaprojektowanie i stworzenie kompleksowego systemu rozwiązań hostingowych i chmurowych, który umożliwia zdalne zarządzanie zasobami wirtualnymi w środowisku chmurowym. System ma na celu zapewnienie elastyczności, wydajności oraz bezpieczeństwa w kontekście hostingu aplikacji i przechowywania danych, przy jednoczesnym zachowaniu intuicyjności interfejsu użytkownika oraz możliwości integracji z istniejącymi rozwiązaniami.

### Znaczenie i aktualność tematu

Podjęcie tematu pracy jest odpowiedzią na rosnące zapotrzebowanie rynku na nowoczesne rozwiązania hostingowe i chmurowe, które umożliwiają szybkie i bezpieczne przetwarzanie danych w dynamicznie zmieniającym się środowisku IT. Według raportu IDC, do 2025 roku ponad 85% przedsiębiorstw będzie wykorzystywać strategię cloud-first, co podkreśla kluczowe znaczenie rozwoju efektywnych i bezpiecznych platform chmurowych.

Dodatkowo, trendy takie jak edge computing, serverless computing oraz rozwój technologii 5G otwierają nowe możliwości w zakresie projektowania systemów chmurowych nowej generacji. Praca wpisuje się w te trendy, proponując rozwiązania, które mogą być adaptowane do przyszłych wymagań technologicznych i biznesowych.

# 2. Zakres funkcjonalny systemu
### Charakterystyka ogólna systemu

Projektowany system stanowi kompleksową platformę hostingową i chmurową, umożliwiającą użytkownikom zakup i zarządzanie różnorodnymi usługami IT poprzez intuicyjny interfejs webowy. System składa się z trzech głównych warstw: publicznego interfejsu sprzedażowego, panelu klienckiego do zarządzania usługami oraz panelu administracyjnego dla operatora platformy.

### Przepływ użytkownika w systemie

Potencjalny klient rozpoczyna swoją interakcję od strony głównej, gdzie może zapoznać się z ofertą usług podzielonych na trzy główne kategorie: hosting web z bazą danych i pocztą email, wirtualne serwery prywatne (VPS) oraz przestrzeń dyskową w chmurze. System oferuje porównywarkę planów oraz kalkulator kosztów, który pomaga w doborze odpowiedniego pakietu.

Proces zakupu rozpoczyna się od rejestracji konta - nowi użytkownicy wypełniają prosty formularz i weryfikują adres email, podczas gdy istniejący klienci logują się do systemu z opcją dodatkowego zabezpieczenia dwuskładnikowego. Po zalogowaniu użytkownik przechodzi przez kreator konfiguracji wybranej usługi, gdzie określa parametry techniczne takie jak wersja PHP dla hostingu, system operacyjny dla VPS czy wielkość przestrzeni dyskowej.

### Panel zarządzania usługami

Po zakupie usługi klient otrzymuje dostęp do przejrzystego dashboardu, który centralizuje zarządzanie wszystkimi wykupionymi produktami. Panel prezentuje status aktywnych usług, terminy płatności oraz umożliwia szybkie wykonywanie podstawowych operacji jak restart serwera czy tworzenie kopii zapasowych. Każda usługa posiada dedykowany panel zarządzania dostosowany do jej specyfiki - hosting oferuje menedżer plików i panel administracyjny bazy danych, VPS udostępnia konsolę systemową i narzędzia monitoringu, a usługa dyskowa pozwala na przeglądanie plików i generowanie linków udostępniania.

### Oferowane usługi

**Hosting Web** to kompleksowe rozwiązanie obejmujące przestrzeń na strony internetowe z obsługą popularnych technologii programistycznych, automatycznie konfigurowane serwery web, bazę danych z panelem administracyjnym oraz pełnowartościowy serwer pocztowy. System automatycznie wykonuje kopie zapasowe i umożliwia przywracanie danych do wybranego momentu w czasie.

**Środowiska VPS** zapewniają użytkownikom dedykowane zasoby serwerowe z pełnym dostępem administracyjnym. Klienci mogą wybierać spośród różnych dystrybucji systemu Linux, otrzymując kompletne środowisko do uruchamiania własnych aplikacji i usług.

**Dysk wirtualny** funkcjonuje jako przestrzeń w chmurze dostępna z dowolnego urządzenia. Usługa umożliwia synchronizację plików między komputerem a urządzeniami mobilnymi, udostępnianie zasobów innym użytkownikom oraz wygodne przesyłanie plików przez przeglądarkę metodą przeciągnij i upuść.

### Zarządzanie platformą

Administrator systemu dysponuje rozbudowanym panelem kontrolnym, który pozwala na kompleksowe zarządzanie platformą. Interfejs administratora umożliwia przeglądanie i edycję kont użytkowników, konfigurację pakietów usług i cenników, tworzenie promocji oraz monitorowanie aktywności na platformie. System loguje wszystkie operacje użytkowników, co zapewnia pełną kontrolę nad bezpieczeństwem i pozwala na szybką reakcję w przypadku problemów.
# 3. Wykorzystywane narzędzia i technologie

### Warstwa prezentacji i sprzedaży

Publiczna strona platformy hostingowej zostanie zbudowana w oparciu o system zarządzania treścią WordPress wraz z wtyczką e-commerce WooCommerce. To rozwiązanie zapewni szybkie wdrożenie funkcjonalności sprzedażowych, łatwą modyfikację treści marketingowych oraz gotowe integracje z systemami płatności. WordPress posłuży jako front-end dla potencjalnych klientów, prezentując ofertę, umożliwiając porównywanie pakietów i finalizację zakupu. WooCommerce obsłuży koszyk, proces checkout, zarządzanie zamówieniami oraz integrację z bramkami płatniczymi takimi jak PayU czy Stripe.

### Panel zarządzania klienta

Główny panel użytkownika zostanie zaimplementowany jako aplikacja webowa w PHP wykorzystująca framework Symfony 6. Framework ten zapewni solidną architekturę MVC, system routingu, ORM Doctrine do komunikacji z bazą danych oraz komponenty bezpieczeństwa. Panel będzie komunikował się z infrastrukturą poprzez API REST, wysyłając komendy do systemu zarządzania wirtualizacją. Każda akcja użytkownika, jak restart serwera czy tworzenie backupu, będzie wywoływała odpowiednie skrypty systemowe poprzez kolejki zadań RabbitMQ, zapewniając asynchroniczne przetwarzanie i skalowalność.

### Infrastruktura hostingu webowego

Usługi hostingowe będą realizowane poprzez konteneryzację z wykorzystaniem Docker oraz orkiestracji Kubernetes. Każdy klient otrzyma izolowany kontener z preinstalowanym stosem LAMP/LEMP. Serwery webowe Apache i Nginx będą dostępne do wyboru jako obrazy Docker, co umożliwi łatwe przełączanie między nimi. Środowiska uruchomieniowe PHP (wersje 7.4, 8.0, 8.1, 8.2), Node.js oraz Python będą również konteneryzowane, pozwalając na równoległe działanie różnych wersji. Reverse proxy na bazie Traefik będzie zarządzać routingiem ruchu do odpowiednich kontenerów oraz automatycznie generować certyfikaty SSL Let's Encrypt.

### System pocztowy

Infrastruktura email zostanie oparta o stos Postfix jako MTA (Mail Transfer Agent) odpowiedzialny za wysyłkę i odbieranie poczty, Dovecot jako serwer IMAP/POP3 dla dostępu do skrzynek oraz Roundcube jako interfejs webmail. System antyspamowy będzie realizowany przez SpamAssassin z dodatkowymi regułami i bazami RBL, wspierany przez ClamAV do skanowania antywirusowego. Dla zarządzania domenami i uwierzytelniania wykorzystany zostanie PowerDNS z automatyczną konfiguracją rekordów SPF, DKIM przez OpenDKIM oraz DMARC. Całość będzie działać w dedykowanych kontenerach Docker z persistent volumes dla przechowywania poczty.

### Systemy bazodanowe

Bazy danych będą oferowane jako usługi konteneryzowane z wykorzystaniem oficjalnych obrazów Docker dla MySQL 8.0, PostgreSQL 14 oraz MongoDB 6. Każda instancja bazy będzie działać w izolowanym kontenerze z dedykowanymi zasobami i wolumenami dla persystencji danych. System będzie wykorzystywał ProxySQL dla MySQL i PgBouncer dla PostgreSQL do zarządzania połączeniami i load balancingu. Automatyczne backupy będą realizowane przez Percona XtraBackup dla MySQL, pg_dump dla PostgreSQL oraz mongodump dla MongoDB, z przechowywaniem kopii w object storage MinIO.

### Wirtualizacja VPS

Środowiska VPS będą uruchamiane przy użyciu technologii KVM (Kernel-based Virtual Machine) zarządzanej przez Proxmox VE lub alternatywnie OpenStack. KVM zapewni pełną wirtualizację sprzętową z near-native performance, umożliwiając uruchamianie różnych systemów operacyjnych. Każdy VPS otrzyma dedykowane zasoby CPU, RAM i dysku z gwarantowaną alokacją. Libvirt będzie służył jako warstwa abstrakcji do zarządzania maszynami wirtualnymi, a QEMU jako hypervisor. Systemy operacyjne Ubuntu, Debian i CentOS będą dostępne jako przygotowane obrazy z cloud-init dla automatycznej konfiguracji początkowej.

### Manager plików w chmurze

Usługa dysku wirtualnego zostanie zaimplementowana jako aplikacja PHP/Symfony współpracująca z Nextcloud API lub jako własne rozwiązanie wykorzystujące Flysystem - abstrakcję PHP dla różnych systemów storage. Backend storage będzie oparty o MinIO (kompatybilny z S3) lub Ceph dla zapewnienia skalowalności i redundancji. Aplikacja będzie oferować interfejs webowy z drag&drop zbudowany w Vue.js, synchronizację przez WebDAV oraz API REST dla aplikacji mobilnych. Symfony będzie zarządzać uprawnieniami, generowaniem linków publicznych oraz wersjonowaniem plików.

### Technologie wspierające

Całość infrastruktury będzie monitorowana przez stack Prometheus i Grafana dla metryk oraz ELK (Elasticsearch, Logstash, Kibana) dla centralizacji logów. Redis posłuży jako cache i broker dla kolejek, a HAProxy zapewni load balancing między węzłami. Ansible będzie wykorzystywany do automatyzacji deploymentu i konfiguracji, podczas gdy GitLab CI/CD obsłuży procesy continuous integration. Backup całej infrastruktury będzie realizowany przez Restic z deduplikacją i szyfrowaniem, przechowywany w zewnętrznym object storage.

System będzie działał na klastrze serwerów fizycznych z systemem Rocky Linux lub Ubuntu Server jako host OS, zapewniając stabilną i bezpieczną podstawę dla całej platformy. Komunikacja między komponentami będzie zabezpieczona przez wewnętrzną sieć VLAN z szyfrowaniem TLS dla wszystkich połączeń.
# 4. Projekt architektury systemu

## 4.1. Wymagania oraz analiza

## 4.2. Wizualizacja abstrakcyjna

### 4.2.1. Proces biznesowy systemu

### 4.2.2. Diagram przepływu danych systemu

# 5. Implementacja systemu

# 6. Testowanie i zabezpieczenia

# 7. Wnioski

# 8. Bibliografia