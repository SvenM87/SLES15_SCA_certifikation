# 02 · Linux-Grundlagen

Wichtige Verzeichnisse sind `/etc`, `/var`, `/usr`, `/home`, `/root`, `/tmp`, `/run` und `/dev`. Absolute Pfade beginnen mit `/`, relative Pfade am aktuellen Verzeichnis.

```bash
pwd
ls -la
stat datei
file datei
mkdir -p projekt/{daten,logs}
cp -a quelle ziel
ln datei hardlink
ln -s /absolutes/ziel symlink
find /var/log -type f -mtime -1 -size +1M
grep -Rni --include='*.conf' 'muster' /etc
```

Hardlinks verweisen auf denselben Inode und überschreiten keine Dateisystemgrenze. Symlinks speichern einen Zielpfad und können verwaisen.

stdin, stdout und stderr haben die Deskriptoren 0, 1 und 2.

```bash
befehl >ausgabe.txt
befehl 2>fehler.txt
befehl >>ausgabe.txt 2>&1
grep root /etc/passwd | cut -d: -f1,7
tar -czf backup.tar.gz verzeichnis/
tar -tf backup.tar.gz
```

Einfache Anführungszeichen verhindern Shell-Expansion, doppelte erlauben Variablenersetzung.

## Vim-Prüfungswissen

Vim startet im Normalmodus. `i`, `a` und `o` wechseln in den Einfügemodus, `Esc` zurück. `/muster` sucht vorwärts, `n` wiederholt die Suche, `dd` löscht eine Zeile, `yy` kopiert sie und `p` fügt ein. `u` macht rückgängig. `:%s/alt/neu/g` ersetzt in der ganzen Datei. `:w`, `:q`, `:wq` und `:q!` speichern oder beenden. Mit `vimdiff alt neu` lassen sich Konfigurationen vergleichen.

## Kontrollfragen

1. Was unterscheidet Hardlink und Symlink?
2. Wie wird nur stderr umgeleitet?
3. Welche Suche findet Dateien der letzten 24 Stunden?
4. Welche Wirkung haben einfache Anführungszeichen?
5. Wie ersetzt man in Vim alle Vorkommen eines Ausdrucks?
