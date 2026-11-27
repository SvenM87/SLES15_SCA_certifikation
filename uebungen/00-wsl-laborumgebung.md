# WSL-Laborumgebung

## Empfehlung

Eine separate, entbehrliche SLES-15-WSL-Instanz verwenden. Vor dem Kurs in PowerShell sichern:

```powershell
wsl --shutdown
wsl --export SLES-15 C:\WSL-Backups\SLES-15-baseline.tar
wsl --import SLES-15-Lab C:\WSL\SLES-15-Lab C:\WSL-Backups\SLES-15-baseline.tar --version 2
```

Im Linux-System:

```bash
export LAB_ROOT="$HOME/sca-labs"
mkdir -p "$LAB_ROOT"
```

Für systemd `/etc/wsl.conf` mit folgendem Inhalt versehen und danach `wsl --shutdown` ausführen:

```ini
[boot]
systemd=true
```

## Sicherheitsregeln

- Keine Übungen in produktiven Instanzen durchführen.
- Windows-Dateien unter `/mnt/c` nicht für Rechte- und Storage-Labs verwenden.
- Für Storage nur Image-Dateien und Loop-Devices im Laborordner verwenden.
- Vor `mkfs`, `pvcreate`, `lvremove`, `losetup -d` oder Änderungen an `/etc/fstab` das Ziel erneut mit `lsblk`, `findmnt` und `losetup -l` prüfen.
- Ein Labor erst nach erfolgreicher Verifikation aufräumen.

## Grenzen von WSL

GRUB, initrd, vollständiger Boot, Root-Btrfs-Rollback, udev mit echten Datenträgern, realistische wicked-Netze, Firewallzonen und Chrony sollten zusätzlich in einer SLES-VM geübt werden.
