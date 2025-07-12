# 🧠 Vim Cheat Sheet for C/C++ Development

## 🟩 Basic Vim Modes

| Mode         | Key                  | Description                         |
|--------------|----------------------|-------------------------------------|
| Normal       | `Esc`                | Default mode (move, copy, paste…)   |
| Insert       | `i`                  | Insert before cursor                |
|              | `I`                  | Insert at beginning of line         |
| Append       | `a`                  | Insert after cursor                 |
|              | `A`                  | Append at end of line               |
| Visual       | `v`                  | Select text character-wise          |
|              | `V`                  | Select entire lines                 |
| Command      | `:`                  | Execute command (e.g., `:wq`)       |

---

## 🟦 Movement

| Key         | Action                          |
|-------------|---------------------------------|
| `h j k l`   | Move left, down, up, right      |
| `w` / `W`   | Next word (W = ignores symbols) |
| `b` / `B`   | Previous word                   |
| `0` / `^`   | Start of line / first non-blank |
| `$`         | End of line                     |
| `gg` / `G`  | Start / End of file             |
| `CTRL-d/u`  | Half-page down/up               |

---

## 🟨 Inserting and Editing

| Command      | Action                              |
|--------------|-------------------------------------|
| `i` / `I`     | Insert before cursor / line         |
| `a` / `A`     | Insert after cursor / line          |
| `o` / `O`     | New line below / above              |
| `x`           | Delete character under cursor       |
| `r<char>`     | Replace character                   |
| `u`           | Undo last change                    |
| `CTRL-r`      | Redo                                |
| `.`           | Repeat last change                  |

---

## 🟧 Copy, Paste, Delete

| Command      | Action                              |
|--------------|-------------------------------------|
| `yy` / `Y`   | Yank (copy) current line             |
| `p` / `P`    | Paste below / above                  |
| `dd`         | Delete (cut) current line            |
| `dw` / `d$`  | Delete word / to end of line         |
| `v` → motion | Visual select and yank/delete/copy   |
| `:registers` | View all yanked/copied content       |

---

## 🟥 Searching

| Command         | Action                                  |
|-----------------|------------------------------------------|
| `/word`         | Search forward for `word`               |
| `?word`         | Search backward for `word`              |
| `n` / `N`       | Repeat search next / previous           |
| `*` / `#`       | Search for word under cursor (forward/back) |

---

## 🟪 Files and Buffers

| Command        | Action                              |
|----------------|-------------------------------------|
| `:e <file>`    | Open file                           |
| `:w`           | Save file                           |
| `:wq` / `ZZ`   | Save and quit                       |
| `:q!`          | Quit without saving                 |
| `:ls`          | List open buffers                   |
| `:b <num>`     | Switch to buffer by number          |

---

## 🟫 Code Navigation (with plugins like YCM / ctags)

| Command        | Action                              |
|----------------|-------------------------------------|
| `gd`           | Go to definition                    |
| `gD`           | Go to declaration                   |
| `CTRL-]`       | Jump to tag (definition)            |
| `CTRL-T`       | Jump back from tag                  |
| `:tag <symbol>`| Search tag for symbol               |
| `:!ctags -R`   | Generate tags for current directory |

---

## 🟨 Auto-indentation Settings

Add to your `.vimrc`:

```vim
set autoindent
set smartindent
set tabstop=4
set shiftwidth=4
set expandtab
```

---

## 🟦 NERDTree (if installed)

| Command           | Action                              |
|-------------------|-------------------------------------|
| `:NERDTreeToggle` | Open/close file tree                |
| `o`               | Open file or directory              |
| `t`               | Open in new tab                     |
| `m`               | Menu (create/delete/rename…)        |

---

## 🟧 Miscellaneous

| Command              | Action                          |
|----------------------|---------------------------------|
| `:syntax on`         | Enable syntax highlighting      |
| `:set number`        | Show line numbers               |
| `:set relativenumber`| Show relative line numbers      |
| `:noh`               | Clear search highlighting       |

## 🔍 Search
| Command | Description                              |
| ------- | ---------------------------------------- |
| `/word` | Search **forward** for `word`            |
| `?word` | Search **backward** for `word`           |
| `n`     | Repeat last search in same direction     |
| `N`     | Repeat last search in opposite direction |
| `*`     | Search forward for word under cursor     |
| `#`     | Search backward for word under cursor    |

## 🔁 Replace
| Command             | Description                                          |
| ------------------- | ---------------------------------------------------- |
| `:s/old/new/`       | Replace first `old` with `new` in **current line**   |
| `:s/old/new/g`      | Replace **all** `old` with `new` in **current line** |
| `:%s/old/new/g`     | Replace in **entire file**                           |
| `:10,20s/old/new/g` | Replace in **lines 10 to 20**                        |
| `:'<,'>s/old/new/g` | Replace in **selected visual lines**                 |
| `:%s/old/new/gc`    | **Confirm** each replacement (`y`/`n`/`a`/`q`)       |

### 🧠 Flags
| Flag | Meaning                                    |
| ---- | ------------------------------------------ |
| `g`  | Global — replace all occurrences on a line |
| `c`  | Confirm each substitution                  |
| `i`  | Ignore case                                |
| `I`  | Case sensitive                             |

Eg:
```bash
:%s/foo/bar/gi
```
Replaces all `foo` with `bar`, ignoring case.
### 📦 Tips
- Use `:noh` to clear search highlights.
- Use `\v` to enable very magic mode for simpler regex:
```bash
:%s/\vfunc\(\d+\)/func_call/g
```

