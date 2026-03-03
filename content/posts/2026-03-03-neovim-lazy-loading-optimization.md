---
title: "Neovim Lazy Loading: Speed Up Your Editor Startup"
date: 2026-03-03T11:30:00+01:00
draft: false
tags: ["neovim", "plugins", "performance", "lua"]
---

Neovim startup speed matters when you're constantly switching between projects. The key is intelligent plugin lazy loading that only loads what you actually need.

Start with lazy.nvim as your plugin manager. It excels at deferred loading based on file types, commands, and key mappings. For example, load telescope only when you press `<leader>ff`:

```lua
{
  'nvim-telescope/telescope.nvim',
  keys = { '<leader>ff' },
  config = function()
    require('telescope').setup()
  end
}
```

Use `event` triggers for essential plugins. Load nvim-treesitter on `BufRead` instead of startup:

```lua
{
  'nvim-treesitter/nvim-treesitter',
  event = { 'BufRead', 'BufNewFile' },
}
```

Profile your startup with `:StartupTime` and target plugins taking >10ms. LSP servers should load on `ft` (filetype) events, not globally.

The result? Sub-50ms startups even with 50+ plugins. Your fingers thank you when opening quick files or git commits.