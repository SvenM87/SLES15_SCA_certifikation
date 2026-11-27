# Labor 02 · Dateien, Shell und Vim

1. Unter `$LAB_ROOT/02-shell` die Verzeichnisse `input`, `output` und `backup` anlegen.
2. Zehn Textdateien mit verschiedenen Endungen und Zeitstempeln erzeugen.
3. Mit `find` nur `.conf`-Dateien der letzten 24 Stunden finden.
4. Inhalte mit `grep`, `cut`, `sort`, `uniq` und einer Pipe auswerten. stdout und stderr getrennt protokollieren.
5. Hardlink und Symlink auf eine Datei erzeugen. Original umbenennen und Verhalten erklären.
6. Eine Konfigurationskopie in Vim suchen, ersetzen und per `diff -u` prüfen.
7. Das Labor mit `tar` archivieren, Inhalt anzeigen und SHA-256-Prüfsumme erzeugen.

## Verifikation

```bash
tar -tf "$LAB_ROOT/02-shell/backup/shell.tar.gz"
sha256sum -c "$LAB_ROOT/02-shell/backup/SHA256SUMS"
```

## Aufräumen

Nur `$LAB_ROOT/02-shell` nach Prüfung entfernen.
