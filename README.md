# Kubernetes - instrukcja uruchomienia przykładowego środowiska.
Dokument ten opisuje proces uruchomienia środowiska Kubernetes w oparciu o oprogramowanie Minikube, zainstalowane na systemie Windows.
Zawiera również, opis procesu zainstalowania dwóch przykładowych aplikacji WEB-owych.

## Uruchomienie klastra Minikube

Wymagane elementy:
- Komputer z procesorem x64 zgodnym z VT, oraz pamięcią operacyjną o wielkości 6GB lub więcej.
- System operacyjny Windows 10 z włączoną funkcją Hyper-V


#### 1. Zainstalowanie aplikacji minikube.exe
Należy otworzyć okno konsoli PowerShell i wykonać nastepujący kod:

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

- Włączenie opcji możliwości użycia load-balancera
```
minikube tunnel
```
> **Uwaga** Powyższe polecenie, nie uwalnia już procesu cmd.exe  W celu kontynuowania należy uruchomić kolejne okno cmd.exe - należy pamiętać o opcji "uruchom jako administrator".

- Uruchomienie dashboardu
```
minikube dashboard
```
Po tym **podobnie** jak wyżej, konieczne jest otwarcie kolejnego okna cmd.exe w trybie administratora.


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


Aplikacja jest dostępna pod adresem IP maszyny wirtualnej Minikube
Aby przetestować jej działanie, należy spróbować wejść na stronę http://






#### Wdrożenie aplikacji w klastrze - za pomocą ściągniętych plików


