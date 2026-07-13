# memorg

> [🇷🇺 Русский](README.md) · 🇬🇧 English

**External memory for Claude Code.** Long-running work with your agent
survives context compaction, fresh sessions, and machine switches —
because it lives in plain markdown files next to your project, not in
the conversation context.

The repo is organized as a plugin marketplace. It currently ships one
plugin — **discussions** — with more to come.

## The problem

Every long dialogue with an agent hits the context limit: the session
gets compacted or starts over — and the agent forgets half the decisions
made, goes in circles, asks the same questions again. It hurts most in
long explorations: system design, research, content planning, legal
drafting — anything that takes dozens of rounds.

## The solution: the discussions plugin

Every big topic gets its own folder with two files:

- **WIP.md** — the living map: what's decided, what's open, what's been
  done. The agent keeps it updated as the conversation goes.
- **design.md** — the final document that matures toward closing.

Plus the automation that makes this a plugin rather than a habit:

- `/discussion-new` creates the structure **and writes an anchor into
  the project's CLAUDE.md** — the discussion map is guaranteed to be in
  context in every new session, no reliance on the agent remembering.
- Verification rules: the agent doesn't declare a question closed
  without re-checking the map; `/discussion-close` closes a topic only
  after a full review (are all decisions written down, is everything
  open either resolved or deliberately deferred).

After compaction or in a new session there is nothing to restore: the
agent re-enters the topic through the map and picks up where it left off.

## Installation

In Claude Code (terminal, desktop app, or IDE extension):

```
/plugin marketplace add SkyTger/memorg
/plugin install discussions@memorg
```

Or, if an agent does the setup — add to `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "memorg": {
      "source": { "source": "github", "repo": "SkyTger/memorg" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": { "discussions@memorg": true }
}
```

Restart the session after installing.

## Usage

1. Open your project folder in Claude Code (always the same one for a
   given topic).
2. Starting a big topic — `/discussion-new`. The agent asks for a name
   and sets up the structure.
3. Then just talk. The agent maintains the map.
4. When the topic is done — `/discussion-close`: completeness check,
   final document, archive.

Discussion files live in `discussions/` inside your project:

```
my-project/
├── CLAUDE.md                  ← anchor to active discussions (maintained automatically)
└── discussions/
    ├── INDEX.md               ← list of topics
    └── my-topic/
        ├── WIP.md             ← living map
        ├── design.md          ← final document (after closing)
        └── notes/             ← materials, raw inputs
```

It's plain markdown: human-readable, versioned with your project's git,
not locked to any tool.

Note: the skills' instruction text is currently in Russian — the agent
follows them regardless of your conversation language; an English
translation is planned.

## Limitations

- Works in Claude Code (CLI, desktop, IDE extensions). Other agentic
  tools (Codex, Gemini CLI, etc.) don't read Claude Code plugins —
  porting the concept to them is planned as separate adaptations. The
  discussion files themselves are portable today: they're just markdown.
- The cloud web version of Claude Code doesn't support plugins.

## License

[MIT](LICENSE)
