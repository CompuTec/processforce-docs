---
sidebar_position: 5
---

# UDFs in PowerShell scripts

On this page, you can check how to set the value for a User Defined Field (UDF) that was added to a ProcessForce object.

## General rule

When you want to set a value on UDF that was added to the ProcessForce object, you need to use the following syntax:

```powershell
$PFObject.UDFItems.Item("U_UDF1").Value =  'some value';
```

Therefore instead of (example for U_LineNo field on Manufacturing Order's header):

```powershell
$mo.U_LineNo = $csvItem.LineNo
```

It should be:

```powershell
$mo.UDFItems.Item("U_LineNo").Value = $csvItem.LineNo
```

The same pattern applies to other UDFs.

## Hour type UDF

For UDF with Hour type, you need to explicitly cast value from the .csv file the o type DateTime. For example, in the .csv file, there is a value in the format HH:MM. An example piece of code for this is:

```powershell
$mo.UDFItems.Item("U_PlannedHours").Value = [datetime] $csvItem.PlannedHours
```
