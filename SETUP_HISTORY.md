# Quartz4-exp Setup History

Дата: 2026-02-08 (Europe/Moscow)

## Что было сделано

1. Создана отдельная директория проекта:
   - `quartz4-site`

2. Развёрнут Quartz 4:
   - Клонирован шаблон Quartz
   - Установлены зависимости (`npm install`)
   - Выполнена инициализация (`npx quartz create`) с настройками:
     - `Empty Quartz`
     - `Treat links as shortest path`

3. Проверка сборки:
   - Выполнено `npx quartz build`
   - Сборка прошла успешно

4. Подготовка деплоя на GitHub Pages:
   - Добавлен workflow `.github/workflows/deploy.yml`
   - Настроен `baseUrl` в `quartz.config.ts`:
     - `shellshshshshshiiii-a11y.github.io/quartz4-exp`

5. Инициализация Git и публикация:
   - Инициализирован git-репозиторий
   - Создана ветка `v4`
   - Сделан initial commit
   - Создан и подключён GitHub-репозиторий:
     - `https://github.com/shellshshshshshiiii-a11y/quartz4-exp`
   - Выполнен push ветки `v4`

6. Включён GitHub Pages:
   - Режим: **GitHub Actions (workflow)**
   - Публичный URL сайта:
     - `https://shellshshshshshiiii-a11y.github.io/quartz4-exp/`

## Дополнительно

- По пути встретились ограничения токена GitHub (`createRepository`, затем `workflow` scope), были исправлены повторной авторизацией `gh` с нужными правами.
- Была диагностирована нехватка места на `/` и обнаружено, что диск 40G, но корневой раздел был 10G (типичная ситуация после увеличения VDS без расширения partition/filesystem).

## Текущее состояние

- Проект `quartz4-exp` развёрнут и запушен.
- Workflow деплоя запущен.
- Сайт доступен по URL выше после завершения деплоя.
