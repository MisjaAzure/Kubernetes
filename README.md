# Warsztaty Azure i F5

### Dokumenty własne

#### 1. Opis uruchomienia niewielkiego środowiska Kubernetes - na fizycznych zasobach (on-premises).

W jego skład wchodzi:
- uruchomienie klastra K8S na bazie oprogramowania Minikube
- uruchomienie w utworzonym klastrze - dwóch aplikacji

   https://github.com/MisjaAzure/Kubernetes/blob/main/kubernetes.md
   
#### 2. Opis konfiguracji backupu Azure.
   Opis przedstawia kolejne kroki, celem skonfigurowania backupu na poziomie plików - z maszyny lokalnej do Azure.

   https://github.com/MisjaAzure/Warsztaty-Azure-F5/blob/main/file-backup-to-Azure.md

#### 3. Opis zestawienia VPN typu Site to site (S2S) - między platformą Azure a środowiskiem on premises.

   https://github.com/MisjaAzure/Warsztaty-Azure-F5/blob/main/vpn-s2s.md


#### 4. Opis zestawienia VPN typu Client to Site (to Azure).

   https://github.com/MisjaAzure/Warsztaty-Azure-F5/blob/main/vpn-c2s.md
   
#### 5. Opis wdrożenia Load Balancera.

   https://github.com/MisjaAzure/Warsztaty-Azure-F5/blob/main/load-balancer.md

Wdrożenie Load Balancera, umożliwia uzyskanie dostępu do Internetu, maszynom pracującym w sieci Azure. Umożliwia również **przekierowanie portów**, celem osiągnięcia dostępu do określonych usług tych maszyn, bez użycia VPN.
Oczywiście, innym rozwiązaniem jest przydzielenie adresów publicznych tworzonym maszynom.

W sytuacji, kiedy nie ma potrzeby tworzyć dostępu do maszyn z zewnątrz a istotne jest tylko to - aby one miały dostęp do Internetu - można wówczas wdrożyć urządzenie nazwane **NAT Gateway**.  Nie daje ono jednak możliwości, jaką jest przekierowanie portów. Daje tylko dostęp do Internetu.

### Odnośniki do materiałów w Internecie

#### 1. Cennik platformy Azure

https://azure.microsoft.com/pl-pl/pricing/#product-pricing

#### 2. Kalkulator cen
https://azure.microsoft.com/pl-pl/pricing/calculator/

#### 3. Azure Hybrid Benefits (AHB)

FAQ

https://azure.microsoft.com/pl-pl/pricing/hybrid-benefit/#faq

Kalkulator

https://azure.microsoft.com/pl-pl/pricing/hybrid-benefit/#calculator

#### 4. Maszyny typu SPOT

Opis + FAQ

https://docs.microsoft.com/pl-pl/azure/virtual-machines/spot-vms

#### 5. Rezerwowane instancje

Opis + FAQ
https://azure.microsoft.com/pl-pl/pricing/reserved-vm-instances/#faq

#### 6. High Availability | Regiony | Deklaracja zgodności z GDPR

Wykaz regionów
https://azure.microsoft.com/en-us/explore/global-infrastructure/data-residency/#overview

Lista par regionów
https://docs.microsoft.com/pl-pl/azure/availability-zones/cross-region-replication-azure 

Interaktywna mapa infrastruktury
https://infrastructuremap.microsoft.com/

#### 7. Dokumentacja dotycząca migracji zasobów on-prem do Azure

https://docs.microsoft.com/pl-pl/azure/migrate/
  
Migracja VMs z hostów Hyper-V
  
https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-hyper-v?tabs=UI

#### 8. Koncepcja AKS (Azure Kubernetes Services)

https://docs.microsoft.com/en-us/azure/aks/concepts-clusters-workloads 
  
Sieć w AKS
  
https://docs.microsoft.com/en-us/azure/aks/concepts-network 

#### 9. Express Route

Lokalizacje partnerów

https://docs.microsoft.com/pl-pl/azure/expressroute/expressroute-locations
