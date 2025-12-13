# Dotfiles

Personal configuration files for development environment.

## Contents

- **nvim/** - Neovim configuration (Kickstart-based)
- **tmux/** - Tmux configuration

## Setup

Clone this repository and create symlinks:

```bash
# Clone
git clone git@github.com:jsachica/config.git ~/.config/dotfiles

# Symlink Neovim config
ln -s ~/.config/dotfiles/nvim ~/.config/nvim

# Symlink tmux config
ln -s ~/.config/dotfiles/tmux/tmux.conf ~/.config/tmux/tmux.conf
```

## Neovim

Kickstart.nvim-based configuration with:
- LSP support (Go, Lua)
- Treesitter syntax highlighting
- Telescope fuzzy finder
- Neo-tree file explorer
- Auto-completion with blink.cmp
- Git integration

See [nvim/README.md](nvim/README.md) for details.

## Tmux

Basic tmux configuration.
