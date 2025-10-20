## Aufgabe 2:
Einstellung `ipv6 unicast-routing`, danach Ip addresse wie ipv4 einzustellen, statt `ip address` für ipv4 soll `ipv6 address` für ipv6 eingegeben werden. Für andere Kommando wie z.B. `show ip interface brief` soll `ip` durch `ipv6` ersetzt werden.

Die Route zeigen: `show ipv6 route`

## Route Einstellung für IPV6
Kommando:
```
ipv6 route <Zielnetz> <Prefix-Länge> <Next-Hop-Adresse>
```

Default Route R2: alles unbekannte Pakete werden auf R2 umgeleitet.
Auf R1: `ipv6 route ::/0 2001:DB8:1:A001::2`
Auf R3: `ipv6 route ::/0 2001:DB8:1:A002::1`

### IP-Addressbuch
| Gerät | Schnittstelle | IPv6-Adresse / Präfix            |
| ----- | ------------- | -------------------------------- |
| R1    | G0/0          | 2001:DB8:1:1::1/64               |
|       | S0/0/0        | 2001:DB8:1:A001::1/64            |
| R2    | G0/0          | 2001:DB8:1:2::1/64               |
|       | S0/0/0        | 2001:DB8:1:A001::2/64            |
|       | S0/0/1        | 2001:DB8:1:A002::1/64            |
| R3    | G0/0          | 2001:DB8:1:3::1/64               |
|       | S0/0/1        | 2001:DB8:1:A002::2/64            |
| PC1   | –             | 2001:DB8:1:1::100/64             |
|       | Gateway       | 2001:DB8:1:1::1 (nicht FE80::1!) |
| PC2   | –             | 2001:DB8:1:2::100/64             |
|       | Gateway       | 2001:DB8:1:2::1                  |
| PC3   | –             | 2001:DB8:1:3::100/64             |
|       | Gateway       | 2001:DB8:1:3::1                  |


**Achtung: Lokale Gateway EInstellung auf PC mit FE80 nicht funktioniert**

- Aufgabenstellung wollte FE80::1/2/3 als Gateway auf PCs.
- Problem: FE80 = Link-Local, gültig nur direkt verbundener Link
- PC1 hängt an R1 G0/0 → FE80::1 erreichbar ✅
- PC1 kann nicht R1 S0/0/0 (ein anderes Subnetz) erreichen ❌
- **Lösung**: PCs als Default-Gateway die globale IPv6-Adresse des direkt verbundenen Routers verwenden (z. B. 2001:DB8:1:1::1).
- **Erkenntnis**: Link-Local funktioniert nur lokal, globale IPv6-Adressen sind routbar.

### Einstellung
```
R1:
ipv6 route 2001:DB8:1:2::/64 S0/0/0 2001:DB8:1:A001::2
ipv6 route 2001:DB8:1:3::/64 S0/0/0 2001:DB8:1:A001::2
```

```
R2:
ipv6 route 2001:DB8:1:1::/64 S0/0/0 2001:DB8:1:A001::1
ipv6 route 2001:DB8:1:3::/64 S0/0/1 2001:DB8:1:A002::2
```

```
R3:
ipv6 route 2001:DB8:1:1::/64 S0/0/1 2001:DB8:1:A002::1
ipv6 route 2001:DB8:1:2::/64 S0/0/1 2001:DB8:1:A002::1
```

