# Labor 08 · RPM, Zypper und YaST

1. Produkte, Services und Repositories samt Alias, URL, Priorität und Refreshstatus erfassen.
2. Ein kleines, entbehrliches Paket suchen, Informationen anzeigen, installieren und entfernen.
3. Mit RPM Paketinhalt, Abhängigkeiten, Signatur, Dokumentation und Eigentümer einer Datei ermitteln.
4. Eine Lab-Kopie einer Paketdatei mit dem Original vergleichen. `rpm -V` auf ein Paket anwenden und Ausgabe erklären.
5. Paket sperren und entsperren.
6. `zypper patch`, `update` und `dist-upgrade` abgrenzen. Nur sichere Listen- oder Dry-run-Operationen nutzen.
7. Dasselbe Paket und Repository in YaST Software Management finden.

## Verifikation

```bash
zypper verify
zypper locks
rpm -q PAKET
rpm -V PAKET
```

Keine Basisprodukte oder System-Repositories entfernen. GPG- und TLS-Prüfung nicht deaktivieren.
