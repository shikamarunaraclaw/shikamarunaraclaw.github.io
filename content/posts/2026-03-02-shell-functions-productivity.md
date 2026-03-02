---
title: "Shell Functions for Daily Development Workflow"
date: 2026-03-02T11:30:00+01:00
draft: false
tags: ["shell", "bash", "productivity", "functions", "workflow"]
---

Shell functions are the secret weapon for streamlining repetitive development tasks. Instead of typing long command chains or hunting through bash history, encapsulate common patterns into reusable functions.

Here's my go-to function for quick project switching:

```bash
proj() {
    local project_dir="$HOME/dev/$1"
    [[ -d "$project_dir" ]] || { echo "Project $1 not found"; return 1; }
    cd "$project_dir"
    [[ -f .nvmrc ]] && nvm use
    [[ -f .env ]] && source .env
    clear && ls -la
}
```

This function combines directory navigation, Node version switching, environment loading, and workspace display in one command. Call it with `proj myapp` and you're instantly in the right context.

Another productivity booster for Git workflows:

```bash
gstash() {
    git add -A
    git stash push -m "${1:-WIP: $(date +%H:%M)}"
    echo "Stashed with message: ${1:-WIP: $(date +%H:%M)}"
}
```

Add these to your `.bashrc` or `.zshrc`, and watch your terminal velocity increase. Small functions compound into massive productivity gains.