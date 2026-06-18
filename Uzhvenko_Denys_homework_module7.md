# Домашнє завдання Module 7. Взаємодія системи і застосунку

**Студент:** Уживенко Денис  
**ОС:** Zorin OS (Ubuntu Noble 24.04)  
**Дата виконання:** 18 червня 2026  
**Обраний варіант:** A — Аналіз життєвого циклу контейнера

---

## Опис завдання

Запустити простий застосунок у Docker-контейнері та проаналізувати його як Linux-процес. Дослідити життєвий цикл контейнера, процеси всередині нього, механізм зупинки та роботу з логами.

---

## Підготовка: Встановлення Docker

### Встановлення Docker

```bash
sudo apt update
sudo apt install docker.io -y
```

### Додавання користувача до групи docker

```bash
sudo usermod -aG docker trainee
```

### Запуск Docker service

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### Перевірка версії

```bash
docker --version
```

**Результат:**
```
Docker version 29.1.3, build 29.1.3-0ubuntu3~24.04.2
```

### Активація групи docker

```bash
newgrp docker
```

> **Примітка:** Після додавання до групи docker потрібно перезайти або виконати `newgrp docker`, щоб зміни набули чинності.

---

## 1. Запуск контейнера

### Команда запуску

```bash
docker run -d --name test-app -p 8080:8000 python:3.9-slim python -m http.server 8000
```

**Пояснення параметрів:**
- `-d` — запуск у фоновому режимі (detached)
- `--name test-app` — ім'я контейнера для зручного управління
- `-p 8080:8000` — проброс порту 8000 контейнера на порт 8080 хоста
- `python:3.9-slim` — образ з мінімальною версією Python 3.9
- `python -m http.server 8000` — команда запуску простого HTTP-сервера на порту 8000

**Результат:**
```
Unable to find image 'python:3.9-slim' locally
3.9-slim: Pulling from library/python
b3ec39b36ae8: Pull complete
ea56f685404a: Pull complete
fc7443084902: Pull complete
38513bd72563: Pull complete
a5e5b1b19090: Download complete
967a6079ce08: Download complete
Digest: sha256:2d97f6910b16bd338d3060f261f53f144965f755599aab1acda1e13cf1731b1b
Status: Downloaded newer image for python:3.9-slim
8225a067b1aab2be88cab027e888befb6584bb45748d7348d0ff638cd66da21a
```

### Перевірка запущених контейнерів

```bash
docker ps
```

**Результат:**
```
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS          PORTS                                       NAMES
8225a067b1aa   python:3.9-slim  "python -m http.serv…"   18 seconds ago   Up 12 seconds   0.0.0.0:8080->8000/tcp, [::]:8080->8000/tcp   test-app
```

✅ **Висновок:** Контейнер успішно запущено. Порт 8000 контейнера проброшено на порт 8080 хоста.

---

## 2. Процес всередині контейнера (PID 1)

### Встановлення утиліт для аналізу процесів

Образ `python:3.9-slim` не містить утиліти `ps`. Встановимо пакет `procps`:

```bash
docker exec -it test-app bash -c "apt update && apt install -y procps"
```

**Результат (скорочено):**
```
Get:1 http://deb.debian.org/debian trixie InRelease [140 kB]
...
Installing: procps
Installing dependencies: libproc2-0 linux-sysctl-defaults psmisc
...
Setting up procps (2:4.0.4-9) ...
```

### Перегляд процесів всередині контейнера

```bash
docker exec -it test-app ps aux
```

**Результат:**
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  3.2  0.5  30396 21908 ?        Ss   10:59   0:00 python -m http.server 8000
root        45  0.0  0.0      0     0 ?        Z    10:59   0:00 [dpkg-preconfigu] <defunct>
root       112 80.0  0.1   6800  4000 pts/1    Rs+  10:59   0:00 ps aux
```

### Пояснення: Чому Python HTTP-сервер має PID 1?

**PID 1** всередині контейнера — це **основний процес контейнера**, який запускається командою `CMD` або `ENTRYPOINT` в Dockerfile (або параметром команди в `docker run`).

**Особливості PID 1 у контейнерах:**

1. **Ініціалізаційний процес:** У звичайній Linux-системі PID 1 — це `systemd` або `init`, який керує всіма іншими процесами. У контейнері Docker PID 1 — це безпосередньо застосунок.

2. **Життєвий цикл контейнера прив'язаний до PID 1:** Коли процес з PID 1 завершується, контейнер також зупиняється.

3. **Обробка сигналів:** PID 1 отримує всі сигнали від Docker (наприклад, SIGTERM при `docker stop`).

4. **Zombie-процеси:** У нашому виводі видно defunct-процес (PID 45) — це zombie-процес, який залишився після завершення встановлення `dpkg-preconfigure`. У нормальній системі `init` (PID 1) "підбирає" такі процеси, але наш Python HTTP-сервер цього не робить, бо не є повноцінним init-процесом.

✅ **Висновок:** `python -m http.server 8000` виконується як PID 1, тому що це єдина команда, яку Docker запустив у контейнері. Контейнер живе, поки цей процес працює.

---

## 3. Завершення контейнера та сигнали

### Перевірка доступності сервісу

```bash
curl http://localhost:8080
```

**Результат:**
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Directory listing for /</title>
</head>
<body>
<h1>Directory listing for /</h1>
<hr>
<ul>
<li><a href=".dockerenv">.dockerenv</a></li>
<li><a href="bin/">bin@</a></li>
<li><a href="boot/">boot/</a></li>
...
</ul>
</body>
</html>
```

✅ HTTP-сервер працює та відповідає на запити.

### Зупинка контейнера

```bash
docker stop test-app
```

**Результат:**
```
test-app
```

### Що відбувається при `docker stop`?

**Механізм зупинки:**

1. **Docker надсилає сигнал SIGTERM (15)** процесу з PID 1 всередині контейнера.
2. **Застосунок отримує 10 секунд** (за замовчуванням) на graceful shutdown — коректне завершення роботи, закриття з'єднань, збереження стану.
3. **Якщо процес не завершується протягом 10 секунд**, Docker надсилає **SIGKILL (9)**, який примусово вбиває процес.

**Що буде, якщо процес ігнорує SIGTERM?**

- Якщо застосунок не обробляє SIGTERM або ігнорує його, він буде працювати до таймауту (10 секунд), після чого отримає SIGKILL.
- SIGKILL **неможливо ігнорувати** — ядро Linux примусово завершує процес.
- Це може призвести до втрати даних, якщо застосунок не встиг коректно завершитися.

**Як змінити таймаут?**

```bash
docker stop -t 30 test-app  # Дати 30 секунд на graceful shutdown
```

### Перевірка статусу після зупинки

```bash
docker ps -a
```

**Результат:**
```
CONTAINER ID   IMAGE            COMMAND                  CREATED        STATUS                     PORTS     NAMES
8225a067b1aa   python:3.9-slim  "python -m http.serv…"   6 minutes ago  Exited (137) 9 seconds ago             test-app
```

**Пояснення статусу `Exited (137)`:**

- Код виходу **137** означає, що процес був вбитий сигналом.
- Формула: `128 + номер_сигналу`
- SIGKILL має номер **9**, тому: `128 + 9 = 137`

✅ **Висновок:** Python HTTP-сервер не обробляє SIGTERM, тому Docker чекав 10 секунд та надіслав SIGKILL (код виходу 137).

---

## 4. Логи контейнера

### Перегляд логів

```bash
docker logs test-app
```

**Результат:**
```
172.17.0.1 - - [18/Jun/2026 10:55:10] "GET / HTTP/1.1" 200 -
```

### Звідки беруться логи?

**Docker перехоплює STDOUT та STDERR** процесу з PID 1 і зберігає їх як логи контейнера.

**Механізм:**

1. Python HTTP-сервер виводить логи запитів у **STDOUT** (стандартний вивід).
2. Docker перенаправляє STDOUT/STDERR у внутрішнє сховище логів (`/var/lib/docker/containers/<container_id>/<container_id>-json.log`).
3. Команда `docker logs` читає цей файл та показує вміст.

**Що записується в логи:**

- Всі повідомлення, які застосунок виводить у консоль (print, logging, помилки)
- У нашому випадку — записи про HTTP-запити

**Формат логу:**
```
172.17.0.1 — IP-адреса клієнта (Docker host)
[18/Jun/2026 10:55:10] — час запиту
"GET / HTTP/1.1" — метод та шлях запиту
200 — HTTP-статус відповіді
```

✅ **Висновок:** Логи контейнера — це вивід STDOUT/STDERR основного процесу. Docker автоматично збирає та зберігає їх.

---

## 5. Очищення

### Видалення контейнера

```bash
docker rm test-app
```

**Результат:**
```
test-app
```

### Видалення образу (опціонально)

```bash
docker rmi python:3.9-slim
```

**Результат:**
```
Untagged: python:3.9-slim
Deleted: sha256:2d97f6910b16bd338d3060f261f53f144965f755599aab1acda1e13cf1731b1b
```

---

## Висновки

У цьому домашньому завданні було досліджено життєвий цикл Docker-контейнера та його взаємодію з операційною системою:

### 1. ✅ Запуск контейнера
- Контейнер запущено з Python HTTP-сервером на порту 8000
- Порт проброшено на хост-систему через `-p 8080:8000`
- Образ `python:3.9-slim` автоматично завантажено з Docker Hub

### 2. ✅ Процес з PID 1
- **Python HTTP-сервер виконується як PID 1** всередині контейнера
- PID 1 — це основний процес контейнера, від якого залежить його життєвий цикл
- На відміну від звичайної системи, де PID 1 — це `systemd`/`init`, у контейнері це безпосередньо застосунок
- Виявлено zombie-процес (defunct), який не "підібрав" PID 1, бо застосунок не є повноцінним init-процесом

### 3. ✅ Завершення через `docker stop`
- Docker надсилає **SIGTERM (15)** процесу PID 1
- Дається **10 секунд** на graceful shutdown
- Якщо процес не завершується, Docker надсилає **SIGKILL (9)**, який неможливо ігнорувати
- Python HTTP-сервер не обробляє SIGTERM, тому був вбитий через SIGKILL (код виходу 137)

### 4. ✅ Логи
- Docker перехоплює **STDOUT та STDERR** основного процесу
- Логи зберігаються у `/var/lib/docker/containers/`
- Команда `docker logs` показує вивід застосунку
- У логах видно HTTP-запит від `curl` з IP-адресою Docker host

### Ключові концепції:

🔹 **Контейнер != Віртуальна машина:** Контейнер — це ізольований процес на хості, а не окрема ОС  
🔹 **PID 1 є критичним:** Життя контейнера залежить від процесу з PID 1  
🔹 **Сигнали керують життєвим циклом:** SIGTERM для graceful shutdown, SIGKILL для примусового завершення  
🔹 **STDOUT/STDERR → Docker logs:** Весь вивід застосунку автоматично збирається Docker

---

**Дата виконання:** 18 червня 2026  
**Час виконання:** 13:00 CEST
