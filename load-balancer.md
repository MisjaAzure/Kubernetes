# Wdrożenie do infrastruktury urządzenia typu Load-Balancer.

## Cel.
Głównym celem, jest umożliwienie nawiązywania połączeń do Internetu, maszynom stojącym w wirtualnej sieci Azure.
Komunikacja ma się odbywać, przy użyciu wspólnego publicznego adresu IP.

Celem numer2, jest umożliwienie dostępu z Internetu do określonej maszyny, poprzez nawiązanie połączenia z IP za którym stoją te maszyny. Wybór określonej maszyny, ma odbywać się poprzez określenie numeru portu w trakcie połączenia.

## Wymagania.

Do wykonania tego przykładu, wymagane są elementy:
- Konto na platformie Azure wraz z Subskrypcją. 
- Wdrożona sieć wirtualna.
- Wdrożone dwie maszyny z systemem Linux, bez przypisanych publicznych adresów IP.
- W konfiguracji **Network Security Groups** interfejsów tych maszyn, musi być dopisana reguła typu **Inbound**, zezwalająca na połączenia z Internetu, przychodzące na port 22/TCP. Poniższa grafika prezentuje wgląd w taką regułę.

![image](/media/lb-00.png)

## Opis czynności.

W wyszukiwarce portalu Azure, należy wpisać fragment z **load balancer** i wybrać pozycję jak zaznaczono na grafice poniżej:

![image](/media/lb-01.png)

Następnie należy wybrać **+ Create**:

![image](/media/lb-02.png)

Otworzy się formularz konfiguracji wdrożenia Load Balancera:

![image](/media/lb-03.png)

Należy go wypełnić zgodnie z własnymi warunkami - proszę zwrócić uwagę na pole **Type** (opcję zaznaczoną na czerwono) aby wartość tam została ustawiona na **Public**

Przechodzimy teraz do konfiguracji grupy ustawień **Frontend IP configuration**. Naciskamy **+ Add a frontend IP configuration** i wypełniamy formularz w sposób jak na grafice:

![image](/media/lb-04.png)

Po zaakceptowaniu danych, klikamy w przycisk **Next: backend pools**. Następnie klikamy w **+ Add a backend pool**:

![image](/media/lb-06.png)
 
Formularz który się pokaże, konfigurujemy jak na grafice poniżej:

![image](/media/lb-07.png)

Klikamy w **+ Add**. Pojawią się dostępne maszyny. Zaznaczamy check-box na obydwu maszynach i wciskamy przycisk **Add** i później **Save**:

![image](/media/lb-08.png)

![image](/media/lb-09.png)

Następnie przechodzimy do konfiguracji reguł dla ruchu przychodzącego. Będziemy konfigurować reguły NAT, dzięki którym stanie się możliwy dostęp do maszyn z zewnątrz. Klikamy w **Next: Inbound rules >**: 

![image](/media/lb-10.png)

Konfigurujemy pierwszą regułę:

![image](/media/lb-11.png)

Pole **Frontend port**, określa port z którego połączenia będą kierowane na VM-kę opisaną w polach: **Target virtual machine** i **Network IP configuration** (maszyna może mieć więcej interfejsów).

Pole **Backend port** - oznacza oczywiście docelowy port na który zostanie skierowane połączenie.

Analogicznie postępujemy budując drugą regułę. Tu uwzględniamy oczywiście drugą maszynę:

![image](/media/lb-12.png)

Po zaakceptowaniu reguł - wciskamy przycisk **Next: Outbound rule**.

Pola formularza ustawiamy jak na grafice:

![image](/media/lb-13.png)

To wszystko. Pozostało tylko wykonać **Review + create**  oraz **Create** - i tym samym zainicjować wdrożenie urządzenia.

Po skończonym wdrożeniu, należy odwiedzić panel **Load Balancera**:

![image](/media/lb-14.png)

Należy odczytać adres IP na jakim jest on wystawiony w Internecie i przetestować możliwość połączenia do maszyn, oraz dostęp do Internetu z wnętrza maszyn.
