## DCE und DTE
DEC: Taktgeber
DTE: Takt-Empfänger

## Route Einstellung
1. R2 ist nicht mit dem PC1 Netz konfiguiert, daher kann er mit R1 kommunizieren kennt aber der Netz von PC1 noch nicht. Lösung: in R2 die Router erstellen, mapping für Pakte aus Netz PC 2 --> R1 mit Serielle Port 172.16.20.1 (ziel), durch `R2(config)# ip route 172.16.30.0 255.255.255.0 172.16.20.1`, also alles für 172.16.30.0, leitet er weiter an 172.16.20.1 (R1).
2. R1 muss auch den PC2 kennen, er muss ähnlich wie R2 eingestellt werden `R1(config)# ip route 172.16.10.0 255.255.255.0 172.16.20.2`, diese kann so interpretiert werden **"alles für 172.16.10.0, schickt an 172.16.20.2"**, damit der R1 weißt, sobald er ein Packet mit Endziel 172.16.10.0 Netz erhaltet, kann er die entsprechende Pfad an R2 (172.16.20.2) nutzen um das Packet weiterzuleiten.
3. Ähnlich wie R1 und R2, muss die R3 auch eingestellt werden. Allerdings aufpassen, die ip routine auf jeweilige Route darf nicht "über die Schnittsteller" definiert werden, z.B. `ip route 192.168.40.0 255.255.255.0 192.168.50.1` (Packet ziel Netz 3 --> serielle Port in R2 auf Abzweigung Netz3) funktioniert nicht. Da R1 ist mit R2 via 172.16.20.1 und 172.16.20.2 angeschlossen, eine "Quer-Einstellung funktioniert hier nicht". Die Verbindung von R1 und R3 kann aber doch in R2 via `ip route 192.168.40.0 255.255.255.255 192.168.50.2 erfolgen`.

**Zusammenfassung**
Einstellung in R1, 2, 3
```
R1:
ip route 192.168.40.0 255.255.255.0 172.16.20.2
ip route 172.16.10.0 255.255.255.0 172.16.20.2

R2:
ip route 172.16.30.0 255.255.255.0 172.16.20.1
ip route 192.168.40.0 255.255.255.0 192.168.50.2

R3:
ip route 172.16.30.0 255.255.255.0 192.168.50.1
ip route 172.16.10.0 255.255.255.0 192.168.50.1
```


