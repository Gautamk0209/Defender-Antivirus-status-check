<#PSScriptInfo

.VERSION 1.0

.AUTHOR Gautam Kumar

.RELEASENOTES
Version 1.0: Initial version. Tested for limited number of lab devices.

.DESCRIPTION 
This script gets Defender antivirus status from the machine. Designed to be deployed from Configmgr script feature or executed manually on machines.
To use this script in remote machine scenario, please add machine name in the hash table
Signature update age = 0 (less than 1 day old)
Quick scan age = 0 (less than 1 day old)

Please test the script in your env before using for bigger chunk of device as entire risk arising
out of the use or performance of the sample script and documentation remains with you.

#> 

try{
#Getting the Antivirus status
$currentstate = Get-WmiObject -Namespace root\Microsoft\SecurityClient -class AntimalwareHealthStatus

#Getting the signature update difference from current date
[datetime]$currentsigtime = $currentstate.AntispywareSignatureUpdateDateTime
$Siglastupdate = ((get-date) - ($currentsigtime))
}
Catch{
Write-Output "Error getting main info : $_.Exception.Message"
}
#conditonal check and output
If(($currentstate.AntivirusEnabled -eq $true) -and ($Siglastupdate.Days -le 3 ) -and ($currentstate.LastQuickScanAge -le 3))
{

    $Prop = [ordered]@{              
        'Status'            = 'Healthy'
        'Antivirus status'  = "< Antivirusenabled = $($currentstate.AntivirusEnabled) > < Antinspywareenabled = $($currentstate.AntispywareEnabled) >"
        'Enabled'           =  $currentstate.Enabled
        'Signature update version' = $currentstate.AntispywareSignatureVersion 
        'Signature update age'     = $Siglastupdate.Days
        'Quick scan age'               = $currentstate.LastQuickScanAge
    }

    $Obj = New-Object -TypeName PSObject -Property $Prop 
    Write-Output $Obj
}
Else {
    $Prop = [ordered]@{              
        'Status'            = 'Unhealthy'
        'Antivirus status'  = "<Antivirusenabled= $($currentstate.AntivirusEnabled) > <Antinspywareenabled = $($currentstate.AntispywareEnabled)>"
        'Enabled'           =  $currentstate.Enabled
        'Signature update version' = $currentstate.AntispywareSignatureVersion 
        'Signature update age'     = $Siglastupdate.Days
        'Quick scan age'           = $currentstate.LastQuickScanAge
    }

    $Obj = New-Object -TypeName PSObject -Property $Prop 
    Write-Output $Obj

    }
