---
title: User-Scanner — установка и настройка
---

# User-Scanner — установка и настройка

Репозиторий: https://github.com/kaifcodec/user-scanner  
PyPI: `user-scanner`

> Инструмент делает OSINT-проверки (email/username) по разным сайтам. Используй только для легитимных задач (свой аккаунт/свой домен/с разрешения), иначе легко уехать в нарушение правил сервисов.

---

## 1) Требования

- **Python 3.10+**
- `pip`
- (Рекомендуется) `venv` для изоляции пакетов

Проверка:

```bash
python3 -V
python3 -m pip -V
```

---

## 2) Установка (Linux / Ubuntu)

### Вариант A — через venv (рекомендуется)

```bash
# зависимости для venv/pip (Ubuntu/Debian)
sudo apt-get update
sudo apt-get install -y python3-venv python3-pip

# создаём окружение
mkdir -p ~/tools/user-scanner && cd ~/tools/user-scanner
python3 -m venv .venv

# активируем
source .venv/bin/activate

# обновляем pip и ставим пакет
python -m pip install --upgrade pip
pip install user-scanner

# проверка
user-scanner --version
user-scanner --help
```

> Примечание: пока активен `venv`, команда `user-scanner` будет доступна в этой сессии.

### Вариант B — глобально (быстро, но менее аккуратно)

```bash
sudo apt-get update
sudo apt-get install -y python3-pip

python3 -m pip install --upgrade pip
sudo python3 -m pip install user-scanner

user-scanner --help
```

---

## 3) Установка (Termux)

(Из README проекта видно, что тестировалось на Termux.)

Примерно так:

```bash
pkg update
pkg install python
python -m pip install --upgrade pip
pip install user-scanner

user-scanner --help
```

---

## 4) Установка (Windows)

1. Поставь Python 3.10+ (галка **Add python.exe to PATH**).
2. Открой PowerShell:

```powershell
py -m pip install --upgrade pip
py -m pip install user-scanner

user-scanner --help
```

---

## 5) Быстрый старт

### Скан одного username

```bash
user-scanner -u john_doe
```

### Скан одного email

```bash
user-scanner -e john_doe@gmail.com
```

### Список доступных модулей

```bash
user-scanner -lu   # username modules
user-scanner -le   # email modules
```

### Выбор категории или конкретного модуля

```bash
user-scanner -u john_doe -c dev
user-scanner -u john_doe -m github
```

---

## 6) Bulk-режим (из файла)

Файл формата: **одна строка = один username/email**.

```bash
user-scanner -uf usernames.txt
user-scanner -ef emails.txt
```

---

## 7) Пермутации username

Если хочешь генерировать вариации по суффиксу/шаблону:

```bash
user-scanner -u john_ -p ab
```

Ограничить количество:

```bash
user-scanner -u john_ -p ab -s 50
```

---

## 8) Прокси

### Файл прокси

`proxies.txt` (по одному на строку). Формат обычно вида:

- `http://user:pass@host:port`
- `socks5://host:port`

Запуск с прокси:

```bash
user-scanner -u john_doe -P proxies.txt
```

Проверка прокси перед сканом (рекомендуется):

```bash
user-scanner -u john_doe -P proxies.txt --validate-proxies
```

---

## 9) Задержка между запросами

Полезно, чтобы меньше ловить rate-limit:

```bash
user-scanner -u john_doe -d 1
```

---

## 10) Экспорт результатов (JSON/CSV)

```bash
# JSON
user-scanner -u john_doe -f json -o out.json

# CSV
user-scanner -u john_doe -f csv -o out.csv
```

---

## 11) Обновление

У инструмента есть флаг обновления:

```bash
user-scanner -U
```

Если ставил в venv — активируй venv перед обновлением.

---

## 12) Типовые проблемы

### `user-scanner: command not found`

- Если ставил в venv: убедись, что venv активирован (`source .venv/bin/activate`).
- Если ставил глобально: проверь, куда установилось (`which user-scanner`) и PATH.

### Ошибки/таймауты

- Проверь интернет/доступ к целевым сайтам.
- Добавь `-d 1` или используй прокси.
- Используй `-v` для подробного вывода.

---

## 13) Мини-рекомендации по “настройке” (практика)

- Начни со **списка модулей** (`-lu/-le`) и выбери нужные категории (`-c`).
- Для повторяемости сохраняй результаты в **JSON** (`-f json -o ...`).
- При большом объёме используй **delay** и/или **прокси**.

Если скажешь, под какой сценарий ты ставишь (email OSINT или поиск никнейма для брендинга), я сделаю более прикладной набор команд и структуру файлов (input/output).