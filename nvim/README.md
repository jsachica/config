# Neovim Configuration
This is the most up to date personal configuration for Neovim on OSX.

## File Structure
```
~/.config/nvim/
├── init.lua                          # Main configuration file (everything is here)
├── lua/
│   ├── kickstart/
│   │   ├── health.lua               # Health check for kickstart
│   │   └── plugins/                 # Optional kickstart plugins (commented out by default)
│   │       ├── autopairs.lua        # Auto-close brackets, quotes
│   │       ├── debug.lua            # DAP debugging support
│   │       ├── gitsigns.lua         # Enhanced git integration
│   │       ├── indent_line.lua      # Show indent guides
│   │       ├── lint.lua             # Linting support
│   │       └── neo-tree.lua         # File explorer tree
│   └── custom/
│       └── plugins/
│           └── init.lua             # Your custom plugins go here
└── lazy-lock.json                   # Plugin version lockfile
```

## Core Settings (`init.lua`)

### Leader Keys

```lua
vim.g.mapleader = ' '
vim.g.maplocalleader = ' '
```

**Leader key** is a prefix for custom keybindings. Set to Space for easy access. Must be configured before plugins load.

```lua
vim.g.have_nerd_font = false
```

Set to `true` if you have a [Nerd Font](https://www.nerdfonts.com/) installed. Enables icons throughout the UI.

### Options

#### Line Numbers

```lua
vim.o.number = true
-- vim.o.relativenumber = true  -- Uncomment for relative line numbers
```

- `number`: Show absolute line numbers on the left
- `relativenumber`: Show distance from current line (useful for Vim motions like `5j`)

#### Mouse & Mode Display

```lua
vim.o.mouse = 'a'
vim.o.showmode = false
```

- `mouse = 'a'`: Enable mouse in all modes (useful for resizing splits)
- `showmode = false`: Don't show mode in command line (statusline already shows it)

#### Clipboard

```lua
vim.schedule(function()
  vim.o.clipboard = 'unnamedplus'
end)
```

Syncs Neovim's clipboard with system clipboard. Scheduled after UI loads to avoid startup delay.

#### Indentation & Formatting

```lua
vim.o.breakindent = true
```

Wrapped lines maintain indentation level for readability.

#### Undo History

```lua
vim.o.undofile = true
```

Saves undo history to file. You can undo changes even after closing and reopening a file.

#### Search

```lua
vim.o.ignorecase = true
vim.o.smartcase = true
```

- `ignorecase`: Search is case-insensitive by default
- `smartcase`: If search contains uppercase, becomes case-sensitive

#### UI

```lua
vim.o.signcolumn = 'yes'
```

Always show sign column (left of line numbers) for git signs, LSP diagnostics, etc. Prevents text shifting.

```lua
vim.o.updatetime = 250
```

Milliseconds of idle time before triggering CursorHold event (affects LSP hover, diagnostics).

```lua
vim.o.timeoutlen = 300
```

Milliseconds to wait for key sequence to complete (e.g., waiting after `<leader>` for next key).

#### Splits

```lua
vim.o.splitright = true
vim.o.splitbelow = true
```

New splits open to the right and below (more intuitive than default).

#### Whitespace Characters

```lua
vim.o.list = true
vim.opt.listchars = { tab = '» ', trail = '·', nbsp = '␣' }
```

Shows visual indicators for tabs, trailing spaces, and non-breaking spaces.

#### Live Substitution Preview

```lua
vim.o.inccommand = 'split'
```

Shows live preview of substitution results in split window as you type (`:s/foo/bar/`).

#### Cursor Line

```lua
vim.o.cursorline = true
```

Highlights the line your cursor is on.

#### Scroll Context

```lua
vim.o.scrolloff = 10
```

Keeps 10 lines visible above/below cursor when scrolling (prevents cursor at screen edge).

#### Unsaved Changes

```lua
vim.o.confirm = true
```

Shows dialog to save when trying to quit with unsaved changes (instead of showing error).

---

## Keymaps

### Clear Search Highlight

```lua
vim.keymap.set('n', '<Esc>', '<cmd>nohlsearch<CR>')
```

Press `Esc` in normal mode to clear search highlighting.

### Diagnostics

```lua
vim.keymap.set('n', '<leader>q', vim.diagnostic.setloclist)
```

`<leader>q` opens diagnostic quickfix list (errors/warnings from LSP).

### Terminal Mode

```lua
vim.keymap.set('t', '<Esc><Esc>', '<C-\\><C-n>')
```

Press `Esc` twice to exit terminal mode.

### Window Navigation

```lua
vim.keymap.set('n', '<C-h>', '<C-w><C-h>')  -- Move to left window
vim.keymap.set('n', '<C-l>', '<C-w><C-l>')  -- Move to right window
vim.keymap.set('n', '<C-j>', '<C-w><C-j>')  -- Move to bottom window
vim.keymap.set('n', '<C-k>', '<C-w><C-k>')  -- Move to top window
```

Use `Ctrl+hjkl` to navigate between split windows.

---

## Autocommands

### Highlight on Yank

```lua
vim.api.nvim_create_autocmd('TextYankPost', {
  callback = function()
    vim.hl.on_yank()
  end,
})
```

Briefly highlights yanked (copied) text for visual feedback.

---

## Plugins

All plugins are managed by [lazy.nvim](https://github.com/folke/lazy.nvim) - a modern, fast plugin manager.

### Plugin Manager Commands

- `:Lazy` - Open plugin manager UI
- `:Lazy update` - Update all plugins
- `:Lazy clean` - Remove unused plugins
- `:Lazy sync` - Update and clean in one command

### Installed Plugins

#### guess-indent.nvim

```lua
'NMAC427/guess-indent.nvim'
```

Automatically detects and sets `tabstop`/`shiftwidth` based on file content.

#### gitsigns.nvim

```lua
{ 'lewis6991/gitsigns.nvim', opts = { ... } }
```

Shows git diff signs in the sign column:
- `+` for added lines
- `~` for changed lines
- `_` for deleted lines

#### which-key.nvim

```lua
{ 'folke/which-key.nvim', event = 'VimEnter', opts = { ... } }
```

Shows popup with available keybindings when you press a leader key. Helps discover keymaps.

**Key groups:**
- `<leader>s` - [S]earch (Telescope)
- `<leader>t` - [T]oggle
- `<leader>h` - Git [H]unk

#### telescope.nvim

```lua
{ 'nvim-telescope/telescope.nvim', dependencies = { ... } }
```

Fuzzy finder for files, text, LSP symbols, and more.

**Keymaps:**
- `<leader>sh` - [S]earch [H]elp
- `<leader>sk` - [S]earch [K]eymaps
- `<leader>sf` - [S]earch [F]iles
- `<leader>sw` - [S]earch current [W]ord
- `<leader>sg` - [S]earch by [G]rep (live text search)
- `<leader>sd` - [S]earch [D]iagnostics
- `<leader>sr` - [S]earch [R]esume (last search)
- `<leader>s.` - [S]earch Recent Files
- `<leader><leader>` - Find existing buffers
- `<leader>/` - Fuzzy search in current buffer
- `<leader>s/` - Search in open files
- `<leader>sn` - Search Neovim config files

**Dependencies:**
- `plenary.nvim` - Lua utility library
- `telescope-fzf-native.nvim` - Fast C-based sorter (requires `make`)
- `telescope-ui-select.nvim` - Use Telescope for vim.ui.select
- `nvim-web-devicons` - File icons (requires Nerd Font)

#### LSP (Language Server Protocol)

##### lazydev.nvim

```lua
{ 'folke/lazydev.nvim', ft = 'lua', opts = { ... } }
```

Configures Lua LSP for Neovim development. Provides completion for Neovim APIs (`vim.*`).

##### nvim-lspconfig

```lua
{ 'neovim/nvim-lspconfig', dependencies = { ... } }
```

Main LSP configuration. Provides features like:
- Go to definition
- Find references
- Autocompletion
- Error checking
- Hover documentation

**LSP Keymaps (when LSP attaches):**
- `grn` - [R]e[n]ame symbol
- `gra` - Code [A]ction
- `grr` - [G]oto [R]eferences
- `gri` - [G]oto [I]mplementation
- `grd` - [G]oto [D]efinition
- `grD` - [G]oto [D]eclaration
- `gO` - Open Document Symbols
- `gW` - Open Workspace Symbols
- `grt` - [G]oto [T]ype Definition
- `<leader>th` - [T]oggle Inlay [H]ints

**Dependencies:**
- **mason.nvim** - LSP/tool installer with `:Mason` command
- **mason-lspconfig.nvim** - Bridges mason and lspconfig
- **mason-tool-installer.nvim** - Auto-installs tools
- **fidget.nvim** - Shows LSP progress notifications
- **blink.cmp** - Provides completion capabilities to LSP

**Diagnostic Configuration:**
- Sorted by severity
- Rounded borders on floating windows
- Only underlines errors (not warnings/hints)
- Virtual text shows diagnostic messages inline

**Language Servers:**

Currently configured for Lua (`lua_ls`). To add more:

```lua
local servers = {
  gopls = {},      -- Go
  ts_ls = {},      -- TypeScript
  rust_analyzer = {}, -- Rust
  -- ... add more
}
```

Run `:Mason` to install language servers.

#### conform.nvim

```lua
{ 'stevearc/conform.nvim', event = { 'BufWritePre' } }
```

Code formatting with format-on-save.

**Keymaps:**
- `<leader>f` - [F]ormat buffer

**Formatters:**
- `stylua` - Lua formatter (auto-installed)

Add more formatters in `formatters_by_ft`:

```lua
formatters_by_ft = {
  lua = { 'stylua' },
  go = { 'gofmt' },
  python = { 'black' },
}
```

#### blink.cmp

```lua
{ 'saghen/blink.cmp', event = 'VimEnter', dependencies = { ... } }
```

Fast, modern completion engine with snippet support.

**Completion Sources:**
- LSP (language servers)
- File paths
- Snippets (via LuaSnip)
- Lazydev (Neovim Lua APIs)

**Keymaps (insert mode):**
- `<C-y>` - Accept completion
- `<Tab>/<S-Tab>` - Navigate snippet fields
- `<C-Space>` - Open menu / show docs
- `<C-n>/<C-p>` or `<Up>/<Down>` - Select next/previous
- `<C-e>` - Hide menu
- `<C-k>` - Toggle signature help

**Dependencies:**
- **LuaSnip** - Snippet engine
- **lazydev.nvim** - Neovim Lua completion

#### tokyonight.nvim

```lua
{ 'folke/tokyonight.nvim', priority = 1000 }
```

Colorscheme with multiple variants:
- `tokyonight-night` (current)
- `tokyonight-storm`
- `tokyonight-moon`
- `tokyonight-day`

Change variant in the config with `vim.cmd.colorscheme 'tokyonight-<variant>'`.

#### todo-comments.nvim

```lua
{ 'folke/todo-comments.nvim', dependencies = { 'nvim-lua/plenary.nvim' } }
```

Highlights and searches for TODO comments in code:
- `TODO:` - Things to do
- `HACK:` - Hacky solutions
- `WARN:` - Warnings
- `PERF:` - Performance issues
- `NOTE:` - Notes
- `FIX:` - Bugs to fix

#### mini.nvim

```lua
{ 'echasnovski/mini.nvim' }
```

Collection of independent modules. Currently using:

**mini.ai** - Better text objects:
- `va)` - [V]isually select [A]round [)]paren
- `yinq` - [Y]ank [I]nside [N]ext [Q]uote
- `ci'` - [C]hange [I]nside [']quote

**mini.surround** - Surround text with brackets/quotes:
- `saiw)` - [S]urround [A]dd [I]nner [W]ord [)]Paren
- `sd'` - [S]urround [D]elete [']quotes
- `sr)'` - [S]urround [R]eplace [)] [']

**mini.statusline** - Simple statusline showing mode, filename, position.

#### nvim-treesitter

```lua
{ 'nvim-treesitter/nvim-treesitter', build = ':TSUpdate' }
```

Advanced syntax highlighting using AST parsing. Much better than regex-based highlighting.

**Auto-installed parsers:**
- bash, c, diff, go, html, lua, luadoc, markdown, markdown_inline, query, vim, vimdoc

**Features:**
- Accurate syntax highlighting
- Smart indentation
- Code-aware text objects

**Commands:**
- `:TSInstall <language>` - Install parser
- `:TSUpdate` - Update parsers
- `:TSInstallInfo` - View installed parsers

#### neo-tree.nvim

```lua
require 'kickstart.plugins.neo-tree'
```

File explorer with tree view, git status integration, and file operations.

**Keybindings:**
- `\` - Toggle file tree

**In tree view:**
- `Enter` - Open file/expand folder
- `a` - Add new file/directory
- `d` - Delete
- `r` - Rename
- `c` - Copy
- `x` - Cut
- `p` - Paste
- `?` - Show all commands

**Features:**
- Shows git status (modified, added, deleted files)
- File icons with Nerd Font
- Quick file operations

**Note:** Requires Nerd Font in terminal for icons. Use "JetBrainsMono Nerd Font Mono" in iTerm2.

#### lazygit.nvim

```lua
{ 'kdheepak/lazygit.nvim', lazy = true }
```

Terminal UI for git inside Neovim. Provides a beautiful interface for all git operations without leaving your editor.

**Keybindings:**
- `<leader>lg` - Open [L]azy[G]it

**Available Commands:**
- `:LazyGit` - Open lazygit in a floating terminal
- `:LazyGitConfig` - Open lazygit configuration file
- `:LazyGitCurrentFile` - Open lazygit focused on current file history
- `:LazyGitFilter` - Open lazygit with commit filter
- `:LazyGitFilterCurrentFile` - Open lazygit filtered to current file

**Features:**
- Full git workflow in a TUI (add, commit, push, pull, stash, etc.)
- Visual diff viewer
- Branch management
- Merge conflict resolution
- Interactive rebase
- Commit history exploration
- File tree with git status

**Dependencies:**
- **plenary.nvim** - Lua utility library
- **lazygit** - Must be installed separately: `brew install lazygit`

**Lazy Loading:**
This plugin is lazy-loaded and only loads when you run one of the commands or use the `<leader>lg` keybinding, keeping startup time fast.

---

## Optional Kickstart Plugins

These are available but commented out by default. Uncomment in `init.lua` to enable:

```lua
require 'kickstart.plugins.debug',      -- DAP debugging
require 'kickstart.plugins.indent_line', -- Indent guides
require 'kickstart.plugins.lint',       -- Linting
require 'kickstart.plugins.autopairs',  -- Auto-close brackets
require 'kickstart.plugins.gitsigns',   -- Git integration keymaps
```

**Note:** neo-tree is already enabled (see Installed Plugins section above).

---

## Custom Plugins

Add your own plugins in `lua/custom/plugins/init.lua` or create separate files in that directory.

The custom plugins import is enabled in `init.lua` with `{ import = 'custom.plugins' }`, which automatically loads all plugins defined in `lua/custom/plugins/*.lua`.

**Currently installed custom plugins:**
- **lazygit.nvim** - See details in the Installed Plugins section above

**To add more plugins:**

```lua
-- lua/custom/plugins/init.lua
return {
  -- lazygit.nvim (already installed)
  {
    'kdheepak/lazygit.nvim',
    lazy = true,
    cmd = { 'LazyGit', 'LazyGitConfig', 'LazyGitCurrentFile', 'LazyGitFilter', 'LazyGitFilterCurrentFile' },
    dependencies = { 'nvim-lua/plenary.nvim' },
    keys = { { '<leader>lg', '<cmd>LazyGit<cr>', desc = 'LazyGit' } },
  },
  -- Add your new plugins here
  {
    'your-username/your-plugin',
    opts = {},
  },
}
```

---

## Learning Resources

- Run `:Tutor` for interactive Vim tutorial
- Run `:help` to browse Neovim documentation
- `<leader>sh` to search help with Telescope
- Read through `init.lua` - every line is documented
- Check `:checkhealth` to diagnose issues

---

## Requirements

- **Neovim** >= 0.10 (latest stable or nightly)
- **Git** - For cloning plugins
- **Nerd Font** - Optional, for icons (set `vim.g.have_nerd_font = true`)
- **ripgrep** - For Telescope live grep
- **fd** - For fast file finding (optional)
- **C compiler** (gcc/clang) - For telescope-fzf-native
- **make** - For building native extensions

### macOS Installation

```bash
brew install neovim ripgrep fd
brew install --cask font-jetbrains-mono-nerd-font
```

---

## Configuration Philosophy

This config follows Kickstart's philosophy:
- **Single-file** - Everything in `init.lua`, easy to understand
- **Well-documented** - Every option explained
- **Batteries included** - LSP, completion, fuzzy finding out of the box
- **Not a distribution** - A starting point you customize and understand
- **Stable** - Lockfile ensures reproducible plugin versions
