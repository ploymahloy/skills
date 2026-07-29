---
name: commit-message
description: >-
  Write conventional commit messages that follow the 50/72 rule. Use when the
  user asks to commit, write a commit message, create a git commit, or apply
  conventional commits. Never commit or push until the user accepts the Shell
  Run / Accept card (⌘↵ / Enter)—not AskQuestion, not typed yes. Requires
  Cursor Run Mode Allowlist without sandbox so the card is not skipped.
---

# Commit Message

Less is more. Prefer a subject-only message. Add a body only when the why is not obvious from the subject.

## Format (STRICT)

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

## Required Cursor settings

The Shell Run card is only a real confirmation gate when:

- **Run Mode** = **Allowlist** (not Auto-review, not Run Everything)
- **Sandbox** = **off** (not “Allowlist with Sandbox”)
- Command Allowlist does **not** include `git commit`, `git push`, or broad `git`

Auto-review + Sandbox can auto-run `git commit` with no card. If those settings are active (or `git commit` is allowlisted), **do not** call Shell for a git write—stop, show the drafted message, and tell the user to switch to Allowlist without sandbox (or confirm in a follow-up chat message before any write).

## Workflow

1. Inspect with read-only git only (`git status`, `git diff`, recent log style).
2. Draft the conventional commit message and show it clearly in the reply.
3. If settings would auto-run the commit (see above): **STOP**. Do not call Shell for write. Ask the user to fix Run Mode or confirm in chat first.
4. Otherwise, in the **same turn**, run `git commit` via Shell with the HEREDOC message (request `git_write` / needed permissions). Do **not** wait for AskQuestion or a typed “yes”.
5. Confirmation **is** the user accepting the Shell **Run / Accept** card (**⌘↵** / Enter). If they reject or dismiss, leave the working tree untouched.
6. Run `git push` only if the user asked to push; that Shell call gets its own Run card.

## Hard rules

- Confirmation **is** the Shell/`git_write` Run card when Allowlist + no sandbox. Treat accept as yes; treat reject/dismiss as no.
- **Never** call Shell for `git commit` / `git push` / other git writes if Auto-review + sandbox or an allowlisted commit would skip the card.
- **Never** call AskQuestion for commit confirmation.
- **Never** ask the user to type `Commit` or `yes` when the Run card is the intended gate.
- **Never** fake a button with markdown, code fences, or prose.
- Do not commit secrets (`.env`, credentials, keys).
- Never use interactive git flags (`-i`).
- Do not amend, force-push, or skip hooks unless the user explicitly requests it.
- Never push unless the user asked to push **and** accepted the push Run card.

## When committing

Pass the message via HEREDOC:

```bash
git commit -m "$(cat <<'EOF'
type(scope): subject

EOF
)"
```
