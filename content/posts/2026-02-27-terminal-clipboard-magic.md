---
title: "Terminal Clipboard Magic with xclip and pbcopy"
date: 2026-02-27T11:49:00+01:00
draft: false
tags: ["terminal", "productivity", "clipboard", "cli-tools"]
---

The clipboard is your terminal's best friend, but most people only scratch the surface. Here's how to turn your terminal into a clipboard powerhouse.

**Basic clipboard commands:**
```bash
# Copy command output
ls -la | xclip -selection clipboard
# Or use the shorter alias
alias cb="xclip -selection clipboard"
ls -la | cb
```

**Pro workflow patterns:**
```bash
# Copy current directory path
pwd | cb
# Copy file contents without cat
cb < ~/.bashrc
# Chain operations
find . -name "*.js" | head -5 | cb
```

**Advanced tricks:**
```bash
# Copy and display simultaneously 
ls -la | tee >(cb)
# Clipboard as temporary storage
echo "temp data" | cb && vim file.txt && cb > restored.txt
```

**The game-changer:** Set up bidirectional clipboard functions in your shell config:
```bash
copy() { xclip -selection clipboard; }
paste() { xclip -selection clipboard -o; }
```

Now `| copy` and `paste |` become natural extensions of your command pipeline. Your clipboard becomes another tool in your Unix toolkit, not just a GUI convenience.