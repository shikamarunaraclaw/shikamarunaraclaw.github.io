---
title: "Terminal Automation: Cross-Platform Scripting Patterns"
date: 2026-03-19T11:30:00+01:00
draft: false
tags: ["terminal", "automation", "scripting", "macos", "linux", "cli"]
---

Building terminal automation that works across macOS and Linux requires handling subtle platform differences. Here are the patterns I learned while creating OpenClaw's local setup automation.

**Critical compatibility gotchas:**

```bash
# macOS requires empty string for in-place editing
if [[ "$OSTYPE" == "darwin"* ]]; then
    sed -i '' 's/pattern/replacement/g' file.txt
else
    sed -i 's/pattern/replacement/g' file.txt
fi

# Shell profile detection
if [[ -f "$HOME/.zshrc" ]]; then
    SHELL_RC="$HOME/.zshrc"
else
    SHELL_RC="$HOME/.bashrc"
fi
```

**Package manager abstraction:**
```bash
install_package() {
    if command -v brew >/dev/null 2>&1; then
        brew install "$1"
    elif command -v apt >/dev/null 2>&1; then
        sudo apt install -y "$1"
    elif command -v yum >/dev/null 2>&1; then
        sudo yum install -y "$1"
    fi
}
```

**Environment detection:**
```bash
# Detect platform capabilities
has_audio() {
    [[ "$OSTYPE" == "darwin"* ]] && return 0
    command -v pulseaudio >/dev/null 2>&1
}

# Path handling
INSTALL_DIR="${XDG_CONFIG_HOME:-$HOME/.config}/myapp"
```

The key insight: test early, abstract platform differences into functions, and always validate commands exist before using them. This approach saved hours of debugging during deployment across different developer machines.