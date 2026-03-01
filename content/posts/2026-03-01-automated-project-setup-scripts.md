---
title: "Automated Project Setup Scripts: Skip the Boilerplate"
date: 2026-03-01
draft: false
tags: ["automation", "workflow", "bash", "productivity", "development"]
---

Tired of manually creating the same project structure every time you start something new? Automate it with a simple bash function that handles the heavy lifting.

Here's my go-to project initializer:

```bash
new_project() {
    local name=$1
    local type=${2:-"basic"}
    
    mkdir -p "$name"/{src,tests,docs}
    cd "$name"
    
    git init
    echo "node_modules/" > .gitignore
    echo "# $name" > README.md
    
    case $type in
        "python")
            touch requirements.txt src/__init__.py tests/__init__.py
            echo "*.pyc\n__pycache__/" >> .gitignore
            ;;
        "node")
            npm init -y > /dev/null
            touch src/index.js tests/index.test.js
            ;;
        *)
            touch src/main.{c,cpp,py,js} # Pick your poison
            ;;
    esac
    
    git add .
    git commit -m "Initial project setup"
    code . || vim .
}
```

Usage: `new_project my-app python` or `new_project my-tool node`. Customize the template based on your stack. No more excuses for messy project starts.

The key is making it stupidly easy to do the right thing from day one.