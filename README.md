# gh-actions

Переиспользуемые composite-экшены GitHub Actions для проектов автоматизации 1С.

Репозиторий подключается по пути `<owner>/<repo>/<action>@<ref>`, например:

```yaml
- uses: LevkinSergey/gh-actions/install-oscript@v1
```

## Доступные экшены

| Экшен | Назначение |
|-------|------------|
| [`install-oscript`](install-oscript/action.yml) | Установка независимого дистрибутива OneScript (Linux/Windows) и базовых пакетов `opm` |
| [`extract-version`](extract-version/action.yml) | Извлечение версии конфигурации из `Configuration/Configuration.mdo` |
| [`load-extension`](load-extension/action.yml) | Загрузка расширения в информационную базу через `ibcmd` |

## install-oscript

```yaml
- name: Установка oscript
  uses: LevkinSergey/gh-actions/install-oscript@v1
  with:
    version: '2.2.0'
```

Входы:

- `version` — версия OneScript (по умолчанию `2.2.0`).

Что делает:

- скачивает и распаковывает дистрибутив OneScript в `$HOME/oscript` (Linux) или `%USERPROFILE%\oscript` (Windows);
- добавляет каталог `bin` в `GITHUB_PATH`;
- выставляет права на исполнение для `oscript` и `opm` (Linux);
- устанавливает пакеты `cli fs logos tempfiles v8runner v8find 1commands 1connector` через `opm`.

## extract-version

```yaml
- name: Извлечение версии проекта
  id: version
  uses: LevkinSergey/gh-actions/extract-version@v1
  with:
    path: ./src/Инструменты/src

- run: echo "Версия ${{ steps.version.outputs.version }}"
```

Входы:

- `path` — путь к каталогу `src` конфигурации (обязательный).

Выходы:

- `version` — версия из `Configuration/Configuration.mdo`.

## load-extension

```yaml
- name: Загрузка расширения
  uses: LevkinSergey/gh-actions/load-extension@v1
  with:
    name: МоеРасширение
    path: ./build/МоеРасширение.cfe
    dbpath: ./build/ib
```

Входы:

- `name` — имя расширения (обязательный);
- `path` — путь к файлу расширения (обязательный);
- `dbpath` — путь к файловой информационной базе (обязательный).

## Версионирование

Релизы публикуются тегами (`v1`, `v1.0.0`). Для стабильных пайплайнов используйте мажорный тег `@v1`,
для максимальной воспроизводимости — пиннинг на конкретный коммит (`@<sha>`).
