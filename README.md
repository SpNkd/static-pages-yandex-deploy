# Static Pages + Yandex Deploy

Этот skill помогает бесплатно выложить веб-проект, состоящий из статической страницы и backend-части.

Статический frontend публикуется на GitHub Pages и/или GitVerse Pages, а backend работает в Yandex Cloud (API Gateway + Cloud Functions и нужное хранилище). В результате получается доступный по ссылке проект без отдельного платного сервера для frontend.

## Что делает skill

- собирает и проверяет статическую страницу;
- публикует frontend на GitHub Pages и GitVerse Pages;
- разворачивает или обновляет backend в Yandex Cloud;
- проверяет, что опубликованный frontend действительно обращается к API;
- сохраняет историю релизов и подсказывает, как откатиться.

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
