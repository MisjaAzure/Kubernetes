# Kubernetes - instrukcja uruchomienia przykładowego środowiska.
Dokument ten opisuje proces uruchoenia środowska Kubernetes w oparciu o oprogramowanie Minikube, zainstalowane na systemie Windows.
Zawiera również, opis procesu zainstalowania dwóch przykładowych aplikacji WEB-owych.
## Uruchomienie klastra Minikube

Wymagane elementy:
- Komputer x64 z pamięcią o wielkości 6GB lub więcej
- System operacyjny Windows z zainstalowaną funkcją Hyper-V
- Zainstalowany program wget. Można go pobrać z tej lokalizacji: http://url.przyklad.com/test.gif

### Proces uruchomienia klastra

#### 1. Zainstalowanie aplikacji minikube.exe
Należy otworzyć okno konsoli PowerShell i wykonać nastepujący kod:

```powershell
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```
#### 2. Dodanie do zmiennej path, wartości wskazującej położenie pliku minikube.exe
Należy wykonać nastepujący kod:
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

- ustawianie domyslnego drivera
```
minikube config set driver hyperv
```

- wystartowanie procesu uruchamiania klastra z określeniem uzytego przełącznika 
```
minikube start --hyperv-virtual-switch="switch0"
```
Po poprawnym wystartowaniu klastra, przystawka zarządznia Hyper-V, powininna przedstawiać sytuację jak na poniższej grafice.

![image](/media/hv2.png)


- Zainstalowanie ingress-controller /będzie potrzebny dla jednej z aplikacji
```
minikube addons enable ingress
```

- Wlaczenie opcji możliwości użycia load-balancera
```
minikube tunnel
```
> **Uwaga** Powyższe polecenie, nie uwalnia już procesu cmd.exe  W celu kontynuowania należy uruchomić koljne okno cmd.exe - należy pamiętać o opcji "uruchom jako administrator".

- Uruchomienie dashboardu
```
minikube dashboard
```
**Podobnie** jak wyżej. Konieczne jes otwarcie kolejnego okna cmd.exe



