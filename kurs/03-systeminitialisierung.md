# 03 · Systeminitialisierung

```text
UEFI/BIOS → GRUB 2 → Kernel + initrd → systemd (PID 1) → target
```

`/etc/default/grub` enthält allgemeine GRUB-Einstellungen. Generierte Konfigurationen werden nicht direkt gepflegt. Kernelparameter des laufenden Systems stehen in `/proc/cmdline`.

```bash
cat /proc/cmdline
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
sudo mkinitrd
systemctl get-default
systemctl list-dependencies multi-user.target
```

systemd-Units umfassen service, socket, target, timer, mount und path.

```bash
systemctl status sshd
sudo systemctl start sshd
sudo systemctl enable sshd
sudo systemctl enable --now sshd
systemctl is-enabled sshd
sudo systemctl daemon-reload
journalctl -b
journalctl -b -1
journalctl -u sshd --since today
systemctl --failed
```

`start` ändert den Laufzeitzustand, `enable` die Startverknüpfungen. `rescue.target` bietet eine minimale Umgebung, `emergency.target` noch weniger. WSL bildet Firmware und GRUB nicht real ab.

## Kontrollfragen

1. Welche Komponente startet PID 1?
2. Was unterscheidet `enable` und `start`?
3. Wie zeigt man Logs des vorherigen Boots?
4. Wann ist `daemon-reload` nötig?
