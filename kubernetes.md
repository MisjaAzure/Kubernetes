# Kubernetes - instrukcja uruchomienia przykładowego środowiska.
Dokument ten opisuje proces uruchomienia środowiska Kubernetes w oparciu o oprogramowanie Minikube, zainstalowane na systemie Windows.
Zawiera również, opis procesu zainstalowania dwóch przykładowych aplikacji WEB-owych.

## Uruchomienie klastra Minikube

Wymagane elementy:
- Komputer z procesorem x64 zgodnym z VT, oraz pamięcią operacyjną o wielkości 6GB lub więcej.
- System operacyjny Windows 10 z włączoną funkcją Hyper-V


#### 1. Zainstalowanie aplikacji minikube.exe
Należy otworzyć okno konsoli PowerShell (jako administrator) i wykonać następujący kod:

```powershell
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```
#### 2. Dodanie do zmiennej path, wartości wskazującej położenie pliku minikube.exe
Należy wykonać następujący kod:
```powershell
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ `
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) `
}

```
#### 3. Stworzenie wirtualnego przełącznika o nazwie &#8220switch0&#8221.
Do tego celu można uzyć graficznej przystawki zarządzania Hyper-V (virtmgmt.msc)

![image](/media/hv.png)


#### 4. Uruchomienie klastra.
Należy urucomić wiersz konsoli cmd.exe w trybie administratora
> **UWAGA!** - Należy wyłączyć ochronę w czasie rzeczywistym, realizowaną przez WindowsDefender. Bez tej czynności, można spodziwać się komunikatów o braku wystarczających uprawnień w trakcie wykonywania kolejnych kroków.

Z konsoli należy wykonać kolejno polecenia:

- ustawianie domyślnego drivera VM
```
minikube config set driver hyperv
```

- wystartowanie procesu uruchamiania klastra z określeniem użytego przełącznika 
```
minikube start --hyperv-virtual-switch="switch0"
```
Po poprawnym wystartowaniu klastra, przystawka zarządznia Hyper-V, powininna przedstawiać sytuację jak na poniższej grafice.

![image](/media/hv2.png)


- Zainstalowanie ingress-controller /będzie potrzebny dla jednej z aplikacji
```
minikube addons enable ingress
```

- Uruchomienie dashboardu /nie jest to konieczne, ale daje graficzny wgląd w stan klastra.
```
minikube dashboard
```
> **Uwaga** Powyższe polecenie, nie uwalnia już procesu cmd.exe  W celu kontynuowania należy uruchomić kolejne okno cmd.exe - należy pamiętać o opcji "uruchom jako administrator".


## Uruchomienie aplikacji Arcadia

#### Pobranie wymaganych plików wdrożeniowych

Proponuję ustalić ścieżkę bieżącą na c:\minikube  - tam będą pobierane pliki. 
Najprościej zrobić to poleceniem:
```
cd \minikube
```
Następnie:

```
curl --output all_apps.yaml --url https://raw.githubusercontent.com/MisjaAzure/Kubernetes/main/deployments/arcadia/all_apps.yaml
```

```
curl --output ingress_arcadia.yaml --url https://raw.githubusercontent.com/MisjaAzure/Kubernetes/main/deployments/arcadia/ingress_arcadia.yaml
```


#### Wdrożenie pobranych plików

```
minikube kubectl -- create -f all_apps.yaml
```

```
minikube kubectl -- create -f ingress_arcadia.yaml
```

Obserwując Dashboard->Workloads - można śledzić stan klastra i zaobserwować kiedy wszystkie serwisy będą już uruchomione. Po tym można przejść do kolejnych czynności.


### Testowanie działania tej aplikacji


Aplikacja jest dostępna pod adresem IP maszyny wirtualnej Minikube. Jednym ze sposobów aby odczytać ten adres, jest wgląd na zakładkę "Networking" w widoku właściwości maszyny. Przykład znajduje się na poniższej grafice.

![image](/media/hv4.png)


Druga (bardziej elegancka) możliwość, to wgląd w Dashboard, Lewe menu/service/Ingresses.  Kolumna Endpoints w głównym oknie - wskazuje adres oraz URL na którym jest wystawiony ingress-controller.  Jak pokazane jest to na grafice poniżej:


![image](/media/hv6.png)


Trzecia możliwość to komenda:

```
minikube service --all --namespace=ingress-nginx
```

Kolumna URL - wskazuje adres IP


--- potrzebny odstep


Aby przetestować  działanie aplikacji, należy wejść na stronę http://example.host.net  Ten FQDN powinien rozwiązywać się na IP maszyny wirtualnej. Aby tak się stało, należy w systemie z którego będziemy wchodzić na podany URL, dodać wpis do pliku hosts - w sposób jak na przykładzie poniżej.

![image](/media/hv3.png)

Podkreślony przykładowy adres, należy zastąpić adresem odczytanym jak to przedstawiono powyżej.


Finalnie, aby w pełni sprawdzić działanie aplikacji, można się do niej zalogować.
Robi się to poprzez  przycisk:

![image](/media/hv5.png)

*Login to: matt | Hasło: ilovef5*
Poprawnie pracująca aplikacja przedstawi się podobna prezencją jak niżej zamieszczona grafika.
Można w niej przeprowadzać mozliwe operacje finansowe.  Oczywiście wszystko dzieje się w środowiku na naszym kmputerze. Moduł wysyłający email's  ocztywiście tylko emuluje taki proces. Żaden email nie trafia na zewnątrz.

![image](/media/hv7.png)



