# 04 · Prozessverwaltung und Scheduling

```bash
ps aux
ps -ef --forest
pgrep -a sshd
pstree -p
top
```

Prozesse besitzen PID, Elternprozess, Benutzerkennung, Zustand, Priorität und Ressourcenverbrauch. Ein Zombie ist beendet, wurde vom Elternprozess aber noch nicht eingesammelt.

```bash
command &
jobs
fg %1
bg %1
nohup command >command.log 2>&1 &
kill -TERM PID
kill -KILL PID
pkill -HUP prozessname
nice -n 10 command
renice 5 -p PID
```

SIGTERM erlaubt kontrolliertes Beenden. SIGKILL kann nicht abgefangen werden. Nice-Werte reichen typischerweise von -20 bis 19. Ein niedriger Wert bedeutet höhere CPU-Priorität.

```bash
crontab -e
crontab -l
at 22:00
systemctl list-timers --all
systemd-analyze calendar 'Mon..Fri 18:00'
```

Cron verwendet fünf Zeitfelder. Systemweite Dateien ergänzen ein Benutzerfeld. `at` plant einmalige Jobs. systemd-Timer können verpasste Läufe nachholen und besitzen Unit-Abhängigkeiten.

## Kontrollfragen

1. Warum SIGTERM vor SIGKILL verwenden?
2. Welcher Nice-Wert erhält mehr CPU-Priorität?
3. Was unterscheidet Cron, `at` und systemd-Timer?
4. Wie findet man Prozesse eines Namens samt Argumenten?
