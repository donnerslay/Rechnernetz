# Praktikum 1
## Voreinstellung
1. `Firewall` deaktivieren, die Shortcut auf desktop verwenden
2. Netzwerkkarte `Labor` verwenden. Slot Nr. is 7, diese slot ist auch auf den Schrank der PC-Switch beschriftet
3. Kable zwischen Slot `No 7` der PC-Swtich und `Console` der Prüfling-Swich zu verbinden
4. Kable mit `Console` der jewilige Switch verbinden, dann `.txt` Datei aus Packet tracer kopieren
5. In putty, `COM4` verwenden um `Switch oder Router` zu besuchen
6. In `Enable Modus`, die Einstellung direkt in Putty Fenster kopieren


**c) OSI Modell**
| **Schicht (OSI)** | **Funktion**                       | **Beispielprotokolle**       | **TCP/IP-Entsprechung** |
| ----------------- | ---------------------------------- | ---------------------------- | ----------------------- |
| 7. Anwendung      | Benutzerinteraktion, Dienste       | HTTP, FTP, DNS, SMTP         | Anwendung               |
| 6. Darstellung    | Datenformatierung, Verschlüsselung | JPEG, ASCII, TLS             | Anwendung               |
| 5. Sitzung        | Sitzungsaufbau/-steuerung          | NetBIOS, RPC                 | Anwendung               |
| 4. Transport      | Ende-zu-Ende-Verbindungen          | TCP, UDP                     | Transport               |
| 3. Vermittlung    | Routing, logische Adressen         | IP, ICMP                     | Internet                |
| 2. Sicherung      | MAC-Adressen, Fehlererkennung      | Ethernet, VLAN (802.1Q), PPP | Netzwerkschichtzugang   |
| 1. Bitübertragung | Elektrische/physische Übertragung  | Kabel, Signale               | Netzwerkschichtzugang   |

**d) Router, Swtich und Hub**
| Komponente | OSI-Schicht                | Funktion                                          | Besonderheiten                                                     |
| ---------- | -------------------------- | ------------------------------------------------- | ------------------------------------------------------------------ |
| **Hub**    | Schicht 1 (Bitübertragung) | Sendet empfangene Signale an **alle Ports**       | Keine MAC-Adresstabelle, keine Intelligenz, große Kollisionsdomäne |
| **Switch** | Schicht 2 (Sicherung)      | Leitet Frames basierend auf MAC-Adressen          | Jede Port = eigene Kollisionsdomäne, VLANs möglich                 |
| **Router** | Schicht 3 (Vermittlung)    | Verbindet Netzwerke, entscheidet über IP-Adressen | Trennung von Broadcastdomänen, Routing zwischen VLANs              |

**e) Netzwerktopologie**
Linientopologie (Punkt zu Punkt). Vorteil: kostengünstig, einfach zu verlegen. Nachteil: Höheausfallswahrscheinlichkeit. Wenige Skalierbarkeit.

Alternative:
`Ring-Topologie:`
Geräte seriell im Kreis verbunden.
Daten laufen von Gerät zu Gerät → fällt ein Knoten aus, ist der Ring unterbrochen.
Wird heute eher virtuell (z. B. Token Ring) genutzt.

`Stern-Topologie (Star Topology).`
Zentrale Geräte (Switches) verbinden Endgeräte sternförmig.
Jeder PC hat eine eigene Leitung zum Switch.
Fällt ein Endgerät oder Kabel aus → andere bleiben unbeeinflusst.
**f) CSMA/CD (Carrier Sense Multiple Access / Collision Detection**)
Verfahren:
Alle Geräte „hören“, ob das Medium frei ist (Carrier Sense).
Wenn frei → senden.
Wenn zwei gleichzeitig senden → Kollision erkannt (Collision Detection).
Geräte warten zufällig (Backoff) und senden erneut.

Wird es hier verwendet?
Nein, in unserer Versuchsumgebung nicht, weil:
Vollduplex-Switches verwendet werden.
Jeder Port hat eine eigene Kollisionsdomäne, daher keine physikalischen Kollisionen.

**g) Kollisions- und Broadcastdomäne**
Ein Router trennt Broadcastdomänen, ein Switch trennt Kollisionsdomänen.
| Begriff              | Bedeutung                                                      | Beispiel in deinem Netzwerk                                |
| -------------------- | -------------------------------------------------------------- | ---------------------------------------------------------- |
| **Kollisionsdomäne** | Bereich, in dem Datenkollisionen auftreten können              | Jede Switch-Port ist eine eigene Kollisionsdomäne          |
| **Broadcastdomäne**  | Bereich, in dem ein Broadcast (z. B. ARP) alle Geräte erreicht | VLAN10 und VLAN20 sind **jeweils eigene Broadcastdomänen** |

**f) MAC-Adressbuch, bitte selbst anpassen**
| Gerät | Interface    | VLAN   | MAC-Adresse (Beispiel) |
| ----- | ------------ | ------ | ---------------------- |
| PC1   | NIC          | VLAN10 | 00:11:22:AA:10:01      |
| PC2   | NIC          | VLAN20 | 00:11:22:AA:20:02      |
| PC3   | NIC          | VLAN20 | 00:11:22:AA:20:03      |
| S1    | Fa0/6 (PC1)  | VLAN10 | 00:0A:41:D9:98:01      |
| S2    | Fa0/6 (PC2)  | VLAN20 | 00:0A:41:D9:98:02      |
| S2    | Fa0/11 (PC3) | VLAN20 | 00:0A:41:D9:98:03      |
| R1    | G0/0.10      | VLAN10 | 00:50:56:FF:10:01      |
| R1    | G0/0.20      | VLAN20 | 00:50:56:FF:20:01      |

**g) Wie füllt der Switch diese Tabelle?**
Bei eingehenden Frames liest der Switch die Quell-MAC-Adresse.
Er merkt sich: „Diese MAC kommt über Port X“ → speichert das in der MAC-Tabelle.
Bei einem Ziel-Frame schaut der Switch in die Tabelle:
Wenn bekannt → Frame an den entsprechenden Port weiterleiten.
Wenn unbekannt → Frame an alle Ports im VLAN broadcasten.

**Wie füllt ein Switch seine MAC-Adresstabelle?**
  
A switch fills its MAC table using a simple learning process:

A device sends a frame to the switch.

The switch reads the source MAC address from the frame header.

It records the MAC address in its table, along with the port the frame came from.

The MAC address is dynamic (learned) and will expire after some time if no traffic is received from that MAC.

The switch then uses the table to forward frames efficiently — it sends traffic only to the port where the destination MAC is located.
💡 In short:

A switch observes incoming frames, notes which port each MAC comes from, and stores this in its table. This is called MAC address learning.

## Gedankenspiel 1
![13247a0e635bf2d7749815dc9076455d.png](:/1aa55eb01fda45b4a2ff76767719c2a6)

1. Protokoll ist **Ethernet**
2. https://de.wikipedia.org/wiki/Datenframe
3. ![a737412be213ab746e86ea6793a2cf65.png](:/b6688ed41f1747e9aa01161375ba71ae)
4. Lokal in Labor

## Aufgabe 2
![8b6d08c58596ea9f3a9a9425582563eb.png](:/5b6176abfa7d43079d4803ed3f5a4a63)

**b) Addressschema**
| Abteilung   | VLAN | Netzwerk        | Beispiel-IP                              | Gateway (R1) |
| ----------- | ---- | --------------- | ---------------------------------------- | ------------ |
| Abteilung A | 10   | 192.168.50.0/24 | PC1 → 192.168.50.10                      | 192.168.50.1 |
| Abteilung B | 20   | 192.168.40.0/24 | PC2 → 192.168.40.20, PC3 → 192.168.40.30 | 192.168.40.1 |

**c) Logische Trennung durch VLANs**
Die logische Trennung von Netzwerken erfolgt über VLANs (Virtual Local Area Networks).
Jedes VLAN bildet eine eigene Broadcast-Domain, wodurch Netzlast und Sicherheitsrisiken reduziert werden.
Das Protokoll IEEE 802.1Q fügt den Ethernet-Frames einen VLAN-Tag (4 Byte) hinzu, der die VLAN-ID enthält.
- Access-Ports → senden ungetaggte Frames (z. B. PC-Ports)
- Trunk-Ports → senden getaggte Frames und transportieren mehrere VLANs über eine Leitung (z. B. S1–S2, S2–R1)
![b1edbf10574e87d91ab48ee673432b33.png](:/7c1bfcaf2ad24830bbfd7768891c8af7)

**d) Rahmen des Protokolls (802.1Q-Trunk)**
![4b52b1ac172e00146e25f6d6d6a8f7d3.png](:/2a6ca9a622ea41a2a76279fceb3e45c4)

| Feld       | Beschreibung                           |
| ---------- | -------------------------------------- |
| Ziel-MAC   | MAC-Adresse des Empfängers             |
| Quell-MAC  | MAC-Adresse des Senders                |
| 802.1Q-Tag | VLAN-ID (z. B. 10 oder 20), Priorität  |
| EtherType  | Protokoll (z. B. IP = 0x0800)          |
| Nutzdaten  | IP-Paket                               |
| FCS        | Frame Check Sequence (Fehlererkennung) |

**e) Router-on-a-Stick**
Der Router R1 verbindet die VLANs über Subinterfaces:
```
g0/0.10  -> VLAN 10 -> 192.168.50.1
g0/0.20  -> VLAN 20 -> 192.168.40.1
```

Jedes Subinterface fungiert als Gateway für ein VLAN.
Dadurch können sich die logisch getrennten Netze über den Router austauschen (Inter-VLAN-Routing).

**g) Vor- und Nachteil**
| Merkmal                            | Netz 1 (ohne VLANs) | Netz 2 (mit VLANs & Router) |
| ---------------------------------- | ------------------- | --------------------------- |
| Broadcast-Domain                   | Eine                | Pro VLAN getrennt           |
| Sicherheit                         | Gering              | Hoch                        |
| Skalierbarkeit                     | Gering              | Hoch                        |
| Kommunikation zwischen Abteilungen | Direkt, unsicher    | Über Router, kontrolliert   |
| Komplexität                        | Einfach             | Etwas höher (VLAN, Routing) |


# CSMA/CD
**Carrier Sense (Trägererkennung)**
- Bevor eine Station sendet, „hört“ sie auf das Medium, um festzustellen, ob jemand anderes gerade sendet.

**Multiple Access (Mehrfachzugriff)**
- Alle Stationen sind gleichberechtigt und können grundsätzlich jederzeit senden, wenn das Medium frei ist.

**Collision Detection (Kollisionserkennung)**
- Wenn zwei Stationen gleichzeitig anfangen zu senden, erkennt jede Station die Kollision (durch Signalstörungen).

**Collision Handling (Kollisionsbehandlung)**
- Beide Stationen brechen die Übertragung sofort ab.
- Sie warten eine zufällige Zeit (Backoff-Time, berechnet nach dem Binary Exponential Backoff-Algorithmus).
- Danach versuchen sie erneut zu senden.
