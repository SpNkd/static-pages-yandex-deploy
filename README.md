# Static Pages + Yandex Deploy

Reusable Codex skill for deploying static web apps to GitHub Pages and GitVerse Pages with a Yandex Cloud API backend.

## Что умеет

- проверяет сборку и статический output;
- деплоит GitHub Pages и GitVerse Pages;
- обновляет Yandex Cloud Function/API без раскрытия секретов;
- сохраняет release history и описывает rollback;
- выполняет smoke-проверки после публикации.

## Установка

Распакуйте содержимое skill в:

```text
~/.codex/skills/static-pages-yandex-deploy/
```

Перезапустите Codex, затем вызовите:

```text
$static-pages-yandex-deploy
```

Перед запуском укажите репозитории, Pages URL, API URL, output directory, base path и существующий Yandex Cloud bucket. Токены и другие секреты в skill не хранятся — передавайте их только через защищённое окружение.

Подробные шаги находятся в [INSTALL.md](INSTALL.md), а правила релизов — в `references/release-history.md`.

## Безопасность

Skill не запрашивает и не сохраняет пользовательские секреты. Push, изменения Cloud Function, DNS и базы выполняются только после явного разрешения владельца проекта.

## Лицензия

MIT — см. [LICENSE](LICENSE).
