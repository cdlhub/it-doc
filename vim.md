ViM
=====

The `<Leader>` key is mapped to `\` by default.

## Insert / Overwrite / Change

 - **i**: insert before the current cursor position.
 - **a**: insert after the current cursor position
 - **I**: insert at the beginning of the current line.
 - **A**: insert/append at the end of the current line.
 - **o**: insert in a new line below the current line.
 - **O**: insert in a new line above the current line.
 - **r**: retype just the character under the cursor.
 - **R**: Enter overtype (replace) mode, where you destructively retype everything until you press ESC.
 - **s**: (substitute) delete the character (letter, number, punctuation, space, etc) under the cursor, and enter insert mode.
 - **c**: the 'change' (retype) command. Follow with a movement command. `cw` is a favorite.
 - **C**: Like `c`, but for the entire line.

## Cut/Copy/Paste

Beware that vim uses its own clipboard. You will have to use specific register to copy/paste from the system clipboard.

 - **P**/**p**: Paste before/after the cursor.

On systems that don't use X11 like OSX or Windows, the `"*` register is used to read and write to the system clipboard. You can specify the register to use before the copy/paste commands. e.g.: `"*yy` to copy the current line in the system clipboard and `"*p` to paste.

### Visual mode

1. Press `v` to enter visual mode (or uppercase `V` to select whole lines, or `Ctrl-v` to select rectangular blocks) and select characters.
2. Move the cursor to the end of what you want to cut.
3. Press `d` to cut (or `y` to copy).
4. Move to where you would like to paste and press `P` to paste before the cursor, or `p` to paste after.

- `d` stands for _delete_.
- `y` stands for _yank_.

### Normal mode

In normal mode, one can copy (yank) with `y{motion}` (resp. delete with `d{motion}`), where `{motion}` is a Vim motion. For example, `yw` copies to the beginning of the next word. Other helpful yanking commands include:

- `yy` or `Y` – yank the current line, including the newline character at the end of the line.
`y$` – yank to the end of the current line (but don't yank the newline character).
- `yiw` – yank the current word (excluding surrounding whitespace)
- `yaw` – yank the current word (including leading or trailing whitespace)

To copy into a register, one can use `"{register}` immediately **before** one of the above commands to copy into the register `{register}`. In normal and visual modes, `"xp` pastes the contents of the register `x`.

This works with special registers as well: `"+p` (or `"*p`) pastes the contents of the clipboard, `"/p` pastes the last search, and `":p` pastes the last command.

**Remark**: Copying/pasting from the system clipboard will not work if `:echo has('clipboard')` returns `0`. In this case, vim is not compiled with the `+clipboard` feature and you'll have to install a different version or recompile it.

## Undo/Redo

 - **u**: Undo the last change (can be repeated to undo preceding commands).
 - **Ctrl-R**: Redo changes which were undone using **u**.
 - **.**: Repeat a previous change, at the current cursor position.
 - **U**: Return the last line which was modified to its original state (reverse all changes in last modified line). It is seldom useful in practice, and is undo-able with **u**.

## Moves

**Reference:** http://vim.wikia.com/wiki/All_the_right_moves

- **e**: Move to the end of a word.
- **w**: Move forward to the beginning of a word.
- **b**: Move backward to the beginning of a word.
- **0**: Move to the beginning of the line.     
- **$**: Move to the end of the line.
- **gg**: Jump to beginning of file.
- **G**: Jump to end of file.
- **_n_G**: Jump to line _n_.
- **H**:   move to top of screen
- **M**: move to middle of screen
- **L**: move to bottom of screen
- **'m**: Jump to the beginning of the line of mark `m`.
- **``** (Two back ticks): Return to the cursor position before the latest jump (undo the jump).
- **%**: Jump to corresponding item, e.g. from an open brace to its matching closing brace.

You can type f<character> to put the cursor on the next character and F<character> for the previous one. the unmentionedT will jump to after the first sought character to the left, and I can confirm that the ; and , repeaters work with all of f/F/t/T

##Searching

###For basic searching:

 - **/pattern**: search forward for pattern
 - **?pattern**: search backward
 - **n**: repeat forward search
 - **N**: repeat backward

###Some variables you might want to set:

 - **:set ignorecase**: case insensitive
 - **:set smartcase**: use case if any caps used
 - **:set incsearch**: show match as search proceeds
 - **:set hlsearch**: search highlighting
 - **:set number/nonumber**: show/hide line numbers

###Search and replace...

 - **:%s/search_for_this/replace_with_this/**: search whole file and replace
 - **:%s/search_for_this/replace_with_this/c**: confirm each replace

## Syntax color-highlighting

 - **:syntax on/off**

It can be set in `.vimrc` with the command:

	syntax on

### Theme

Theme can be set with `colo <theme-name>`

```
blue.vim
darkblue.vim
default.vim
delek.vim
desert.vim
elflord.vim
evening.vim
koehler.vim
morning.vim
murphy.vim
pablo.vim
peachpuff.vim
ron.vim
shine.vim
slate.vim
torte.vim
zellner.vim
```

## Range (apply command to multiple lines)

Ranges may precede most _colon_ commands and cause them to be executed on a line or lines. For example `:3,7d` would delete lines 3-7. Ranges are commonly combined with the `:s` command to perform a replacement on several lines, as with `:.,$s/pattern/string/g` to make a replacement from the current line to the end of the file.

 - **:n,m**: lines n-m
 - **:.**: current line
 - **:$**: last line
 - **:'c**: marker c
 - **:%**: all lines in file
 - **:g/pattern/**: all lines that contain pattern

## References

- [LFCS: How to Install and Use vi/vim as a Full Text Editor](http://www.tecmint.com/vi-editor-usage/) by TecMint.
- [Learn Useful ‘Vi/Vim’ Editor Tips and Tricks to Enhance Your Skills – Part 1](http://www.tecmint.com/learn-vi-and-vim-editor-tips-and-tricks-in-linux/) by TecMint.
- [8 Interesting ‘Vi/Vim’ Editor Tips and Tricks for Every Linux Administrator – Part 2](http://www.tecmint.com/how-to-use-vi-and-vim-editor-in-linux/) by TecMint.
