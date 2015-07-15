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

## Arrays:

### Creating Arrays
1.  
    To create an Array just separate the elements with commas. Create an array named `$myArray` containing elements with a mix of data types:

        $myArray = 1,"Hello",3.5,"World"

2.  
    Or using explicit syntax:
    
        $myArray = @(1,"Hello",3.5,"World")
    
3.  
    To distribute the values back into individual variables:

        $var1,$var2,$var3,$var4=$myArray
4.  
    Create an array containing several numeric (int) values:

        $myArray = 1,2,3,4,5,6,7
    
    or using the range operator (..):
    
        $myArray = (1..7)
    
    or strongly typed:

        [int[]] $myArray = 12,64,8,64,12   

5.  
    Create an empty array:
       
        $myArray = @()

6.  
    Create an array with a single element:
    
        $myArray = @("Hello World")

7.  
    Create a Multi-dimensional array:

        $myMultiArray = @(
                     (1,2,3),
                     (40,50,60)
           )

### Add Values to Arrays

1.  *Concatenating Arrays -*
    
    This is done using the + operator. consider the following code:
    
        $A = @(1, 2)
        $B = @(3, 4)
        $C = $A + $B
        $C
    
    This code outputs:
        
        1
        2
        3
        4
    
2.  *Appending to the end of an Array -*
    
    As a corollary f the above, this is achieved by using the `+=` operator.

### Retrieve elements from array.

1.  Return All Elements:

        $myArray

2.  Return 3rd Element:

        $myArray[3]

3.  Return the 5th element through to the 10th element (inclusive) in an array:

        $myArray[4..9]

## Common CmdLets:

### Set-Location

#### Description
The `Set-Location` cmdlet is roughly equivalent to the `cd` command found in Cmd.exe. e.g.

    Set-Location c:\scripts

However, you aren’t limited to navigating through the file system (for more information, see the `Get-PSDrive` cmdlet). For example, this command places you in the `HKEY_CURRENT_USER\Software\Microsoft\Windows` portion of the registry:

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
