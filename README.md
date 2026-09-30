# Dotfiles Manage Skill

Manage dotfiles in $HOME tracked by the bare repo at ~/.cfg (the myconfig alias, git --git-dir=$HOME/.cfg/ --work-tree=$HOME). Use when the user mentions myconfig, or asks to edit, stage, commit, or push tracked $HOME files such as ~/.zshrc, ~/.gitconfig-*, agent harness instruction files, or agent skill symlinks. Not for editing skill content in a skill that is its own git repo (commit in that repo) or for creating and wiring new skills.

## Purpose

This repository contains the `dotfiles-manage` agent skill. The canonical agent instructions live in [`SKILL.md`](SKILL.md).

## Contents

- `SKILL.md`: Skill metadata and agent workflow instructions.
- `agents/openai.yaml`: OpenAI/Codex UI metadata for this skill.

## Dependencies

- MCP dependencies: None detected.

## Usage

Install or link this repository as a skill directory for an agent that supports `SKILL.md` based skills.
