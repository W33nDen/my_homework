# Домашнє завдання №4. Пакети, сервіси та журнали

**Студент:** Уживенко Денис  
**ОС:** Zorin OS (Ubuntu Noble 24.04)  
**Дата виконання:** 19 травня 2026

---

## Завдання 1. Менеджери пакетів

### 1.1 Оновлення списку пакетів

```bash
sudo apt update
```

**Результат:**
```
Hit:1 https://security.ubuntu.com/ubuntu noble-security InRelease
Hit:2 http://nl.archive.ubuntu.com/ubuntu noble InRelease
Hit:3 http://nl.archive.ubuntu.com/ubuntu noble-updates InRelease
Hit:4 http://nl.archive.ubuntu.com/ubuntu noble-backports InRelease
...
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
```

### 1.2 Встановлення пакету `tree`

```bash
sudo apt install tree -y
```

**Результат:**
```
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
tree is already the newest version (2.1.1-2ubuntu3.24.04.2).
tree set to manually installed.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

> **Примітка:** Пакет `tree` вже був встановлений у системі.

### 1.3 Перевірка версії

```bash
tree --version
```

**Результат:**
```
tree v2.1.1 © 1996 - 2023 by Steve Baker, Thomas Moore, Francesc Rocher, Florian Sesser, Kyosuke Tokoro
```

### 1.4 Видалення пакету

```bash
sudo apt remove tree -y
```

**Результат:**
```
The following packages will be REMOVED:
  tree
0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
After this operation, 111 kB disk space will be freed.
Removing tree (2.1.1-2ubuntu3.24.04.2) ...
Processing triggers for man-db (2.12.0-4build2)...
```

---

## Завдання 2. Керування сервісами через systemctl

### 2.1 Перевірка статусу сервісу `cron`

```bash
sudo systemctl status cron
```

**Результат:**
```
● cron.service - Regular background program processing daemon
     Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled; preset: enabled)
     Active: active (running) since Tue 2026-05-19 12:24:23 CEST; 54min ago
       Docs: man:cron(8)
   Main PID: 674 (cron)
      Tasks: 1 (limit: 4454)
     Memory: 2.0M (peak: 6.8M)
        CPU: 387ms
     CGroup: /system.slice/cron.service
             └─674 /usr/sbin/cron -f -P
```

### 2.2 Зупинка сервісу

```bash
sudo systemctl stop cron
```

### 2.3 Запуск сервісу

```bash
sudo systemctl start cron
```

### 2.4 Додавання до автозавантаження

```bash
sudo systemctl enable cron
```

**Результат:**
```
Synchronizing state of cron.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing /usr/lib/systemd/systemd-sysv-install enable cron
```

### 2.5 Перевірка статусу після перезапуску

```bash
sudo systemctl status cron
```

**Результат:**
```
● cron.service - Regular background program processing daemon
     Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled; preset: enabled)
     Active: active (running) since Tue 2026-05-19 13:22:04 CEST; 40s ago
       Docs: man:cron(8)
   Main PID: 14502 (cron)
      Tasks: 1 (limit: 4454)
     Memory: 372.0K (peak: 500.0K)
        CPU: 7ms
```

> **Висновок:** Сервіс `cron` успішно зупинено, перезапущено та додано до автозавантаження (`enabled`).

---

## Завдання 3. Робота з логами

### 3.1 Останні 10 рядків `/var/log/syslog`

```bash
sudo tail -n 10 /var/log/syslog
```

**Результат:**
```
2026-05-19T13:23:38.277379+02:00 w3nden-Aspire-V5-572G brave-browser.desktop[3311]: [341934:25:0519/132338.276109:ERROR:restricted_cookie_manager.cc(1160)] ...
2026-05-19T13:24:08.075192+02:00 w3nden-Aspire-V5-572G AptDaemon: INFO: Quitting due to inactivity
2026-05-19T13:24:08.075926+02:00 w3nden-Aspire-V5-572G AptDaemon: INFO: Quitting was requested
2026-05-19T13:25:01.459201+02:00 w3nden-Aspire-V5-572G CRON[14936]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
2026-05-19T13:25:23.401585+02:00 w3nden-Aspire-V5-572G gnome-keyring-d[1565]: asked to register item /org/freedesktop/secrets/collection/login/5, but it's already registered
2026-05-19T13:25:23.656037+02:00 w3nden-Aspire-V5-572G gnome-keyring-d[1565]: asked to register item /org/freedesktop/secrets/collection/login/4, but it's already registered
2026-05-19T13:26:28.098252+02:00 w3nden-Aspire-V5-572G brave-browser.desktop[3311]: [ERROR:restricted_cookie_manager.cc(1151)] site_for_cookies ...
2026-05-19T13:26:28.098463+02:00 w3nden-Aspire-V5-572G brave-browser.desktop[3311]: [ERROR:restricted_cookie_manager.cc(1160)] top_frame_origin from renderer...
```

### 3.2 Помилки через `journalctl` (рівень "err")

```bash
sudo journalctl -p err --no-pager | tail -n 20
```

**Результат:**
```
mei 19 12:24:15 w3nden-Aspire-V5-572G kernel: ACPI Error: Aborting method \_SB.PCI0.PEG0.PEGP.DD02._BCL due to previous error (AE_NOT_FOUND)
mei 19 12:24:17 w3nden-Aspire-V5-572G kernel: nouveau 0000:01:00.0: drm: failed to create ce channel, -22
mei 19 12:24:17 w3nden-Aspire-V5-572G kernel: nouveau 0000:01:00.0: bus: MMIO write of ffff8e1f FAULT at 6013d4 [ PRIVRING ]
mei 19 12:24:25 w3nden-Aspire-V5-572G kernel: nouveau 0000:01:00.0: bus: MMIO write of 533fc21f FAULT at 6013d4 [ PRIVRING ]
mei 19 12:24:50 w3nden-Aspire-V5-572G kernel: nouveau 0000:01:00.0: msvld: unable to load firmware data
mei 19 12:24:58 w3nden-Aspire-V5-572G kernel: nouveau 0000:01:00.0: msvld: init failed, -19
mei 19 12:25:11 w3nden-Aspire-V5-572G gdm-password[1528]: gkr-pam: unable to locate daemon control file
mei 19 12:25:12 w3nden-Aspire-V5-572G gdm[31005]: Gdm: on_display_added: assertion 'GDM_IS_REMOTE_DISPLAY (display)' failed
mei 19 12:25:14 w3nden-Aspire-V5-572G systemd[1542]: Failed to start app-gnome-gnome\x2dkeyring\x2dpkcs11-1718.scope
mei 19 12:25:14 w3nden-Aspire-V5-572G systemd[1542]: Failed to start app-gnome-gnome\x2dkeyring\x2dsecrets-1721.scope
mei 19 12:25:14 w3nden-Aspire-V5-572G systemd[1542]: Failed to start app-gnome-gnome\x2dkeyring\x2dssh-1715.scope
mei 19 12:25:15 w3nden-Aspire-V5-572G kernel: nouveau 0000:01:00.0: bus: MMIO write of 0000001f FAULT at 6013d4 [ PRIVRING ]
mei 19 12:25:17 w3nden-Aspire-V5-572G kernel: nouveau 0000:01:00.0: msvld: unable to load firmware data
mei 19 12:25:17 w3nden-Aspire-V5-572G kernel: nouveau 0000:01:00.0: msvld: init failed, -19
```

> **Примітка:** Більшість помилок пов'язані з драйвером `nouveau` для графічної карти NVIDIA та ACPI.

### 3.3 Журнал сервісу `cron`

```bash
sudo journalctl -u cron --no-pager | tail -n 20
```

**Результат:**
```
mei 19 12:45:01 w3nden-Aspire-V5-572G CRON[12658]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
mei 19 12:45:01 w3nden-Aspire-V5-572G CRON[12657]: pam_unix(cron:session): session closed for user root
mei 19 12:55:01 w3nden-Aspire-V5-572G CRON[12689]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
mei 19 12:55:01 w3nden-Aspire-V5-572G CRON[12690]: (root) CMD (command -v debian-sa1 > /dev/null && debian-sa1 1 1)
mei 19 12:55:01 w3nden-Aspire-V5-572G CRON[12689]: pam_unix(cron:session): session closed for user root
mei 19 13:17:01 w3nden-Aspire-V5-572G CRON[14378]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
mei 19 13:17:01 w3nden-Aspire-V5-572G CRON[14379]: (root) CMD (cd /; run-parts --report /etc/cron.hourly)
mei 19 13:18:26 w3nden-Aspire-V5-572G CRON[14378]: pam_unix(cron:session): session closed for user root
mei 19 13:21:07 w3nden-Aspire-V5-572G systemd[1]: Stopping cron.service - Regular background program processing daemon...
mei 19 13:21:07 w3nden-Aspire-V5-572G systemd[1]: cron.service: Deactivated successfully.
mei 19 13:21:07 w3nden-Aspire-V5-572G systemd[1]: Stopped cron.service - Regular background program processing daemon.
mei 19 13:22:04 w3nden-Aspire-V5-572G systemd[1]: Started cron.service - Regular background program processing daemon.
mei 19 13:22:04 w3nden-Aspire-V5-572G cron[14502]: (CRON) INFO (pidfile fd = 3)
mei 19 13:22:04 w3nden-Aspire-V5-572G cron[14502]: (cron.service) Referenced but unset environment variable evaluates to an empty string: $EXTRA_OPTS
mei 19 13:22:04 w3nden-Aspire-V5-572G cron[14502]: (CRON) INFO (Skipping @reboot jobs -- not system startup)
```

> **Висновок:** Журнал показує регулярну роботу cron-завдань, а також зупинку та перезапуск сервісу під час виконання Завдання 2.

---

## Завдання 4. Створення власного сервісу

### 4.1 Створення bash-скрипту

```bash
sudo nano /usr/local/bin/myscript.sh
```

**Вміст файлу `/usr/local/bin/myscript.sh`:**
```bash
#!/bin/bash
echo "Script ran at: $(date)" >> /var/log/myscript.log
```

**Перевірка вмісту:**
```bash
cat /usr/local/bin/myscript.sh
```

**Результат:**
```
#!/bin/bash
echo "Script ran at: $(date)" >> /var/log/myscript.log
```

### 4.2 Надання прав на виконання

```bash
sudo chmod +x /usr/local/bin/myscript.sh
```

### 4.3 Створення systemd service файлу

```bash
sudo nano /etc/systemd/system/myscript.service
```

**Вміст файлу `/etc/systemd/system/myscript.service`:**
```ini
[Unit]
Description=My Custom Script Service

[Service]
ExecStart=/usr/local/bin/myscript.sh

[Install]
WantedBy=multi-user.target
```

### 4.4 Перезавантаження конфігурації systemd

```bash
sudo systemctl daemon-reload
```

### 4.5 Запуск сервісу

```bash
sudo systemctl start myscript.service
```

### 4.6 Перевірка статусу сервісу

```bash
sudo systemctl status myscript.service
```

**Результат:**
```
○ myscript.service - My Custom Script Service
     Loaded: loaded (/etc/systemd/system/myscript.service; disabled; preset: enabled)
     Active: inactive (dead)

mei 19 13:30:51 w3nden-Aspire-V5-572G systemd[1]: Started myscript.service - My Custom Script Service.
mei 19 13:30:51 w3nden-Aspire-V5-572G systemd[1]: myscript.service: Deactivated successfully.
mei 19 13:49:16 w3nden-Aspire-V5-572G systemd[1]: Started myscript.service - My Custom Script Service.
mei 19 13:49:16 w3nden-Aspire-V5-572G systemd[1]: myscript.service: Deactivated successfully.
mei 19 13:53:39 w3nden-Aspire-V5-572G systemd[1]: Started myscript.service - My Custom Script Service.
mei 19 13:53:39 w3nden-Aspire-V5-572G systemd[1]: myscript.service: Deactivated successfully.
```

> **Примітка:** Статус `inactive (dead)` є нормальним для одноразового сервісу типу `oneshot`. Сервіс виконується та одразу завершується.

### 4.7 Перевірка логу

```bash
cat /var/log/myscript.log
```

**Результат:**
```
Script ran at: di 19 mei 2026 13:53:39 CEST
```

> **Висновок:** Сервіс успішно створено та запущено. Скрипт записав мітку часу у файл `/var/log/myscript.log`.

---

## Висновки

Всі 4 завдання виконано успішно:

1. ✅ **Менеджери пакетів** — оновлено список пакетів, встановлено/видалено `tree`, перевірено версію
2. ✅ **Керування сервісами** — перевірено статус `cron`, зупинено/запущено сервіс, додано до автозавантаження
3. ✅ **Робота з логами** — переглянуто системні логи через `tail` та `journalctl`, відфільтровано помилки
4. ✅ **Власний сервіс** — створено bash-скрипт, налаштовано systemd service, запущено та перевірено роботу

Усі команди виконано в терміналі Zorin OS (базується на Ubuntu 24.04 Noble) о 19 травня 2026 року.
