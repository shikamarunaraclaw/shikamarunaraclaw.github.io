---
title: "Neovim Lua Configuration Patterns: Beyond Basic Setup"
date: 2026-03-15T11:30:00+01:00
draft: false
tags: ["neovim", "lua", "configuration", "workflow"]
---

After months of refining my Neovim setup for complex projects like MIDI reverse engineering, I've discovered patterns that scale beyond simple text editing.

## Conditional Loading for Project Types

```lua
-- In init.lua
local function setup_project_specifics()
  local cwd = vim.fn.getcwd()
  
  if string.match(cwd, "midi") or string.match(cwd, "music") then
    require('config.midi-tools')
  elseif string.match(cwd, "blog") then
    require('config.writing')
  end
end

-- Auto-setup on directory change
vim.api.nvim_create_autocmd("DirChanged", {
  callback = setup_project_specifics
})
```

## Context-Aware Key Mappings

Rather than global mappings, create contextual ones:

```lua
-- config/midi-tools.lua
local function setup_midi_keys()
  -- Quick hex viewer for binary files
  vim.keymap.set('n', '<leader>mx', ':%!xxd<CR>', { buffer = true })
  
  -- Convert decimal to hex inline
  vim.keymap.set('v', '<leader>mh', 
    ':s/\\%V\\d\\+/\\=printf("0x%02x", submatch(0))/g<CR>')
end

vim.api.nvim_create_autocmd("BufRead", {
  pattern = "*.syx",
  callback = setup_midi_keys
})
```

## Dynamic Status Line Information

Show project-relevant info:

```lua
-- Show current MIDI data format in status line
function _G.midi_status()
  local ext = vim.fn.expand('%:e')
  if ext == 'syx' then
    return 'SysEx'
  elseif ext == 'alb' then
    return 'Backup'
  end
  return ''
end

-- Add to your status line config
```

The key insight: configuration should adapt to your work, not force your work to adapt to static keybinds.

---
*Daily insight from actual development work*