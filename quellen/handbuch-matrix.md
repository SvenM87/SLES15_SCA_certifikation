# Matrix der lokalen Handbücher

Alle lokalen PDFs dokumentieren SLES 15 SP7. Die Prüfung heißt SLES 15 (2025 Update). Grundkonzepte und etablierte Befehle sind geeignet. SP7-spezifische Neuerungen gelten nur dann als Prüfungsstoff, wenn sie in den offiziellen Zielen stehen.

| Handbuch | Verwendung im Kurs | Besonders relevante Abschnitte |
|---|---|---|
| `book-administration_de.pdf` | Hauptquelle für fast alle Module | Shell/FHS, sudo, Benutzer, Updates, YaST, Zypper/RPM, Snapper, Boot/GRUB/systemd, Netzwerk/wicked, Logging, chrony, Support |
| `book-storage_en.pdf` | Hauptquelle für Storage | Dateisysteme und Mounts, UUID/udev, LVM und Snapshots, Btrfs, Software-RAID |
| `book-security_en.pdf` | Benutzer, Rechte und Remote-Administration | PAM, Benutzer/Passwörter/sudo, cron/at, ACL, OpenSSH, firewalld |
| `book-tuning_en.pdf` | Prozesse, Monitoring und Diagnose | `ps`, `top`, nice/renice, Speicher, `ip`, `ss`, `/proc`, Logs und logrotate |
| `article-virtualization-best-practices_en.pdf` | Optionale Vertiefung | Ressourcen, Storage und Netzwerk virtueller Systeme, nicht Kernstoff |
| `kiwi_en.pdf` | Optionale Laborvertiefung | WSL-Container-Image, nicht Kernstoff der Prüfung |

## Lesereihenfolge

1. Kursmodul lesen und Labor durchführen.
2. Bei Verständnislücken das passende Kapitel im Verwaltungshandbuch lesen.
3. Für Storage, Security oder Performance das jeweilige Fachhandbuch ergänzen.
4. Randthemen nicht auf Kosten der [offiziellen Ziele](../pruefung/zielematrix.md) vertiefen.
