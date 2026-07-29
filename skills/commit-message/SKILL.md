---
name: commit-message
description: >-
  Write conventional commit messages that follow the 50/72 rule. Use when the
  user asks to commit, write a commit message, create a git commit, or apply
  conventional commits.
---

# Commit Message

Less is more. Prefer a subject-only message. Add a body only when the why is not obvious from the subject.

## Format

```
type(scope): subject

optional body wrapped at 72 columns
```

- **type** (required): `feat` | `fix` | `docs` | `style` | `refactor` | `perf` | `test` | `build` | `ci` | `chore` | `revert`
- **scope** (optional): short noun, e.g. `auth`, `api`
- **subject**: imperative mood, no trailing period, ≤ **50** characters
- **body** (optional): blank line after subject; wrap at **72**; explain why, not what

## Examples

```
feat(auth): add JWT login endpoint
```

```
fix(reports): use UTC for date display

Local timezone conversion caused off-by-one day errors in exports.
```

## When committing

1. Only commit when the user explicitly asks.
2. Pass the message via HEREDOC:

```bash
git commit -m "$(cat <<'EOF'
type(scope): subject

EOF
)"
```

3. Never use interactive git flags (`-i`).
4. Do not commit secrets (`.env`, credentials, keys).
5. Do not amend, force-push, or skip hooks unless the user explicitly requests it.
