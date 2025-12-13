# Tmux Configuration

This directory contains the tmux configuration for enhanced terminal multiplexing.

## Configuration File

- `tmux.conf` - Main tmux configuration file

## Configuration Overview

### True Color Support

```tmux
set-option -sa terminal-overrides ",xterm*:Tc"
```

Enables 24-bit true color support for xterm-compatible terminals. The `-sa` flag appends to the existing `terminal-overrides` option, and `Tc` enables true color capability. This ensures colors in Neovim and other terminal applications render correctly with full RGB support.

### Custom Prefix Key

```tmux
set -g prefix `
unbind C-b
bind ` send-prefix
```

Changes the default tmux prefix from `Ctrl+b` to the backtick key `` ` ``. This configuration:
- Sets the new prefix to backtick
- Unbinds the default `Ctrl+b` prefix to avoid conflicts
- Binds backtick twice (`` ` ` + `` ` ``) to send a literal backtick character to the terminal when needed

### Window Navigation

```tmux
bind -n M-H previous-window
bind -n M-L next-window
```

Enables quick window switching using Shift+Alt+H/L (vim-style navigation):
- `M-H` (Alt+Shift+H) - Switch to the previous window
- `M-L` (Alt+Shift+L) - Switch to the next window
- The `-n` flag means these work without requiring the prefix key

### Mouse Support

```tmux
set -g mouse on
```

Enables comprehensive mouse support in tmux, allowing you to:
- Click to select panes
- Drag pane borders to resize
- Click window names in the status bar to switch windows
- Scroll with the mouse wheel to navigate through pane history

### Plugin Management

```tmux
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'christoomey/vim-tmux-navigator'
```

Uses TPM (Tmux Plugin Manager) to manage plugins:

#### tpm
The plugin manager itself. Handles installation, updates, and loading of tmux plugins.

#### tmux-sensible
Provides a set of sensible default tmux options that most users would want, including:
- Better default key bindings
- Improved scrollback buffer
- UTF-8 support
- Better status bar refresh intervals
- Focus events enabled for Neovim compatibility

#### vim-tmux-navigator
Enables seamless navigation between tmux panes and Vim/Neovim splits using the same key bindings:
- `Ctrl+h` - Move left
- `Ctrl+j` - Move down
- `Ctrl+k` - Move up
- `Ctrl+l` - Move right

This plugin intelligently detects whether you're at the edge of a Vim split or tmux pane and navigates accordingly.

### Plugin Initialization

```tmux
run '~/.tmux/plugins/tpm/tpm'
```

Initializes the TPM plugin manager. This line must be at the very bottom of the configuration file to ensure all plugin declarations are loaded first.

## Plugin Installation

To install the configured plugins:

1. Install TPM:
   ```bash
   git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
   ```

2. Inside tmux, press `` ` `` + `I` (capital i) to fetch and install the plugins

3. Reload tmux configuration:
   ```bash
   tmux source ~/.config/dotfiles/tmux/tmux.conf
   ```

## Key Bindings Summary

| Key Binding | Action |
|-------------|--------|
| `` ` `` | Prefix key |
| `` ` ` + ` ` `` | Send literal backtick |
| `Alt+Shift+H` | Previous window |
| `Alt+Shift+L` | Next window |
| `Ctrl+h/j/k/l` | Navigate panes/vim splits (via vim-tmux-navigator) |
| `` ` `` + `I` | Install plugins |
| `` ` `` + `U` | Update plugins |
| `` ` `` + `Alt+u` | Uninstall plugins |

## Notes

- The configuration includes commented-out mouse scroll fixes for iTerm2. These are legacy workarounds and generally not needed with modern tmux and terminal emulators.
- Commented-out mouse options (`mouse-select-pane`, `mouse-resize-pane`, etc.) are from tmux versions prior to 2.1. The single `set -g mouse on` option now handles all mouse functionality.
