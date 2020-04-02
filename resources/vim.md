# Vim

### Key notations

* `<CR>`: carriage return, i.e. `Enter`.
* `<C-A>`: `ctrl` \(^ on Mac\) + `a`.

### Help

* `:h`: Seek help. For example, `:h zz` shows what `zz` does.
* `<C-]>`: follow a link in help manual. `<C-T>`: go back.

### Motions

#### Basics

* `hjkl`.
* `%`: move between matching braces \(`()`, `[]`, `{}`\).
* `(`/`)`: move to previous/next sentence.
* `{`/`}`: move to previous/next paragraph \(delimited by empty lines\).
* `<N>` + motion: repeat motions, e.g. `5l`, `4{`.
* `/abc`: move to the first occurance of "abc". Delete until the first "abc": `d/abc<CR>`.

#### Line Level

* word: delimited by `-`/`_`/whitespaces. WORD: delimited by whitespaces.
* `w`/`b`: move between word starts. `W`/`B`: move between WORD starts. **Uppercase motion is "bigger".**
* `e`/`ge`: move between word ends. `E`/`gE`: move between WORD ends. **g version is the reverse.**
* `fX`/`FX` \(X is any character\): move to the next/previous X. If not found, the cursor stays. **Uppercase motion is the reverse.**
* `tX`/`TX`: move until \(cursor will be before\) next/previous X. Delete until dot: `dt.`.
* `;`/`,`: repeat f/F/t/T in the same/reverse direction.
* `0`/`$`: move to the start/end of current line: .
* `^`: move to the first non-whitespace character of current line.
* `-`/`<CR>`: move to the start of previous/next line.

#### Display Lines

* `gj`/`gk`: move between display lines. **g version is for display lines.**
* `g0`: move to start of a display line.

#### Page Level

* `H`/`M`/`L`: move cursor to the first/middle/last line of the screen.
* `zt`/`z<CR>`/`zb`: redraw buffer so that the current line is the first/middle/last line.
* `<C-B>`/`<C-F>`: move one page up/down.
* `<C-U>`/`<C-D>`: move half page up/down.

#### Buffer Level

* Move to start/end of buffer: `gg`/`G`.
* Move to line `<N>`: `<N>G`/`<N>gg`.
* Move to `<N>` percent: `<N>%`. \(No point to jump between matching parenthesis 50 times, so it was repurposed.\)

General command format in normal mode: `verb + number + movement`.

| Command | Verb | Movement | Note |
| :--- | :--- | :--- | :--- |
| `>G` | `>` indent | `G` from current line to end of file |  |
| `c$` | `c` change | `$` from current position to end of line | `C` is the same. `D` is `d$`. |

Normal mode =&gt; insert mode

* `i`: insert before cursor
* `a`: insert after cursor \(append\). Move to the end of current word and append: `ea`. Move to the end of previous word and append: `gea`.
* `s`: delete current character and insert \(substitute\)

Uppercase version: for line

* `I`: insert at the beginning of the line
* `A`: insert at the end of the line
* `S`: delete current line and insert
* `o`: insert below current line

Uppercase version: invert

* `O`: insert before current line

Search

* `*`/`#`: search current word forward/backwards

Substitution

substitute in one line, first occurances:

:s/target/replacement

repeat substitution: `&`

### Normal Mode

Move cursor with arrow keys

Ctrl C is the same as esc: insert mode to normal mode

parentheses move between sentences. Curly braces paragraph.

d\) delete a sentence

double qoute a: use a register Then yy

Mark

ma mark the current location to a

30G or :30 go to the 30th line

Automatically: mark dot, the line with the last change. Go back: single quote dot.

Lowercase mark is local to buffer. Uppercase mark is global.

Jump history: back ctrl O forward??

Go to the mark: single quote a.

Read:

`:r filename`: read content of a file and insert below

`:r !ls`

`:-1r filename`: insert the file in the previous line

vimrc

show line number :set number \(or `:se nu` for short\) :se nonu \(remove line number\)

:noremap   \(use space to scroll down\)

Abbreviations:

:abb \_ab abcdefg

Then in insert mode, \_ab will be expanded to abcdefg.

\_ab\(ctrl-V\) space will not be expanded.

fix typo: `:abb recieve receive`

Run shell command: `:! command`

`:com! Py ! python %` define a command \(override if existed\)

name of command: Py.

content of command: ! \(external\) % is current file name.

to call this command: `:Py`

`:set` see current set option.

`:set clipboard=unnamedplus` integrate system's clipboard with vim's

start Vim with commands

`vim file.txt +8`: same as `vim file.txt` then `:8`

`vim +1,2d +wq conway.txt` or`vim conway.txt +1,2d +wq`

delete first two lines

`vim file.txt +/Kramm` go to the first instance

vim can work with zip or tar files directly!! Vim treats then as just directories.

Open file from name

if in file1, it says `file2`, use `gf` \(go to file\) to open that file.

if you have a error log file containing line numbers \(e.g. `hello.go:2`\), `gF` will open the file on that line.

### Part II Files

#### Open multiple files `:e`

* Open new file in current window: `:e filename`, `:e <TAB>`, `:e prefix<TAB>`.
* Open current directory: `:e .`.
* Reopen current file \(discard all unsaved changes\): `:e!`.
* Open new file in a new window: `:new filename`.

#### Buffers

A buffer is an opened file.

* List all buffers: `:ls`. `%` is current file.
* Navigate between buffers: `:bp`, `:bn`, `:bfirst`, `:blast`, `:b3` \(number in `:ls`\), `:b filename`, `:b <TAB>`, `:b prefix<TAB>`.
* Delete a buffer \(close one file\): `:bd`, `:bd filename`.

#### Windows

Windows are views to a buffer. Useful when you want to see two parts of the same file at the same time.

* Create a new window \(split\): `<C-W>s` or `:split` split horizontally; `<C-W>v` or `:vsplit` split vertically.
* Move between windows: `<C-W>w`, but `<C-W><C-W>` is probably easier to type.
* Close a window: `:close` or `<C-W>c`.
* Close all other windows: `:only` or `<C-W>o`.

### Part V Patterns

#### Search

* Repeat last search: `/<CR>`.
* Go through search history: `/ <UP>`.

Tip 81 C-r C-w auto complete search term

Tip 82 match numbers :%s///gn

Tip 83 :%s/abc/e cursor will be at the end of matching pattern 14. Substitution TIp 87 :\[range\]s/pattern/string/flags flags g: global: modify all matches in one line c: confirm n: do not replace. just to report matching numbers e: suppress error message: no match &: reuse previous flags Replacement string: \t tab \r new line ~ previous string \1, \2: captures \0 or &: all patterns

Tip 89 confirm: a: all: replace this and all the following occurrences l: last. replace here and quit q: quit C-e/C-y: go to previous/next screen

Tip 90 leave pattern empty and vim will use last pattern

Tip 91 replace with register content. :%s/\=@0/g

Tip92 search in whole file :s/target/replace/g g& 15. Global Commands Tip 97 :\[range\] global\[!\]/pattern/\[cmd\] range: default is all file \(%\). Other ex command default is current line \(.\) pattern: default is previous pattern cmd: default is :print !: inverse. only run command on no match lines. :v

Tip 98 :g/re/p ====&gt; grep :g/re/d: delete matching lines :v/re/d: delete non matching lines Tip 99 empty a register: qaq collect TODO :g/TODO/yank A use A to append to register a.

Tip 100 :global/{start}/ .,{finish} \[cmd\]

## To Read

Vim命令合集 [http://blog.jobbole.com/114641/?utm\_source=blog.jobbole.com&utm\_medium=relatedPosts](http://blog.jobbole.com/114641/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)

5个好用的Vim插件 [http://blog.jobbole.com/114666/](http://blog.jobbole.com/114666/)

SpaceVim [https://github.com/SpaceVim/SpaceVim](https://github.com/SpaceVim/SpaceVim)

[https://learnxinyminutes.com/docs/vim/](https://learnxinyminutes.com/docs/vim/) Vim 101 movement [https://medium.com/usevim/vim-101-quick-movement-c12889e759e0](https://medium.com/usevim/vim-101-quick-movement-c12889e759e0) 移动窗口和tab [https://www.kawabangga.com/posts/2196](https://www.kawabangga.com/posts/2196)

插件推荐 [https://www.zlovezl.cn/articles/vim-plugins-cannot-live-without/](https://www.zlovezl.cn/articles/vim-plugins-cannot-live-without/) [https://www.zlovezl.cn/articles/my-vim-plugins-for-python/](https://www.zlovezl.cn/articles/my-vim-plugins-for-python/)

Carl smith vim plugins Vim-fugitive Gitv Syntastic Vim-gitgutter Vim-ipython Vim-markdown

Reload vimrc [https://medium.com/usevim/reloading-your-vimrc-bdbc7e6e9665](https://medium.com/usevim/reloading-your-vimrc-bdbc7e6e9665) Five time saving tricks of vim [http://blog.unixphilosopher.com/2015/02/five-weird-vim-tricks.html](http://blog.unixphilosopher.com/2015/02/five-weird-vim-tricks.html) [https://medium.com/usevim/five-time-saving-vim-tricks-45aae3d9211](https://medium.com/usevim/five-time-saving-vim-tricks-45aae3d9211)

Google doc notes [https://docs.google.com/document/d/1U539hLM\_heDKxIhrgyrOnqlj67VTyeOdprEOWaXx-64/edit\#](https://docs.google.com/document/d/1U539hLM_heDKxIhrgyrOnqlj67VTyeOdprEOWaXx-64/edit#)

[https://docs.google.com/document/d/1u39-RWdw\_cO1mwvIvvg9F6G78UwWxjbr4-Q7B5f5GlA/edit\#heading=h.pqe9ayxg5oim](https://docs.google.com/document/d/1u39-RWdw_cO1mwvIvvg9F6G78UwWxjbr4-Q7B5f5GlA/edit#heading=h.pqe9ayxg5oim)

[https://drive.google.com/drive/folders/18CxDvfKF7VyUQejAWLd48wCMB4kkuFfK](https://drive.google.com/drive/folders/18CxDvfKF7VyUQejAWLd48wCMB4kkuFfK)

NeoVim [https://github.com/neovim/neovim](https://github.com/neovim/neovim)

Learn Vimscript the hard way \(中文\) [http://learnvimscriptthehardway.onefloweroneworld.com/](http://learnvimscriptthehardway.onefloweroneworld.com/)

Vim plugin

Spf13 [https://github.com/spf13/spf13-vim](https://github.com/spf13/spf13-vim) Airline [https://github.com/vim-airline/vim-airline](https://github.com/vim-airline/vim-airline)

[https://vimawesome.com/](https://vimawesome.com/)

Vim filname +123 =&gt; opens the file at line 123

Study the .vimrc!!

Linux and Vim notes [https://catonmat.net/linux-and-vim-notes](https://catonmat.net/linux-and-vim-notes)

Some Vim plugins [https://catonmat.net/vim-plugins](https://catonmat.net/vim-plugins)

Moving around in Vim [https://dev.to/iggredible/vim-cheatsheet-to-move-around-in-a-file-plus-tips-to-use-them-bme](https://dev.to/iggredible/vim-cheatsheet-to-move-around-in-a-file-plus-tips-to-use-them-bme)





