---
name: dotfiles-manage
description: Manage dotfiles in $HOME tracked by the bare repo at ~/.cfg (the myconfig alias, git --git-dir=$HOME/.cfg/ --work-tree=$HOME). Use when the user mentions myconfig, or asks to edit, stage, commit, or push tracked $HOME files such as ~/.zshrc, ~/.gitconfig-*, harness instruction files (~/.claude/CLAUDE.md, ~/.codex/AGENTS.md, ~/.copilot/copilot-instructions.md), ~/.agents/.skill-lock.json, or agent skill symlinks. Not for editing skill content in a ~/.agents/skills/* skill repo (each is its own git repo; use git-commit there) or creating and wiring new skills (use add-skill).
---

# Dotfiles Manage

Manage dotfiles tracked via the bare-repo pattern using the `myconfig` alias.
Shared working rules come from the `agent-policy` skill; this skill only adds
what is specific to the `$HOME` bare repo.

## Setup

The alias is defined in `~/.zshrc`:

```bash
alias myconfig='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
```

Aliases are not available in non-interactive agent shells. Use the explicit
command instead:

```bash
/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME <subcommand>
```

**zsh gotcha:** do not store the command in a variable (`CFG="/usr/bin/git
--git-dir=…"; $CFG status`). zsh does not word-split unquoted `$VAR`, so it
tries to run a single program literally named `/usr/bin/git --git-dir=…` and
fails with "no such file or directory". Use the explicit command above, or a
shell function:

```zsh
myconfig() { /usr/bin/git --git-dir="$HOME/.cfg/" --work-tree="$HOME" "$@"; }
```

## Usage

```bash
myconfig status
myconfig diff -- <path>
myconfig add -- <path>...
myconfig commit -m "message"
myconfig push
```

## Known Tracked Files

Check with `myconfig ls-files` (run from `$HOME`, or paths are shown relative
to the current directory). Tracked files include:

- Shell: `~/.zshrc`, `~/.zshenv`, `~/.p10k.zsh`, `~/.quadra_aliases`
- Git: `~/.gitconfig`, `~/.gitconfig-private`, `~/.gitconfig-public`
- Harness instruction files: `~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`,
  `~/.copilot/copilot-instructions.md`
- Agent config: `~/.codex/config.toml`
- Shared skill lock: `~/.agents/.skill-lock.json`
- Agent skill symlink directories: `~/.claude/skills/`, `~/.codex/skills/`,
  `~/.copilot/skills/` — the entries are symlinks to
  `~/.agents/skills/<name>` (see the `agent-policy` shared skill layout rule).
  `~/.gemini/antigravity-cli/skills/` holds the same symlinks but is not
  currently tracked.
- Recovery tooling: `~/.config/myconfig/`

Skill content itself is **not** tracked here: each `~/.agents/skills/<name>`
is its own git repo. Commit skill changes in that repo, not via `myconfig`.

## Harness Instruction Files

`~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`, and
`~/.copilot/copilot-instructions.md` are thin pointers to the `agent-policy`
skill. Change working rules in `~/.agents/skills/agent-policy` (its own repo),
not in the harness files. Edit a harness file only to change the pointer or
something genuinely harness-specific, then commit it via `myconfig`.

## Committing

- Follow `agent-policy` (`references/git.md`) and the `git-commit` skill for
  commit identity, message style, and the `Co-Authored-By:` trailer. If no
  identity is configured, use a one-off `git -c user.name=… -c user.email=…`;
  never write git config to fix it.
- The bare repo usually has unrelated, pre-existing modifications. Stage
  **explicit paths only** (`myconfig add -- <path>`). Never use `add -A`,
  `add .`, `add -u`, or `commit -a`, and never stage a file you did not change
  in this task. Check `myconfig diff --cached --stat` before committing.
- Respect commit message conventions (Jira key prefix when applicable).

## Autonomy

Within the `agent-policy` trusted paths (`~/.agents/`, `~/.claude/`,
`~/.codex/`, `~/.copilot/`):

- Non-destructive edits, staging your own changes, and committing when asked
  need no extra confirmation.
- Pushing the bare repo's current branch normally, when the user asks, needs
  no extra confirmation after the commit succeeds.

Always confirm first, per `agent-policy` (`references/git.md`,
`references/guardrails.md`):

- Any git config change, in any scope, including `~/.gitconfig*` edits that
  change git behaviour.
- Force pushes, history rewrites, `reset --hard`, `clean`, and deleting
  tracked files or branches.
- Edits to other dotfiles outside the trusted paths (`~/.zshrc`, `~/.ssh/config`,
  …): summarise the change and confirm.

Run one command per call for routine checks so auto-approval patterns match.

## Guardrails

- Never use plain `git` for dotfile operations in `$HOME`.
- Never run `git init` in `$HOME`.
- Never expose the bare repo to normal git commands (for example by setting
  `GIT_DIR` globally or creating `$HOME/.git`).
