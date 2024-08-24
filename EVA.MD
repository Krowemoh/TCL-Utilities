# EVA Editor

EVA is a modal editor for UniVerse and D3. It is multivalue aware and can be used to edit BASIC programs, PROCs, multivalue records and any other files that you can access from UniVerse and D3.

Commands starting with `:` are commands you can run in command mode. You can enter this mode by press shift and the `:` key in normal mode.

```
EVA BP SOME.FILE.NAME
```

There is an easy mode that starts in Insert Mode and uses `Ctrl S` and `Ctrl Q` to save and quit.

```
EVA --EASY BP SOME.FILE.NAME
```

## Quick Start
`escape` - Enter Normal mode  
`:q` - Exit  
`:w` - Save a file  
`i` - Insert mode  
`/` - Search mode  
`arrow-keys` - Movement  
`:help` - Help

## Features
		01. Persistent Undo History
		02. Syntax Highlighting for BASIC and PROCs
		03. BASIC code formatter
		04. Auto Complete based on the current file
		05. BASIC snippets for commonly used functions like MATREAD and LOCATE
		06. Natively handle multivalue data and encoding
		07. Explode, edit and view multivalue data easily
		08. Simple searching and replacing
		09. View dictionaries alongside record data
		10. Open linux file paths and write out linux files
  		11. Easy Mode starts in Insert mode and Ctrl S and Ctrl Q are available

## Modes
1. Normal Mode - This is the default mode  
2. Insert Mode - Press "i" to enter insert mode, this is how you edit things  
3. Command Mode - Press ":" to enter command mode, here you can type in commands to run  
4. Visual Mode -  Press "v" to copy text from a single line  
5. Search Mode - Press "/" to enter search mode
6. Easy Mode - This will open in a familiar editing environment

## Opening Files
`EVA BP SOME.FILE` - This will open a file inside BP.  
`EVA BP SOME.FILE --NO-LOCK` - This will open a file without locking it.
`EVA STUDENT.FILE 47` - This will open a data file.  
`EVA DICT STUDENT.FILE FIRST.NAME` - This will open a dictionary  

## Easy Mode Keys
`Ctrl S` - Save  
`Ctrl Q` - Quit  
`Ctrl Y` - Redo  
`Ctrl Z` - Undo  
`Ctrl F` - Find  

## Normal Mode Keys
`Escape` - Will put you in normal mode  
`i` - enter Insert mode, you can edit and insert characters  
`space` - Space will highlight a column of characters  
`:` - Command Mode  
`v` - Enter visual mode and copy part of a line  
`v -> $` - Inside Visual mode, you can use $ to go to the end of a line  
`v -> d` - Inside Visual mode, you can use d to delete a portion of the a line  

`/` - Enter search mode and type in a string to search for  
`ctrl+f` - Enter search mode and type in a string to search for  
`n` - Go to the next search result  
`N` - Go to the previous search result  

`c` - Add "* TEMP" to the beginning of a line  
`e` - Add a new line containing just "*" after the cursor  
`s` - Add a internal subroutine template

`d` - This command can be followed by another "d" or a number to delete that many lines  
`o` - Insert a blank new line and enter insert mode  
`x` - Delete the character at the cursor  
`r` - Replace the character at the cursor with the next keypress  
`u` - Undo the last change  
`ctrl+r` - Redo the last undo  

`y` - Yank the current line  
`p` - Paste the current buffer 
`1` - Place a starting mark, the will be the starting line for a block
`\`` - Place an ending mark, this will be the end of a block

```
` - Place a starting mark, the will be the starting line for a block  
1 - Place an ending mark, this will be the end of a block  
```

`up-arrow` - Move the cursor up  
`down-arrow` - Move the cursor down  
`left-arrow` - Move the cursor to the left  
`right-arrow` - Move the cursor to the right  

`Backspace` - Will move the cursor to the right  
`Enter` - Will move the cursor down  

`h` - Move the cursor left    
`j` - Move the cursor down    
`k` - Move the cursor up  
`l` - Move the cursor right  

`A` - Move the cursor left    
`S` - Move the cursor down    
`W` - Move the cursor up  
`D` - Move the cursor right  

`G` - Place the cursor on the last line  
`$` - Place the cursor at the end of the line  
`0` - Place the cursor at the beginning of the line  

`"` - Enclose a word in double quotes
`'` - Enclose a word in single quotes

## Editor Mode Commands
`:help` - Display the help  
`:^` - Enter up-arrow mode to view non-printable characters  
`:syntax={BASIC,PROC,DATA}` - Change the syntax highlighting  
`:{lineNumber}` - This will put the cursor on that line number  
`:e` - Go to %END
`:gf` - Typing gf on a CALL SUBROUTINE will open the subroutine in a view only buffer

`:invert` - This will inverse the case of a line  
`:lower` - Transform the line to lowercase  
`:upper` - Transform the line to uppercase  

`:numbers` - Toggle the line numbers   
`:f` - Format the file  

`:dec` - Decompile a file - ASI
`:prec` - Precompile a file - ASI
`:comp` - Decompile and Precompile a file - ASI
`:archive` - Archive a record in a file, creates a copy to ARCHIVE-{FILE} with a datestamp  

`:vlist` - View the object code source of a BASIC program  

`:fa` - Toggle the file attributes, only works in dictionaries  
`:dict` - View the dictionary in a seperate buffer  

`:count` {string} - Count the occurnace of the string  
`:s/{SS}/{RR}/{number=1}` - Replace SS with RR in the current line and next number of lines  
`:%s/{SS}/{RR}` - Replace SS with RR globally  

`:a{num} {string}` - Append a string to the next number of lines  

`:dt{lineNumber}` - Delete to a specific line number  
`:d{number}` - Delete the next number of lines  
`:drop` - Delete a block that has been marked  

`:ev` - Explode into a multivalue line  

`:load {file} {record}` - This will prompt for the line start and end, load in this lines from a seperate file  
`:w` - Save the record  
`:r` - Save and run a file, the output will be captured to a seperate buffer  
`:fd` - Delete the record  

`:q` - Exit a file  
`:exk` - Exit a file and exit the readnext loop  
`:x` - Exit a file and exit the readnext loop  
