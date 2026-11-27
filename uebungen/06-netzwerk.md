# Labor 06 · Netzwerkdiagnose

1. Interfaces, Namen, MAC-Adressen, IP-Adressen, Präfixe, Default Route und WSL-Gateway erfassen.
2. `/etc/hosts`, `/etc/resolv.conf`, `/etc/nsswitch.conf` und vorhandene wicked-Dateien vergleichen.
3. `localhost` und einen externen Namen mit `getent hosts` auflösen.
4. Erreichbarkeit schichtweise mit `ip`, `ping`, `tracepath` und `curl` untersuchen.
5. Lauschende Ports und bestehende Verbindungen mit `ss` ermitteln.
6. Eine zusätzliche Testadresse nur temporär an `lo` setzen, verifizieren und entfernen.
7. Für ein vorgegebenes Fehlerbild eine Diagnosefolge formulieren.

## Verifikation

```bash
ip -br address
ip route
getent hosts localhost
ss -lntup
```

WSL-NAT und automatisch erzeugtes `resolv.conf` im Ergebnis berücksichtigen. Persistente wicked-Konfiguration zusätzlich in einer VM üben.
