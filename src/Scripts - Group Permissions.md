<#
.SYNOPSIS
Gets all of a user or groups permissions and returns an array

.DESCRIPTION
This will check an array of folders, including everything under those folders,
for folders that are not inheriting permissions and the identity listed matches
the one provided, then return an array of the folders found.

.PARAMETER ShareArray
This has to be an array of all the folders you want checked. 

It should have at least two columns:

Name: The name of the folder, used only for reference.
Path: The full path to the folder

.PARAMETER IdentityReference
The identity you want to check permissions for. It should be in the format "DOMAIN\USERNAME"

.EXAMPLE
$ShareArray = Import-Csv ".\ShareArray.csv"

$ShareArray

Name  Path
----  ----
Users E:\Home Directory

Get-UserPermissions -ShareArray $ShareArray -IdentityReference "DOMAIN\GroupName"

FolderName        : \\?\E:\Home Directory\Test
IdentityReference : DOMAIN\GroupName
FileSystemRights  : FullControl
AccessControlType : Allow
InheritanceFlags  : ContainerInherit, ObjectInherit

.NOTES
General notes
#>
function Get-UserPermissions {
    [CmdletBinding()]
    param (
        # A 2d array of shares with the columns "Name" and "Path"
        [Parameter(Mandatory)][array]$ShareArray,
        # A group to check permissions for, in the format DOMAIN\Name
        [Parameter(Mandatory)][string]$IdentityReference
    )
    
    begin {
        <# Add test to make sure column names are correct
        if (-not($ShareArray.Name -and $ShareArray.Path)) {
            $Message = "The imported array does not use the column titles 'Name' and 'Path'. Please update the column titles and try again."
            Write-Error -Message $Message
            Return
        } else {
            $Message = "Column titles are correct."
            Write-Host $Message
        } #>
    }
    
    process {
        $CounterA = 0
        $Output = @()

        ForEach ($Share in $ShareArray) {
            $CounterA++
            $ActivityA = "Processing " + $Share.Name + " Share"
            Write-Progress -Activity $ActivityA -PercentComplete (($CounterA / $ShareArray.count) * 100) -Status (($CounterA / $ShareArray.count) * 100)
            $SharePath = "\\?\" + $Share.Path
            $FolderPath = @(Get-Item -Path $SharePath  | Where-Object {(Get-Acl -Path $_.FullName).Access.IdentityReference -eq $IdentityReference})
            $FolderPath += Get-ChildItem -Directory -Path $SharePath -Recurse -Force | Where-Object {((Get-Acl -Path $_.FullName).Access.IsInherited -eq $false) -and ((Get-Acl -Path $_.FullName).Access.IdentityReference -eq $IdentityReference)}
            $CounterB = 0
            ForEach ($Folder in $FolderPath) {
                $CounterB++
                $ActivityB = "Processing " + $Folder.Name + " Folder"
                Write-Progress -Id 1 -Activity $ActivityB -Status (($CounterB / $FolderPath.count) * 100) -PercentComplete (($CounterB / $FolderPath.count) * 100)
                $ACLPath = $Folder.FullName
                $Acl = Get-Acl -Path $ACLPath
                $CounterC = 0
                ForEach ($Access in ($Acl.Access | Where-Object {$_.IdentityReference -eq $IdentityReference})) {
                    $CounterC++
                    $ActivityC = "Processing " + $Acl.Path + " ACL for " + $Access.IdentityReference
                    Write-Verbose $ActivityC
                    Write-Progress -Id 2 -Activity $ActivityC -Status (($CounterC / $Acl.Access.count) * 100) -PercentComplete (($CounterC / $Acl.Access.count) * 100)
                    $Properties = [ordered]@{
                        'FolderName'=$Folder.FullName;
                        'IdentityReference'=$Access.IdentityReference;
                        'FileSystemRights'=$Access.FileSystemRights;
                        'AccessControlType'=$Access.AccessControlType;
                        'InheritanceFlags'=$Access.InheritanceFlags
                    }
                    $Output += New-Object -TypeName PSObject -Property $Properties
                }
            }
        }
    Write-Verbose $Output.count
    Return $Output
    }
    
    end {
        
    }
}
