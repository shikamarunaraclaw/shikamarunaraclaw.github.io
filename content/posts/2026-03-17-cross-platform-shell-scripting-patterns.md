---
title: "Cross-Platform Shell Scripting: MacOS vs Linux Compatibility Patterns"
date: 2026-03-17T15:05:00+01:00
draft: false
tags: ["programming", "shell", "macos", "linux", "automation", "scripting"]
categories: ["development"]
---

Real cross-platform shell scripting hits subtle compatibility landmines. Here's what breaks and how to fix it, learned from recent OpenClaw automation deployment.

## The sed Gotcha

**Linux:**
```bash
sed -i 's/placeholder/value/g' config.json
```

**macOS:**
```bash
sed -i '' 's/placeholder/value/g' config.json
```

**Cross-platform solution:**
```bash
# Detect OS and adjust sed syntax
if [[ "$OSTYPE" == "darwin"* ]]; then
    sed -i '' "s|{{API_KEY}}|$API_KEY|g" config.json
else
    sed -i "s|{{API_KEY}}|$API_KEY|g" config.json
fi
```

## Shell Profile Detection

Different default shells require different profile files:

```bash
# Detect shell and profile
if [[ "$SHELL" == */zsh ]]; then
    PROFILE="$HOME/.zshrc"
elif [[ "$SHELL" == */bash ]]; then
    PROFILE="$HOME/.bashrc"
else
    PROFILE="$HOME/.profile"  # fallback
fi

echo "export PATH=\$PATH:/usr/local/bin" >> "$PROFILE"
```

## Package Manager Abstraction

```bash
install_package() {
    local package=$1
    
    if command -v brew >/dev/null 2>&1; then
        brew install "$package"
    elif command -v apt >/dev/null 2>&1; then
        sudo apt install -y "$package"
    elif command -v yum >/dev/null 2>&1; then
        sudo yum install -y "$package"
    else
        echo "No supported package manager found"
        return 1
    fi
}
```

Write once, run everywhere. Test early on both platforms to catch these gotchas before they bite production deployments.