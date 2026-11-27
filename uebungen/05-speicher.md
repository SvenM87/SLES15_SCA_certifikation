# Labor 05 · Sicheres Storage-Lab

> Ausschließlich Image-Dateien unter `$LAB_ROOT` verwenden. Vor jedem destruktiven Befehl das Ziel prüfen.

1. Eine 2-GiB-Image-Datei erzeugen und mit `losetup --find --show` an ein Loop-Device binden.
2. GPT und Partition anlegen, ein Dateisystem erzeugen, Label und UUID anzeigen.
3. Unter `$LAB_ROOT/05-storage/mnt` mounten und mit `findmnt`, `df`, `du`, `lsblk`, `blkid` prüfen.
4. Einen fstab-Eintrag mit UUID und `nofail` anlegen. Vorher fstab sichern, danach `mount -a` testen.
5. Mit zwei weiteren Images zwei PVs, eine VG und ein LV erzeugen. Dateisystem mounten und mit `lvextend -r` vergrößern.
6. Falls `/proc/filesystems` Btrfs enthält, ein Btrfs-Image, Subvolumes und Snapshots anlegen sowie Snapper untersuchen. Andernfalls diesen Teil in einer VM durchführen.
7. Erklären, wie ein LVM-Snapshot und ein Btrfs-Snapshot sich unterscheiden.

## Verifikation

Eine Prüfdatei bleibt nach der Erweiterung lesbar. `mount -a` ist fehlerfrei. `lvs` und `df -h` zeigen die vergrößerte Kapazität.

## Aufräumen

Unmounten, fstab-Labzeile entfernen, LV/VG/PV in dieser Reihenfolge entfernen, Loop-Devices lösen und erst dann Images löschen. Zielidentität jeweils erneut prüfen.
