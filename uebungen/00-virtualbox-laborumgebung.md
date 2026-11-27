# VirtualBox-Laborumgebung

Die verbindliche Laborumgebung ist eine eigenständige SLES-15-SP7-VM unter Oracle VirtualBox. Sie bildet Boot, systemd, Blockgeräte, LVM, Btrfs, Snapper, wicked und Remote-Zugriff realistisch ab.

## Empfohlenes Installationsmedium

Für einen Intel-/AMD-PC mit zuverlässigem Internetzugang verwenden wir:

```text
SLE-15-SP7-Online-x86_64-GM-Media1.iso
```

Das Online-Medium ist rund 612 MB groß und laut SUSE für reguläre Installationen mit Internetzugang vorgesehen. Es gilt für die SLE-Produktfamilie. Während der Installation wird SLES als Produkt ausgewählt. Benötigte Pakete werden aus den registrierten Online-Repositories geladen.

Für eine Installation ohne Internetzugang ist stattdessen dieses Medium geeignet:

```text
SLE-15-SP7-Full-x86_64-GM-Media1.iso
```

Das Full-Medium ist rund 17,2 GB groß. `Media2` enthält Quellpakete und wird für das Labor nicht benötigt. GM ist der ursprüngliche freigegebene SP7-Stand. Ein angebotenes QU-Medium enthält bereits spätere Wartungsupdates und ist ebenfalls geeignet. Für reproduzierbare Kursanweisungen verwenden wir GM und installieren anschließend alle verfügbaren Updates.

Auf einem ARM-Windows-Rechner ist `x86_64` keine passende native Architektur. Dort muss die Virtualisierungsplattform und ein `aarch64`-Medium gesondert gewählt werden.

## Download verifizieren

Zusammen mit der ISO die zugehörige `.sha256`-Datei und nach Möglichkeit deren `.asc`-Signatur herunterladen. Unter Linux beziehungsweise WSL lässt sich die Prüfsumme im Downloadverzeichnis kontrollieren:

```bash
sha256sum -c SLE-15-SP7-Online-x86_64-GM-Media1.iso.sha256
```

Die Prüfung muss erfolgreich sein, bevor die ISO in VirtualBox eingebunden wird. Eine abweichende Prüfsumme bedeutet, dass das Image erneut aus dem offiziellen SUSE-Downloadportal geladen werden muss.

Das Image `SLES15-SP7-Minimal-VM.x86_64-VMware-GM.vmdk.xz` ist eine vorkonfigurierte Minimal-VM für VMware. VirtualBox kann die entpackte VMDK grundsätzlich als Festplatte verwenden. Für diesen Kurs ist sie nur eine Ausweichoption, weil sie für VMware vorkonfiguriert ist, die Installation überspringt und möglicherweise benötigte YaST- und Administrationspakete nicht enthält.

Wer das Minimal-Image dennoch verwendet, entpackt es zunächst mit `xz -dk DATEI.vmdk.xz`, erstellt in VirtualBox eine neue Linux-VM und bindet die vorhandene VMDK ein. Bootmodus, Storage-Controller, Netzwerk und installierte Pakete müssen anschließend geprüft werden.

## VM anlegen

Empfohlene Ausgangskonfiguration:

| Komponente | Einstellung |
|---|---|
| Typ | Linux / SUSE Linux Enterprise 64-bit |
| CPU | 2 virtuelle CPUs |
| RAM | 4 GiB, bei ausreichendem Host 6 GiB |
| Systemdisk | 40 GiB, dynamisch alloziert |
| Zusatzdisk 1 | 10 GiB für Partitionen und LVM |
| Zusatzdisk 2 | 20 GiB für LVM, Btrfs und Snapshots |
| Adapter 1 | NAT für Updates und Repositories |
| Adapter 2 | Host-only-Netz für statische Konfiguration und Remote-Zugriff |
| Firmware | UEFI aktivieren, wenn der Host es zuverlässig unterstützt |

Die zusätzlichen Disks bleiben bei der Installation unberührt. Sie sind ausschließlich für die Storage-Labore vorgesehen.

## SLES installieren

1. ISO als optisches Medium einlegen und die VM starten.
2. Standardsystem mit Btrfs als Root-Dateisystem installieren.
3. Einen administrativen Laborbenutzer anlegen.
4. SSH-Server, YaST und grundlegende Administrationswerkzeuge installieren.
5. System nur mit eigener Evaluation oder gültiger SUSE-Registrierung registrieren. Ohne Registrierung können viele Übungen ausgeführt werden, Software- und Patch-Labore benötigen jedoch erreichbare Repositories.
6. Nach dem ersten erfolgreichen Boot ISO auswerfen und Systemzustand prüfen.

```bash
cat /etc/os-release
uname -r
systemctl --failed
lsblk -f
ip -br address
sudo snapper list
```

## Netzwerkmodell

Adapter 1 bleibt für ausgehenden Internetzugriff auf NAT. Adapter 2 erhält im Host-only-Netz später eine statische Adresse. Dadurch lassen sich wicked, mehrere Interfaces, SSH, VNC/RDP, Ports und Diagnose üben, ohne das physische LAN zu verändern.

## Snapshots

Nach der Installation einen ausgeschalteten Snapshot `00-baseline` erstellen. Weitere empfohlene Snapshots:

- `01-vor-systemd`
- `02-vor-storage`
- `03-vor-netzwerk`
- `04-vor-remote-access`

VirtualBox-Snapshots sichern den Zustand der gesamten VM. Sie sind von Btrfs- und LVM-Snapshots zu unterscheiden, die innerhalb von SLES verwaltet werden.

## Sicherheitsregeln

- Ausschließlich die beiden Labordisks partitionieren. Vor jedem destruktiven Befehl Ziel, Größe und Modell mit `lsblk` prüfen.
- Keine produktiven Zugangsdaten, SSH-Schlüssel oder SUSE-Registrierungscodes in Screenshots oder Kursnotizen speichern.
- Vor Bootloader-, Storage- und Netzwerkeingriffen einen ausgeschalteten VirtualBox-Snapshot erstellen.
- Host-only-Netz für eingehende Labordienste verwenden. Dienste nicht unnötig an das physische LAN weiterleiten.
- Rücksetzen eines Labors erfolgt über den dokumentierten Aufräumweg oder einen passenden VM-Snapshot.

## Optionale WSL-Nutzung

WSL kann zusätzlich für kurze Wiederholungen zu Bash, Vim, Textfiltern, Benutzern, Rechten und Prozessen dienen. Die verbindlichen Laborergebnisse werden in der VirtualBox-VM erzeugt.
