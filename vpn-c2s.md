# Zestawienie VPN typu Client to Site (C2S) na platformie Azure.

## Wymagania.

Wymagane elementy:
- Konto na platformie Azure wraz z Subskrypcją. 
- Wdrożona sieć wirtualna.
- Maszyna w Azure - z którą można przetestować połączenie.
- Maszyna Windows (komputer klienta) - w środowisku on premises.

## Opis czynności.

### 1. Instalacja zasobu **Virtual network gateway** (bramy VPN).

Instalację Virtual network gateway - opisałem w instrukcji dotyczącej VPN typu S2S. https://github.com/MisjaAzure/Warsztaty-Azure-F5/blob/main/vpn-s2s.md


### 2. Konfiguracja bramy VPN.

W głównym panelu zasobu **Virtual network gateway**, neleży kliknąć w **point-to-site configuration**,  jak przedstawia grafika:

![image](/media/c2s-f-01.png)

zostanie wyświetlony formularz: 

![image](/media/c2s-f-02.png)

Klikamy w **Configure now**. Zostają odkryte pola opcji konfigurujących bramę VPN:

![image](/media/c2s-f-03.png)

W polu **address pool** - określamy adresację jaka będzie przydzielana dla klientów łączących się za pośrenictwem tej bramy. Adresacja ta nie może pokrywać się z żadną pulą sieci w Azure lub on premises.

W polu **Tunnel type** ustalamy typ połączenia VPN. W tym labie wybrałem SSTP.

Pole **Authentication type** określa sposób w jaki użytkownicy będą się uwierzytelniać.

Pod sekcją **Root certificates** wypełaniamy pola:
- **Name** - dowolna nazwa
- **Public certificate data** - tu wklejamy certyfikat CA, które wystawiło certyfikat(y) dla klientów.

#### Wygenerowanie potrzebnego certyfikatu Root CA i certyfikatu dla klienta.

Przechodzimy do pulpitu maszyny, która będzie naszym testowym klientem. 
Uruchamiamy power-shell i wykonujemy dwa poniższe skrypty:

```
$cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
```

```
New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

Pierwszy skrypt - generuje certyfikat Root CA.

Drugi skrypt - tworzy certyfikat dla klienta, którego wystawcą jest utworzone wcześniej CA

Przebieg powyższego przedstawia poniższa grafika: 

![image](/media/c2s-f-04.png)

Teraz, należy wyeksportować stworzony certyfikat Root CA, który będzie wklejony w pole **Public certificate data**.

Można to zrobić, wykonując kroki jak opisane jest to poniżej.

Należy uruchomić przystawkę **certmgr.msc**:

![image](/media/c2s-f-05.png)

W drzewie certyfikatów wchodzimy do **Personal/Certificates** i wykonujemy dwuklik na pozycji **P2SRootCert**, następnie klikamy zakładkę **Details** oraz przycisk **Copy to File**.

![image](/media/c2s-f-50.png)

Ukaże się kreator eksportu do pliku.

Poniższe cztery grafiki prezentują kolejne etapy eksportu certyfikatu. Certyfikat eksportujemy w formacie Base64, bez klucza prywatnego.

![image](/media/c2s-f-06.png)

![image](/media/c2s-f-07.png)

![image](/media/c2s-f-08.png)

![image](/media/c2s-f-09.png)

W kolejnym kroku, otwieramy w notesie (albo innym edytorze tekstowym) wyeksportowany plik - i kopiujemy do schowka (albo do innego pliku) zasadniczą część certyfikatu. Przedstawia to obrazowo, poniższa grafika:

![image](/media/c2s-f-10.png)

Wskazany ciąg znaków w dowolny sposób umieszczamyc w schowku, aby móc go wkleić w formularzu konfiguracji bramy VPN:


![image](/media/c2s-f-11.png)

W celu zakończenia konfiguarcji bramy, pozostało tylko naciśnięcie przycisku **Save** (1) oraz ściągnięcie **klienta VPN** (2).

![image](/media/c2s-f-12.png)

Klienta dostarczamy na komputer który będzie miał nawiązać połączenie.

### 3. Konfiguracja klienta VPN.

Rozpakowujemy dostarczony plik .zip

Wchodzimy do katalogu o nazwie odpowiadającej  architekturze  komputera. Uruchamiamy instalator. Po bardzo krótkim procesie instalacji, dostępne będzie nowe połączenie VPN, zainstalowane w standardowym panelu sieciowym Windows.

Powyższe czynności przedstawia grafika:

![image](/media/c2s-f-13.png)

Pozostaje już tylko nawiązanie połączenia i przetestowanie łączności do zasobu w Azure:

![image](/media/c2s-f-14.png)
