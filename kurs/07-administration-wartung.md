# 07 · Administration und Wartung

## Benutzer, Gruppen und Rechte

```bash
sudo useradd -m -s /bin/bash alice
sudo passwd alice
sudo usermod -aG wheel alice
id alice
getent passwd alice
sudo chage -l alice
```

`/etc/passwd` enthält Kontodaten, `/etc/shadow` Passwort-Hashes und Alterung, `/etc/group` Gruppen. Ohne `-a` kann `usermod -G` vorhandene Zusatzgruppen ersetzen.

```bash
chmod 640 datei
chown alice:projekt datei
chmod g+s gemeinsames-verzeichnis
chmod +t austausch
getfacl datei
setfacl -m u:bob:rw datei
setfacl -m d:g:projekt:rwx verzeichnis
```

setgid auf Verzeichnissen vererbt die Gruppe. Sticky Bit schützt fremde Dateien in gemeinsam beschreibbaren Verzeichnissen. Sudo-Regeln mit `visudo` prüfen und nach Least Privilege gestalten.

Reale und effektive IDs unterscheiden Kontoinhaber und aktuell für Zugriffsprüfungen verwendete Identität. setuid/setgid-Programme, `sudo`, `su` und `newgrp` können effektive Identitäten verändern. `id`, `id -u`, `id -ru`, `id -g` und `id -rg` machen dies sichtbar. setuid auf Skripten ist auf Linux gewöhnlich unwirksam und keine sichere Delegationsmethode.

## Remote-Administration

```bash
ssh-keygen -t ed25519
ssh-copy-id user@host
sudo sshd -t
sudo systemctl reload sshd
```

SSH, VNC und RDP unterscheiden sich in Protokoll und Einsatzzweck. SSH ist der Standard für Shell und Tunneling. VNC teilt eine grafische Sitzung. RDP bietet eine entfernte Desktop-Sitzung, auf SLES etwa über xrdp. Serverkonfiguration und Firewallfreigabe müssen zusammenpassen.

Bei VNC wird zwischen einmaligen und persistenten Display-Sitzungen unterschieden. In jedem Verfahren sind Dienststatus, lauschender Port, Benutzerberechtigung, Verschlüsselung und Firewall separat zu prüfen.

## Logging und Support

```bash
journalctl -p warning..alert
journalctl --disk-usage
dmesg --level=err,warn
logger -t sca-test 'Testmeldung'
logrotate -d /etc/logrotate.conf
sudo supportconfig
```

`rsyslog` verarbeitet klassische Syslog-Nachrichten, `systemd-journald` strukturiert das Journal. Logrotate rotiert Dateilogs. `supportconfig` sammelt Diagnoseinformationen. Archive können sensible Daten enthalten und müssen geschützt übermittelt werden.

Rsyslog-Regeln wählen Nachrichten über Facility und Severity aus und schreiben sie etwa nach `/var/log/messages`. Änderungen vor einem Reload mit `rsyslogd -N1` prüfen. Kernelmeldungen sind über `dmesg`, `journalctl -k` und abhängig von der Konfiguration in Dateilogs sichtbar.

## Kontrollfragen

1. Was bewirkt `usermod -aG`?
2. Welche Wirkung hat setgid auf einem Verzeichnis?
3. Wie prüft man `sshd_config` vor einem Reload?
4. Wozu dient `supportconfig`?
