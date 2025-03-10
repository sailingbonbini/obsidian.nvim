<h1 align="center">obsidian.nvim</h1>
<div><h4 align="center"><a href="#setup">Setup</a> · <a href="#configuration-options">Configure</a> · <a href="#contributing">Contribute</a> · <a href="https://github.com/obsidian-nvim/obsidian.nvim/discussions">Discuss</a></h4></div>
<div align="center"><a href="https://github.com/obsidian-nvim/obsidian.nvim/releases/latest"><img alt="Latest release" src="https://img.shields.io/github/v/release/obsidian-nvim/obsidian.nvim?style=for-the-badge&logo=starship&logoColor=D9E0EE&labelColor=302D41&&color=d9b3ff&include_prerelease&sort=semver" /></a> <a href="https://github.com/obsidian-nvim/obsidian.nvim/pulse"><img alt="Last commit" src="https://img.shields.io/github/last-commit/obsidian-nvim/obsidian.nvim?style=for-the-badge&logo=github&logoColor=D9E0EE&labelColor=302D41&color=9fdf9f"/></a> <a href="https://github.com/neovim/neovim/releases/latest"><img alt="Latest Neovim" src="https://img.shields.io/github/v/release/neovim/neovim?style=for-the-badge&logo=neovim&logoColor=D9E0EE&label=Neovim&labelColor=302D41&color=99d6ff&sort=semver" /></a> <a href="http://www.lua.org/"><img alt="Made with Lua" src="https://img.shields.io/badge/Built%20with%20Lua-grey?style=for-the-badge&logo=lua&logoColor=D9E0EE&label=Lua&labelColor=302D41&color=b3b3ff"></a> <a href="https://www.buymeacoffee.com/epwalsh"><img alt="Buy me a coffee" src="https://img.shields.io/badge/Buy%20me%20a%20coffee-grey?style=for-the-badge&logo=buymeacoffee&logoColor=D9E0EE&label=Sponsor&labelColor=302D41&color=ffff99" /></a></div>
<hr>

A Neovim plugin for writing and navigating [Obsidian](https://obsidian.md) vaults, written in Lua.

Built for people who love the concept of Obsidian -- a simple, markdown-based notes app -- but love Neovim too much to stand typing characters into anything else.

## Features 

▶️ **Completion:** Ultra-fast, asynchronous autocompletion for note references and tags via [nvim-cmp](https://github.com/hrsh7th/nvim-cmp) or [blink.cmp](https://github.com/Saghen/blink.cmp) 

🏃 **Navigation:** Navigate throughout your vault by typing `gf` on any link to another note.

📷 **Images:** Paste images into notes.

💅 **Syntax:** Additional markdown syntax highlighting, concealing, and extmarks for references, tags, and check-boxes.

## See it in action

<video width="640" height="360" controls>
  <source src="./doc/assets/obsidian-nvim.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

## Setup

### System requirements

- NeoVim >= 0.8.0 
- If you want completion and search features (recommended) you'll need [ripgrep](https://github.com/BurntSushi/ripgrep) to be installed and on your `$PATH`.

Specific operating systems also require additional dependencies in order to use all of obsidian.nvim's functionality:

- **Windows WSL** users need [`wsl-open`](https://gitlab.com/4U6U57/wsl-open) for the `:ObsidianOpen` command.
- **MacOS** users need [`pngpaste`](https://github.com/jcsalterego/pngpaste) (`brew install pngpaste`) for the `:ObsidianPasteImg` command.
- **Linux** users need xclip (X11) or wl-clipboard (Wayland) for the `:ObsidianPasteImg` command.

Search functionality (e.g. via the `:ObsidianSearch` and `:ObsidianQuickSwitch` commands) also requires a picker, such as:
- [telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)
- [mini.pick](https://github.com/echasnovski/mini.pick)
- [snacks.nvim](https://github.com/folke/snacks.nvim)

### Install and configure

To configure obsidian.nvim you just need to call `require("obsidian").setup({ ... })` with the desired options.

#### Using [`lazy.nvim`](https://github.com/folke/lazy.nvim)

<details><summary>...</summary>

```lua
return {
  "obsidian-nvim/obsidian.nvim",
  version = "*", -- recommended, use latest release instead of latest commit
  lazy = true,
  ft = "markdown",
  -- Replace the above line with this if you only want to load obsidian.nvim for markdown files in your vault:
  -- event = {
  --   -- If you want to use the home shortcut '~' here you need to call 'vim.fn.expand'.
  --   -- E.g. "BufReadPre " .. vim.fn.expand "~" .. "/my-vault/*.md"
  --   -- refer to `:h file-pattern` for more examples
  --   "BufReadPre path/to/my-vault/*.md",
  --   "BufNewFile path/to/my-vault/*.md",
  -- },
  dependencies = {
    -- Required.
    "nvim-lua/plenary.nvim",

    -- see below for full list of optional dependencies 👇
  },
  opts = {
    workspaces = {
      {
        name = "personal",
        path = "~/vaults/personal",
      },
      {
        name = "work",
        path = "~/vaults/work",
      },
    },

    -- see below for full list of options 👇
  },
}
```

</details>

#### Using [`rocks.nvim`](https://github.com/nvim-neorocks/rocks.nvim)

<details><summary>...</summary>

```vim
:Rocks install obsidian
```

</details>

#### Using [`packer.nvim`](https://github.com/wbthomason/packer.nvim)

It is not recommended because packer.nvim is currently unmaintained

<details><summary>...</summary>

```lua
use {
  "obsidian-nvim/obsidian.nvim",
  tag = "*", -- recommended, use latest release instead of latest commit
  requires = {
    -- Required.
    "nvim-lua/plenary.nvim",

    -- see below for full list of optional dependencies 👇
  },
  config = function()
    require("obsidian").setup {
      workspaces = {
        {
          name = "personal",
          path = "~/vaults/personal",
        },
        {
          name = "work",
          path = "~/vaults/work",
        },
      },

      -- see below for full list of options 👇
    }
  end,
}
```

</details>

### Plugin dependencies

The only **required** plugin dependency is [plenary.nvim](https://github.com/nvim-lua/plenary.nvim), but there are a number of optional dependencies that enhance the obsidian.nvim experience.

**Completion:**

- **[recommended]** [hrsh7th/nvim-cmp](https://github.com/hrsh7th/nvim-cmp)
- [blink.cmp](https://github.com/Saghen/blink.cmp) (new)

**Pickers:**

- **[recommended]** [nvim-telescope/telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)
- [ibhagwan/fzf-lua](https://github.com/ibhagwan/fzf-lua)
- [Mini.Pick](https://github.com/echasnovski/mini.pick) from the mini.nvim library
- [Snacks.Picker](https://github.com/folke/snacks.nvim/blob/main/docs/picker.md) from the snacks.nvim library

**Syntax highlighting:**

See [syntax highlighting](#syntax-highlighting) for more details.

- For base syntax highlighting:
  - **[recommended]** [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
  - [preservim/vim-markdown](https://github.com/preservim/vim-markdown)
- For additional syntax features:
  - [render-markdown.nvim](https://github.com/MeanderingProgrammer/render-markdown.nvim)
  - [markview.nvim](https://github.com/OXY2DEV/markview.nvim)

**Miscellaneous:**

- 🆕 [pomo.nvim](https://github.com/epwalsh/pomo.nvim): for running lightweight [pomodoro](https://en.wikipedia.org/wiki/Pomodoro_Technique) timers.

If you choose to use any of these you should include them in the "dependencies" or "requires" field of the obsidian.nvim plugin spec for your package manager.

## Contributing

Please read the [CONTRIBUTING](https://github.com/obsidian-nvim/obsidian.nvim/blob/main/.github/CONTRIBUTING.md) guide before submitting a pull request.
