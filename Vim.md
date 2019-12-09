# Vim

## Motions

Motions is an event that moves the cursor to a different position. Motions can be preceded by a number `n`, which will repeat the motion `n` times.

### Basic motions

`h`: left

`j`: down

`k`: up

`l`: right

### Horizontal motions

- `words` are delimited by blank spaces or punctuation
- `WORDS` are delimited by black spaces

**Whole line**

`0`: move at the beginning of the line

`^`: move at first the non blank charactore of the line

`$`: move at the end of the line

`g_`: move at the last non blank character of the line

`I`: move at the beginning of line and set INSERT MODE

`A`: move at the end of the line and set INSERT MODE

`o`: insert a new line under the cursor, move to it and set INSERT MODE

`O`: insert a new line above the cursor, move to it and set INSERT MODE

**Words**

`w`: move to next word

`W`: move to nest WORD

`e`: move at the end of current word

`E`: move at the end of current WORD

`b`: move at the beginning of current word (or preceding word if already at the beginning)

`B`: move at the beginning of current WORD (or preceding WORD if already at the beginning)

**Find characters**

`t<char>`: move forward un**t**il `<char>`

`T<char>`: move backward un**t**il `<char>`

`f<char>`: move forward on `<char>`

`F<char>`: move backward on `<char>`

`;`: repeat last move of this list forward

`,`: repeat last move of this list backward

### Vertical motions

`%`: Go to the next opening/closing character/tag. If cursor is on a `{`, it will move to the corresponding `}` .

**Whole file**

`gg`: move at the top of the file

`G`: move at the bottom of the file

**Specific line**

`:<n><Enter>`: move at line number `<n>`

`<n>%`: move at the percentage of the file (50% on 10 lines will move to the 5th line)

**Block motions**

`(`: move to previous sentence

`)`: move to next sentence

`{`: move to previous empty line

`}`: move to next empty line

**Searching**

`/<text><Enter>`: move the cursor on the first occurence of `<text>` after the cursor

`n`: go to the next occurence of `<text>`

`N`: go to previius occurence of `<text>`

`*`: go to next occurence of the word which is under the cursor

`#`: go to previous occurence of the word which is under the cursor

### Screen motions

`zt`: move the screen so that cursor is at the **t**op of the screen

`zz`: move the screen so that cursor is at the middle of the screen

`zb`: move the screen so that cursor is at the **b**ottom of the screen

## Actions

Actions modify the text in a certain way, they are followed by a motion. A capital letter action offen acts on the whole line. The deleted or yanked characters are stored in the registers. Type `:reg` to see the content of all the registers.

- `line`: the line
- `whole line`: the line including the `<CR>` character at the end

### Registers

Registers are places in which you can store characters, see `:reg`.

`"<lowercase_letter>`: set in register `<lowercase_letter>`

`"<uppercase_letter>`: append in register `<lowercase_letter>`

`"<register>p`: paste from register

### Basic actions

`d<motion>`: delete

`dd`: delete whole line

`D`: delete until the end of the line

`y<motion>`: yank (copy)

`yy`: yank (copy) whole line

`Y`: yank (copy) line

`c<motion>`: change (delete and set INSERT MODE)

`cc`: change whole line

`C`: change until end of line

`p`: paste after cursor. If the content is a `whole line`, paste at the next line

`P`: paste before cursor. If the content is a `whole line`, paste at the previous line

### Character/line actions

r<char> replace character under cursor by `<char>`

`R`: set REPLACE MODE

`x`: delete current char

`X`: delete char before cursor

`s`: delete current char and set INSERT MODE

`S`: delete current line and set INSERT MODE

### inside/around

A text object can be a word or a WORD, for example. This is useful when you are in the middle of a text object and you want to act on the whole text object.

`<action>i<text_object>`: action **i**nside text object. For example `ci'` on a text which is between `''` will change everything **I**NSIDE `''`. `ciw` will change the word

`<action>a<text_object>`: action **a**round text object. For example `ca(` on a text which is between parenthesis will change the word and the parenthesis. `caw` will also change the word
