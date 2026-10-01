---
name: dotfiles-manage
description: Manage dotfiles in $HOME tracked by the bare repo at ~/.cfg (the myconfig alias, git --git-dir=$HOME/.cfg/ --work-tree=$HOME). Use when the user mentions myconfig, or asks to edit, stage, commit, or push tracked $HOME files such as ~/.zshrc, ~/.gitconfig-*, agent harness instruction files, or agent skill symlinks. Not for editing skill content in a skill that is its own git repo (commit in that repo) or for creating and wiring new skills.
---

# Dotfiles Manage

Manage dotfiles tracked via the bare-repo pattern using the `myconfig` alias.
Follow the user's standing instructions for commit, push, and safety rules;
this skill only adds what is specific to the `$HOME` bare repo.

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

## Tracked Files

Check with `myconfig ls-files` (run from `$HOME`, or paths are shown relative
to the current directory). Typical tracked files:

- Shell: `~/.zshrc`, `~/.zshenv`, `~/.p10k.zsh`
- Git: `~/.gitconfig` and any included `~/.gitconfig-*` files
- Agent harness instruction files (for example `~/.claude/CLAUDE.md`,
  `~/.codex/AGENTS.md`, `~/.copilot/copilot-instructions.md`)
- Agent skill directories whose entries are symlinks to a shared skills
  directory

When a skill is its own git repo, its content is **not** tracked here. Commit
skill changes in that repo, not via `myconfig`.

## Harness Instruction Files

If the harness instruction files only point to a shared rules source (a rules
skill or shared document), change the rules there, not in the harness files.
Edit a harness file only to change the pointer or something genuinely
harness-specific.

## Committing

- Use the repository's existing commit identity and message conventions. If
  no identity is configured, use a one-off `git -c user.name=… -c
  user.email=…`; never write git config to fix it.
- The bare repo usually has unrelated, pre-existing modifications. Stage
  **explicit paths only** (`myconfig add -- <path>`). Never use `add -A`,
  `add .`, `add -u`, or `commit -a`, and never stage a file you did not change
  in this task. Check `myconfig diff --cached --stat` before committing.

## Autonomy

- Non-destructive edits to paths the user has marked as trusted, staging your
  own changes, and committing when asked need no extra confirmation.
- Pushing the current branch normally, when the user asks, needs no extra
  confirmation after the commit succeeds.

Always confirm first:

- Any git config change, in any scope, including `~/.gitconfig*` edits that
  change git behaviour.
- Force pushes, history rewrites, `reset --hard`, `clean`, and deleting
  tracked files or branches.
- Edits to other dotfiles (`~/.zshrc`, `~/.ssh/config`, …): summarise the
  change and confirm.

Run one command per call for routine checks so auto-approval patterns match.

## Guardrails

- Never use plain `git` for dotfile operations in `$HOME`.
- Never run `git init` in `$HOME`.
- Never expose the bare repo to normal git commands (for example by setting
  `GIT_DIR` globally or creating `$HOME/.git`).
