# Labor 03 · systemd, Timer und Journal

Voraussetzung ist systemd in WSL. GRUB-Aufgaben werden nur analysiert und später in einer VM ausgeführt.

1. Default Target, laufende Units, Unit-Dateien und fehlgeschlagene Units ermitteln.
2. Eine Unit `sca-lab.service` erstellen, die einen Zeitstempel nach `$LAB_ROOT/03-systemd/run.log` schreibt.
3. `daemon-reload`, `start`, `enable`, `is-active` und `is-enabled` verwenden und unterscheiden.
4. Einen `sca-lab.timer` für wiederholte Ausführung erstellen.
5. Einen absichtlichen Fehler in `ExecStart` diagnostizieren, korrigieren und mit `journalctl -u` belegen.
6. `/proc/cmdline`, vorhandene GRUB-Dateien und `lsinitrd` untersuchen, ohne sie in WSL zu ändern.

## Verifikation

```bash
systemctl is-active sca-lab.service
systemctl is-enabled sca-lab.timer
systemctl list-timers sca-lab.timer
journalctl -u sca-lab.service --no-pager
```

## Aufräumen

Units stoppen, deaktivieren, deren Dateien entfernen und `systemctl daemon-reload` ausführen.
