# Opening a Scratch Buffer in Neovim / LazyVim

Sometimes you want to jot something down in Neovim without creating a file or
being prompted to save on exit. There are a few ways to do this.

## Quickest — open without a filename

```bash
nvim
```

Write whatever you need, then quit without saving:

```
:q!
```

## New unnamed buffer from inside Neovim

```
:enew
```

Opens a blank, unnamed buffer. Exit with `:q!` when done.

## Proper no-save scratch buffer

```
:enew | setlocal buftype=nofile noswapfile
```

Setting `buftype=nofile` tells Neovim this buffer has no backing file, so `:q`
works cleanly without a "save?" prompt.

## LazyVim shortcut (snacks.nvim)

LazyVim ships with `snacks.nvim`, which provides a persistent scratch buffer
out of the box:

```
<leader>S
```

This opens a dedicated scratch buffer that survives across splits but is never
written to disk — ideal for quick notes or throwaway experiments.
