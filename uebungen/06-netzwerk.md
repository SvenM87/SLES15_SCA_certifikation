# Labor 06 · Netzwerkdiagnose

Vor Beginn Snapshot `03-vor-netzwerk` erstellen. Adapter 1 verwendet NAT, Adapter 2 das VirtualBox-Host-only-Netz.

1. Interfaces, Namen, MAC-Adressen, IP-Adressen, Präfixe und Default Route erfassen. MAC-Adressen mit den VirtualBox-Einstellungen abgleichen.
2. `/etc/hosts`, `/etc/resolv.conf`, `/etc/nsswitch.conf` und vorhandene wicked-Dateien vergleichen.
3. `localhost` und einen externen Namen mit `getent hosts` auflösen.
4. Erreichbarkeit schichtweise mit `ip`, `ping`, `tracepath` und `curl` untersuchen.
5. Lauschende Ports und bestehende Verbindungen mit `ss` ermitteln.
6. Adapter 2 zunächst mit `ip` temporär und anschließend mit wicked persistent konfigurieren. NAT-Adapter und Default Route nicht verändern.
7. Host-only-Verbindung zwischen Windows-Host und SLES-VM per Ping und SSH testen.
8. Eine zusätzliche Testadresse nur temporär an `lo` setzen, verifizieren und entfernen.
9. Eine falsche Präfixlänge oder DNS-Einstellung gezielt diagnostizieren und korrigieren.
10. Dasselbe Interface in YaST Network Settings identifizieren und Konfiguration vergleichen.

## Verifikation

```bash
ip -br address
ip route
getent hosts localhost
ss -lntup
```

VirtualBox-NAT, Host-only-Netz und SLES-Konfiguration getrennt dokumentieren. Nach dem Labor muss die NAT-Verbindung weiter funktionieren.
