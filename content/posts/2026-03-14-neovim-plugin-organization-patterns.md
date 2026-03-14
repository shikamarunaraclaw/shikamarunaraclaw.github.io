---
title: "Neovim Plugin Organization: The Modular Config Approach"
date: 2026-03-14T11:30:00+01:00
draft: false
tags: ["neovim", "lua", "plugins", "configuration", "lazy.nvim"]
---

After managing hundreds of Neovim plugins across multiple projects, I've learned that config organization matters more than the plugins themselves. Here's my current approach that scales.

**Separate concerns, not plugins.** Instead of cramming everything into `init.lua`, I organize by functionality:

```lua
-- ~/.config/nvim/lua/config/
├── editor.lua      -- core editing (treesitter, surround)
├── navigation.lua  -- telescope, oil.nvim, harpoon
├── lsp.lua         -- language servers, completion
├── ui.lua          -- statusline, themes, decorations
└── tools.lua       -- git, debugging, formatting
```

**Plugin specs follow the same pattern.** Each file returns a lazy.nvim spec:

```lua
-- navigation.lua
return {
  {
    'nvim-telescope/telescope.nvim',
    dependencies = {'nvim-lua/plenary.nvim'},
    config = function()
      require('telescope').setup({
        defaults = {file_ignore_patterns = {'node_modules', '.git'}},
      })
    end,
    keys = {
      {'<leader>ff', '<cmd>Telescope find_files<cr>'},
      {'<leader>fg', '<cmd>Telescope live_grep<cr>'},
    },
  }
}
```

**Load conditionally, not universally.** Use lazy.nvim's conditional loading to keep startup fast. Load LSP configs only for relevant filetypes, load debugging tools only when needed.

The result? Clean separation, faster startup, and configs that actually make sense six months later when you need to debug something.