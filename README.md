# Agent Skills

Reusable [Agent Skills](https://agentskills.io/) for Cursor and other coding agents.

## Skills

| Skill | Description |
| --- | --- |
| [`commit-message`](skills/commit-message/) | Conventional commits with the 50/72 rule. Confirm via Shell Run card (⌘↵); use Allowlist without sandbox. |
| [`tdd-test`](skills/tdd-test/) | Failing tests from product requirements. No mocks. Stop and ask before Green. |

## Install (global)

Install for Cursor across all projects:

```bash
npx skills add ploymahloy/skills -g -a cursor -y
```

Install only one skill:

```bash
npx skills add ploymahloy/skills --skill commit-message -g -a cursor -y
```

Replace `ploymahloy/skills` with this repository’s GitHub path (e.g. `your-user/skills`).

## Install (project-local)

From another repo’s root (without `-g`):

```bash
npx skills add ploymahloy/skills -a cursor -y
```

## Local development

Before the repo is on GitHub, symlink a skill into Cursor’s global skills directory:

```bash
mkdir -p ~/.cursor/skills
ln -sfn /path/to/skills/skills/commit-message ~/.cursor/skills/commit-message
```

## Layout

```
skills/
└── <skill-name>/
    └── SKILL.md
```

The [skills CLI](https://github.com/vercel-labs/skills) discovers skills under `skills/`.
