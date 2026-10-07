# Правила работы с репозиториями

Проект **Elementary**, организация GitHub: [`solomain-games`](https://github.com/solomain-games).

## Репозитории

Один репозиторий на сервис или крупную часть проекта.

Организация общая для всех проектов студии, поэтому имя каждого репозитория начинается с префикса проекта: `elementary-`. Локально репозитории лежат рядом в папке `elementary/`, и там префикс не нужен: папки называются коротко (`docs`, `game-core`, ...).

```powershell
git clone git@github.com:solomain-games/elementary-game-core.git game-core
```

| Репозиторий | Назначение | Видимость |
|---|---|---|
| `elementary-docs` | ТЗ, план, ADR, протокол, эти правила | публичный |
| `elementary-game-core` | правила игры, чистая Java-библиотека | публичный |
| `elementary-game-service` | комнаты, партии, WebSocket | публичный |
| `elementary-user-service` | пользователи, гости, JWT | публичный |
| `elementary-gateway` | шлюз (Spring Cloud Gateway) | публичный |
| `elementary-frontend` | веб-клиент (React + TypeScript) | публичный |
| `elementary-infra` | Docker Compose, настройка серверов | публичный |
| `elementary-cases` | дела: карты, вопросы, ответы | **приватный** |

Секреты (ключи, пароли, токены) **никогда** не попадают в Git, в том числе в публичные и приватные репозитории. Они хранятся в `.env`-файлах вне репозитория и в секретах GitHub Actions.

## Ветки (GitHub Flow)

- `main` всегда в рабочем состоянии: собирается, тесты проходят. Прямые коммиты в `main` запрещены настройками репозитория.
- Любая работа ведётся в отдельной ветке от свежего `main` и попадает в `main` через Pull Request.
- Имя ветки: `<тип>/<код задачи латиницей>-<кратко-латиницей>`. Только латиница: кириллические буквы в имени ветки GitHub помечает как скрытые символы, потому что они неотличимы от латинских (А и A).

```
feature/Ya4-turn-actions
fix/G6-lock-timeout
docs/T1-adr
ci/A3-github-actions
```

| Модуль | Т | Я | Г | П | Ш | Л | С | К | Ф | Д | А | И | Н |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| В ветке | T | Ya | G | P | Sh | L | S | K | F | D | A | I | N |

В сообщениях коммитов и заголовках PR код задачи пишется как в плане, кириллицей: `feat(Я4): ...`.

- После вливания PR ветка удаляется (включить в настройках репозитория: *Automatically delete head branches*).

## Коммиты (Conventional Commits)

Формат: `<тип>(<код задачи>): <что сделано>`. Описание пишется по-английски, в повелительном наклонении, со строчной буквы и без точки в конце.

```
feat(Я4): add play card action
fix(Г6): release game lock on exception
test(Я3): cover dealing for 7 players
docs(Т1): add ADR template
```

| Тип | Когда |
|---|---|
| `feat` | новая функциональность |
| `fix` | исправление ошибки |
| `test` | только тесты |
| `refactor` | переделка без изменения поведения |
| `docs` | документация |
| `build` | Maven, npm, Dockerfile, зависимости |
| `ci` | GitHub Actions |
| `chore` | прочая рутина |

Несовместимое изменение (например, ломающее протокол или API библиотеки) помечается `!` после типа и строкой `BREAKING CHANGE:` в теле коммита: `feat(Я2)!: rename GameState.hand to hands`.

## Pull Request

- Один PR — одна задача (или её логическая часть).
- Заголовок PR оформляется по тем же правилам, что и коммит: `feat(Я4): add play card action`.
- В описании: что сделано, как проверить, ссылка на задачу в `PLAN.md`.
- Вливание через **Squash and merge**: в `main` попадает один аккуратный коммит с заголовком PR.
- Когда настроен CI (задачи А3, А4), PR вливается только при зелёной проверке.

## Версии

Библиотеки (`game-core`) версионируются по [семантическому версионированию](https://semver.org/lang/ru/): `MAJOR.MINOR.PATCH`.

- `PATCH` — исправление без изменения API;
- `MINOR` — новая функциональность, обратно совместимая;
- `MAJOR` — несовместимые изменения. До версии `1.0.0` несовместимые изменения допускаются в `MINOR`.

## Обязательные файлы в каждом репозитории

- `README.md`: что это, как собрать и запустить локально, как запустить тесты.
- `.gitignore`: под язык и инструменты репозитория, плюс `.idea/`, `.vscode/`, `.env`.
- `.gitattributes`: единые переводы строк (LF) независимо от ОС:

```
* text=auto eol=lf
*.cmd text eol=crlf
*.bat text eol=crlf
*.png binary
*.jpg binary
*.webp binary
*.mp3 binary
*.mp4 binary
*.jar binary
```

## Защита ветки `main`

В каждом репозитории: **Settings → Rules → Rulesets → New branch ruleset**.

- Name: `protect-main`, Enforcement status: **Active**.
- Target branches: **Include default branch**.
- Включить правила:
  - **Restrict deletions**;
  - **Block force pushes**;
  - **Require a pull request before merging**, Required approvals: **0** (разработчик один и не может одобрить собственный PR);
  - **Require status checks to pass**: добавляется после настройки CI (А3, А4).
- Bypass list оставить пустым.
