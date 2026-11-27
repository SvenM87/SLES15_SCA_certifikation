# 06 · Netzwerkverwaltung

Eine IPv4-Präfixlänge beschreibt den Netzanteil. `/24` entspricht `255.255.255.0`. Entfernte Netze benötigen eine Route, meist über ein Default Gateway.

```bash
ip -br link
ip -br address
ip route
ip neigh
ss -lntup
ping -c 3 ZIEL
tracepath ZIEL
getent hosts example.org
```

SLES verwendet traditionell wicked. Dateien liegen unter `/etc/sysconfig/network/`, etwa `ifcfg-*`, `routes` und `config`.

```bash
sudo wicked show all
sudo wicked ifup eth0
sudo wicked ifdown eth0
sudo yast2 lan
```

Je nach Installation kann NetworkManager aktiv sein. Dann gelten `nmcli connection show` und `nmcli device status`. Werkzeuge nicht ungeprüft mischen.

`/etc/hosts` liefert lokale statische Zuordnungen. `/etc/nsswitch.conf` legt die Reihenfolge der Namensquellen fest. In der Labor-VM werden ein NAT- und ein Host-only-Interface getrennt konfiguriert.

## Diagnosefolge

1. Link und Interface prüfen.
2. Adresse und Präfix prüfen.
3. Route und Gateway prüfen.
4. Ziel per IP testen.
5. Namensauflösung testen.
6. Port und lokalen Dienst prüfen.
7. Firewall und Gegenstelle prüfen.

## Kontrollfragen

1. Welcher Befehl zeigt die Default Route?
2. Wo liegen wicked-Konfigurationen?
3. Warum ist `getent hosts` für die Systemauflösung aussagekräftig?
4. Wie zeigt man lauschende TCP- und UDP-Ports?
