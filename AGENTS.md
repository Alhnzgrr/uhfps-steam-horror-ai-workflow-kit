# UHFPS Steam Horror AI Workflow Kit Repository

## Repository Role

This repository is the source of truth for a UHFPS + Zenject AI workflow kit.

It stores reusable workflow content for Steam horror Unity projects. It does not
replace UHFPS, Zenject, or project-specific game code.

## Source Layout

```text
kit/
  AGENTS.md
  agents/
  commands/
  rules/
  skills/
  templates/
  workflows/
```

## Editing Rules

- Treat `kit/` as the canonical installable workflow content.
- Keep rules specific to UHFPS, Zenject, Steam PC horror, and production Unity work.
- Do not add mobile or hybrid-casual assumptions unless a project explicitly asks for them.
- Do not store real project runtime artifacts in this repository.
- Prefer small, role-specific files over one large manifesto.

## Intended Use

Install or copy `kit/` into a Unity project and point the AI assistant at
`kit/AGENTS.md`.

The assistant should load only the files needed for the task.

## Session Loading

Do not reload kit files repeatedly in the same AI session.

At session start, load `kit/AGENTS.md` once. After that, reuse already loaded
agent, rule, and skill context unless the task scope changes, the user says the
kit changed, or a new file is required.
