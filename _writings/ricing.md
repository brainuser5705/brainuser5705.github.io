---
base-title: A Ricing Coobook for MacOS 
base-description: neovim, oh-my-zsh
time: Oct 22, 2025

draft: true
---

## How do I set up...
- [barebone Neovim setup?](#neovim)
- [a Nerdfont for Terminal?](#nerd-font)

<a href="neovim"></a>
# barebone Neovim setup?

This barebones Neovim setup has:
- *lazy.nim* plugin manager
- structured `plugins/` directory setup
- color scheme
- nvim-tree plugin (for demo on how to install a plugin)

## Step 1: lazy.nim 

Follow the installation guide, using the *Structured Setup* and set up your `plugins/` folder following `Usage > Structuring Your Plugins`. 

Your directory should look like the following:
```
/Users/ashliew/.config/nvim
├── init.lua
├── lazy-lock.json
├── lua
│   ├── brainuser5705
│   │   ├── lazy.lua
│   │   └── settings.lua
│   └── plugins                 // each file in this directory is for a plugin
│       ├── devicons.lua
│       ├── init.lua
│       ├── tree.lua
│       └── treesitter.lua
└── README.md
```

## Step 3: nvim-tree

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

This step requires a Nerd Font in order to properly display icons. See [Nerd Font](#nerd-Font).

---

<a href="nerd-font"></a>
# a Nerdfont for Terminal? 

Nerdfonts are often required for several Neovim plugins for proper display of icons.

1. Download a [**Nerd Fonts**](https://www.nerdfonts.com/font-downloads) on your system.
    - Alternatively, you can download all the fonts with `brew install font-hack-nerd-font`
2. Click on the downloaded font file to install it.
2. Go to *Terminal > Settings > Profiles > Font* and select a Nerd Font.

---

<a href="oh-my-zsh"></a>
