# Zero to Hero > 01 - Training Notes > PowerShell
## 02 - PowerShell Training > 00 - Fundamentals of PowerShell

| LinkedIn Training  | Intro to PowerShell 7 | PowerShell 7 Essential Training |
|:------------------|:----------:|:----------:|

## Overview / Summary
| Commands Learned | Specific Notes | Extended Notes |
| :----| :------- | :--- |
| Import-Module -ListAvailable  -PSDiagnostics | Add a Pipeline Operator ( &#124; ) to export into next section | Setting up PowerShell / Intro |
| Enable/Disable-PSTrace| Get-Module -module| Get Module Help/Notes |
| Import-Module [ModuleName] &#124; Get-Module &#124; ft | Modify & import only needed functions/cmdlets of modules. | Modify with operators below to find similar modules vs commands|
| Get-Command \*PSTrace\* &#124; ft | Get all commands that contain PSTrace | Modify to other commands found |
| Initialize Variables with variable = $variableName] | Applies to elements ($variableName.element1.subProperty) | Critical to scripting |

```powershell
Import-Module PSDiagnostics -function Disable-PSTrace, Enable-PSTrace	
```

- After you import the module, you add a -prefix on the module & anytime you Get-Module, it will print only that list of active functions/cmdlets imported/not disabled.

```powershell
$session = New-PSSession - ComputerName PC#1
Get-Module -PSSession $session -ListAvailable -Name PSDiagnostics
```



| Discovering Commands | Specific Notes | Extended Notes |
| :----| :------- | :--- |
| Get-Help | Get-Help cmdlet | Get-Help  Custom Formatting |
| Get-Command | Returns PSGetCommandInfo | View more elements in sysAudits |
| Get-ChildItem | Set-Location  | Note: -PassThru Auth |
| Set-Location | Operators: =,?,[] | Zero or More (=) : Exactly 1 (?) : String Matching ( [] ) |


```PowerShell
# Utilize PSGetCommandInfo & pipeline for more granular control of elements in a system
Get-Command -Verb Add / Get-Command -Noun Computer / Get-Command -CommandType Cmdlet
Get-Command *event* / Find-Command #Gets PSGetCommandInfo
```
### General Get-Help Examples
```PowerShell
Get-Help Format-Table | Out-Host -paging or -help [cmdlet]
Get-Help Format-Table Detailed #Full or Detailed
#Example Use
Get-Help Get-ChildItem -parameter *
Get-Help *
Get-Help -Name remoting 
```

### Utilizing Variables & Pipelines
```PowerShell
 ($_parameterB.count = Import-Csv -FilePath | Get-Service | Sort-Object -property Status #Sorting, Variables, Exporting Chain

Get-Member # Give methods & properties
Select-Object # Give properties but not members.
```

### Get all services, exclude stopped services, show display names only:

```PowerShell
Get-Service | WHERE {$_.status -eq 'Running'} | SELECT displayName | Export-CSV or Out-File "C:\file1"
```

#### Terse CMDs = "Format-Table" to  'ft' / Get-Command to gcm
```PowerShell
Get-Command | Where-Object { $_.parameterSets.count -gt 2 } | format-list Name
```
- Turns into:
```PowerShell
Gcm | ? { $_.parameterSets.count -gt 2 } fl Name
Write-Output = echo
```


### PowerShell Objects
Use this when looking for unknown elements of a service	
```PowerShell
Get-Service | Where-Object { $_.Status }
Get-Service | Where-Object { $_.Status -eq "Running"} | Select-Object DisplayName

# When looking for more details / service details
Get-Service -ServiceName 'Dnscache' | Select-Object -Property 'StartType'

# Set a variable & then use a sub-element to pull details
$svcDNSCache = Get-Service -ServiceName 'Dnscache'
$svcDNSCache.SubElement

# Manually inspect processes
Get-Service | Get-Member -MemberType 'Method'
Get-Service -serviceName * | Select-Object -Property 'Status', 'DisplayName' | Sort-Object -property 'Status' -Descending

```

How to improve in my day-to-day?
- Setup a lab of virtual machines
- PSRemote from DC to Member Server to Workstation
- Be able to copy a powershell script from domain controller down.
- Get a list of my administrative duties & see if any can be shortened via PowerShell

## Advanced-PowerShell-Functions
```PowerShell
[CmdletBinding()] Param() #Function Basic
```

```PowerShell
Function Stop-Everything {
[CmdletBinding(SupportsShouldProcess)] Param()
Get-Service * | Stop-Service
}
```


- Open Arrays /PowerShell-Variable-Storage/Array-Storage / Readdressing of PowerShell-Arrays
- Elements of an array are stored as $regPowerShellArray.storedElements
- $JSONarray = @{}
- $regPowerShellArray = @()
- Example of $Array.Element Manipulation

```powershell
$XMLPath = 'C:\Temp\XMLTest.XML'
$ClientList = [xml](Get-Content $XMLPath)
$UserIwantToModify = $ClientList.SetB_Roster.Subset_Employee[2002] #Value in array
$UserIwantToModify.jobTitle="Terminated!" #Even if this value doesn't exist, you will end up creating a new sub element.
$ClientList.Save($XMLPath) 

#This will all be for nothing unless you save the file. Everything is stored local until then.
Get-Service | ConvertTo-Json | Out-File C:\Temp\jsonTest.json
```
- WebInvoke = JSON = REST API Applications

## Pipelining
The basics of pipelining to an element + Advanced-Get-ChildItems:
```PowerShell
Get-Command | Where-Object {$_.Name -like '*Invoke*' } #Setting multiple parameters/blocks

Get-Command | Where-Object { () -and/ne/like () } # Basic Syntax

```

### Step-by-Step Feature / Inclusion of PowerShell's Get-ChildItem cmdlets
```PowerShell
Get-ChildItem C:\ -Name
Get-ChildItem Path C:\Documents\[path] -Recursive -Force
Get-ChildItem -Path $Location -Recurse -Include *.xlsx -Force 
# Swap -Include for -Exclude

Get-ChildItem -path $Location -Recurse | Where-Object -FilterScript { ($_.LastWriteTime -eq '2023-01-01') -and (FIELD TWO) }

Get-ChildItem -path $Location -Recurse | Where-Object -FilterScript { (FIELD ONE) -and (FIELD TWO) }

New-Item -path "$($Location)\PSFolderNew" -ItemType Directory -Force
New-Item -path "$($Location)\PSFolderNew\PSFile.txt" -ItemType File -Force
$document = 'myTestDocument.doc' 
$document | Out-File -FilePath "$($Location)\PSDocument.txt"

Remove-Item -path "$($Location)\PSDocument.txt\"
Copy-Item -path "$($Location)\PSDocument.txt\" -Destination "$($Location)\Users\PSDocument-Copy.txt"
Get-ChildItem "$($Location)\TextFiles\*.txt" | Rename-Item -NewName {$_.Name -replace .\.txt$','.bak'}
```

### Import CSV & Write-Host
```PowerShell
Import-Csv "$($Location)\Employees.csv" | ForEach-Object 
{Write-Host "$($_.FirstName), $($_.LastName) born $($_.DateOfBirth)"}
```

### How to use: -WhatIf & -Confirm

- Whatif will show what process is going to occur
- Confirm will show you what process is going to occur but then ask for permission before accessing / changing the data > Changes data

#### Converting to HTML | Controller Scripting | Automated Webpage Deployment

```PowerShell
[INSERT_SCRIPT_TO_CONVERT] | ConvertTo-HTML | Out-File -path C:\Temp\doc.html
```

- This allows you to create HTML files that output as a webpage. 
- You can routinely run reports that allow a web page to be updated  "on-the-fly" i.e. Controller Scripting

```
Txt = White Space
CSV = Punctuation
XML = Markup language / Tables of data
HTML = <table> <tr> <td>
XML = Similar to HTML, but more broken down <XML Version> <Tag1> <Subtag1>
```

## XML Data Step-by-Step
```powershell
$XMLPath = 'C:\Temp\XML.xml'
$EmployeeRoster = [XML](Get-Content $XMLPath)
$EmployeeRoster.roster.employee | Format-Table
You can use
$EmployeeRoster.SelectNodes("//name")	Writing to XML Configuration:
$XMLPath = 'C:\Temp\XMLTest.XML'
$EmployeeRoster = [xml](Get-Content $XMLPath)
$EmployeeRoster.roster.employee | Format-Table
- When working with individual users, set them into a variable.
$Bernie = $EmployeeRoster.roster.employee[7] // Check for Bernie with typing $Bernie
Bernie.jobdescription="Landscaping" // To update this information:
$EmployeeRoster.Save($XMLPath) 
/**Save XML File (w/o doing will cause error) & xpath syntax, where any tag related to name will pull**/
$EmployeeRoster.SelectSingleNode ("//employee[4]") 

```
- In XML - the listArray starts @ [1] 
- PowerShell - listArray starts @ [0] 
## XML Data / Advanced Information

- Child Elements of each Primary Element
- How to add a child element into their record:	
- "JavaScript Object Notation" = .JSON
- JSON or XML = Multiple data types, data and objects, and hierarchical structure.
- You can then ADD or REMOVE XML Information. 
Test it: 
```powershell
Get-Service | Export-CLIxml C:\Temp\xmlTest.xml
Get-Service | ConvertTo-Json | Out-File C:\Temp\jsonTest.json
$XMLPath = 'C:\Temp\XMLTest.XML'
$EmployeeRoster = [xml](Get-Content $XMLPath)	
$Bernie = $EmployeeRoster.roster.employee[7]	  
$Bernie.AppendChild($EmployeeRoster.CreateElement("pe_1year"))	
$Bernie.pe_1year = "7/18/2018 Exceeds"
$NewHire = $Bernie.Clone()  //Add a new record (CLONE METHOD)	
$NewHire.Id/Name/JD/DateofHire/EmailAddress = 'value'
$EmployeeRoster.Roster.AppendChild($NewHire)
DO NOT FORGET: $EmployeeRoster.Save($XMLPath)

```   
### XML vs JSON 
XML: Better cmdlet support, more robust 
XML: Normally larger file size than JSON   
XML: More header information & tags to close/open each element
JSON: Sits in between Get-Something | ConvertFrom-Json | Do-Something
JSON: Smaller file, faster web transfer
JSON: Nesting elements, but XML has much better machine level support (e.g) Web-Invoke of REST API

```powershell
Invoke-WebRequest -Method get -URI HTTPS://[siteinformation].com/api/posts/all 
| Select-Object -ExpandProperty Content | ConvertFrom-Json | ft
```

### Utilizing JSON 
```json
Start the script with 
@"
[
 {
    "firstName":"Robert"
}
]
"@ | ConvertFrom-Json | FT

```