# VI EDITOR (LINUX)

Based on the tutorial written by Michael C. Pierce and Robert K. Ware, Colorado
School of Mines using ideas supplied by Charles Smith, Colorado State University.

To enter to the tutorial, type: `vimtutor`

## Lesson 1 Summary: DETELE CHAR, INSERT BEFORE & APPEND AFTER

1. The cursor is moved using either the arrow keys or the hjkl keys.  
   `h(left) j (down) k(up) l (right)`

2. To start Vim from the shell prompt:  
   `vim FILENAME \<ENTER\>`

3. To exist Vim:  
   `\<ESC\> :q! \<ENTER\>` to trash all changes  
   `\<ESC\> :wq \<ENTER\>` to save the changes

4. To delete the character at the cursor: `x`
5. To insert or append text type:
   - to insert before the cursor: `i`
   - to append after the line: `A`

## Lesson 2 Summary: DELETE, UNDO & REDO

1. To delete from the cursor up to the next word: `dw`
2. To delete from the cursor to the end of a line type: `d$`
3. To delete the whole line type: `dd`
4. To repeat a motion preprend it with a number: `2w` (to delete two words `d2w`)
5. To move to the start of the line: `0`
6. To undo previous actions, type: `u`
7. to undo all the changes on a line: `U`
8. To undo the undo's: `CTRL+R`

## Lesson 3 Summary: PUT, REPLACE & CHANGE

1. To **put** back text that has just been deleted, type `p` . This puts the deleted text **AFTER** the cursor (if a line was deleted it will go on the line below the cursor).
2. To **replace** the character under the cursor, type `r`and then the character you want to have there.
3. The **change** operatos allow syou to change from the cursor to where the motion takes you. eg. Type `ce` to change from the cursor to the end of the word, `c$`to change to the end of a line.
4. The format for change is: `c [number] motion`

## Lesson 4 Summary: MOVE, SEARCH & SUBSTITUTE

1. `CTRL+G` displays your location int he file and the file status.
   - `G` moves to the end of the file.
   - `number G` moves to that line number.
   - `gg` moves to the first line.
2. Type `/` followed by a phrase searches FORWARD for the phrase.
3. Type `?` followed by a phrase searches BACKWARDS
   - press `n` to find the next occurrence
   - press `N` to find the opposite direction
   - `CTRL+O` takes you back to older position, `CTRL+I` to newer positions.
4. To substitute new for the first old ina line type: `:s/old/new`  
   To substitute new for all 'old's on al ine type: `:s/old/new/g`  
   To substitute phrases between two line #'s type: `#,#s/old/new/g`  
   To substitute all occurrences in the file type: `%s/old/new/g`
   To ask for conformation each time add 'c': `%s/old/new/gc`

<!-- :TODO: Finish this lesson -->

## Lesson 5 Summary: [Under construction...]

## Lesson 6 Summary: OPEN, APPEND, E COMMAND & COPY - PASTE

1. Type `o` to open a line **BELOW** the cursor and start Insert mode.  
   Type `O` to open a line **ABOVE** the cursor.
2. Type `a` to insert text **AFTER** the cursor.  
   Type `A` to insert text **AFTER the end of the line**.
3. The `e` command move to the end of a word. **USEFULL**
4. The `y` operator yanks (copies) text, `p` puts (pastes) it.
5. Type `R` enteres Replace mode until `<ESC>` is pressed.
6. Typing `":set xxx"` sets the opetion "xxx". Some options are:
   - 'ic' 'ignorecase' ignore upper/lower case when searching
   - 'is' 'incsearch' show partial matches for a serach phrase
   - 'hls' 'hlsearch' highlight all matching phrases
7. Prepend "no" to switch an option off: `:set noic`

<!-- :TODO Finish this lesson -->

## Lesson 7: [under construction]
