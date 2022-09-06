# Zestawienie VPN typu S2S, między platformą Azure a środowiskiem on premises.

## Wymagania.

Wymagane elementy:
- Konto na platformie Azure wraz z Subskrypcją. 
- Wdrożona sieć wirtualna.
- Maszyna w Azure - z którą można przetestować połączenie.
- Środowisko on premises z dowolną bramą obsługującą połączenia IPSEC/VPN, wystawioną na publicznym adresie (może być adres dynamiczny + DDNS).


## Opis czynności.

### 1. Instalacja zasobu Virtual network gateway (bramy VPN).

W pasku wyszukiwania portalu Azure, należy wpisać "virtual network gateway" i wybrać  wyszukaną pozycję:

![image](/media/s2s-f-01.png)

Naciskamy **+ Create**:

![image](/media/s2s-f-02.png)

Powinien pokazać się formularz, jak na poniższej grafice:

![image](/media/s2s-f-03.png)

Należy wypełnić go zgodnie z możliwymi wyborami. Proszę zwrócić uwagę na oznaczone pozycje i takie same wybory zastosować.

Kilka słów na temat pozycji opisanej jako **Gateway subnet address range**.  Jest to adresacja dodatkowej podsieci, która zostanie stworzona w trakcie wdrażania tego zasobu. Tą adresację można ustalić samodzielnie. Oczywiście musi ona zawierać się w adresacji głównej (root) wirtualnej sieci (na formularzu pozycja: Virtual network). 

Po wypełnieniu formularza, należy zastosować przycisk **Review + create** a później **Create**.

Po stworzeniu zasobu (trwa to dłuższą chwilę), należy wejść w jego główny panel:

![image](/media/s2s-f-04.png)

### 2. Utworzenie połączenia.

Należy wejść w opcję Settings/Connections (jak na powyższej grafice, zaznaczono na czerwono):

![image](/media/s2s-f-05.png)

Wciskamy **+ Add**:

![image](/media/s2s-f-06.png)

Szczególnie istotne opcje, są wyszczególnione na grafice.

W pozycji (1) należy wybrać dany **virtual network gateway** (kiedy mamy tylko jeden - lista nie rozwija się).

W pozycji (2) - określamy wersję protokołu IKE. Kiedy nie zachodzi konieczność użycia IKEv1 - poprawnym wyborem będzie IKEv2.

W polu (3) - należy podać klucz PSK - który będzie również użyty w bramie po stronie on premises.

#### Opisanie konfiguracji bramy on premises.

W polu (4) - wybieramy konfigurację bramy on premises.  Kiedy nie mamy wcześniej zdefiniowanej żadnej bramy - pokaże się nam formularz:

![image](/media/s2s-f-07.png)

Wciskamy **+ Create new**:

![image](/media/s2s-f-08.png)

W polu (1) określamy sposób - w który podamy adres lokalnej bramy.
 
W polu (2) określamy jej adres (IP albo FQDN - zależnie od wcześniejszego wyboru).

Pole (3) - określa adresacje sieci on premises.

Na końcu pracy z tym formularzem  klikamy OK.

Wrócimy do okna w postaci zbliżonej do poniższego:

![image](/media/s2s-f-09.png)

Klikamy **OK**

Wrócimy do **Overiew** naszej bramy:

![image](/media/s2s-f-10.png)

W miejscu zaznaczonym na czerwono - zobaczymy status naszego połączenia.

### 3. Konfiguracja bramy on premises.

Teraz następuje pora na skonfigurowanie urządzenia znajdującego się w lokalu on premises.

Dla uproszczenia konfiguracji i uściślenia  wymagań stawianych przez bramę w Azure - możemy skorzystać z możliwości wyeksportowania gotowego pliku (skryptu) konfiguracji dla  niektórych urządzeń, lub choćby oglądnięcia wymaganych ustawień - i zaimplementowania ich po stronie **dowolnego rozwiązania IPSEC/VPN**.

Robimy to poprzez kliknięcie w nazwę połączenia:

![image](/media/s2s-f-11.png)

Pojawi się formularz:

![image](/media/s2s-f-12.png)

z którego pobrać należy najbardziej zbliżony do naszych potrzeb plik konfiguracyjny.

Po skonfigurowaniu urządzenia on premises i poprawnym nawiązaniu połączenia między urządzeniami, powinniśmy uzyskać możliwość komunikacji między maszynami on premises a maszynami w Azure - przy użyciu adresacji prywatnej (określonej w parametrach połączenia).

> Warto pamiętać o tym, że w konfiguracji połączenia, może zajść potrzeba włączenia opcji jak na grafice poniżej:

![image](/media/s2s-f-13.png)

Jest to konieczne w sytuacji, gdy urządzenie on premises nie obsługuje **Route-based VPN**, lecz **policy-based VPN**.
