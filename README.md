# Personal-Powershell-Readme
Contains all knowledge about Powershell and command line that i know.

## Basic Tricks
1. *Showing All The functions*
    
        dir function:
    
    It appears to be the case that the functions are stored in a drive `function:` and listing them out lists all the functions.
    
2. *Start a new powershell window from powershell*
    
    Any one of the following:
    
        start-process powershell
        start powershell

3. *List Environment Variable*
    
    The `$Env` object contains the environment variables. e.g. in order to display the `PATH` environment variable,
    
        $Env:path

4. *Show Function Code*
    
    The `$Function` object contains all the functions. In order to display the code of a function `mkdir`, we do:
    
        $Function:mkdir
    
    However, if the function name contains `-`, then we must use `{}` as:
    
        ${Function:Get-AliasPattern}

## Common CmdLets:

### Set-Location

#### Description
The `Set-Location` cmdlet is roughly equivalent to the `cd` command found in Cmd.exe. e.g.

    Set-Location c:\scripts

However, you aren’t limited to navigating through the file system (for more information, see the Get-PSDrive cmdlet). For example, this command places you in the HKEY\_CURRENT\_USER\Software\Microsoft\Windows portion of the registry:

    Set-Location HKCU:\Software\Microsoft\Windows

#### Aliases:

    sl
    cd
    pwd
    chdir

#### Link
  [https://technet.microsoft.com/en-us/library/ee176962.aspx](https://technet.microsoft.com/en-us/library/ee176962.aspx)

### Add-Content

#### Description
One use of the Add-Content cmdlet is to append data to a text file. e.g. Add "The End" to a text file.

    Add-Content c:\scripts\test.txt "The End"

The string can contain special characters. e.g. Appending on a new line

    Add-Content c:\scripts\test.txt "`nThe End"

Or the filenames can contain wildcards e.g. Appending a timestamp

    $A = Get-Date; Add-Content c:\scripts\*.log $A
    
#### Special Characters
The following Special characters can be used in the inserted string

    `0 -- Null
    `a -- Alert           (exclusive to powershell console. causes beep)
    `b -- Backspace
    `n -- New line
    `r -- Carriage return
    `t -- Horizontal tab
    `' -- Single quote
    `" -- Double quote

#### Aliases
    ac

#### Link
[https://technet.microsoft.com/en-us/library/ee156791.aspx]()

### Clear-Content

#### Description
The Clear-Content cmdlet enables you to erase the contents of a file without deleting the file itself. e.g. clearing a text file -

    Clear-Content c:\scripts\test.txt

It can be used to clear any kind of file though.

#### Aliases
    clc

#### Link
[https://technet.microsoft.com/en-us/library/ee156808.aspx]()

### Copy-Item

#### Description
Used to copy a file or folder to a new location. copies the file `Test.txt` from the `C:\Scripts` folder to the `C:\Test` folder:

    Copy-Item c:\scripts\test.txt c:\test

Want to copy all the items in C:\Scripts (including subfolders) to C:\Test? Then simply use a wildcard character, like so:

    Copy-Item c:\scripts\* c:\test

Finally, this command puts a copy of the folder `C:\Scripts` inside the folder `C:\Test`; in other words, the copied information will be copied to a folder named `C:\Test\Scripts`. Here’s the command:

    Copy-Item c:\scripts c:\test -recurse

#### Aliases
    cpi
    cp
    copy

#### Link
[https://technet.microsoft.com/en-us/library/ee156818.aspx]()

### Compare-Object
### Get-History
### Add-History
### Export-Clixml
### Import-Clixml
### Convert-Path
