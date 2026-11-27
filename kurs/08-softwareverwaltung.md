# 08 · Softwareverwaltung

RPM verwaltet installierte Pakete und lokale RPM-Dateien. Zypper löst Abhängigkeiten über konfigurierte Repositories auf.

```bash
rpm -qa
rpm -qi bash
rpm -ql bash
rpm -qf /bin/bash
rpm -V bash
rpm -qp --requires paket.rpm
rpm --checksig paket.rpm
```

`rpm -V` vergleicht Metadaten und Prüfsummen mit der RPM-Datenbank. Eine Abweichung benötigt Kontext und beweist allein keinen Angriff.

```bash
sudo zypper refresh
zypper repos -u
zypper services
zypper search --details nginx
zypper info paket
sudo zypper install paket
sudo zypper remove paket
sudo zypper update
sudo zypper patch
zypper list-patches
zypper verify
```

`update` aktualisiert Pakete, `patch` wendet Wartungspatches an. `dist-upgrade` führt einen Distributionsabgleich durch und gehört nicht in den gewöhnlichen Updateablauf.

```bash
sudo zypper addrepo -f URL alias
sudo zypper modifyrepo -p 90 alias
zypper packages --orphaned
zypper patterns
zypper products
sudo zypper addlock paket
```

Kleinere Prioritätszahlen gewinnen. GPG- und TLS-Prüfungen bleiben aktiviert. YaST Software Management und Repository Management bilden dieselben Paket- und Repo-Konzepte grafisch oder in ncurses ab.

## Kontrollfragen

1. Wie findet man das Paket einer installierten Datei?
2. Was unterscheidet `patch`, `update` und `dist-upgrade`?
3. Welche Repository-Priorität gewinnt, 90 oder 99?
4. Was zeigt `rpm -V`?
