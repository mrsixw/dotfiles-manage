---
name: dotfiles-manage
description: Manage dotfiles tracked via the bare-repo pattern using the myconfig alias (git --git-dir=$HOME/.cfg/ --work-tree=$HOME). Use when the user mentions myconfig, asks to edit or commit dotfiles, or works with files tracked in $HOME such as ~/.zshrc, ~/.codex/AGENTS.md, ~/.gitconfig-*, or similar.
---

# Dotfiles Manage

Manage dotfiles tracked via the bare-repo pattern using the `myconfig` alias.

## Setup

The alias is defined in `~/.zshrc`:
```bash
alias myconfig='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
```

## Usage

Use `myconfig` instead of `git` for all dotfile operations:

```bash
myconfig status
myconfig diff
myconfig add <file>
myconfig commit -m "message"
myconfig push
```

## Known Tracked Files

Files typically tracked include:
- `~/.zshrc`, `~/.bashrc`
- `~/.gitconfig`, `~/.gitconfig-private`, `~/.gitconfig-public`
- `~/.codex/AGENTS.md`
- `~/.codex/skills/` (custom skills)

## Chaining

After editing any dotfile, stage and commit via `myconfig` when the user asks for a commit.
Apply the same git identity enforcement rules from AGENTS.md before committing.

## Autonomy

For dotfiles workflows, default to execution instead of confirmation prompts.

- Do not ask extra "proceed?" or "confirm?" questions for non-destructive dotfiles actions.
- If the user asks to commit, run the full commit workflow directly.
- If the user asks to push, push directly after confirming commit success.
- Use direct commands, not shell wrappers: `/usr/bin/git --git-dir=$HOME/.cfg --work-tree=$HOME <subcommand>`.
- Avoid multi-command shell chains for routine checks; run one command per call to maximize auto-approval matching.
- Ask for confirmation only for destructive/high-risk actions (for example force-push, history rewrite, hard reset, or deleting tracked files/branches).

## Guardrails

- Never use plain `git` for dotfile operations in `$HOME`
- Never run `git init` in `$HOME`
- Never expose the bare repo to normal git commands
- Respect commit message conventions (JIRA key prefix when applicable)
