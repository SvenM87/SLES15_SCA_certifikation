# 09 · Erweiterte Administration

## Bash-Automatisierung

```bash
#!/usr/bin/env bash
set -u

if [[ $# -ne 1 ]]; then
  echo "Verwendung: $0 DATEI" >&2
  exit 2
fi

if [[ -r $1 ]]; then
  wc -l -- "$1"
else
  echo "Nicht lesbar: $1" >&2
  exit 1
fi
```

`$?` ist der letzte Exitcode, `$#` die Argumentanzahl und `$@` alle Argumente. Kontrollstrukturen sind `if`, `case`, `for`, `while` und Funktionen. Eingaben quoten und Fehler aussagekräftig melden.

## Kernel und Laufzeitparameter

```bash
lsmod
modinfo modulname
sudo modprobe modulname
sysctl net.ipv4.ip_forward
sudo sysctl -w net.ipv4.ip_forward=1
sudo sysctl --system
```

Werte unter `/proc/sys` sind Laufzeitwerte. Persistente Einstellungen liegen in `/etc/sysctl.conf` oder Dateien unter `/etc/sysctl.d/`.

## Wartung und Sicherung

```bash
rsync -aHAX --numeric-ids /quelle/ /ziel/
tar --acls --xattrs -cpf backup.tar /etc
systemctl --failed
journalctl -b -p warning
```

Eine Sicherung gilt erst nach Restore-Test als verlässlich. Bei Fehlersuche zuerst Symptome und Zeitpunkt erfassen, dann Schicht für Schicht Zustand prüfen, nur eine Hypothese ändern und den Erfolg messen.

## Kontrollfragen

1. Wofür stehen `$#`, `$@` und `$?`?
2. Wie wird ein sysctl-Wert persistent?
3. Welche Metadaten sollte ein Systembackup erhalten?
4. Warum sollte man pro Diagnoseversuch nur eine Hypothese ändern?
