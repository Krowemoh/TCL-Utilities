# NovaShell
A Fancier Shell for MultiValue Databases

## Installation

Clone the repository, copy over a single file and compile it! 

There are compile time directives you can use to compile NovaShell for different database types or different platforms. The default compilation will compile NovaShell for UniVerse on Linux. 

If you are using D3 10.2 or earlier, you'll need to use the compile time directives and also a cleaner program as ifdefs are not respected.

At the command line:
```
$ git clone git@github.com:Krowemoh/NovaShell.git
$ cd NovaShell
$ cp NSH /path/to/BP/NSH
```

In Universe
```
> BASIC BP NSH
> CATALOG BP NSH
> NSH
```

If everything went well, you should now see a brand new prompt!
```
user:13@ACCOUNT>
```

### Compiling NovaShell

Inside the NSH file is compile time directives.

These directives look like this:
```
*
* COMPILER DIRECTIVES
*
$DEFINE DATABASE.UV
$DEFINE PLATFORM.LINUX
*
```

You will need to set a database type and a platform before copying the file to a BP and running `BASIC BP NSH`.

The possible options for the database type are:

```
$DEFINE DATABASE.UV
$DEFINE DATABASE.D3
```

The possible platform options are:
```
$DEFINE PLATFORM.LINUX
$DEFINE PLATFORM.WINDOWS
```

### D3 10.2 Specific Installation

In D3 10.2, you'll need to strip out the ifdefs that unneeded and enable the code blocks manually. There is a python3 script included here you can use to do this.

Once the directives are set in NSH, you can then do the following:

```
$ git clone git@github.com:Krowemoh/NovaShell.git
$ cd NovaShell
# Edit NSH with the correct directives
# Rename NSH to NSH.ORIGINAL
$ python3 undef NSH.ORIGINAL > NSH
```

Now you should have a compile ready version of NSH that you can copy to a BP folder and then compile it through D3.

## Features

- Custom Prompt: Prompt displays username, port and account
- Tab for Suggestions: Allows for commands, files and items inside files to be suggested when you hit tab 
  - If you type LIS at the prompt and then tab, you’ll get a list of all the verbs that start with LIS
  - If you do LIST INVENTORY and then hit tab, you’ll get all the files that start with INVENTORY
  - If you do LIST INVENTORY-FILE AZ12, you’ll get all the items start with AZ12
- Permanent Command History: Save commands to /tmp/.nsh, this can be changed for a more permanent location.
- Up/Down: Allows navigation of the entire command history
- Left/right to edit commands - Edit commands and move the cursor around in a line.
- Man Pages: Displays the help pages in a slightly more straightforward format. 
  - MAN LIST
- Man Basic: Help pages for basic can be found by doing MAN BASIC {ITEM} 
  - MAN BASIC MATREAD
- Linux access: Shortcut to get to Linux and added ability to execute shell commands quickly (Same thing as doing sh in tcl)
  - !curl google.ca

## Hacking
Feel free to open up the file and make any changes you like, please submit them if you think others might get something out of it as well!
