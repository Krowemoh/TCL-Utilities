# TCL-Utilities

Helpful tools for Pick

## Tools

| Tools | Description |
|---------|-------------|
|EVA|Vim like editor for Pick|
|NPM|Package manager to install the packages listed here|
|NSH|Fish like shell for Pick|

## Commands

A set of utilities to make PICK easier to manage. Square brackets mean the word is optional. Curly braces are where you can substitute what you want in the command.

| Command | Description |
|---------|-------------|
| ADD-DICT | Add dictionary entries to a file |
| ADD-PATH|Add a F Pointer to a Linux Path|
| ARCHIVE | Archive records |
| CAT|Print a record to the screen|
| CLIZER.DRIVER|Update a record from TCL|
| CLONE|Copy records in PICK|
| DEPRECOMPILE|Deprecompile BASIC programs|
| EVENT|Create events and added to an EVENT-FILE|
| FTP|Upload and download files using ftp|
| GREP|Regex search in BASIC, ^.*$|
| IMPORT-TEMP|Import records from IMPORT-TEMP-FILE|
| MOVE|Move records around in PICK|
| NEW-FILE|Create files with a default|
| OS.RELEASE|Show OS information|
| PRECOMPILE|Precompile BASIC programs|
| PS2|List UniVerse port information|
| RENAME|Rename files and dictionaries|
| SCP|Upload and download files using ssh|
| UV.LICENSE|Show UniVerse license information|

## Subroutines

|Subroutine|Description|
|----------|-----------|
|CLIZER||
|GET.ARGUMENTS||
|GET.NEXT.ID||
|MOVE.FILES||
|FILE.TRANSFER.PROTOCOL||
|FILE.TRANSFER.PROTOCOL.SECURE||
|MD5||
|RENDER.MARKDOWN||
|SECURE.COPY||
|SECURE.FILE.TRANSFER.PROTOCOL||
|SHA1||
|SHA256||
|SHA512||
|URLDECODE||
|URLENCODE||

## JSON

|Subroutine|Description|
|----------|-----------|
|JSON.ADD||
|JSON.CREATE.ARRAY||
|JSON.CREATE.OBJECT||
|JSON.PARSE||
|JSON.QUERY||

## Data Structures

|Subroutine|Description|
|----------|-----------|
|MAP.SET|
|MAP.HAS||
|MAP.GET||
|MAP.DELETE||
|STACK.PUSH||
|STACK.POP||
|STACK.PEEK||

## File Operations

|Subroutine|Description|
|----------|-----------|
|LIST.DIRECTORY||

## Web

|Program|Description|
|----------|-----------|
|BITE-THE-APPLE||
|SERAPHIM||
|METATRON||

### ADD-DICT

Add a A type and an S type PICK style dictionary to a file:

```
FLAGS: 
   -F,  --FILE.NAME 
   -A,  --ATTRIBUTE.NUMBER
   -N,  --ATTRIBUTE.NAME
   -CV, --CONVERSION
   -CR, --CORRELATIVE
   -J,  --JUSTIFICATION
   -L,  --LENGTH

EXAMPLES: 
   ADD-DICT -F {FILE-NAME} -A {ATTR.#} -N {DICT.NAME}
   ADD-DICT -F {FILE-NAME} -A {ATTR.#} -N {DICT.NAME} -J R -CV MD2
```

### ADD-PATH

Add a F Pointer to a Linux Path

```
    ADD-PATH {MD.NAME} {/path/to/folder}
```

### ARCHIVE

Archive records to a ARCHIVE-FILENAME file:

```
ARCHIVE BP TEST
```

### CAT

Print a record to the screen:

```
    CAT [DICT] {FILE} {RECORD}
    CAT [DICT] {FILE}
```

### CLIZER.DRIVER

Update a record from TCL:

```
    CLIZER.DRIVER SOMETHING-FILE --ATTRIBUTE.NAME VALUE
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

### DEPRECOMPILE

Deprecompile a BASIC program that has $FILE-NAME:

```
DEPRECOMPILE BP TEST
```

### EVENT

Create events and added to an EVENT-FILE:

```
    EVENT --SUBJECT "This is an event" --START-DATE "12AUG2024" --START-TIME 14:30
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

### IMPORT-TEMP

Import records from IMPORT-TEMP-FILE

```
   Import all records in IMPORT-TEMP-FILE
        IMPORT-TEMP {DEST.FILE}

   Import a specific record from IMPORT-TEMP-FILE
        IMPORT-TEMP {DEST.FILE} {RECORD}
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

### OS.RELEASE

Show OS information:

```
OS.RELEASE
```

### PRECOMPILE

Precompile a BASIC program that has $FILE-NAME

```
PRECOMPILE BP TEST
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

### UV.LICENSE

Show UniVerse license information:

```
UV.LICENSE
```

## Library Subroutines

### CLIZER

Create a CLI interface based on a file dictionary:

```
   SUBROUTINE CLIZER(FILENAME,ARGS)
```

### GET.ARGUMENTS

Used for argument parsing, ARGS will be an attribute marked list of what was typed in at TCL.

```
    CALL GET.ARGUMENTS(ARGS)
```

### GET.NEXT.ID

Get next available id, searches the entire file and finds the largest key currently.

```
    CALL GET.NEXT.ID(NEXT.ID,FILE)
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

### FILE.TRANSFER.PROTOCOL.SECURE

Used as a base for the FTPS command line utility:

```
    SUBROUTINE FILE.TRANSFER.PROTOCOL.SECURE(RESULTS,SERVER,USERNAME,PASSWORD,LOCAL.PATH,REMOTE.PATH,DIRECTION)
```

### MD5

MD5 hashing:

```
    SUBROUTINE MD5(TEXT,HASH)
```

#### RENDER.MARKDOWN

Render markdown to html:

```
   SUBROUTINE RENDER.MARKDOWN(MARKDOWN,HTML)
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

### SHA1

SHA1 hashing:

```
    SUBROUTINE SHA1(TEXT,HASH)
```

### SHA256

SHA256 hashing:

```
    SUBROUTINE SHA256(TEXT,HASH)
```

### SHA512

SHA512 hashing:

```
    SUBROUTINE SHA512(TEXT,HASH)
```

### URLDECODE

Percent decode a URL:

```
    SUBROUTINE URLDECODE(TEXT,URL)
```

### URLENCODE

Percent encode a URL:

```
    SUBROUTINE URLENCODE(TEXT,URL)
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

## Data Structures

#### MAP.SET

Add a key to a hash map:

```
    SUBROUTINE MAP.SET(MAT MAP,SIZE,KEY,VALUE)
```

#### MAP.HAS

Check if a hash map contains a key:

```
    SUBROUTINE MAP.HAS(MAT MAP,SIZE,KEY,FLAG)
```

#### MAP.GET

Get a key from a hash map:

```
    SUBROUTINE MAP.GET(MAT MAP,SIZE,KEY,VALUE)
```

#### MAP.DELETE

Delete a key from a hash map:

```
    SUBROUTINE MAP.DELETE(MAT MAP,SIZE,KEY)
```

#### STACK.PUSH

Push something onto the stack:

```
    SUBROUTINE STACK.PUSH(MAT STACK,SIZE,VALUE)
```
#### STACK.POP

Pop something off the stack:

```
    SUBROUTINE STACK.POP(MAT STACK,SIZE,VALUE)
```

#### STACK.PEEK

Get something from the stack without popping it:

```
    SUBROUTINE STACK.PEEK(MAT STACK,SIZE,VALUE)
```
## File Operations

#### LIST.DIRECTORY

List a directory, this returns a list of paths and their types:

```
    SUBROUTINE LIST.DIRECTORY(PATH,FILE.LIST)
```

## Programs


#### BITE-THE-APPLE

Set up a SERAPHIM project:

```
    RUN BP BITE-THE-APPLE
```

#### SERAPHIM

A simple web server:

```
    SERAPHIM {APPLICATION}
```

#### METATRON

Manage SERAPHIM Servers:

```
    METATRON START {APPLICATION}
    METATRON STOP {APPLICATION}
```
