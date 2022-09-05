# Backup plików z maszyny Windows on premises - do zasobów Azure.

## Wymagania

Wymagane elementy:
- Konto na platformie Azure wraz z Subskrypcją. 
- Maszyna (maszyny) pracująca w środowisku Windows (Server lub Workstation).


## Opis kolejnych czynności.


###Instalacja agenta (ew. agentów)

W pasku wyszukiwania portalu Azure, należy wpisać "recovery service vault" (1) i kliknąć w wyszukaną pozycje (2).

![image](/media/backup-f-01.png)

W wyświetlone pola formularza, należy wpisać pożądane i możliwe do zastosowania dane - jak w przykładzie poniżej.

![image](/media/backup-f-02.png)

Po uzupełnieniu pól, wciskamy przycisk opisany **Review + Create**, następnie **Create** i czekamy chwilkę na utworzenie zasobu typu **Recovery Services vault** o nazwie którą nadaliśmy.

Wchodzimy w zasób. Powinien się ukazać widok zbliżony do poniższego:

![image](/media/backup-f-03.png)

gdzie wybieramy  pozycję **Backup**

Po tym, pokaże się formularz w którym wartości (wybierane z list) ustawiamy jak na grafice:

![image](/media/backup-f-04.png)

Wciskamy przycisk **Prepare Infrastructure**

Na kolejnym panelu, wybieramy pozycję **Download Agent for Windows Server or Windows Client**:

![image](/media/backup-f-05.png)

Agenta instalujemy na maszynie, której pliki będziemy zabezpieczać.

Instalacja agenta jest łatwa i oczywista.  Gdy dojdziemy do etapu instalacji jak przedstawiany poniżej:

![image](/media/backup-f-06.png)

ściągamy wówczas plik z danymi potrzebnymi do rejestracji agenta w platformie Azure.

Wykonuje się to poprzez: 
- Powrót do portalu Azure
- włączenie check-box z opisem **Already downloaded or using latest Recovery Services Agent**
- Następnie klikamy w przycisk **Download**. Powinien zostać pobrany plik z danymi do rejestracji

Powyższe czynności prezentuje grafika:

![image](/media/backup-f-07.png)

W kolejnym kroku, w oknie setup-u agenta wciskamy przycisk **Proceed Registration**  i na kolejnym oknie po wybraniu **Browse** - wskazujemy plik z danymi rejestracyjnymi.

![image](/media/backup-f-08.png)

W kolejny oknie (Encryption Setting):

![image](/media/backup-f-09.png)

naciskamy przycisk **Generate Passphrase** i określamy ścieżkę do miejsca gdzie zostanie wygenerowany plik z kluczem szyfrującym backup.

Po wciśnięciu **Finish**  powinniśmy zobaczyć okno:

![image](/media/backup-f-10.png)

w którego opcjach finiszujemy instalację agenta w j.w. pokazany sposób.

###Wykonanie/zaplanowanie backupu

Celem zaplanowania backupu, wchodzimy w opcje aplikacji, tak jak pokazane jest to na grafice:

![image](/media/backup-f-11.png)

Poniższe 4 grafiki, przedstawiają kolejno występujące po sobie opcje konfiguracji backup-u:

![image](/media/backup-f-12.png)

![image](/media/backup-f-13.png)

![image](/media/backup-f-14.png)

![image](/media/backup-f-15.png)

Funkcjonalność tych opcji, nie odbiega od mechanizmów znanych z innych rozwiązań backupu, może za wyjątkiem opcji przedstawionych na ostatniej z tych grafik.

Istnieje bowiem możliwość, dostarczenia danych backupu do data-center Azure, za pomocą dysku zamówionego w Azure - lub przy pomocy własnego dysku (musi być on zgodny z wymaganiami Azure)

Kolejne 3 grafiki przedstawiają uruchomienie backupu na żądanie.

![image](/media/backup-f-16.png)

![image](/media/backup-f-17.png)

![image](/media/backup-f-18.png)

Po wykonaniu pierwszego backupu, status agenta w portalu Azure, powinien wyglądać jak na poniższej grafice:

![image](/media/backup-f-19.png)


###Odtworzenie plików z backupu.

Pliki odtwarzamy poprzez wywołanie polecenia **Recover Data** jak to przedstawiono poniżej:

![image](/media/backup-f-20.png)

Kolejne opcje ustawień, pozwalają wybrać z którego dnia dane zostaną udostępnione do odtworzenia. Udostępnienie danych backupu, następuje poprzez użycie przycisku **Mount**. Montuje to zasoby backupu, jako kolejny dysk w maszynie.
 
![image](/media/backup-f-21.png)

Na ostatnim ekranie konfiguracji udostępnienia danych backupu, można wcisnąć przycisk **Browse**:

![image](/media/backup-f-23.png)



albo przejść do udostępnionych danych, poprzez eksplorator plików Windows:

![image](/media/backup-f-22.png)

Aby zakończyć dostęp do danych backupu, należy wcisnąć przycisk **Unmount**

![image](/media/backup-f-24.png)
