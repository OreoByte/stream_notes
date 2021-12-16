# vi tricks #
# NOTE; (vim "has it's own white space syntax, if forget vimrc")

---

# vim basics

## edit file
	`vim filename.txt`
	`vi filename.txt`

## enter insert mode
	`i` = "enter insert mode on the where the cursor is currently at"

	`o` = "enter insert mode on the next new line"

`a` = "enter insert mode in front on the next single character, append char"
`A = "enter insert mode at the end of the line, append line"

## exit insert mode back to command mode
	`escape-key`
	`ctrl+c`
	
# enter prompt for <command-mode>
	`:`
	`shift + ; = :`
		* type command in here

% exit vim
1. quit without saving
	`:q!`

2. quit and save
	`:x`
	`:wq`
	`esc + Z + Z`

		* Note: `Zs` are both capitalized

3. just write to file without exiting vim
	`:w`
	
## {$} undo/redo what you have typed in vim
* undo
	`u-key`

* redo
	`ctrl+r`

---

# move around in vim
## {% basic movement %}

	`h` = "move to the left"
	`j` = "move down a character
	`k` = "move up a character"
	`l` = "move right a character"

### jump to the end or start of the document

	`gg` = "jump to the start of doc"
	`G` = "jump to end of doc"

### move to top or bottom of file (or to a specific line)
	`:13` == go to line number, 13

* before entering vim
	`vim +<line-number> <filename.txt>`
	`vim +10 xayah`

## {mark file location, "jump to past file location"}

```
m + a
` + a
```

### {$} jump the two most used marks

```
` + `
```

## jump around on the same line

### jump to the (start) of the line
	`0`
0r	`shift + ^`

### jump to (end) of the line
	`shift + $`

### jump before or after the next character, ("on the same line")

	`w` = "jump to the start of the next word"
	`W` = "jump to the start of the next word & special characters"
	
	`e` = "jumps to the end of the current word"
	`E` = "jumps to the end of current word & special characters"

	`b` = "start of the previous word"
	`B` = "start of teh previous word & special characters"

---

## grep through the vim file
	`/<search-string>`
	
	* enter-key to save string you are trying to grep for

	* search down the page, "next result"
		`n`
	* search up the page, "previous result"
		`shift + n`

% using the same method but in reverse with <?> instead of </>
	`?<search-string>`

	* enter-key to save string you are trying to grep for

	* search down the page, "next result"
		`shift + n`
	* search up the page, "previous result"
		`n`

NOTE; "this also works in less when searching through large files"

---

# delete, copy, cut, paste, etc (lines of text in vim/vi) #

## copying text

	`yy` = {normal copy single line}

### from a block mode
	`v` = {enter visual-copy mode}
0r
	`ctrl + v` = {enter visual-block-copy}

## paste what you have copied within the vim buffer
	`:reg`

### paste whatever has been copied
	`p` = "from the cursor location paste to the next line"
	`P` = "from the cursor location paste to the previous line"

## deleting text / cutting text
	* what looks like deleting is just cutting text to the vim registry buffer

### delete a single line single line edit
	`dd` = "deletes the whole currently selected line"

	`shift + d` = "deletes all text in front of the cursor"

### deleting multiple lines of text

1. place cursor on the character you want to remove
2. select copy mode
	`ctrl + v`
		* (visual block mode)
0r
	`v`
		* (visual line mode)

3. move to high light all characters you want to remove
4. press (x) 0R (d)
	* delete them all at once

---

# how to copy vim selected text outside the vim clipboard
	* work in progress

---

# How to edit multiple files on a box without having to change without exiting

## how to jump between different local files without exiting vim <buffers>

### list all buffers
	`:ls`
0r
	`:buffers`

### swap between multiple vim buffers
* by the buffer number

	:buff <buffer-number>
0r	
	`:b <buffer-number>`
0r	
	`:b <buffer-title>`

	`:buff1`
	`:b2`

* by the previous or next buffer
	`:bnext`
	`:bprev`

### rename the currently selected buffer name/title
	`:file <new-buffer-name>`
	`:file xayah_two`

### {%} remove the current buffer
	`:bd`
	`:bd!`

{$} spliting the buffer
	https://vim.fandom.com/wiki/Buffers

## `Extra` how edit remote files
	`vi scp://username@host//path/to/somefile.txt`
	`vim scp://username@host//path/to/somefile.txt`

---

# Split the vim editor to show to different files on the same window #
## splitting vim screen vertically
	`ctrl + w + v`
0r	
	`:set` 
		* `ctrl + w + r` = "move to the right side"
		* `ctrl + w + l` = "move to the left side"

## splitting vim screen horizontally
	`ctrl + w + s`
		* `ctrl + w + j` = "jump to the bottom split"
		* `ctrl + w + k` = "jump to the top split

## split within command mode
1. split vertically
	`:`
	`:vsplit`

2. split horizontally
	`:`
	`:split`

## split with a new filename

	`:split <new-filename>`
0r
	`:vsplit <new-filename>`

% split with a file that already exists

	`:split /path/to/file`
0r
	`:vsplit /path/to/file`

	`:split /tmp/filename.txt`

# exit from a split, "same way you would exit out vim
	`:x`
	`:wq`
	`:q!`

------------------------------------------------------------------
# vim programmable macros #
# simple append to line micro % {macro basics}

1. enter record mode
	`q + <letter to bind the macro to>`
	`q + q`

* double check what you have set as a macro (with command mode "reg")
	`:reg`

2. <create macro pattern>

3. save macro
	`q`

4. execute macro
	`@@`

------------------------------------------------------------------
# Command mode cheat sheet #

# prevent pasting large amount of text from creating wrong indentations
	`:set paste`

# execute commands that will interact with the text in the file without exiting
* xxd example

	`:%!xxd`
	`:%!xxd -r`

# find and replace vim cheatsheet

## find and replace string pattern (replace <hello> with <goodbye>)

	`:%s/hello/goodbye`

* add line number to double check the text is on a single line
	`:set number`

# change the encoding type of the file being edited with vim

	`:set encoding=utf-8`

		`file filename.txt` (verify if the encoding was changed)
0r
	set encoding VS set fileencoding

NOTE; (add "set bomb" to help editor consider the file as UTF8)
	`:set bomb`
	`:set fileencoding=utf-8` > `:wq`

* more reading here
	https://stackoverflow.com/questions/778069/how-can-i-change-a-files-encoding-with-vim

---

# vim fun // userful stuff #

`vi -x filename` (add passowrd protection to a file)

# {$} when you forget the sudo
	`esc`
	`:w !sudo tee %`

---
---

# nano tricks #

`ctrl + w` == ctrl find for nano
`alt + w`  == (jump to every string specified by "ctrl+w")

`alt + /` == jump to end of file
`alt + \` == jump to start of file

##  cut and paste text
* cut
	`ctrl + k`

* paste from the nano buffer
	`ctrl + u`

# %%/!\ copy and paste walls of text /!\%%

## enter mark (selection mode)
	`alt + shift + A`

* after highlighting everything with the arrow keys
* copy selected text to the nano buffer
	`alt + shift + 6`

* paste copied text
	`ctrl + u`

## %/!\ NOTE; (this can also be used to, "cut and paste large amount of text")

```
alt + shift + A
alt + shift + 6
	
ctrl + k
ctrl + u
```

# {!} when you forget sudo in nano
	ctrl-x -> y

##  change the write path to directory you can write to when you save
	`ctrl+x + y`

	File Name to Write: <location where you can write>
	File Name to Write: `/tmp/filename`

## will have to copy the file name in the desired location later with sudo

	`sudo cp /tmp/filename /path/to/desired/place/filename_you_want`
	`sudo cp /tmp/filename /opt/new_filename`

-----------------------------------------------------------------------------
# gedit (GUI text editor like leafpad)
	`sudo -E gedit filename.txt`
	`-E (Preserve your env)`
