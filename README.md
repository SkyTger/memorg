# memory-framework

Плагин для Claude Code: ведение длинных обсуждений так, чтобы контекст
переживал автокомпакты и новые сессии.

## Что внутри

- **/discussion-new** — заводит структурированное обсуждение: `WIP.md`
  (живая карта: что решено / что открыто / что сделано) + `design.md`
  (накапливаемый финал). Если обсуждение ведётся в рабочей папке — сам
  создаёт/дополняет `CLAUDE.md` с якорем на WIP, чтобы карта обсуждения
  автоматически была в контексте каждой сессии.
- **/discussion-close** — закрывает обсуждение: обязательная сверка
  полноты, финальный `design.md`, архивация, снятие якоря.

## Установка

Вариант для человека: в Claude Code выполнить

```
/plugin marketplace add SkyTger/claude-memory-framework
/plugin install memory-framework@skytger
```

Вариант для агента: добавить в `~/.claude/settings.json`

```json
{
  "extraKnownMarketplaces": {
    "skytger": {
      "source": { "source": "github", "repo": "SkyTger/claude-memory-framework" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": { "memory-framework@skytger": true }
}
```

## Как пользоваться

1. Открой свою рабочую папку в Claude Code (всегда одну и ту же для
   одной темы).
2. Новая большая тема — скажи `/discussion-new` (агент спросит имя).
3. Дальше просто разговаривай. Агент ведёт карту решений сам; после
   сбоя или новой сессии он вернётся в тему по карте.
4. Тема доведена до финала — `/discussion-close`.

Версия v1: только слой обсуждений. Слои памяти и knowledge-bank — позже.
