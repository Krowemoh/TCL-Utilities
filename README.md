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

### GREP

Regex search in BASIC:

```
GREP - Regular Expression Search

GREP [DICT] {FILE} {RECORD.ID} {REGEX}
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

### GET.ARGUMENTS

Used for argument parsing, ARGS will be an attribute marked list of what was typed in at TCL.

```
    CALL GET.ARGUMENTS(ARGS)
```

### MOVE.FILES

Used as a base for COPY, RENAME and MOVE. The DELETE.FLAG will decide if the source record should be deleted or not.

```
    SUBROUTINE MOVE.FILES(ARGS,DELETE.FLAG)
```

## JSON Subroutines

### JSON Creation

#### JSON.ADD

Add keys and values to a multivalue string. This will form the basis of the object.

```
    BUFFER = ''
    CALL JSON.ADD(BUFFER,'key','value)
```

#### JSON.CREATE.ARRAY

Convert a multivalue list to an array.

```
    BUFFER = 1 : @AM : 2 : @AM : 3
    CALL JSON.CREATE.ARRAY(BUFFER)
```

#### JSON.CREATE.OBJECT

Convert a buffer to an object.

```
    BUFFER = ''
    CALL JSON.ADD(BUFFER,'id',123)
    CALL JSON.ADD(BUFFER,'description','This is a description')
    CALL JSON.CREATE.OBJECT(BUFFER)
```

### JSON Parsing

#### JSON.PARSE

Convert a json object to a multivalue string that can be queried.

```
    CALL JSON.PARSE(RAW.TEXT,JSON)
```

#### JSON.QUERY

```
    CALL JSON.QUERY(JSON,'.firstName',FIRST.NAME)
```

#### Example

```
    URL = 'https://dummyjson.com/users/1'
    EXECUTE 'SH -c "curl ' : URL : '"' CAPTURING RAW.TEXT
*    
    CALL JSON.PARSE(RAW.TEXT,JSON)
*    
    CALL JSON.QUERY(JSON,'.firstName',FIRST.NAME)
```


