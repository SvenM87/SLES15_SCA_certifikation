# Labor 09 · Automatisierung und integrative Fehlersuche

Ein Skript `sca-audit.sh` erstellen, das:

1. ohne root lesbare System-, Storage-, Netzwerk- und Dienstinformationen sammelt,
2. Optionen mit `getopts` verarbeitet,
3. Voraussetzungen prüft,
4. Funktionen, Schleifen, `case` und verständliche Exitcodes verwendet,
5. Ausgaben in ein datiertes Verzeichnis schreibt,
6. Fehler nach stderr schreibt und sensible Daten auslässt,
7. das Ergebnis mit `tar --acls --xattrs` archiviert und eine SHA-256-Prüfsumme erzeugt.

Danach ein Fehlerbild bearbeiten, in dem ein Dienst wegen falscher Dateirechte nicht startet und der Zielport nicht lauscht. Erst Status und Journal erfassen, dann Rechte korrigieren, Dienst starten und Port prüfen.

## Verifikation

```bash
bash -n sca-audit.sh
shellcheck sca-audit.sh  # falls installiert
./sca-audit.sh -h
sha256sum -c SHA256SUMS
```

Das Skript muss bei fehlenden Argumenten und nicht ausführbaren Teilprüfungen definierte Exitcodes liefern.
