# Labor 03 · Boot, systemd, Timer und Journal

Vor Beginn einen ausgeschalteten VirtualBox-Snapshot `01-vor-systemd` erstellen.

1. Default Target, laufende Units, Unit-Dateien und fehlgeschlagene Units ermitteln.
2. Eine Unit `sca-lab.service` erstellen, die einen Zeitstempel nach `$LAB_ROOT/03-systemd/run.log` schreibt.
3. `daemon-reload`, `start`, `enable`, `is-active` und `is-enabled` verwenden und unterscheiden.
4. Einen `sca-lab.timer` für wiederholte Ausführung erstellen.
5. Einen absichtlichen Fehler in `ExecStart` diagnostizieren, korrigieren und mit `journalctl -u` belegen.
6. Bootkette von UEFI über GRUB, Kernel und initrd bis PID 1 dokumentieren.
7. `/proc/cmdline`, `/etc/default/grub`, die generierte GRUB-Konfiguration und `lsinitrd` untersuchen.
8. Im GRUB-Menü einmalig einen ungefährlichen Kernelparameter ergänzen und nach dem Boot über `/proc/cmdline` verifizieren.
9. Default Target zwischen `graphical.target` und `multi-user.target` wechseln und den Effekt nach einem Neustart prüfen.
10. Nach Snapshot einen fehlerhaften, mit `nofail` abgesicherten Laboreintrag in `/etc/fstab` diagnostizieren. Danach den korrekten Zustand wiederherstellen.

## Verifikation

```bash
systemctl is-active sca-lab.service
systemctl is-enabled sca-lab.timer
systemctl list-timers sca-lab.timer
journalctl -u sca-lab.service --no-pager
```

## Aufräumen

Units stoppen, deaktivieren, deren Dateien entfernen und `systemctl daemon-reload` ausführen.

Falls das System nicht mehr normal startet, über das GRUB-Menü ein Rettungsziel wählen oder den VirtualBox-Snapshot zurückspielen. Die erfolgreiche Rettung und Ursache dokumentieren.
