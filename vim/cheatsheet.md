# My VIM cheatsheet

My very own cheatsheet

### Default

#### -- NORMAL ---

|     Mapping      | Summary                                                      |
| :--------------: | :----------------------------------------------------------- |
|       `x`        | Delete the character under the cursor                        |
|       `A`        | Move cursor to end of line to append text                    |
|       `dw`       | Makes a word disappear (cursor @ beginning of word)          |
|       `D`        | Deletes to the end of the line                               |
|       `w`        | Moves forward by a word                                      |
|       `b`        | Moves backwards by a word                                    |
| `:q` or `:quit`  | Quit out of Vim                                              |
|      `:q!`       | Quit out of Vim without saving                               |
| `:w` or `:write` | Write the current file (save)                                |
|      `:wq:`      | Save all changes and quit out of Vim                         |
|     `j`, `k`     | Move the cursor down / up                                    |
|     `h`, `l`     | Move the cursor left / right                                 |
|       `o`        | Drops down a line and enters insert mode                     |
|     `<S-o>`      | Goes up a line and enters insert mode                        |
|     `<S-c>`      | Deletes the remainder of the line then goes into insert mode |

#### -- INSERT --

| Mapping  | Summary                                      |
| :------: | :------------------------------------------- |
| `<esc>`  | Exit out of any mode back into normal mode   |
|   `i`    | Go into 'insert mode' to enter text          |
| `<S-i>`  | Go into insert mode at the start of the line |
| `Ctrl-T` | Indents current line whilst in insert mode   |
| `Ctrl-D` | Undents current line whilst in insert mode   |
|   `/`    | Seach file for text                          |

### Understanding custom mappings

|  Key   | Meaning                           |
| :----: | :-------------------------------- |
|  `C`   | `Ctrl`                            |
|  `A`   | `Alt`                             |
|  `S`   | `Shift`                           |
| `<CR>` | Hitting the enter / return button |

### My custom mappings

|  Mapping   | Summary                                    |
| :--------: | :----------------------------------------- |
| `jk`, `kj` | Exit out of any mode back into normal mode |
