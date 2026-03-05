---
title: "Terminal Productivity: ripgrep and Modern Search Tools"
date: 2026-03-05T11:30:00+01:00
draft: false
tags: ["terminal", "cli", "productivity", "search", "tools"]
---

Traditional `grep` is fine for basic searches, but modern alternatives like `ripgrep` (`rg`) transform how you navigate codebases. Here's why I've completely replaced `grep` in my daily workflow.

**ripgrep advantages:**
- **Speed**: 10-50x faster than grep on large codebases
- **Smart defaults**: Ignores `.gitignore` patterns automatically
- **Better output**: Color-coded, line numbers, and context by default
- **Unicode support**: Handles modern text files properly

**Essential ripgrep patterns:**

```bash
# Search with context lines
rg "function.*auth" -A 3 -B 2

# Search specific file types
rg "TODO" -t js -t py

# Case insensitive with whole words
rg -i -w "config"

# Search and replace preview
rg "oldPattern" --replace "newPattern" --dry-run
```

**Combine with other tools:**
```bash
# Find and edit files containing pattern
rg -l "errorHandler" | xargs nvim

# Count occurrences per file
rg "import.*react" --count-matches
```

The speed difference is most noticeable in large repositories. Where `grep` takes 30 seconds, `rg` finishes in under 2 seconds. Combined with `fzf` for fuzzy finding, it creates an incredibly fast code exploration workflow.