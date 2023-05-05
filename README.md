# TCL-Utilities
Helpful Commands for PICK TCL

## Commands

A set of utilities to make PICK easier to manage. Square brackets mean the word is optional. Curly braces are where you can substitute what you want in the command.

### CAT

Print a record to the screen:

```
    CAT [DICT] {FILE} {RECORD}
    CAT [DICT] {FILE}
```

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

### FTP

Upload and download files using ftp:

```
    Send a file to a server:
        FTP {/path/to/file} {username:password@server:/path/to/file}

    Get a file from a server:
        FTP {username:password@server:/path/to/file} {/path/to/file}
```

### GREP

Regex search in BASIC, ^.*$:

```
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

### SCP

Upload and download files using ssh:

```
    Send a file to a server:
        SCP {/path/to/file} {username:password@server[:port]:/path/to/file}

    Get a file from a server:
        SCP {username:password@server[:port]:/path/to/file} {/path/to/file}
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

### FILE.TRANSFER.PROTOCOL

Used as a base for the FTP command line utility:

```
    SUBROUTINE FILE.TRANSFER.PROTOCOL(RESULTS,SERVER,USERNAME,PASSWORD,LOCAL.PATH,REMOTE.PATH,DIRECTION)
```

### SECURE.COPY

Used as a base for the SCP command line utility: 

```
    SUBROUTINE SECURE.COPY(RESULTS,SERVER,USERNAME,PASSWORD,LOCAL.PATH,REMOTE.PATH,DIRECTION)
```

### SECURE.FILE.TRANSFER.PROTOCOL

Used as a base for the SFTP command line utility:

```
    SUBROUTINE SECURE.FILE.TRANSFER.PROTOCOL(RESULTS,SERVER,USERNAME,PASSWORD,LOCAL.PATH,REMOTE.PATH,DIRECTION)
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


