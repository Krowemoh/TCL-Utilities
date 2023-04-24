# TCL-Utilities
Helpful Commands for PICK TCL

## Commands

A set of utilities to make PICK easier to manage. Square brackets mean the word is optional. Curly braces are where you can substitute what you want in the command.

### CLONE

Copy records in PICK:

```
    Copy a file:
        CLONE [DICT] {SRC.FILE} {SRC.RECORD} [DICT] {DEST.FILE} {DEST.RECORD}

    Copy a file without renaming:
        CLONE [DICT] {SRC.FILE} {SRC.RECORD} [DICT] {DEST.FILE}

    Copy files using an active list:
        CLONE [DICT] {SRC.FILE} [DICT] {DEST.FILE}

    Copy a file to the underlying OS:
        CLONE [DICT] {SRC.FILE} {SRC.RECORD} {/tmp/} {new-name.txt}

    Copy an active list to a folder:
        CLONE [DICT] {SRC.FILE} {/tmp/backups/}
```

### MOVE

Move records around in PICK:

```
    Move a file:
        MOVE [DICT] {SRC.FILE} {SRC.RECORD} [DICT] {DEST.FILE} {DEST.RECORD}

    Move a file without renaming:
        MOVE [DICT] {SRC.FILE} {SRC.RECORD} [DICT] {DEST.FILE}

    Move files using an active list:
        MOVE [DICT] {SRC.FILE} [DICT] {DEST.FILE}

    Move a file to the underlying OS:
        MOVE [DICT] {SRC.FILE} {SRC.RECORD} {/tmp/} {new-name.txt}

    Move an active list to a folder:
        MOVE [DICT] {SRC.FILE} {/tmp/backups/}
```

### NEW-FILE

Create files with a default:

```
NEW-FILE {FILE-NAME}
```

### RENAME

Rename files and dictionaries:

```
RENAME [DICT] {FILE} {CURRENT.NAME} {NEW.NAME}
```

## Library Subroutines

### MOVE.FILES

Used as a base for COPY, RENAME and MOVE. The DELETE.FLAG will decide if the source record should be deleted or not.

```
    SUBROUTINE MOVE.FILES(ARGS,DELETE.FLAG)
```

### GET.ARGUMENTS

Used for argument parsing, ARGS will be an attribute marked list of what was typed in at TCL.

```
    CALL GET.ARGUMENTS(ARGS)
```
