# Labor 05 · Partitionen, LVM, Btrfs und Snapper

> Vor Beginn die VM ausschalten, die beiden zusätzlichen VirtualBox-Disks kontrollieren und Snapshot `02-vor-storage` erstellen. Die Systemdisk darf nie Ziel eines Storage-Befehls sein.

1. Mit `lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL` Systemdisk und beide Labordisks eindeutig identifizieren. Die Gerätenamen als `LAB_DISK1` und `LAB_DISK2` dokumentieren.
2. Auf der kleineren Labordisk eine GPT mit zwei Partitionen anlegen. Ziel unmittelbar vor dem Schreiben erneut prüfen.
3. Ein XFS-Dateisystem erzeugen, Label und UUID setzen, unter `/srv/sca-data` mounten und mit `findmnt`, `df`, `du`, `lsblk` und `blkid` prüfen.
4. Einen fstab-Eintrag mit UUID anlegen. Vorher fstab sichern, danach `findmnt --verify` und `mount -a` ausführen.
5. Freie Partitionen beziehungsweise die zweite Labordisk als PVs initialisieren, eine VG `vg_sca` und ein LV `lv_data` erstellen. Das Dateisystem mounten und mit `lvextend -r` vergrößern.
6. Einen LVM-Snapshot erstellen, eine Testdatei verändern und den Zustand des Snapshots separat einhängen und vergleichen.
7. Auf einem dafür reservierten Bereich Btrfs erstellen, Subvolumes und Read-only/Read-write-Snapshots anlegen.
8. Eine Snapper-Konfiguration für den Btrfs-Labormount erstellen, Pre/Post-Snapshots erzeugen und `snapper status` sowie `snapper diff` auswerten.
9. Root-Btrfs- und Snapper-Konfiguration des SLES-Systems untersuchen. Ein vollständiges System-Rollback nur nach zusätzlichem Snapshot und gemäß Handbuch durchführen.
10. LVM-, Btrfs- und VirtualBox-Snapshots hinsichtlich Ebene, Konsistenz, Speicherort und Wiederherstellung vergleichen.

## Verifikation

Eine Prüfdatei bleibt nach der Erweiterung lesbar. `findmnt --verify` und `mount -a` sind fehlerfrei. `pvs`, `vgs`, `lvs`, `df -h`, `btrfs subvolume list` und `snapper list` zeigen die erwarteten Objekte.

## Aufräumen

Unmounten, fstab-Labzeile entfernen und mit `mount -a` prüfen. Snapshots und Subvolumes entfernen, dann LV, VG und PV in dieser Reihenfolge abbauen. Vor jeder Entfernung Zielidentität erneut prüfen. Alternativ VM ausschalten und Snapshot `02-vor-storage` wiederherstellen.
