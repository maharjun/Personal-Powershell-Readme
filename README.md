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

5. *Access members of temporary objects*
    
    This is done by surrounding the command that generates the object with brackets 
    (or by passing down the pipeline and calling `Get-Member` if you want to view all 
    members). eg:

        (Get-Alias).definition | clip
        Get-Alias | Get-Member
    
6. *Sorting Array*

    Look at `Sort-Object`

7. *Loading a custom function*

    You have to dot source them i.e. like below:

        . .\build_funtions.ps1
        . .\build_builddefs.ps1
    
8. *Remove a variable*

    Use `Remove-Variable` as below
    
        $A = "New Variable"
        Remove-Variable A
    
    Notice the lack of `$` when removing. this is because we are not reading the value of 
    the symbol, merely erasing the symbol.
    
9.  *Using Assignment with Pipe*
    
    One can use assignment operator along with the pipe concatenator to store the final
    result.
    
        $A = @(1,4,3,2,1,3) | Sort-Object | Get-Unique
    
    the result in `$A` is `1,2,3,4`
    
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

## Branching and Logical Ops

### Comparison Operators

#### Description

  Comparison operators let you specify conditions for comparing values and
  finding values that match specified patterns. To use a comparison operator,
  specify the values that you want to compare together with an operator that
  separates these values.

#### List

    -eq
    -ne
    -gt
    -ge   
    -lt
    -le
    -Like
    -NotLike
    -Match
    -NotMatch
    -Contains
    -NotContains
    -In
    -NotIn
    -Replace

#### Details

1.  
    By default, all comparison operators are case-insensitive. To make a 
    comparison operator case-sensitive, precede the operator name with a "c".
    For Example -

        -eq -> -ceq
    
    To make case-insensitivity explict:
    
        -eq -> -ieq
2.  
    When the input to an operator is a scalar value, comparison operators
    return a Boolean value. When the input is a collection of values, the 
    comparison operators return any matching values. If there are no matches
    in a collection, comparison operators do not return anything. 
    
    The exceptions are the containment operators (`-Contains`, `-NotContains`),
    the In operators (`-In`, `-NotIn`), and the type operators (`-Is`, `-IsNot`),
    which always return a Boolean value.
    
3.  *Operator Details -*
    
        -eq           Equal to. Includes an identical value.
        -ne           Not equal to. Includes a different value.
        -gt           Greater-than.
        -ge           Greater-than or equal to.
        -lt           Less-than.
        -le           Less-than or equal to.
        -like         Match using the wildcard character (*).
        -NotLike      Does not match using the wildcard character (*).
                      
        -Match        Matches a string using regular expressions. 
                      When the input is scalar, it populates the
                      $Matches automatic variable. 
                      
        -NotMatch     Does not Match (using -Match)
                      
        -Contains     Containment operator. Tells whether a collection of reference
                      values includes a single test value. Always returns a Boolean 
                      value. Returns TRUE only when the test value exactly matches  
                      at least one of the reference values.
                      
                      When the test value is a collection, the Contains operator 
                      uses reference equality. It returns TRUE only when one of the 
                      reference values is the same instance of the test value object.
                      
        -NotContains  Negation of -Contains
        -In           Reversed argument list version of -Contains
        -NotIn        Negation of -In

### Logical Operators

     -and     Perform a logical AND of the left and right operands
     -or      Perform a logical OR  of the left and right operands
     -xor     Perform a logical XOR of the left and right operands
     -not     Perform a logical NOT of the left and right operands
     -band    Perform a logical binary AND of the left and right operands
     -bor     Perform a logical binary OR  of the left and right operands
     -bxor    Perform a logical binary XOR of the left and right operands
     -bnot    Perform a logical binary NOT of the left and right operands

### Links
[Comparison Operators](https://technet.microsoft.com/en-us/library/hh847759.aspx)  
[Logical Operators](http://www.techotopia.com/index.php/Basic_Windows_PowerShell_1.0_Operators#The_Addition_Operator)

## Filtering

### Where-Object

This is used to filter objects from a collection using a criterion. It requires the 
collection to be passed using Pipes

e.g.

    $ExeAliases = $AliasList | where{$_.definition -Match ".*\.exe"}

This puts all Aliases into `$ExeAliases` which end in `.exe`

## Regular Expressions

### Named Capture groups

In the case of normal regex, when we use `()` to capture something, we typically use a 
number to refer to them, like `\1`. However, Powershell offers the ability to name these
capture groups using the following syntax

    (?<name> other regex)

note that the angle brackets are not symbolic but actually a part of the syntax. To see it
in work, consider the following example

    $A = @( "SunBurn" , "SunBathe" , "SunLight",
            "SunShine", "SunScreen")
    
    $A | ForEach-Object{
        $_ -match "Sun(?<UniquePart>.*)"
        $X += $Matches.UniquePart
    }
    
    $X

The Capture group can also be accessed as:

    $Matches['UniquePart']
    
The output is:

    Burn
    Bathe
    Light
    Shine
    Screen
    
### Commands inside Strings

Powershell allows us to insert commands inside strings in order to describe what to 
compare against. The syntax is as follows.
    
    "$(Command requence returning object)other stuff"

For example, the function `Get-AliasPattern git` returns
## Modules

### Importing modules
One uses the `Import-Module` command as:

To import the posh-git module:

    $posh_git_folder = "C:\Posh\Git\Folder\Path"
    Import-Module $posh_git_folder\posh-git.psm1

To Import the Powershell Utilities module:

    Import-Module Microsoft.PowerShell.Utility

### Default Module Folder
The environmet variable `PSModulePath` contains the path for the default modules folder. 
It can be viewed as follows:

    $Env:PSModulePath

### Retrieving Existing Modules
Use the `Get-Module` command as:
    
    Get-Module
    Get-Module -Name <Module Name [with wildcards]>

### Links

[Get-Module](https://technet.microsoft.com/en-us/library/hh849700.aspx)  
[Importing Modules](https://msdn.microsoft.com/en-us/library/dd878284(v=vs.85).aspx)

## Common CmdLets:

### Set-Location

#### Description
The `Set-Location` cmdlet is roughly equivalent to the `cd` command found in Cmd.exe. e.g.

    Set-Location c:\scripts

However, you aren’t limited to navigating through the file system (for more information, 
see the `Get-PSDrive` cmdlet). For example, this command places you in the 
`HKEY_CURRENT_USER\Software\Microsoft\Windows` portion of the registry:

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
One use of the Add-Content cmdlet is to append data to a text file. e.g. Add "The End" to 
a text file.

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
[https://technet.microsoft.com/en-us/library/ee156791.aspx](https://technet.microsoft.com/en-us/library/ee156791.aspx)

### Clear-Content

#### Description
The Clear-Content cmdlet enables you to erase the contents of a file without deleting the 
file itself. e.g. clearing a text file -

    Clear-Content c:\scripts\test.txt

It can be used to clear any kind of file though.

#### Aliases
    clc

#### Link
[https://technet.microsoft.com/en-us/library/ee156808.aspx](https://technet.microsoft.com/en-us/library/ee156808.aspx)

### Copy-Item

#### Description
Used to copy a file or folder to a new location. copies the file `Test.txt` from the 
`C:\Scripts` folder to the `C:\Test` folder:

    Copy-Item c:\scripts\test.txt c:\test

Want to copy all the items in C:\Scripts (including subfolders) to C:\Test? Then simply 
use a wildcard character, like so:

    Copy-Item c:\scripts\* c:\test

Finally, this command puts a copy of the folder `C:\Scripts` inside the folder `C:\Test`; 
in other words, the copied information will be copied to a folder named `C:\Test\Scripts`.
Here’s the command:

    Copy-Item c:\scripts c:\test -recurse

#### Aliases
    cpi
    cp
    copy

#### Link
[https://technet.microsoft.com/en-us/library/ee156818.aspx](https://technet.microsoft.com/en-us/library/ee156818.aspx)

### Test-Path
#### Description
Determines whether all elements of a path exist. e.g. the following checks for the 
existence of the function TabExpansion
    
    Test-Path Function:\TabExpansion

#### Link
[https://technet.microsoft.com/en-us/library/hh849776.aspx](https://technet.microsoft.com/en-us/library/hh849776.aspx)

### Rename-Item

#### Description
Renames an item in a Windows PowerShell provider namespace. e.g. renaming of a function:

    Rename-Item Function:\TabExpansion TabExpansionBackup

#### Link
[https://technet.microsoft.com/en-us/library/hh849763.aspx](https://technet.microsoft.com/en-us/library/hh849763.aspx)

### Sort-Object
#### Description
Used to sort a list of objects by properties. Object is passes in pipeline. Default is 
ascending order. for descending, use `-descending` flag.

#### Examples

1. Sorting a List of Numbers in Ascending order
    
        $A = @(1,2,4, 3, 5, 2, 1) | Sort-Object
        $A
    
    Output is:
        
        1 1 2 2 3 4 5 <vertically>
        
2. Sort using property 

        Get-EventLog system -newest 5 | Sort-Object eventid

3. Sort in descending order

        Get-EventLog system -newest 5 | Sort-Object eventid -descending

4. Sort using pairs of properties

        Get-ChildItem c:\scripts | Sort-Object extension,length
   
    Output:
        
        -a---          5/5/2006   9:09 PM      53358 tee.txt
        -a---         5/15/2006   8:57 AM        377 new_excel.vbs
        -a---         3/15/2006  10:23 AM        399 imapi.vbs
        -a---          3/3/2006   2:46 PM        537 methods.vbs
        -a---          3/3/2006   2:55 PM        698 read-write.vbs
        -a---         3/11/2006  10:10 PM        978 imapi2.vbs
        -a---         3/17/2006   8:21 AM       1105 winsat.vbs
        -a---          5/5/2006   2:55 PM      19225 test.vbs
        -a---          4/4/2006   8:30 AM    2616487 HoneyPie.wma

#### Aliases

    sort

#### Link
[Sort-Object](https://technet.microsoft.com/en-us/library/ee176968.aspx)

### Get-Unique

#### Description
Given a sorted collection of items, the Get-Unique cmdlet returns just the unique items 
in that collection. For Example.
    
    $A = @(1,4,2,3,1,4,2,2,3,1,4,3)
    $A | Sort-Object | Get-Unique

The Output is

    1
    2
    3
    4

#### Aliases

    gu

#### Links

[Get-Unique](https://technet.microsoft.com/en-us/library/ee176859.aspx)

### Compare-Object
### Get-History
### Add-History
### Export-Clixml
### Import-Clixml
### Convert-Path
