# Labor 04 · Prozesse und Zeitplanung

1. Einen lang laufenden Testprozess im Vordergrund starten, anhalten und mit `bg` fortsetzen.
2. PID, PPID, Zustand, Benutzer, Nice-Wert und Kommando mit `ps`, `pgrep`, `pstree` und `/proc` bestimmen.
3. Priorität mit `nice` und `renice` ändern.
4. Ein Skript schreiben, das SIGTERM protokolliert und sauber beendet. SIGHUP, SIGTERM und zuletzt an einem neuen Prozess SIGKILL testen.
5. Einen wiederkehrenden Cronjob, einen einmaligen `at`-Job und einen systemd-Timer planen.
6. PATH- und Logging-Unterschiede geplanter Jobs dokumentieren.

## Verifikation

Alle drei Scheduler schreiben unterschiedlich markierte Einträge in `$LAB_ROOT/04-prozesse/schedule.log`. Nach SIGTERM existiert die gespeicherte PID nicht mehr.

## Aufräumen

Nur Prozesse mit im Labor gespeicherter PID beenden. Crontab-Eintrag, at-Job und Timer entfernen.
