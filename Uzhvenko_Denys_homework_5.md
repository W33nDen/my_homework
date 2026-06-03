# Homework module 5

## Завдання 1. Мережева діагностика

**Виконані команди та вивід:**
```bash
trainee@w3nden-Aspire-V5-572G:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp4s0f1: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether 08:9e:01:e9:1b:9e brd ff:ff:ff:ff:ff:ff
3: wlp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 80:56:f2:09:43:c3 brd ff:ff:ff:ff:ff:ff
    inet 10.199.129.251/27 brd 10.199.129.255 scope global dynamic noprefixroute wlp3s0
       valid_lft 84766sec preferred_lft 84766sec
    inet6 fe80::c29d:2827:1e41:cece/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
trainee@w3nden-Aspire-V5-572G:~$ ping -c 4 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=120 time=5.98 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=120 time=6.00 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=120 time=6.16 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=120 time=8.49 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 5.975/6.653/8.487/1.060 ms
trainee@w3nden-Aspire-V5-572G:~$ ss -tulpn
Netid  State   Recv-Q  Send-Q   Local Address:Port    Peer Address:Port Process 
udp    UNCONN  0       0              0.0.0.0:5353         0.0.0.0:*            
udp    UNCONN  0       0           127.0.0.54:53           0.0.0.0:*            
udp    UNCONN  0       0        127.0.0.53%lo:53           0.0.0.0:*            
udp    UNCONN  0       0              0.0.0.0:51265        0.0.0.0:*            
udp    UNCONN  0       0                 [::]:5353            [::]:*            
udp    UNCONN  0       0                 [::]:43711           [::]:*            
tcp    LISTEN  0       4096         127.0.0.1:631          0.0.0.0:*            
tcp    LISTEN  0       4096     127.0.0.53%lo:53           0.0.0.0:*            
tcp    LISTEN  0       4096        127.0.0.54:53           0.0.0.0:*            
tcp    LISTEN  0       4096             [::1]:631             [::]:*            
```

**Короткий коментар:**  
Активний інтерфейс `wlp3s0` має локальну IP‑адресу `10.199.129.251/27`. Доступ до інтернету є (0% втрати пакетів при `ping 8.8.8.8`), а `ss -tulpn` показує локальні служби, наприклад CUPS, що слухає порт 631.

---

## Завдання 2. SSH-доступ з ключами та config

**Виконані команди та вивід:**
```bash
trainee@w3nden-Aspire-V5-572G:~$ sudo apt update
[sudo] password for trainee:           
Hit:1 http://nl.archive.ubuntu.com/ubuntu noble InRelease
Hit:2 http://nl.archive.ubuntu.com/ubuntu noble-updates InRelease              
Hit:3 http://security.ubuntu.com/ubuntu noble-security InRelease               
Hit:4 http://nl.archive.ubuntu.com/ubuntu noble-backports InRelease            
Hit:5 https://brave-browser-apt-release.s3.brave.com stable InRelease          
Hit:6 https://dl.google.com/linux/chrome-stable/deb stable InRelease           
Hit:7 https://ppa.launchpadcontent.net/zorinos/apps/ubuntu noble InRelease     
Hit:8 https://ppa.launchpadcontent.net/zorinos/drivers/ubuntu noble InRelease  
Hit:9 https://ppa.launchpadcontent.net/zorinos/patches/ubuntu noble InRelease
Hit:10 https://ppa.launchpadcontent.net/zorinos/stable/ubuntu noble InRelease
Hit:11 https://us-central1-apt.pkg.dev/projects/antigravity-auto-updater-dev antigravity-debian InRelease
Hit:12 https://packages.zorinos.com/stable noble InRelease
Hit:13 https://packages.zorinos.com/patches noble InRelease
Hit:14 https://packages.zorinos.com/apps noble InRelease
Hit:15 https://packages.zorinos.com/drivers noble InRelease
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
3 packages can be upgraded. Run 'apt list --upgradable' to see them.
N: Skipping acquire of configured file 'main/binary-i386/Packages' as repository 'https://us-central1-apt.pkg.dev/projects/antigravity-auto-updater-dev antigravity-debian InRelease' doesn't support architecture 'i386'
trainee@w3nden-Aspire-V5-572G:~$ sudo apt install openssh-server
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libwoff1 zorin-os-feedback
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  ncurses-term openssh-sftp-server ssh-import-id
Suggested packages:
  molly-guard monkeysphere ssh-askpass
The following NEW packages will be installed:
  ncurses-term openssh-server openssh-sftp-server ssh-import-id
0 upgraded, 4 newly installed, 0 to remove and 3 not upgraded.
Need to get 832 kB of archives.
After this operation, 6.747 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://nl.archive.ubuntu.com/ubuntu noble-updates/main amd64 openssh-sftp-server amd64 1:9.6p1-3ubuntu13.16 [37,3 kB]
Get:2 http://nl.archive.ubuntu.com/ubuntu noble-updates/main amd64 openssh-server amd64 1:9.6p1-3ubuntu13.16 [510 kB]
Get:3 http://nl.archive.ubuntu.com/ubuntu noble/main amd64 ncurses-term all 6.4+20240113-1ubuntu2 [275 kB]
Get:4 http://nl.archive.ubuntu.com/ubuntu noble-updates/main amd64 ssh-import-id all 5.11-0ubuntu2.24.04.1 [10,1 kB]
Fetched 832 kB in 0s (2.816 kB/s)         
Preconfiguring packages ...
Selecting previously unselected package openssh-sftp-server.
(Reading database ... 292854 files and directories currently installed.)
Preparing to unpack .../openssh-sftp-server_1%3a9.6p1-3ubuntu13.16_amd64.deb ...
Unpacking openssh-sftp-server (1:9.6p1-3ubuntu13.16) ...
Selecting previously unselected package openssh-server.
Preparing to unpack .../openssh-server_1%3a9.6p1-3ubuntu13.16_amd64.deb ...
Unpacking openssh-server (1:9.6p1-3ubuntu13.16) ...
Selecting previously unselected package ncurses-term.
Preparing to unpack .../ncurses-term_6.4+20240113-1ubuntu2_all.deb ...
Unpacking ncurses-term (6.4+20240113-1ubuntu2) ...
Selecting previously unselected package ssh-import-id.
Preparing to unpack .../ssh-import-id_5.11-0ubuntu2.24.04.1_all.deb ...
Unpacking ssh-import-id (5.11-0ubuntu2.24.04.1) ...
Setting up openssh-sftp-server (1:9.6p1-3ubuntu13.16) ...
Setting up openssh-server (1:9.6p1-3ubuntu13.16) ...

Creating config file /etc/ssh/sshd_config with new version
Creating SSH2 RSA key; this may take some time ...
3072 SHA256:gd/aqias+l2Tvv1ZeAkRV1DF5UDRBiAvFTmDa+Q2jBw root@w3nden-Aspire-V5-57
2G (RSA)
Creating SSH2 ECDSA key; this may take some time ...
256 SHA256:+lS121FB58QvzMqAe83WBafjTt/7UXvMPLf6saaiMV4 root@w3nden-Aspire-V5-572
G (ECDSA)
Creating SSH2 ED25519 key; this may take some time ...
256 SHA256:Jgd44+7uz3duPz8qoyhPJPx/EGUUUIw2y3TFZgy6glQ root@w3nden-Aspire-V5-572
G (ED25519)
Created symlink /etc/systemd/system/sockets.target.wants/ssh.socket → /usr/lib/s
ystemd/system/ssh.socket.
Created symlink /etc/systemd/system/ssh.service.requires/ssh.socket → /usr/lib/s
ystemd/system/ssh.socket.
Setting up ssh-import-id (5.11-0ubuntu2.24.04.1) ...
Setting up ncurses-term (6.4+20240113-1ubuntu2) ...
Processing triggers for man-db (2.12.0-4build2) ...
Processing triggers for ufw (0.36.2-6) ...
trainee@w3nden-Aspire-V5-572G:~$ sudo systemctl enable --now ssh
Synchronizing state of ssh.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install enable ssh
Created symlink /etc/systemd/system/sshd.service → /usr/lib/systemd/system/ssh.service.
Created symlink /etc/systemd/system/multi-user.target.wants/ssh.service → /usr/lib/systemd/system/ssh.service.
trainee@w3nden-Aspire-V5-572G:~$ sudo systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enab>
     Active: active (running) since Wed 2026-06-03 15:22:11 CEST; 18s ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 41669 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCES>
   Main PID: 41671 (sshd)
      Tasks: 1 (limit: 4454)
     Memory: 2.1M (peak: 2.5M)
        CPU: 53ms
     CGroup: /system.slice/ssh.service
             └─41671 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

jun 03 15:22:11 w3nden-Aspire-V5-572G systemd: Starting ssh.service - OpenBS>[1]
jun 03 15:22:11 w3nden-Aspire-V5-572G sshd: Server listening on 0.0.0.0 >
jun 03 15:22:11 w3nden-Aspire-V5-572G sshd: Server listening on :: port >
jun 03 15:22:11 w3nden-Aspire-V5-572G systemd: Started ssh.service - OpenBSD>[1]
```

**Короткий коментар:**  
SSH‑сервер встановлено та увімкнено, служба `ssh` працює в стані `active (running)` і слухає порт 22 на всіх інтерфейсах.

---

**Генерація ключа та додавання на localhost:**
```bash
trainee@w3nden-Aspire-V5-572G:~$ whoami
trainee
trainee@w3nden-Aspire-V5-572G:~$ ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/trainee/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/trainee/.ssh/id_ed25519
Your public key has been saved in /home/trainee/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:kUjZmrjHWU/8g/9N+chGp/ZdjPxqjs6BCgif4IRoiIo trainee@w3nden-Aspire-V5-572G
The key's randomart image is:
+--[ED25519 256]--+
|      .o         |
|     .....       |
|     ..oo.       |
|+.  . o ..o      |
|=.+  o oSo o     |
|+o +.o+   o.o..oo|
|E . +..   ...oo++|
|       . . ..+==+|
|        .  .+**o*|
+----[SHA256]-----+
trainee@w3nden-Aspire-V5-572G:~$ ssh-copy-id trainee@localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:Jgd44+7uz3duPz8qoyhPJPx/EGUUUIw2y3TFZgy6glQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
trainee@localhost's password: 
Permission denied, please try again.
trainee@localhost's password: 
Permission denied, please try again.
trainee@localhost's password: 
trainee@localhost: Permission denied (publickey,password).
trainee@w3nden-Aspire-V5-572G:~$ ssh-copy-id trainee@localhost
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
trainee@localhost's password: 
Permission denied, please try again.
trainee@localhost's password: 
Permission denied, please try again.
trainee@localhost's password: 
trainee@localhost: Permission denied (publickey,password).
trainee@w3nden-Aspire-V5-572G:~$ ssh trainee@localhost
trainee@localhost's password: 
Welcome to Zorin OS 18.1 (GNU/Linux 6.17.0-29-generic x86_64)
...
trainee@w3nden-Aspire-V5-572G:~$ ssh-copy-id trainee@localhost
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/trainee/.ssh/id_ed25519.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
trainee@localhost's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'trainee@localhost'"
and check to make sure that only the key(s) you wanted were added.

trainee@w3nden-Aspire-V5-572G:~$ ssh trainee@localhost
Welcome to Zorin OS 18.1 (GNU/Linux 6.17.0-29-generic x86_64)
...
Last login: Wed Jun  3 15:29:16 2026 from 127.0.0.1
trainee@w3nden-Aspire-V5-572G:~$ exit
logout
Connection to localhost closed.
```

**Короткий коментар:**  
Було створено ключову пару для користувача `trainee` і після кількох спроб ключ успішно додано на `localhost`. Після цього вхід через `ssh trainee@localhost` відбувається за допомогою ключа.

---

**Налаштування `~/.ssh/config` і alias `myserver`:**
```bash
trainee@w3nden-Aspire-V5-572G:~$ nano ~/.ssh/config
trainee@w3nden-Aspire-V5-572G:~$ shh myserver
Command 'shh' not found, did you mean:
  command 'sh' from deb dash (0.5.12-6ubuntu1)
  command 'ssh' from deb openssh-client (1:9.6p1-3ubuntu13.16)
  command 'shc' from deb shc (4.0.3-1)
Try: sudo apt install <deb name>
trainee@w3nden-Aspire-V5-572G:~$ nano ~/.ssh/config
trainee@w3nden-Aspire-V5-572G:~$ ssh myserver
Welcome to Zorin OS 18.1 (GNU/Linux 6.17.0-29-generic x86_64)
...
Last login: Wed Jun  3 15:32:25 2026 from 127.0.0.1
trainee@w3nden-Aspire-V5-572G:~$ exit
logout
Connection to localhost closed.
```

**Короткий коментар:**  
У файлі `~/.ssh/config` налаштовано Host `myserver`, що вказує на `localhost` для користувача `trainee`. Після цього підключення командою `ssh myserver` працює без пароля.

---

## Завдання 3. Копіювання файлів між машинами

**Виконані команди та вивід:**
```bash
trainee@w3nden-Aspire-V5-572G:~$ echo "test" > test.txt
trainee@w3nden-Aspire-V5-572G:~$ scp test.txt myserver:~/test.txt
test.txt                                                                                                                              100%    5     4.8KB/s   00:00    
trainee@w3nden-Aspire-V5-572G:~$ ssh myserver
Welcome to Zorin OS 18.1 (GNU/Linux 6.17.0-29-generic x86_64)
...
Last login: Wed Jun  3 15:36:12 2026 from 127.0.0.1
trainee@w3nden-Aspire-V5-572G:~$ mkdir -p ~/sync_folder
trainee@w3nden-Aspire-V5-572G:~$ exit
logout
Connection to localhost closed.
trainee@w3nden-Aspire-V5-572G:~$ mkdir -p local_sync
trainee@w3nden-Aspire-V5-572G:~$ cp test.txt local_sync/
trainee@w3nden-Aspire-V5-572G:~$ rsync -av local_sync/ myserver:~/sync_folder/
sending incremental file list
./
test.txt

sent 137 bytes  received 38 bytes  350,00 bytes/sec
total size is 5  speedup is 0,03
trainee@w3nden-Aspire-V5-572G:~$ sftp myserver
Connected to myserver.
sftp> ls
Desktop       Documents     Downloads     Music         Pictures      Public        Templates     Videos        local_sync    sync_folder   test.txt      
sftp> cd sync_folder
sftp> exit
trainee@w3nden-Aspire-V5-572G:~$ cd sync_folder
trainee@w3nden-Aspire-V5-572G:~/sync_folder$ ls
test.txt
trainee@w3nden-Aspire-V5-572G:~/sync_folder$ exit
logout
Connection to localhost closed.
trainee@w3nden-Aspire-V5-572G:~$
```

**Короткий коментар:**  
Файл `test.txt` створено локально, скопійовано на `myserver` через `scp` і синхронізовано в директорію `~/sync_folder` за допомогою `rsync`. Перевірка через `sftp` та команду `ls` у `~/sync_folder` показала наявність `test.txt`, шлях до файлу на «сервері» — `/home/trainee/sync_folder/test.txt`.
