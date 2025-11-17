# Praktikum 3
Gegeben ist ein Netzwerkschema mit einer, vermutlich hybriden, Linien Topologie. Die Router sind linienförmig (Punkt-zu-Punkt) miteinander verbunden. Wie die Stub-Netzwerke gestaltet sind, ist unklar. Die Hälfte der Stub-Netzwerke werden über Loopback-Adressen abgebildet, was in der folgenden Aufzählung mit "Lo" gekennzeichnet wird.

Aus der Netzwerkskizze ergeben sich 6 Subnetze mit folgenden Größen:
- Subnetz 1: 16.001 Hosts
- Subnetz 2: 8.001 Hosts Lo
- Subnetz 3: 4.001 Hosts Lo
- Subnetz 4: 2.001 Hosts
- Subnetz 5: 1.001 Hosts Lo
- Subnetz 6: 501 Hosts

Außerdem sind die drei Router seriell in Linie miteinander verbunden (R1 --- R2 --- R3). Jede serielle Punkt-zu-Punkt Verbindung erfordert ein eigenes Subnetz. Daher werden für die Router zwei weitere Subnetze benötigt
- Subnetz 7: 2 Hosts (R1 --- R2)
- Subnetz 8: 2 Hosts (R2 --- R3)

Es sind zudem gängige Konventionen zu berücksichtigen, die besagen, dass die Router die jeweils ersten, oder letzten verfügbaren Gateway-Adressen des jeweiligen Netzwerkes zugewiesen bekommen.

Ebenfalls gegeben, ist der Addressblock **172.16.128.0/17**, sowie der Loopback-Adressraum **127.0.0.0/8**. 
## Aufgabe 1
### a) 
Die Anzahl der gegebenen Hosts beträgt 31.510 (10 Adressen allein für die Router), der gegebene Adressblock mit dem Netzwerkpräfix /17 verfügt über 32.767 möglichen Adressen. Somit ist dieser groß genug, um ein Netzwerk zu realisieren.  

### b)
Zur Realisierung der 8 Subnetze ist es notwendig, Host-Bits aus der Netzmaske zu "borgen" und damit neue Netzwerkadressen zu erzeugen. 

Der Bedarf an Adressen je Subnetz wird in der folgenden Auflistung durch den Netzwerkpräfix angezeigt.
- Subnetz 1: 16.001 Hosts -> /18
- Subnetz 2: 8.001 Hosts -> /19
- Subnetz 3: 4.001 Hosts -> /20
- Subnetz 4: 2.001 Hosts -> /21
- Subnetz 5: 1.001 Hosts -> /22
- Subnetz 6: 501 Hosts -> /23
- Subnetz 7: 2 Hosts -> /30
- Subnetz 8: 2 Hosts -> /30

Es ist also möglich, die geforderte Anzahl an Subnetze zu erzeugen.

In der folgenden Tabelle wurden die Loopback-Adressbereiche für die Loopbackadressen bereits berücksichtigt. 

### Subnetze
| Nr | Subnetzbeschreibung | Anzahl Hosts | Netzwerkadresse | Erste gültige Adresse | Broadcastadresse |
| -- | ------------------- | ------------ | --------------- | --------------------- | ---------------- |
| 1  | Subnetz 1           |       16.001 | 172.16.128.0    | 172.16.128.1          | 172.16.191.255   |PC2 
| 2  | Subnetz 2 Lo        |        8.001 | 172.16.192.0    | 172.16.192.1          | 172.16.223.255   |Lo2
| 3  | Subnetz 3 Lo        |        4.001 | 172.16.224.0    | 172.16.224.1          | 172.16.239.255   |Lo1
| 4  | Subnetz 4           |        2.001 | 172.16.240.0    | 172.16.240.1          | 172.16.247.255   |PC1
| 5  | Subnetz 5 Lo        |        1.001 | 172.16.248.0    | 172.16.248.1          | 172.16.251.255   |Lo3
| 6  | Subnetz 6           |          501 | 172.16.252.0    | 172.16.252.1          | 172.16.253.255   |PC3
| 7  | Subnetz 7 Router 1  |            2 | 172.16.255.0    | 172.16.255.1          | 172.16.255.3     |
| 8  | Subnetz 8 Router 2  |            2 | 172.16.255.4    | 172.16.255.5          | 172.16.255.7     |

### Adressschema
| Gerät | Interface | IP-Adresse   | Subnetzmaske    | CIDR |
| ----- | --------- | ----------   | ------------    | ---- |
| PC1   | Labor     | 172.16.240.2 | 255.255.248.0   | /21  |
| PC2   | Labor     | 172.16.128.2 | 255.255.192.0   | /18  |
| PC3   | Labor     | 172.16.252.2 | 255.255.254.0   | /23  |
| R1    | Gi0/0     | 172.16.240.1 | 255.255.248.0   | /21  |
| R1    | Lo0       | 172.16.224.1 | 255.255.240.0   | /20  |
| R1    | S0/0/0    | 172.16.255.1 | 255.255.255.252 | /30  |
| R2    | Gi0/0     | 172.16.128.1 | 255.255.192.0   | /18  |
| R2    | Lo1       | 172.16.192.1 | 255.255.224.0   | /19  |
| R2    | S0/0/0    | 172.16.255.2 | 255.255.255.252 | /30  |
| R2    | S0/0/1    | 172.16.255.5 | 255.255.255.252 | /30  |
| R3    | Gi0/0     | 172.16.252.1 | 255.255.254.0   | /23  |
| R3    | Lo2       | 172.16.248.1 | 255.255.252.0   | /22  |
| R3    | S0/0/1    | 172.16.255.6 | 255.255.255.252 | /30  |

### c)
Netzwerkskizze hier einfügen.

### d)
Netzwerk in Cisco Packet Tracer abgebildet.

### e)
Konfiguration der Taktrate der seriellen Anschlüsse auf 64.000 Bits pro Sekunde in Cisco Packet Tracer.

### f)
Statische Routen in Cisco Packet Tracer abgebildet.

### g)
Funktionsfähigkeit getestet.

## Aufgabe 2
Der IPv6-Adressblock **2001:db8:d112::/48** wurde zugewiesen.

### a)
IPv6 Adressen unterscheiden sich von IPv4 Adressen in folgendem:
- Der abbildbare Adressraum ist größer (IPv4 mit 4,3 x 10^9, IPv6 mit 3,4 x 10^38 Adressen).
- Die zustandslose automatische Konfiguration von IPv6 Adressen ist möglich (SLAAC).
- Subnetting wurde im Konzept berücksichtigt. IPv6 Adressen bestehen aus [Global Routing Prefix]+[Subnet ID]+[Interface ID].
-  Die Broadcast Adresse ist nicht mehr vorhanden, dafür ist diese Funktion über die "All-nodes multicast group" abgebildet.
  
Die Anzahl der möglichen Subnetze bei einem /64 Adressblock beträgt 65.535. Das ergibt sich wie folgt: Der Global Routing Prefix beträgt 48 Bit (3 Hextets = 48 Bits), dies wird von 64 abgezogen. Es bleibt ein Hextet = 16 Bit für die Subnet ID übrig.

### b)
### Adressschema
Netzwerk 1: 2001:db8:d112:1::/64  
Netzwerk 2: 2001:db8:d112:2::/64  
Netzwerk 3: 2001:db8:d112:3::/64  
Netzwerk 4: 2001:db8:d112:4::/64  
Netzwerk 5: 2001:db8:d112:5::/64  
Netzwerk 6: 2001:db8:d112:6::/64  
Netzwerk 7: 2001:db8:d112:7::/64  
Netzwerk 8: 2001:db8:d112:8::/64  

| Gerät | Interface |     IPv6-Adresse      | 
| ----- | --------- | --------------------- | 
| PC1   | Labor     | 2001:db8:d112:4::2/64 | 
| PC2   | Labor     | 2001:db8:d112:1::2/64 | 
| PC3   | Labor     | 2001:db8:d112:6::2/64 | 
| R1    | Gi0/0     | 2001:db8:d112:4::1/64 | 
| R1    | Lo0       | 2001:db8:d112:3::1/64 | 
| R1    | S0/0/0    | 2001:db8:d112:7::1/64 | 
| R2    | Gi0/0     | 2001:db8:d112:1::1/64 | 
| R2    | Lo1       | 2001:db8:d112:2::1/64 | 
| R2    | S0/0/0    | 2001:db8:d112:7::2/64 | 
| R2    | S0/0/1    | 2001:db8:d112:8::1/64 | 
| R3    | Gi0/0     | 2001:db8:d112:6::1/64 | 
| R3    | Lo2       | 2001:db8:d112:5::1/64 | 
| R3    | S0/0/1    | 2001:db8:d112:8::2/64 | 