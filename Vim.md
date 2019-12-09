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

**Words**

`w`: move to next word

`W`: move to nest WORD

`e`: move at the end of current word

`E`: move at the end of current WORD

`b`: move at the beginning of current word (or preceding word if already at the beginning)

`B`: move at the beginning of current WORD (or preceding WORD if already at the beginning)

**Find characters**

`t<char>`: move forward un**t**il <char>

`T<char>`: move backward un**t**il <char>

`f<char>`: move forward on <char>

`F<char>`: move backward on <char>

`;`: repeat last move of this list forward

`,`: repeat last move of this list backward

### Vertical motions

`%`: Go to the next opening/closing character/tag. If cursor is on a `{`, it will move to the corresponding `}` .

**Whole file**

`gg`: move at the top of the file

`G`: move at the bottom of the file

**Specific line**

`:<n><Enter>`: move at line number <n>

`<n>%`: move at the percentage of the file (50% on 10 lines will move to the 5th line)

**Block motions**

`(`: move to previous sentence

`)`: move to next sentence

`{`: move to previous empty line

`}`: move to next empty line

**Searching**

`/<text><Enter>`: move the cursor on the first occurence of <text> after the cursor

`n`: go to the next occurence of <text>

`N`: go to previius occurence of <text>

`*`: go to next occurence of the word which is under the cursor

`#`: go to previous occurence of the word which is under the cursor
