---
title: "MarshalOptions Property Example (VB)"
 
 
manager: soliver
ms.date: 11/16/2014
ms.audience: Developer
ms.topic: reference
  
localization_priority: Normal
ms.assetid: f48ad901-7ce8-af6c-e312-51777466cd35
description: "This example uses the MarshalOptions property to specify what rows are sent back to the server — All Rows or only Modified Rows."
---

# MarshalOptions Property Example (VB)

This example uses the [MarshalOptions](marshaloptions-property-ado.md) property to specify what rows are sent back to the server — All Rows or only Modified Rows. 
  
```
 
'BeginMarshalOptionsVB 
 
 'To integrate this code 
 'replace the data source and initial catalog values 
 'in the connection string 
 
Public Sub Main() 
 On Error GoTo ErrorHandler 
 
 Dim rstEmployees As ADODB.Recordset 
 Dim Cnxn As ADODB.Connection 
 Dim strCnxn As String 
 Dim strSQLEmployees As String 
 
 Dim strOldFirst As String 
 Dim strOldLast As String 
 Dim strMessage As String 
 Dim strMarshalAll As String 
 Dim strMarshalModified As String 
 
 ' Open connection 
 strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" &amp; _ 
 "Initial Catalog='Pubs';Integrated Security='SSPI';" 
 Set Cnxn = New ADODB.Connection 
 Cnxn.Open strCnxn 
 
 ' open recordset with names from Employees table 
 ' and set some properties through object refs 
 Set rstEmployees = New ADODB.Recordset 
 rstEmployees.CursorType = adOpenKeyset 
 rstEmployees.LockType = adLockOptimistic 
 rstEmployees.CursorLocation = adUseClient 
 
 strSQLEmployees = "SELECT fname, lname FROM Employee ORDER BY lname" 
 
 rstEmployees.Open strSQLEmployees, Cnxn, , , adCmdText 
 ' empty properties have been set above 
 
 ' Store original data 
 strOldFirst = rstEmployees!fname 
 strOldLast = rstEmployees!lname 
 
 ' Change data in edit buffer 
 rstEmployees!fname = "Linda" 
 rstEmployees!lname = "Kobara" 
 
 ' Show contents of buffer and get user input 
 strMessage = "Edit in progress:" &amp; vbCr &amp; _ 
 " Original data = " &amp; strOldFirst &amp; " " &amp; _ 
 strOldLast &amp; vbCr &amp; " Data in buffer = " &amp; _ 
 rstEmployees!fname &amp; " " &amp; rstEmployees!lname &amp; vbCr &amp; vbCr &amp; _ 
 "Use Update to replace the original data with " &amp; _ 
 "the buffered data in the Recordset?" 
 strMarshalAll = "Would you like to send all the rows " &amp; _ 
 "in the recordset back to the server?" 
 strMarshalModified = "Would you like to send only " &amp; _ 
 "modified rows back to the server?" 
 
 If MsgBox(strMessage, vbYesNo) = vbYes Then 
 If MsgBox(strMarshalAll, vbYesNo) = vbYes Then 
 rstEmployees.MarshalOptions = adMarshalAll 
 rstEmployees.Update 
 ElseIf MsgBox(strMarshalModified, vbYesNo) = vbYes Then 
 rstEmployees.MarshalOptions = adMarshalModifiedOnly 
 rstEmployees.Update 
 End If 
 End If 
 
 ' sShow the resulting data 
 MsgBox "Data in recordset = " &amp; rstEmployees!fname &amp; " " &amp; _ 
 rstEmployees!lname 
 
 ' restore original data because this is a demonstration 
 If Not (strOldFirst = rstEmployees!fname And _ 
 strOldLast = rstEmployees!lname) Then 
 rstEmployees!fname = strOldFirst 
 rstEmployees!lname = strOldLast 
 rstEmployees.Update 
 End If 
 
 ' clean up 
 rstEmployees.Close 
 Cnxn.Close 
 Set rstEmployees = Nothing 
 Set Cnxn = Nothing 
 Exit Sub 
 
ErrorHandler: 
 ' clean up 
 If Not rstEmployees Is Nothing Then 
 If rstEmployees.State = adStateOpen Then rstEmployees.Close 
 End If 
 Set rstEmployees = Nothing 
 
 If Not Cnxn Is Nothing Then 
 If Cnxn.State = adStateOpen Then Cnxn.Close 
 End If 
 Set Cnxn = Nothing 
 
 If Err <> 0 Then 
 MsgBox Err.Source &amp; "-->" &amp; Err.Description, , "Error" 
 End If 
End Sub 
'EndMarshalOptionsVB 

```

