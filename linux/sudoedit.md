# Edit privileged files with your normal editor

Avoid running `sudo nvim /etc/example.conf`. That starts Neovim as `root`, so
it uses root's configuration and separate LazyVim plugins rather than yours.

Use `sudoedit` (or its short form, `sudo -e`) instead:

```bash
sudoedit /etc/example.conf
```

`sudoedit` copies the file to a temporary location, opens it as your normal
user, then validates and writes the result back with elevated privileges. Your
usual Neovim configuration, plugins, and LSP setup remain available.

Set Neovim as the editor sudo uses in your shell configuration:

```bash
export EDITOR=nvim
export SUDO_EDITOR=nvim
```

`SUDO_EDITOR` takes precedence for `sudoedit`; `EDITOR` also configures the
default editor used by other command-line programs.
