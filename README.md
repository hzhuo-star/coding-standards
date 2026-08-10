# Coding standards

The [`coding-standards/`](coding-standards/) directory preserves the policy language and examples from Dillon Mulroy's [`coding-standards` skill](https://github.com/dmmulroy/skills/blob/main/coding-standards/SKILL.md), reorganized only for progressive disclosure.

The root `SKILL.md` contains the original description, decision priority, core principles, and existing-code adoption policy. Every remaining upstream top-level section lives unchanged in its own reference file and loads only when relevant.

## Adopt in a repository

Copy `coding-standards/` into the repository—for example at `.agent-context/coding-standards/`—and add one line to the nearest `AGENTS.md`:

```md
For TypeScript engineering, read `.agent-context/coding-standards/SKILL.md` and every reference it routes to for the affected behavior.
```

## Install as a native skill

Copy the same `coding-standards/` directory to the host's project or user skill location:

- Codex project: `.agents/skills/coding-standards/`
- Codex user: `~/.agents/skills/coding-standards/`
- Claude Code project: `.claude/skills/coding-standards/`
- Claude Code user: `~/.claude/skills/coding-standards/`

Native installation and the `AGENTS.md` pointer are alternatives; using both is unnecessary.

See [`NOTICE.md`](NOTICE.md) for the pinned upstream revision and license provenance.
