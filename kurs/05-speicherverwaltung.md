# 05 · Speicherverwaltung

## Geräte, Partitionen und Dateisysteme

```bash
lsblk -f
blkid
findmnt
df -hT
du -xhd1 /var
sudo fdisk -l
sudo parted -l
```

GPT ist der moderne Partitionstabellentyp. UUIDs sind in `/etc/fstab` stabiler als Gerätenamen. Ein fstab-Eintrag enthält Gerät, Mountpoint, Typ, Optionen, dump und fsck-Reihenfolge. Vor einem Neustart immer `mount -a` prüfen.

```bash
sudo mkfs.xfs /dev/vdb1
sudo mkfs.btrfs /dev/vdb2
sudo mount /dev/vdb1 /srv/data
sudo umount /srv/data
sudo yast2 disk
```

## LVM

Physical Volumes bilden Volume Groups, aus denen Logical Volumes entstehen.

```bash
sudo pvcreate /dev/vdb1
sudo vgcreate vgdata /dev/vdb1
sudo lvcreate -n lvapp -L 5G vgdata
sudo lvextend -r -L +2G /dev/vgdata/lvapp
pvs; vgs; lvs
```

XFS lässt sich nicht verkleinern. LVM-Snapshots benötigen freien Platz in der VG und sind kein langfristiges Backup.

LVM liest Einstellungen aus `/etc/lvm/lvm.conf`. `vgcfgbackup` sichert VG-Metadaten. YaST Partitioner kann Partitionen, Dateisysteme, LVM und Mounts verwalten. Vor dem Anwenden immer die Zusammenfassung und Zielgeräte kontrollieren.

## Btrfs und Snapper

SLES nutzt Btrfs häufig für `/`. Subvolumes teilen Speicher und können getrennt behandelt werden.

```bash
sudo btrfs filesystem show
sudo btrfs subvolume list /
sudo snapper list
sudo snapper create --description 'vor Änderung'
sudo snapper status NUMMER
sudo snapper diff NUMMER..0
```

Snapshots liegen meist auf demselben Speicher und ersetzen kein externes Backup. Der Boot aus einem Read-only-Snapshot und ein anschließendes Rollback sollten in einer VM geübt werden.

YaST zeigt Btrfs-Volumes und Subvolumes im Partitioner. Snapper-Konfigurationen liegen unter `/etc/snapper/configs/`. Pre/Post-Paare dokumentieren Änderungen, Timeline-Snapshots entstehen zeitgesteuert. Cleanup-Regeln begrenzen Anzahl und Alter.

## Swap, Kapazität und Quotas

```bash
swapon --show
sudo mkswap /dev/vdb3
sudo swapon /dev/vdb3
quota -s
repquota -a
```

## Kontrollfragen

1. Warum UUIDs in `/etc/fstab` verwenden?
2. Welche Ebenen bildet LVM?
3. Was bewirkt `lvextend -r`?
4. Weshalb ist ein Snapshot kein Backup?
