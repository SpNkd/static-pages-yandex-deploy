# Установка skill для деплоя Pages + Yandex

1. Распакуй архив так, чтобы появилась папка `~/.codex/skills/static-pages-yandex-deploy/`.
2. Перезапусти Codex или обнови список skills.
3. Проверь, что появился skill `static-pages-yandex-deploy`.

Явный вызов:

```text
$static-pages-yandex-deploy

Задеплой мой статический проект на GitHub Pages и GitVerse Pages.
Backend использует существующие Yandex Cloud Function, API Gateway и YDB.
Разрешаю push в оба репозитория и обновление Cloud Function.
Не трогай данные в базе.
```

Перед запуском укажи репозитории, Pages URL, API URL, output directory, base path и существующий Yandex bucket.

Skill не содержит секретов и не должен получать токены через исходный код, workflow или сообщения. Авторизацию и разрешения на push, Cloud Function, DNS и базу нужно давать явно.
