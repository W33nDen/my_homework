# Домашнє завдання №3. Процеси та ресурси

## Завдання 1. Перевірка процесів

```bash
cd ~
pwd
/home/w3nden

ps aux
```

### Результат
У виводі `ps aux` видно, що серед активних процесів найбільше пам’яті споживав `brave` з PID `24405` та `25648`, а також `gnome-shell` з PID `9961`.

```bash
top
```

### Результат
У `top` процеси `brave` та `gnome-shell` мали найбільше значення `%MEM`.  
Поточна оболонка мала PID:

```bash
echo $$
25862
```

---

## Завдання 2. Фонові процеси

```bash
sleep 1000 &
 26389[2]

jobs
+  Running                 sleep 1000 &[2]

fg %1
sleep 1000
^Z
+  Stopped                 sleep 1000[2]

jobs
+  Stopped                 sleep 1000[2]

ps aux | grep "sleep 1000"
w3nden     26389  0.0  0.0  11140  2224 pts/0    T    21:44   0:00 sleep 1000
w3nden     26401  0.0  0.0  12128  2440 pts/0    S+   21:45   0:00 grep --color=auto sleep 1000

kill 12345
bash: kill: (12345) - No such process

nohup sleep 1000 &
 26412[3]
nohup: ignoring input and appending output to 'nohup.out'
```

### Результат
Команда `sleep 1000` була запущена у фоні, переведена в foreground, зупинена через `Ctrl+Z`, а потім перевірена через `jobs` і `ps aux | grep`.  
Команда `kill 12345` повернула помилку, бо PID був неправильний.

---

## Завдання 3. Пріоритет процесів та обмеження

```bash
nice -n 10 sleep 500 &
 26416[4]

ps -o pid,ni,cmd -C sleep
    PID  NI CMD
  26389   0 sleep 1000
  26412   0 sleep 1000
  26416  10 sleep 500

sudo renice -n 5 -p 23456
[sudo] password for w3nden:
renice: failed to get priority for 23456 (process ID): No such process

ulimit -a
real-time non-blocking time  (microseconds, -R) unlimited
core file size              (blocks, -c) 0
data seg size               (kbytes, -d) unlimited
scheduling priority                 (-e) 0
file size                   (blocks, -f) unlimited
pending signals                     (-i) 14851
max locked memory           (kbytes, -l) 485876
max memory size             (kbytes, -m) unlimited
open files                          (-n) 1024
pipe size                (512 bytes, -p) 8
POSIX message queues         (bytes, -q) 819200
real-time priority                  (-r) 0
stack size                  (kbytes, -s) 8192
cpu time                   (seconds, -t) unlimited
max user processes                  (-u) 14851
virtual memory              (kbytes, -v) unlimited
file locks                          (-x) unlimited
```

### Результат
Процес `sleep 500` був запущений із `nice 10`, що видно у колонці `NI`.  
Спроба `renice` не вдалася, бо PID `23456` не існував у системі.

---

## Завдання 4. Диск і пам’ять

```bash
df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           380M  1,7M  378M   1% /run
/dev/sda3       687G   14G  638G   3% /
tmpfs           1,9G  144M  1,8G   8% /dev/shm
tmpfs           5,0M     0  5,0M   0% /run/lock
/dev/sda2       512M  6,2M  506M   2% /boot/efi
tmpfs           380M  156K  380M   1% /run/user/1000

free -h
               total        used        free      shared  buff/cache   available
Mem:           3,7Gi       1,9Gi       351Mi       381Mi       2,1Gi       1,8Gi
Swap:          3,9Gi       521Mi       3,4Gi
```

### Результат
Система має `3,7 GiB` RAM, з яких використано `1,9 GiB`, а також `3,9 GiB` swap.  
На диску основний розділ `/dev/sda3` має `687G`, із яких зайнято `14G`.

---

## Додатково

```bash
# Поточний користувач
w3nden

# PID поточної оболонки
25862

# Домашній каталог
/home/w3nden
```
