---
sidebar_position: 3
---

# Tutorial: Manufacturing Order Status Update

Here you can find a complete description of the Manufacturing Order Status Update script.

---

Clear-Host is clearing console output. The rest are comments regarding the script and the script version:

```powershell showLineNumbers
#region #Script info
Clear-Host
########################################################################
# CompuTec PowerShell Script - Manufacturing Order Status Update - tutorial
########################################################################
$SCRIPT_VERSION = "3.0"
# Last tested PF version: ProcessForce 9.3 (9.30.140) PL: 04 R1 HF1 (64-bit)
# Description:
#      Base on SQL Query this script will update Manufacturing Order Status.
#      SQL Query can be easily changed - it must return three columns:
#        * DocEntry - Manufacturing Order DocEntry
#        * DocNum - Manufacuturing Order Document Number
#        * StatusCode - Manufacuturing Order Status
# Troubleshooting:
#   https://connect.computec.pl/display/PF930EN/FAQ+PowerShell
########################################################################
Write-Host -backgroundcolor Yellow -foregroundcolor DarkBlue ("Script Version:" + $SCRIPT_VERSION)
#endregion
 
#region #PF API library usage
# You need to check in what architecture PowerShell ISE is running (x64 or x86),
# you need run PowerShell ISE in the same architecture like PF API is installed (check in Windows -> Programs & Features)
# Examples:
#     SAP Client x64 + PF x64 installed on DB/Company => PF API x64 => Windows PowerShell ISE
#     SAP Client x86 + PF x86 installed on DB/Company => PF API x86 => Windows PowerShell ISE x86
```

Loading ProcessForce API library. This library is used to connect to SAP Business One and perform tasks on ProcessForce objects:

```powershell
[System.Reflection.Assembly]::LoadWithPartialName("CompuTec.ProcessForce.API")
```

Defining directory where configuration.xml file is presented. This file contains connection details like server address, username etc.:

```powershell
#region #Datbase/Company connection settings
$csvImportCatalog = $PSScriptRoot + "\"
#If you are using lower version of PowerShell than PowerShell 4.0 you can use static path
# $csvImportCatalog = "C:\PS\PF\";
 
#configuration xml
```

Checking if the path to the configuration.xml file is correct:

```powershell
$configurationXMLFilePath = -join ($csvImportCatalog, "configuration.xml");
if (!(Test-Path $configurationXMLFilePath -PathType Leaf)) {
    Write-Host -BackgroundColor Red ([string]::Format("File: {0} don't exists.", $configurationXMLFilePath));
    return;
}
```

Loading connection details from the configuration.xml file:

```powershell
[xml] $configurationXml = Get-Content -Encoding UTF8 $configurationXMLFilePath
$xmlConnection = $configurationXml.SelectSingleNode("/configuration/connection");
```

Asking the user for confirmation. We are presenting connection information and asking if the user wants to continue. The purpose of this step is to prevent running the script on the wrong database:

```powershell
$pfcCompany = [CompuTec.ProcessForce.API.ProcessForceCompanyInitializator]::CreateCompany()
```

Connection properties are assigned from the configuration.xml file to a Company object:

```powershell
$pfcCompany.LicenseServer = $xmlConnection.LicenseServer;
$pfcCompany.SQLServer = $xmlConnection.SQLServer;
$pfcCompany.DbServerType = [SAPbobsCOM.BoDataServerTypes]::[string]$xmlConnection.DbServerType;
$pfcCompany.Databasename = $xmlConnection.Database;
$pfcCompany.UserName = $xmlConnection.UserName;
$pfcCompany.Password = $xmlConnection.Password;
```

This method allows checking the ProcessForce add-on version:

```powershell
write-host -backgroundcolor yellow -foregroundcolor black  "Trying connect..."
$version = [CompuTec.Core.CoreConfiguration+DatabaseSetup]::AddonVersion
write-host -backgroundcolor green -foregroundcolor black "PF API Library:" $version';' 'Host:'(Get-WmiObject Win32_OperatingSystem).CSName';' 'OSArchitecture:' (Get-WmiObject Win32_OperatingSystem).OSArchitecture
```

Connecting to the company. We are doing this in a try-catch block that allows us to catch any exception and maintain it in a catch block:

```powershell
try {
    [Diagnostics.CodeAnalysis.SuppressMessageAttribute('PSUseDeclaredVarsMoreThanAssignments', 'code')]
    $code = $pfcCompany.Connect()
  
    write-host -backgroundcolor green -foregroundcolor black "Connected to:" $pfcCompany.SapCompany.CompanyName "/ " $pfcCompany.SapCompany.CompanyDB"" "Sap Company version: " $pfcCompany.SapCompany.Version
}
```

If there was any exception during connection, a message about it is displayed to a user:

```powershell
catch {
    #Show error messages & stop the script
    write-host "Connection Failure: " -backgroundcolor red -foregroundcolor white $_.Exception.Message
  
    write-host "LicenseServer:" $pfcCompany.LicenseServer
    write-host "SQLServer:" $pfcCompany.SQLServer
    write-host "DbServerType:" $pfcCompany.DbServerType
    write-host "Databasename" $pfcCompany.Databasename
    write-host "UserName:" $pfcCompany.UserName
}
```

An additional check if we are connected to a company:

```powershell
#If company is not connected - stops the script
if (-not $pfcCompany.IsConnected) {
    write-host -backgroundcolor yellow -foregroundcolor black "Company is not connected"
    return
}
```

Starting a try block:

```powershell
try {
```

This is an SQL query command that is executed to get information from a database. Here you can specify any query you like. Please make sure that this query returns the following columns "DocEntry", "DocNum", "StatusCode" as these are expected by the script:

```powershell
$SQLQuery = "SELECT ""DocEntry"", ""DocNum"", 'CL' AS ""StatusCode"" FROM ""@CT_PF_OMOR"" WHERE ""U_RequiredDate"" < '2018-07-01' AND ""U_Status"" = 'FI' ";
```

A QueryManager object is created using ProcessForce API. This object will be used for running the SQL query defined above:

```powershell
$queryManager = New-Object 'CompuTec.Core.DI.Database.QueryManager'
```

We assign our query to the QueryManager object:

```powershell
$queryManager.CommandText = $SQLQuery;
```

Actual query execution. Results are assigned to the $recordSet variable:

```powershell
$recordSet = $queryManager.Execute($pfcCompany.Token);
```

Getting information about the number of returned recordSet:

```powershell
$recordCount = $recordSet.RecordCount;
```

Checking if the number of records is greater than 0. If this is true, proceed:

```powershell
if ($recordCount -gt 0 ) {
```

Starting loop in which we will read returned records one after another until the end and use information from them to update Manufacturing Order:

```powershell
while (!$recordSet.EoF) {
```

Starting try block inside the loop. We add this additional try-catch block inside the loop to be able to continue updating Manufacturing Orders even if, for some of them update will result in failure:

```powershell
try {
```

Getting DocEntry, DocNum and StatusCode from the current recordSet:

```powershell
$DocEntry = $recordSet.Fields.Item('DocEntry').Value;
$DocNum = $recordSet.Fields.Item('DocNum').Value;
$Status = $recordSet.Fields.Item('StatusCode').Value;
```

Creation of Manufacturing Order Object. This is done using CreatePFObject method on a Company object. It is possible to create different ProcessForce objects using this method. To create a different object, you will need to change ManufacturingOrder to desired object code. For example, for ItemDetails it will look like this: $pfcCompany.CreatePFObject([CompuTec.ProcessForce.API.Core.ObjectTypes]::ItemDetails):

```powershell
$mo = $pfcCompany.CreatePFObject([CompuTec.ProcessForce.API.Core.ObjectTypes]::ManufacturingOrder)
```

Loading of desired Manufacturing Order using GetByKey method on Manufacturing Order object (variable $mo). As a parameter we are passing DocEntry of Manufacturing Order:

```powershell
$result = $mo.GetByKey($DocEntry);
```

Checking if loading of Manufacturing Order was successful by checking the value of the $result variable. This variable holds the value returned from the GetByKey method. If the value is different from 0, it means that loading of Manufacturing Order failed - MOR with given DocEntry does not exist:

```powershell
if ($result -ne 0) {
    $err = [string]::Format("Manufacturing Order with DocEntry:{0}, DocNum:{1} don't exists", [string]$DocEntry, [string]$DocNum);
    Throw [System.Exception]($err);
}
```

This is a switch method. It allows us to set a correct status from ProcessForce API to the $MORStatus variable. This way we can check if provided status code is correct. If not, an exception is thrown(140):

```powershell
switch ($Status) {
    "NS" {
        $MORstatus = [CompuTec.ProcessForce.API.Enumerators.ManufacturingOrderStatus]::NotScheduled
        break;
    }
    "SC" {
        $MORstatus = [CompuTec.ProcessForce.API.Enumerators.ManufacturingOrderStatus]::Scheduled
        break;
    }
    "RL" {
        $MORstatus = [CompuTec.ProcessForce.API.Enumerators.ManufacturingOrderStatus]::Released
        break;
    }
    "ST" {
        $MORstatus = [CompuTec.ProcessForce.API.Enumerators.ManufacturingOrderStatus]::Started
        break;
    }
    "FI" {
        $MORstatus = [CompuTec.ProcessForce.API.Enumerators.ManufacturingOrderStatus]::Finished
        break;
    }
    "CL" {
        $MORstatus = [CompuTec.ProcessForce.API.Enumerators.ManufacturingOrderStatus]::Closed
        break;
    }
    default {
        $err = [string]::Format("Incorrect Status Code {0}. Possible values are: NS, SC, RL, ST, FI, CL", [string]$Status);
        Throw [System.Exception]($err);
        break;
    }
}
```

This is an actual change of status on the Manufacturing Order. Previously determined $MORStatus variable is used:

```powershell
$mo.U_Status = $MORstatus;
```

Until now the changes have not been saved. This is done here using the Update method on the Manufacturing Order object. The result of this method is written to $updateResult variable. If it is lower than 0, it means the operation has failed:

```powershell
$updateResult = $mo.Update();
```

Checking the result of the update operation and if it failed, an exception is thrown. It is important to see that we are getting information about what went wrong from the method GetLastErrorDescription on the Company object:

```powershell
if ($updateResult -lt 0) {   
    $err = $pfcCompany.GetLastErrorDescription()
    Throw [System.Exception] ($err)
}
```

Catching an exception and displaying information about the exception to the user inside the loop. This is the catch block for the loop opened in line 100:

```powershell
}
catch {
    $err = $_.Exception.Message;
    $ms = [string]::Format("Error when updating Manufacturing Order. DocEntry:{0}, DocNum:{1}. Details: {2}", [string]$DocEntry, [string]$DocNum, $err);
    Write-Host -BackgroundColor DarkRed -ForegroundColor White $ms
}
```

***This part is very important*** because it is fetching the next record from the RecordSet. It is in the Finally block because it needs to be executed regardless the exception occurred or not:

```powershell
Finally {
    $recordSet.MoveNext();
}
```

Closing loop:

```powershell
    }
     
    Write-Host -BackgroundColor Yellow -ForegroundColor Blue ([string]::Format("{0} Manufacturing Orders updated. Operation completed", [string]$recordCount));
}
```

Displaying information that there were no records returned from the SQL query:

```powershell
else {
    Write-Host -BackgroundColor Yellow -ForegroundColor Blue "SQL Query didn't return any errors"
}
```

The Catch block, try block that was opened in line 91. If an exception occurs on this level, it displays information about the exception. In the Finally block we are disconnecting from SAP Business One:

```powershell
}
Catch {
    $err = $_.Exception.Message;
    $ms = [string]::Format("Exception occured:{0}", $err);
    Write-Host -BackgroundColor DarkRed -ForegroundColor White $ms
}
Finally {
    #region Close connection
    if ($pfcCompany.IsConnected) {
        $pfcCompany.Disconnect()
        Write-Host '';
        write-host  –backgroundcolor green –foregroundcolor black "Disconnected from the company"
    }
    #endregion
}
```
