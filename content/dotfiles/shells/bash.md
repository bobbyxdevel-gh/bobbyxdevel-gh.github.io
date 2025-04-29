---
title: "Bash Configuration"
date: 2025-04-29
draft: false
---

# My Bash configuration files and setup.

## `~/.bashrc`

```bash
#
# ~/.bashrc
#

# If not running interactively, don't do anything
[[ $- != *i* ]] && return

alias ls='eza --group-directories-first --color=auto'
alias grep='grep --color=auto'
PS1='[\u@\h \W]\$ '

alias ff='fastfetch --config /usr/share/fastfetch/presets/examples/10.jsonc'
```

## `~/.bash_profile`

```bash
#
# ~/.bash_profile
#

[[ -f ~/.bashrc ]] && . ~/.bashrc
```
