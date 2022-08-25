# Kubernetes - instrukcja uruchomienia przykładowego środowiska.
Dokument ten, opisuje proces uruchomienia środowiska Kubernetes w oparciu o oprogramowanie Minikube, zainstalowane na systemie operacyjnym Windows.\
Zawiera również, opis procesu instalowania dwóch przykładowych aplikacji WEB-owych.

## Uruchomienie klastra Minikube  / strona domowa projektu: https://minikube.sigs.k8s.io/docs/

Wymagane elementy:
- Komputer z procesorem x64 zgodnym z VT
- 6GB lub więcej pamięci operacyjnej
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

#### 3. Stworzenie wirtualnego przełącznika o nazwie "switch0".
Do tego celu można użyć graficznej przystawki zarządzania Hyper-V (virtmgmt.msc)

![image](/media/hv.png)


#### 4. Uruchomienie klastra.
Należy uruchomić wiersz konsoli cmd.exe w trybie administratora.
> **UWAGA!** - Należy wyłączyć ochronę w czasie rzeczywistym, realizowaną przez WindowsDefender. Bez tej czynności, można spodziewać się komunikatów o braku wystarczających uprawnień w trakcie wykonywania kolejnych kroków.

Z konsoli należy wykonać kolejno polecenia:

- ustawianie domyślnego drivera VM
```
minikube config set driver hyperv
```

- wystartowanie procesu uruchamiania klastra z określeniem przełącznika
```
minikube start --hyperv-virtual-switch="switch0"
```
Po poprawnym wystartowaniu klastra, przystawka zarządzania Hyper-V, powinna przedstawiać sytuację podobną do przedstawionej na poniższej grafice:

![image](/media/hv2.png)


- Zainstalowanie Ingress-Controller /będzie potrzebny dla jednej z aplikacji
```
minikube addons enable ingress
```

- Uruchomienie Dashboardu /nie jest to konieczne, ale daje graficzny wgląd w stan klastra.
```
minikube dashboard
```
> **Uwaga** Powyższe polecenie, nie uwalnia już procesu cmd.exe  W celu kontynuowania należy uruchomić kolejne okno cmd.exe. Pamiętać należy o opcji "uruchom jako administrator".


## Uruchomienie aplikacji Arcadia

#### 1. Pobranie plików wdrożeniowych

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


#### 2. Wdrożenie pobranych plików

```
minikube kubectl -- create -f all_apps.yaml
```

```
minikube kubectl -- create -f ingress_arcadia.yaml
```

Obserwując Dashboard klastra - pozycja Workloads z lewego panelu - można śledzić stan klastra i zaobserwować kiedy wszystkie serwisy będą już uruchomione. Po tym można przejść do kolejnych czynności.


#### 3. Testowanie działania aplikacji.


Aplikacja jest dostępna pod adresem IP maszyny wirtualnej Minikube. Jednym ze sposobów aby uzyskać ten adres, jest wgląd na zakładkę "Networking" w widoku właściwości maszyny. Przykład znajduje się na poniższej grafice.

![image](/media/hv4.png)


Druga (bardziej elegancka) możliwość, to wgląd w Dashboard, Menu/Service/Ingresses.  Kolumna Endpoints w głównym oknie - wskazuje adres oraz URL na którym jest wystawiony Ingress-Controller.  Pokazane jest to na poniższej grafice:


![image](/media/hv6.png)


Trzecia możliwość to komenda:

```
minikube service --all --namespace=ingress-nginx
```

Kolumna URL wtświetlonej tabeli - wskazuje adres IP.\


Aby przetestować działanie aplikacji, należy wejść na stronę http://example.host.net  Ten FQDN powinien być rozwiązywany na IP maszyny wirtualnej. Aby tak się stało, należy w systemie z którego będziemy wchodzić na podany URL, dodać wpis do pliku *hosts* - w sposób jak na przykładzie poniżej.

![image](/media/hv3.png)

Podkreślony przykładowy adres, należy zastąpić adresem odczytanym jak to przedstawiono powyżej.\

Finalnie, aby w pełni sprawdzić działanie aplikacji, powinno się do niej zalogować.\
Robi się to poprzez  przycisk:

![image](/media/hv5.png)

Login to: matt | Hasło: ilovef5\
Poprawnie pracująca aplikacja, przedstawi się podobną prezencją jak niżej zamieszczona grafika.
Można w niej przeprowadzać operacje finansowe.  Wszystko oczywiście dzieje się w środowisku na naszym komputerze. Moduł wysyłający email-e  oczywiście tylko emuluje taki proces. Żaden email nie jest wysyłany na zewnątrz.

![image](/media/hv7.png)
