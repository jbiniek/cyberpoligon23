CYBERpoligon ISAC-GIG Politechnika Śląska, 22 czerwca 2023

# CYBERpoligon - zadania dla uczestników
 
 
 


## Informacje ogólne

### Zasady

Uczestnicy będą podzieleni na dwuosobowe zespoły.

Wszystkie zadania są punktowane. Zwycięży zespół z największą liczbą punktów – aczkolwiek stawiamy na dobrą zabawę i w razie kłopotów będziemy podpowiadać co należy zrobić.
 Dlatego może się okazać, że istotne okaże się drugie kryterium, jakim jest czas wykonania zadań. Z tego powodu, jeśli zależy ci na zwycięstwie, to przygotowując się zwróć uwagę nie tylko na to jak wykonać dane zadanie – ale też przemyśl zawczasu jak _najszybciej_ osiągnąć dany efekt i jak optymalnie podzielić się pracą w obrębie zespołu.

### Dostęp do środowiska

Każdy zespół będzie miał przypisane środowisko składające się z dwóch maszyn wirtualnych: _server1_ oraz _server2_. Na miejscu dostaniecie indywidualny adres do połączenia ssh z maszynami. Dla wszystkich maszyn będą identyczne dane logowania:

Użytkownik: suse
 Hasło: zielonykameleon23

### Przygotowanie

W rozwiązaniu zadań z bezpieczeństwa Linuxa pomocne będą:

• znajomość konfiguracji zapory sieciowej

• znajomość konfiguracji serwera FTP oraz certyfikatów SSL/TLS

• znajomość uprawnień w systemie GNU/Linux

• znajomość mechanizmu konteneryzacji

• znajomość konfiguracji serwerów WWW

A także: [https://documentation.suse.com/sles/15-SP4/](https://documentation.suse.com/sles/15-SP4/)

W rozwiązaniu zadań z bezpieczeństwa Kubernetes pomocne będą:

- Podstawowa wiedza z zakresu koncepcji Kubernetes np.
  - [https://kubernetes.io](https://kubernetes.io/)
  - [https://kubernetes.io/docs/home](https://kubernetes.io/docs/home)
  - lub udział w Rancher Rodeo (najbliższe terminy dla PL: 12 czerwca)
- Znajomość koncepcji CVE (Common Vulnerabilities and Exposures) oraz Regulatory Compliances
- Znajomość podstawowych funkcji oprogramowania NeuVector:
  - [https://open-docs.neuvector.com](https://open-docs.neuvector.com/) lub udział w NeuVector Rodeo (najbliższy termin dla PL: 25 i 28 kwietnia)

## Bezpieczeństwo serwerów Linux (90 minut)

### Zadanie 1

Skonfiguruj server1 tak, aby był on bramą domyślną dla server2. Wynikiem zadania ma być możliwość uzyskania zasobów dostępnych w Internecie przez server2. Postaraj się, aby nie ucierpiał na tym poziom zabezpieczeń na server1 (blokowanie portów przez zaporę), a także aby konfiguracja była permanentna - pozostawała taka sama po restarcie obu serwerów.

_Podpowiedź: w systemach SUSE można skonfigurować wiele ustawień sieciowych za pomocą YaST._

### Zadanie 2

Na server1 udostępnij /srv/www/htdocs używając protokołu SFTP oraz FTPES zachowując następujące warunki dla FTPES:

• certyfikat self-signed ważny przez 3 lata

• długość klucza 4096 bitów

• wymuszenie szyfrowania

• brak możliwości logowania oraz transferu plików bez szyfrowania

_Podpowiedź: OpenSSH może dostarczyć funkcjonalność SFPT, a vsftpd - FTPES._

_Podpowiedź: Pamiętaj, że może potrzebne być przepuszczenie vsfptd przez firewall._

Następnie skonfiguruj dostęp do htdocs tylko dla użytkowników 'web-developer' oraz 'tester', z hasłem 'XXX'. Obaj użytkownicy mają być w grupie głównej 'dev-team' – utwórz ją.

_Podpowiedź: YaST pozwala łatwo zarządzać grupami i użytkownikami._

Skonfiguruj dostęp w taki sposób, aby pliki przez nich tworzone miały:

- jako właściciela użytkownika, który dany plik stworzył
 - grupę̨ właścicielską̨ dev-team
 - uprawnienia u+rw, 6 dla grupy właścicielskiej
 - tylko odczyt dla pozostałych.

Upewnij się̨, że użytkownicy 'wwwrun' oraz 'nginx' mają pełen dostęp do plików, ale NIE dodawaj ich do grupy 'dev-team'.

Zablokuj dostęp użytkownikom anonimowym.

Udostępnij pliki z /srv/www/htdocs używając serwera nginx, upewnij się̨, że możliwy jest dostęp po HTTP - utwórz prostą stronę̨ wyświetlającą nazwę̨ twojego zespołu.

### Zadanie 3

Na serwerze server2 utwórz plik docker-compose w wersji trzeciej, w którym zdefiniujesz podstawową infrastrukturę dla aplikacji internetowej, trzy osobne kontenery:

• web serwer
 • PHP
 • MySQL

Infrastruktura powinna używać możliwie najnowszych wersji usług i udostępniać wynik funkcji phpinfo();

Zapewnij komunikację pomiędzy kontenerami w odosobnionej sieci. Skonfiguruj volumeny dockera lub mapowanie katalogu tak, aby dane zapisywane w bazie oraz pliki tworzone przez aplikację w jej katalogu pozostawały po wyłączeniu/restarcie infrastruktury (polecenie docker-compose down).

### Zadanie 4

Aplikację z zadania trzeciego udostępnij w Internecie. Użyj serwera nginx na server1. Aplikację umieść́ pod inną nazwą domenową niż pliki z /srv/www/htdocs. Domenę możesz wymyślić, ale musi być teoretycznie "poprawnym" adresem hosta.

Zablokuj dostęp do aplikacji użytkownikom łączącym z adresów IP pochodzących z terytorium Federacji Rosyjskiej. Zapewnij automatyczne aktualizowanie blokady o nowe adresy IP.

Podpowiedź: nginx zawiera moduł GeoIP.

### Zadanie 5

Skonfiguruj server2 w taki sposób, aby był on zarządzany przez server1 za

pomocą SALT. High state powinien podstawiać plik /etc/info z następującą treścią:
 „Centrally managed server".

_Podpowiedź: pakiety salt-master i salt-minion można zainstalować bezpośrednio przez zypper._

## Bezpieczeństwo Kubernetes (90 minut)

### Zadania Rancher

Otrzymasz adres interfejsu webowego Ranchera w dniu konkursu.

Utwórz trzech użytkowników: Adam, który ma uprawnienia do administracji całego środowiska; Beata, która ma uprawnienia tylko do klastra local; oraz Czarek, który ma uprawnienia tylko do projektu default w klastrze local.

Utwórz klaster produkcyjny używając AWS, z następującymi warunkami:

• przypisanie zasad bezpieczeństwa (Pod Security Policy, lub Pod Security Admission) z dowolnego template

• przypisanie klastra do nowego właściciela (Owner), użytkownik: Czarek

Utwórz w klastrze Neuvector nowy projekt „cyber", a w nim namespace „poligon"; zastosuj dla projektu ograniczenie ilości zasobów do 1 CPU i 1GB RAM (quota).

### Zadania NeuVector

1. Zainstaluj aplikację wordpress w klastrze, namespace wordpress
_Podpowiedź: Możesz do tego użyć helm cli albo interfejsu graficznego Ranchera (Apps)_

1. Przeskanuj rejestr kontenerowy z następującymi parametrami:
 Name: docker-example

Registry: https://registry.hub.docker.com

Filter: elastic/logstash:7.13.3

Rescan after CVE: yes

Scan Layers: yes

Periodic: no

1. Przeszukaj środowisko pod kątem podatności CVE-2023-1326.
2. Utwórz zasadę "admission control" blokującą użycie obrazów zawierających więcej niż 10 podatności o severity „High", oraz blokującą deployment w namespace „default".
3. Przechwyć dowolne pakiety (Pakiet Capture) z poda wordpress.
4. Utwórz regułę wykrywającą numer karty kredytowej przy pomocy sensora DLP. Przypisz sensor do odpowiedniego poda aplikacji wordpress i przetestuj jej działanie poprzez próbę utworzenia wpisu zawierającego numer: 5575110509607387.
5. Utwórz zasadę sieciową "Network Rule" blokującą komunikację z poda wordpress do zewnętrznego internetu.
6. Włącz tryb Monitor dla poda (podów) bazy danych wordpressa oraz tryb Protect dla głównego poda wordpressa.
7. Utwórz zasadę blokującą proces ls w obrębie poda wordpress.
8. Narusz powyższą zasadę logując się do kontenera wordpress i następnie uruchamiając ls. Przejrzyj komunikaty NeuVector i użyj podpowiedzi, aby utworzyć regułę zezwalającą na uruchomienie ls w obrębie poda wordpress.
9. Utwórz zasadę stosującą kwarantannę poda w przypadku naruszenia reguł sieciowych. Następnie zaloguj się do poda wordpress i spróbuj nawiązać połączenie z internetem aby spowodować zesłanie go do kwarantanny.
_Podpowiedź: połączenie z internetem zostało pośrednio zakazane w zadaniu 8_
10. Wyeksportuj zasady zabezpieczeń wordpressa w formacie Custom Resource Definition, w trybie „Protect".

##
 Powodzenia!
