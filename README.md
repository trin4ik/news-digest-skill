# news-digest skill

## Структура

```
news-digest/
  SKILL.md                          # основной файл скилла
  templates/
    item.md                         # шаблон одного элемента новости
    digest.md                       # шаблон обёртки дайджеста
    empty.md                        # используется, когда ничего не подошло
    scheduling-prompt.md            # шаблон промпта для внешних планировщиков
  defaults/
    settings.yaml                   # дефолтные настройки для свежей установки
  README.md                         # этот файл
```

## Установка

1. Положи папку `news-digest/` (целиком, **без** `README.md`, если хочешь чистую установку) в свою директорию скиллов.
2. При первом вызове скилл создаст `<workspace>/news-digest-data/` и запустит мастер настройки. Дефолтный `settings.yaml` и шаблоны копируются из папки скилла.

Минимум файлов для рабочей установки скилла:

```
news-digest/
  SKILL.md
  templates/{item,digest,empty,scheduling-prompt}.md
  defaults/settings.yaml
```

## Миграция с v1 (Trin)

Если у вас уже есть `news-digest-data/settings.yaml` со схемой версии 1:

1. Сделайте бэкап существующего `settings.yaml`.
2. У мигрированного конфига `init.complete: true`, поэтому скилл пропускает мастер и сразу переходит к сбору.
3. Существующие `seen.jsonl` и `source-notes.md` сохраняются как есть.
4. `feedback.jsonl` создастся при первом событии фидбека — действий не требуется.
5. Шаблоны: один раз скопируйте `templates/*.md` из папки скилла в `news-digest-data/templates/`. Редактируйте по вкусу.

Изменения с v1 на v2:

- Удалён топ-уровневый `schedule:` (скилл больше не управляет cron).
- Удалён `output.item_format` (рендеринг управляется шаблонами).
- Удалён `source_policy.use_configured_sources_as_hints_not_limits` (теперь всегда true).
- Добавлен `init` (состояние онбординга).
- Добавлен `retention.feedback_compaction_months`.
- Добавлены пер-темные `blocked_sources`, `boost_keywords`, `penalty_keywords`.
- Добавлен `feedback.jsonl` (append-only журнал фидбека).
- Добавлен `templates/` (рендеринг дайджеста).

## Расписание

Скилл никогда не регистрирует cron-задачи сам. Чтобы получать дайджесты по расписанию, попроси скилл «помоги настроить расписание» / «help me set up scheduling» — он отрендерит `templates/scheduling-prompt.md` с твоими слотами/временами и выдаст промпт для установки в твой собственный планировщик (OpenClaw cron, системный cron + CLI-агент и т.д.).
# news-digest-skill
