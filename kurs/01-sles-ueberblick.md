# 01 · Überblick über SLES 15

## Lernziele

SLE-Produktlinie, Lebenszyklus, Service Packs, Module, Extensions, Registrierung, YaST und Systeminformationen beherrschen.

SLES ist eine kommerziell unterstützte Linux-Distribution. Service Packs liefern gepflegte Versionsstände. Module stellen Paketkanäle wie Basesystem oder Server Applications bereit. Extensions ergänzen Funktionen und können ein separates Abonnement benötigen. Registrierung verbindet ein System mit SCC oder einem lokalen RMT-Server.

```bash
cat /etc/os-release
uname -r
uname -m
hostnamectl
lscpu
lsmem
SUSEConnect --status-text
```

`/etc/os-release` beschreibt das installierte Betriebssystem. `uname` beschreibt den laufenden Kernel. Nach einem Kernelupdate kann bis zum Neustart noch die alte Kernelversion laufen.

YaST ist als ncurses- und grafische Oberfläche verfügbar. Administratoren müssen die zugrunde liegenden Dienste und Dateien dennoch kennen.

```bash
sudo yast
sudo yast2 users
man zypper
apropos partition
rpm -qd paketname
```

## Kontrollfragen

1. Welche Datei identifiziert den installierten SLES-Stand?
2. Was unterscheidet Module, Extensions und RPM-Pakete?
3. Weshalb kann `uname -r` nach einem Update den alten Kernel zeigen?
4. Wie findet man Manpage und Paketdokumentation zu einem Werkzeug?
