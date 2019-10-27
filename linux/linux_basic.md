# Linux Command Line Bash Shortcuts

## Move Cursor

- `CTRL+A` : moves the cursor to the start of a **line**.
- `CTRL+E` : moves the cursor to the end of the **line**.
- `CTRL+B` : moves the cursor back one **character** at a time.
- `CTRL+F` : moves the cursor forward one **character** at a time.
- `CTRL+Left Arrow` or `ALT+B` : moves the cursor back one **word** at a time.
- `CTRL+Right Arrow` or `ALT+C` : moves the cursor forward one **word** at a time.

## Delete Text

- `CTRL+D` : remove the character under the cursor
- `CTRL+K` : remove all text from the cursor to the end of the line.
- `CTRL+X + Backspace` : remove all the text from the cursor to the beginning of the line.

## Bash Bang (!) Command

- `!!` : execute last command
- `!top` : execute the most recent command that starts with 'top'
- `!top:p` : display the command that !top would run
- `!$` : execute the last word of the previous command (if last command is 'cat tecmint.txt', then !\$ would try to run 'tecmint.txt')
- `!$:p` : display the word that !\$ would execute.
- `!*` : display the last word of the previous command
- `!*:p` : display the last word that !\* would substitute
