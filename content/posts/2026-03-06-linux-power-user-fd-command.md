---
title: "Master File Finding with fd: The Modern find Alternative"
date: 2026-03-06T11:30:00+01:00
draft: false
tags: ["linux", "cli", "productivity", "tools", "fd", "file-management"]
---

The traditional `find` command is powerful but verbose. Enter `fd` - a simple, fast, and user-friendly alternative that respects your .gitignore files by default.

## Key Advantages

**Speed**: Written in Rust, `fd` is significantly faster than `find` for most operations.

**Smart defaults**: Automatically ignores hidden files, .git directories, and respects .gitignore patterns.

**Intuitive syntax**: `fd pattern` instead of `find . -name "*pattern*"`.

## Essential Usage Patterns

```bash
# Find files containing "config"
fd config

# Search specific file types
fd -e js -e ts  # JavaScript/TypeScript files
fd . --type f --extension md  # Markdown files only

# Include hidden files when needed
fd -H pattern

# Search in specific directory
fd pattern ~/projects

# Execute commands on results
fd -e py -x python3 -m py_compile {}
```

The `-x` flag executes commands on each result, with `{}` as the placeholder for the found file.

Install via package manager: `apt install fd-find` (Ubuntu) or `brew install fd` (macOS). 

Your file searching just became exponentially more efficient.