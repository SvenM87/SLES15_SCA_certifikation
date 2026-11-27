# Labor 07 · Benutzer, Rechte, SSH und Logs

1. Temporäre Gruppen `scaops`, `scaaudit` und zwei Laborbenutzer anlegen.
2. Primär- und Zusatzgruppen, Passwortalterung, Sperren und `/etc/skel` untersuchen.
3. Ein gemeinsames SGID-Verzeichnis und ein Sticky-Bit-Austauschverzeichnis erstellen.
4. POSIX-ACLs setzen, Maske erklären, ACL sichern und wiederherstellen.
5. Eine eng begrenzte sudo-Regel in `/etc/sudoers.d/` mit `visudo -cf` prüfen.
6. Ed25519-Schlüssel für einen Laborbenutzer erzeugen und Anmeldung gegen `localhost` testen. `sshd -t` vor Reload verwenden.
7. Mit `logger` Meldungen verschiedener Priorität erzeugen, im Journal filtern und eine Lab-Logrotate-Regel im Debugmodus prüfen.
8. `supportconfig` ausführen oder, falls nicht installiert, Zweck, Ausgabeort und Datenschutz erklären.
9. VNC und RDP konzeptionell SSH gegenüberstellen.

## Verifikation

```bash
id scauser1
getfacl "$LAB_ROOT/07-admin/shared"
sudo visudo -cf /etc/sudoers.d/sca-lab
sudo sshd -t
journalctl -t sca-lab --since today --no-pager
```

## Aufräumen

Sudo-Regel, Schlüssel, temporäre Benutzer und Gruppen sowie Lab-Verzeichnisse gezielt entfernen.
