# Kubernetes
Dokument ten opisuje proces uruchoenia środowska Kubernetes w oparciu o oprogramowanie Minikube, zainstalowane na systemie Windows.
Zawiera również, opis procesu zainstalowania dwóch przykładowych aplikacji WEB-owych.
## Uruchomienie Minikube

Potrzebne elementy:
- Komputer x64 z pamięcią o wielkości 6GB lub więcej
- System operacyjny Windows z zainstalowaną funkcją Hyper-V
- Zainstalowany program wget. Można go pobrać z tej lokalizacji: http://url.przyklad.com/test.gif
### Proces uruchomienia klastra
1. Zainstalowanie aplikacji minikube.exe Należy otworzyć okno konsoli PowerShell i wykonać nastepujący kod:

```powershell
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```
2. Dodanie do zmiennej path wartości wskazującej  położenie pliku minikube.exe

```powershell
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ `
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) `
}

```
3. Stworzenie wirtualnego przełącznika o nazwie "switch0". Do tego celu można uzyć graficznej przystawki zarządzania Hyper-V (virtmgmt.msc)

![image](/media/hv.png)

1. asasdasd


### Kubernetes### Kubernetes
#### Kubernetes
##### Kubernetes



