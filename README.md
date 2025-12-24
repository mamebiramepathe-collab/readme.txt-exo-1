# readme.txt-exo-1
Les étapes de config de l'Exercice  01 : Packet Tracer 
Exercice 01 : Packet Tracer - Création d’un « MiniLab »
Matériel Nécessaire
* 1 Routeur Cisco 1941
* 3 Switches (PT)
* 3 Points d’Accès Wi-Fi (PT-AC)
* 3 PC Portables
* 6 PC Fixes
* 3 Téléphones IP Cisco 7960
________________


Étape 1 : Configuration des VLANs
1. Création des VLANs sur les Switches
Pour chaque switch, entrez en mode de configuration et créez les VLANs nécessaires :
bash
enable
configure terminal
vlan 1
 name VoIP
vlan 10
 name PC_Fixes
vlan 20
 name WiFi
vlan 30
 name Administration


2. Attribution des Ports aux VLANs
Configurez les ports sur chaque switch comme suit :
* Port 8 : Administration (VLAN 30)
* Ports 6-7 : PC fixes (VLAN 20)
* Ports 4-5 : Points d’accès Wi-Fi (VLAN 10)
* Ports 2-3 : Téléphones IP (VLAN 1)
* Ports 1 et 9 : Uplink (Trunk)
Exemple de Configuration des Ports sur un Switch
bash
interface range fa0/1, fa0/9
 switchport mode trunk


interface fa0/2
 switchport mode access
 switchport access vlan 1


interface fa0/3
 switchport mode access
 switchport access vlan 1


interface fa0/4
 switchport mode access
 switchport access vlan 10


interface fa0/5
 switchport mode access
 switchport access vlan 10


interface fa0/6
 switchport mode access
 switchport access vlan 20


interface fa0/7
 switchport mode access
 switchport access vlan 20


interface fa0/8
 switchport mode access
 switchport access vlan 30


3. Configuration du Trunk
Assurez-vous que les ports 1 et 9 de chaque switch sont configurés en mode trunk :
bash
interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30


interface fa0/9
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30


________________


Étape 2 : Configuration du Routeur Cisco 1941
4. Configuration des Interfaces VLAN
Configurez les interfaces correspondant à chaque VLAN sur le routeur :
bash
enable
configure terminal
interface GigabitEthernet0/0
 no shutdown


alors a partir de ce moment j’ai créer des sous interfaces

interface  GigabitEthernet0/0.1  (pour Vlan 1)
encapsulation dot1Q 1
 ip address 192.168.0.1 255.255.255.0  ; VLAN 1 (VoIP)


interface  GigabitEthernet0/0.10  (pour Vlan 10)
encapsulation dot1Q 1
 ip address 192.168.10.1 255.255.255.0 VLAN 10 (PC fixes)


interface  GigabitEthernet0/0.20  (pour Vlan 20)
encapsulation dot1Q 1
 ip address 192.168.20.1 255.255.255.0 ; VLAN 20 (Wi-Fi)


interface  GigabitEthernet0/0.30  (pour Vlan 30)
encapsulation dot1Q 1
 ip address 192.168.30.1 255.255.255.0 ; VLAN 30 (Administration)


5. Configuration du DHCP
Configurez les pools DHCP pour chaque VLAN :
bash
ip dhcp pool VLAN1
 network 192.168.0.0 255.255.255.0
 range 192.168.0.10 192.168.0.50


ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 range 192.168.10.10 192.168.10.50


ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 range 192.168.20.10 192.168.20.50


ip dhcp pool VLAN30
 network 192.168.30.0 255.255.255.0
 range 192.168.30.10 192.168.30.50


________________
