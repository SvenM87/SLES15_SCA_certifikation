# Übungsumgebung mit VirtualBox und SLES 15 SP7

Diese Anleitung richtet die verbindliche Laborumgebung ein. Verwendet werden Oracle VirtualBox und:

```text
SLE-15-SP7-Full-x86_64-GM-Media1.iso
```

Die Full-Media1-ISO enthält die Binärpakete. `Media2` enthält Quellpakete und wird nicht benötigt.

## 1. Voraussetzungen

- Windows 10 oder 11 auf einem Intel-/AMD-64-Bit-System
- aktivierte Hardwarevirtualisierung, Intel VT-x oder AMD-V
- aktuelle VirtualBox-Version
- mindestens 12 GiB RAM, empfohlen 16 GiB oder mehr
- etwa 100 GiB freier Plattenspeicher für VM, Zusatzdisks und Snapshots
- Full-Media1-ISO und zugehörige `.sha256`-Datei
- SUSE-Konto und für Online-Repositories ein gültiger Registrierungs- oder Evaluierungscode

Auf Windows-on-ARM ist die `x86_64`-ISO keine passende native Grundlage.

## 2. ISO prüfen

In PowerShell im Downloadverzeichnis:

```powershell
Get-FileHash .\SLE-15-SP7-Full-x86_64-GM-Media1.iso -Algorithm SHA256
Get-Content .\SLE-15-SP7-Full-x86_64-GM-Media1.iso.sha256
```

Beide Hashwerte müssen identisch sein. Bei einer Abweichung die ISO erneut aus dem offiziellen SUSE-Portal herunterladen.

Unter Linux:

```bash
sha256sum -c SLE-15-SP7-Full-x86_64-GM-Media1.iso.sha256
```

## 3. VirtualBox vorbereiten

VirtualBox mit den Standardkomponenten installieren. Das Extension Pack ist nicht erforderlich. Falls es eingesetzt wird, muss seine Version genau zur VirtualBox-Version passen.

Wenn keine 64-Bit-Gastsysteme angeboten werden:

1. Hardwarevirtualisierung im UEFI/BIOS kontrollieren.
2. VirtualBox aktualisieren.
3. Windows neu starten.
4. Erst danach mögliche Konflikte mit anderen Hypervisoren untersuchen.

Aktuelle VirtualBox-Versionen können auf vielen Windows-Systemen parallel zu Hyper-V und WSL 2 arbeiten, teilweise langsamer. Windows-Sicherheitsfunktionen nicht vorschnell deaktivieren.

## 4. Host-only-Netz anlegen

Über **Tools → Network** beziehungsweise **Network Manager** ein Host-only-Netz erstellen:

| Einstellung | Wert |
|---|---|
| IPv4-Adresse des Hosts | `192.168.56.1` |
| Netzmaske | `255.255.255.0` |
| DHCP-Server | deaktiviert |

Die VM erhält später `192.168.56.10/24`. Falls `192.168.56.0/24` bereits verwendet wird, ein anderes privates `/24`-Netz wählen und alle folgenden Adressen anpassen.

## 5. Virtuelle Maschine anlegen

In VirtualBox **New** auswählen:

| Komponente | Einstellung |
|---|---|
| Name | `SLES15-SP7-SCA-Lab` |
| ISO Image | `SLE-15-SP7-Full-x86_64-GM-Media1.iso` |
| Unattended Installation | überspringen oder deaktivieren |
| Typ | Linux |
| Version | SUSE Linux Enterprise 64-bit, alternativ Other Linux 64-bit |
| Arbeitsspeicher | 6 GiB |
| Prozessoren | 2, bei leistungsfähigem Host 4 |
| Systemdisk | 60 GiB VDI, dynamisch alloziert |
| Firmware | EFI aktivieren |

Unter **Settings** kontrollieren:

- **System → Motherboard:** Optical vor Hard Disk
- **System → Processor:** höchstens die Hälfte der Host-Threads
- **Display:** 64 oder 128 MiB Videospeicher
- **Storage:** ISO am optischen Laufwerk, Systemdisk am SATA-Controller
- **Network → Adapter 1:** NAT und Cable Connected
- **Network → Adapter 2:** Host-only Adapter und Cable Connected
- **Shared Folders:** keine
- **Shared Clipboard/Drag and Drop:** zunächst deaktiviert

Die Labordisks werden erst nach der Installation angelegt. Dadurch sinkt das Risiko, SLES auf der falschen Disk zu installieren.

## 6. SLES installieren

VM starten und im Bootmenü **Installation** wählen.

### Sprache und Produkt

1. Sprache, Tastaturlayout und Zeitzone einstellen.
2. Lizenzbedingungen lesen und akzeptieren.
3. **SUSE Linux Enterprise Server 15 SP7** auswählen.

### Registrierung

Bei vorhandener Evaluation oder Subskription:

1. **Register System via scc.suse.com** auswählen.
2. E-Mail-Adresse des SUSE-Kontos und Registrierungscode eingeben.
3. Registrierungsergebnis kontrollieren.

Der Registrierungscode ist kein Kontopasswort. Er darf nicht in Git, Screenshots oder Kursnotizen gelangen.

Die Full-Media-Installation kann ohne Systemregistrierung abgeschlossen werden. Dann fehlen später offizielle Online-Repositories, Updates und eventuell zusätzliche Modulpakete.

### Module und Extensions

Mindestens aktivieren:

- Basesystem Module
- Server Applications Module

Weitere Module nur bei Bedarf. High Availability, SAP und Real Time gehören nicht zum Kernkurs.

### Netzwerk

- NAT-Adapter per DHCP konfigurieren.
- Host-only-Adapter zunächst unkonfiguriert lassen.
- Hostnamen `sles-sca-lab` verwenden.
- Internet- und Registrierungsverbindung testen.

### Partitionierung

Für die 60-GiB-Systemdisk die vorgeschlagene Partitionierung mit Btrfs für `/` verwenden. Vor dem Bestätigen kontrollieren:

- nur die 60-GiB-Systemdisk wird verändert
- `/` verwendet Btrfs
- Snapshots sind aktiviert
- Swap ist vorhanden
- Boot- und EFI-Systempartition liegen auf der Systemdisk

Manuelle Partitionierung wird später auf separaten Labordisks geübt.

### Benutzer und Software

1. Starkes Root-Passwort setzen.
2. Benutzer `student` mit eigenem Passwort anlegen.
3. Automatische Anmeldung deaktiviert lassen.
4. Den vorgeschlagenen Standardsystemumfang verwenden.
5. Vor **Install** Produkt, Bootloader, Partitionierung, Netzwerk und Module kontrollieren.

## 7. Erster Start

Nach dem Neustart die ISO unter **Settings → Storage** auswerfen. Als `student` anmelden:

```bash
cat /etc/os-release
uname -r
hostnamectl
systemctl --failed
lsblk -f
ip -br address
sudo snapper list
```

`systemctl --failed` sollte keine ungeklärten Fehler melden. Systemdisk, Btrfs-Subvolumes und Snapper-Konfiguration müssen erkennbar sein.

## 8. Registrierung, Repositories und Updates

```bash
sudo SUSEConnect --status-text
sudo zypper repos --uri
sudo zypper services
sudo zypper refresh
```

Bei registriertem System müssen die aktivierten Produktmodule und Update-Repositories erscheinen. TLS- und Signaturprüfungen nicht deaktivieren.

```bash
sudo zypper patch
sudo reboot
```

Danach `uname -r`, `systemctl --failed` und `zypper list-patches` erneut prüfen.

## 9. Laborwerkzeuge installieren

```bash
zypper search --installed-only vim acl lvm2 btrfsprogs snapper wicked openssh rsyslog logrotate supportutils curl
sudo zypper install vim acl lvm2 btrfsprogs snapper wicked openssh rsyslog logrotate supportutils curl
sudo yast
rpm -q yast2-network yast2-users yast2-storage-ng
```

Falls ein Paket nicht existiert, kein unsicheres Fremdrepository hinzufügen. Zuerst Registrierung, Module und Paketnamen prüfen.

## 10. Host-only-Interface konfigurieren

```bash
ip -br link
ip -br address
sudo yast2 lan
```

Den Host-only-Adapter anhand seiner VirtualBox-MAC-Adresse identifizieren und setzen:

- statische Adresse `192.168.56.10/24`
- kein Standardgateway
- kein zusätzlicher DNS-Server
- Aktivierung beim Boot

Der NAT-Adapter behält DHCP und die Default Route.

```bash
ip -br address
ip route
ping -c 3 192.168.56.1
```

Vom Windows-Host:

```powershell
ping 192.168.56.10
```

## 11. SSH-Zugriff einrichten

```bash
sudo systemctl enable --now sshd
sudo sshd -t
ss -lntp | grep ':22'
```

Vom Windows-Host:

```powershell
ssh student@192.168.56.10
```

Für den ersten Test reicht Passwortanmeldung im isolierten Host-only-Netz. Schlüsselauthentifizierung und Härtung folgen im Administrationslabor.

## 12. Labordisks hinzufügen

VM herunterfahren:

```bash
sudo systemctl poweroff
```

Unter **Settings → Storage** am SATA-Controller zwei dynamisch allozierte VDI-Disks erstellen:

| Disk | Größe | Zweck |
|---|---:|---|
| Labordisk 1 | 10 GiB | GPT, Partitionen, Dateisysteme |
| Labordisk 2 | 20 GiB | LVM, Btrfs und Snapshots |

VM starten:

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL,SERIAL
sudo fdisk -l
```

Die Labordisks dürfen keine Partitionen oder Dateisysteme enthalten. Namen wie `/dev/sdb` und `/dev/sdc` sind wahrscheinlich, dürfen aber nie ungeprüft angenommen werden.

## 13. Baseline-Snapshot erstellen

```bash
sudo zypper verify
systemctl --failed
sudo systemctl poweroff
```

In VirtualBox einen ausgeschalteten Snapshot `00-baseline` anlegen:

```text
SLES 15 SP7 installiert und aktualisiert, NAT und Host-only aktiv,
SSH erreichbar, zwei leere Labordisks vorhanden.
```

Weitere Wiederherstellungspunkte:

- `01-vor-systemd`
- `02-vor-storage`
- `03-vor-netzwerk`
- `04-vor-remote-access`

VirtualBox-Snapshots sind von LVM- und Btrfs-Snapshots innerhalb von SLES zu unterscheiden.

## 14. Abnahmetest

- [ ] SLES 15 SP7 startet von der Systemdisk
- [ ] Root-Dateisystem verwendet Btrfs
- [ ] Snapper zeigt Konfiguration und Snapshots
- [ ] NAT-Adapter erreicht die benötigten SUSE-Repositories
- [ ] Host-only-Adresse ist vom Windows-Host erreichbar
- [ ] SSH-Verbindung als `student` funktioniert
- [ ] YaST startet
- [ ] `zypper refresh` und `zypper verify` laufen erfolgreich
- [ ] zwei leere Labordisks sind eindeutig identifiziert
- [ ] `systemctl --failed` enthält keine ungeklärten Fehler
- [ ] ausgeschalteter Snapshot `00-baseline` existiert

Fehlschläge müssen vor dem ersten Fachlabor geklärt werden.

## 15. Sicherheits- und Rücksetzregeln

- Vor `fdisk`, `parted`, `mkfs`, `pvcreate` oder `lvremove` Gerätename, Größe, Modell und Mountstatus erneut prüfen.
- Die Systemdisk darf nie Ziel eines Storage-Labors sein.
- Registrierungsdaten und Passwörter nicht in Shell-Historie, Screenshots oder Git speichern.
- Host-only-Netz für eingehende Labordienste verwenden.
- Zuerst den dokumentierten Aufräumweg verwenden. Bei unüberschaubarem Zustand VM ausschalten und passenden Snapshot wiederherstellen.

## Optionale WSL-Nutzung

WSL eignet sich ergänzend für kurze Wiederholungen. Verbindliche Laborergebnisse werden in der VirtualBox-VM erzeugt.
