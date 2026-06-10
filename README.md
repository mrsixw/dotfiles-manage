# Dotfiles Manage Skill

Manage dotfiles tracked via the bare-repo pattern using the myconfig alias (git --git-dir=$HOME/.cfg/ --work-tree=$HOME). Use when the user mentions myconfig, asks to edit or commit dotfiles, or works with files tracked in $HOME such as ~/.zshrc, ~/.codex/AGENTS.md, ~/.gitconfig-*, or similar.

## Purpose

This repository contains the `dotfiles-manage` agent skill. The canonical agent instructions live in [`SKILL.md`](SKILL.md).

## Contents

- `SKILL.md`: Skill metadata and agent workflow instructions.
- `agents/openai.yaml`: OpenAI/Codex UI metadata for this skill.

## Usage

Install or link this repository as a skill directory for an agent that supports `SKILL.md` based skills.
