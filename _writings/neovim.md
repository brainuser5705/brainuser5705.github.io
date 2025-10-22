---
base-title: Setting up my Neovim configs
base-description: Figuring out neovim, a walkthrough of what I did
time: Oct 22, 2025

draft: true
---

# Installation

# Plugins

## Step 1: Plugin Manager

To add plugins to Neovim, you need a plugin manager. I am using *lazy.nim*.

Follow the installation guide, using the *Structured Setup* and set up your `plugins/` folder following `Usage > Structuring Your Plugins`. 

## nvim-tree

Add the plugin module to `plugins/`. 
```lua
return {
  {
    "nvim-tree/nvim-tree.lua",
    version = "*",
    lazy = false,
    dependencies = {
      "nvim-tree/nvim-web-devicons",
    },
    config = function ()
      require("nvim-tree").setup({
        sort = { 
          sorter = "case_sensitive" 
        }
      })
    end
  }
}
```

1. Install Nerd Fonts on your system.
2. Change the font through the terminal.
3. Learn the commands.



