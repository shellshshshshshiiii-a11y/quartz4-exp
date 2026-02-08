---
title: LazyPorts — отчёт по тестированию
---

# LazyPorts — отчёт по тестированию

Репозиторий: https://github.com/v9mirza/LazyPorts

## TL;DR

- LazyPorts успешно **собран и запущен** на сервере.
- Это **TUI-приложение**, ему нужен настоящий TTY (в обычном non-interactive запуске падает с ошибкой `/dev/tty`).
- Функции просмотра слушающих портов и просмотра деталей соединения (Enter) — работают.
- Функцию **kill процесса** намеренно не тестировал, чтобы не прибить важные сервисы.

## Среда тестирования

- OS: Linux (Ubuntu-based)
- Установка/сборка: через Go toolchain

## Установка (как было сделано в тесте)

LazyPorts в текущей версии требует Go и содержит в `go.mod`:

- `go 1.24.0`
- `toolchain go1.24.12`

На сервере Go изначально отсутствовал, поэтому был установлен официальный Go из tarball (go.dev/dl), затем установлен LazyPorts:

```bash
# 1) установить Go
cd /tmp
curl -fsSLO https://go.dev/dl/go1.25.7.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.25.7.linux-amd64.tar.gz

# 2) поставить lazyports
export PATH=/usr/local/go/bin:$PATH

go install github.com/v9mirza/lazyports@main
sudo cp "$(go env GOPATH)/bin/lazyports" /usr/local/bin/lazyports
```

## Запуск

```bash
lazyports
```

Важно: запускать из интерактивного терминала/TTY.

При попытке запуска без TTY наблюдалась ошибка:

```
Error running program: could not open a new TTY: open /dev/tty: no such device or address
```

## Что проверено

### 1) Отображение списка портов

После запуска отображается таблица слушающих портов (TCP/UDP), PID и процесс.

На тестовом хосте LazyPorts корректно показал, например:

- `22/tcp` → `sshd`
- `53/tcp`/`53/udp` → `systemd-resolve`
- `18789/tcp`, `18792/tcp` → `openclaw-gateway`
- `18080/tcp` → `python3` (http.server)

### 2) Просмотр деталей соединения

На выбранной строке нажимается `Enter` → открывается окно `Connection Details`.

Проверено на `22/tcp` (sshd): отображаются порт/протокол/PID/адрес/состояние/процесс и команда запуска.

## Что НЕ проверял (осознанно)

- `k` (kill процесса) — не тестировал, чтобы не остановить важные демоны.

## Вывод

Инструмент рабочий, полезен как визуальная «панель портов».

Если хочешь — могу:

1) поставить его как «постоянный» системный бинарник (уже скопирован в `/usr/local/bin/lazyports`),
2) добавить в документацию краткий «cheatsheet» по клавишам,
3) предложить безопасный сценарий теста `k` на специально запущенном тестовом процессе (например, временный `python -m http.server`).
