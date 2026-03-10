---
title: "Linux Power-User File Management: fd, fzf, and ripgrep"
date: 2026-03-10T10:30:00Z
draft: false
tags: ["linux", "cli", "productivity", "file-management", "power-user"]
---

The holy trinity of Linux file management goes beyond `find` and `grep`. Here's how modern tools revolutionize daily workflows.

**fd** replaces `find` with sane defaults and speed:
```bash
# Find all Python files modified in last week
fd -e py -t f --changed-within 1week

# Exclude common directories automatically
fd "config" --exclude node_modules --exclude .git
```

**ripgrep (rg)** makes searching instant:
```bash
# Search with context and file types
rg "function.*auth" -A 3 -B 2 --type js

# Multi-line regex search
rg -U "class.*\n.*constructor" --type ts
```

**fzf** ties everything together with fuzzy finding:
```bash
# Interactive file edit
vim $(fd -t f | fzf --preview 'bat --color=always {}')

# Process killer with preview
ps aux | fzf --bind 'enter:become(kill {2})'
```

**Pro workflow**: Combine all three with shell functions. Add to `.bashrc`:
```bash
ff() { fd -t f | fzf --preview 'bat --color=always {}' | xargs -r $EDITOR; }
```

These tools reduce cognitive load from remembering syntax to focusing on intent. Speed + discoverability = productivity multiplier.