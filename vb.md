- insert the follow function into a new module and then press F5 to execute:
```
Sub PasswordBreaker()
    'Breaks worksheet password protection.

    Dim i As Integer, j As Integer, k As Integer
    Dim l As Integer, m As Integer, n As Integer
    Dim i1 As Integer, i2 As Integer, i3 As Integer
    Dim i4 As Integer, i5 As Integer, i6 As Integer

    On Error Resume Next

    For i = 65 To 66: For j = 65 To 66: For k = 65 To 66
    For l = 65 To 66: For m = 65 To 66: For i1 = 65 To 66
    For i2 = 65 To 66: For i3 = 65 To 66: For i4 = 65 To 66
    For i5 = 65 To 66: For i6 = 65 To 66: For n = 32 To 126

        ActiveSheet.Unprotect Chr(i) & Chr(j) & Chr(k) & _
            Chr(l) & Chr(m) & Chr(i1) & Chr(i2) & Chr(i3) & _
            Chr(i4) & Chr(i5) & Chr(i6) & Chr(n)

        If ActiveSheet.ProtectContents = False Then
            MsgBox "Password is " & Chr(i) & Chr(j) & _
                Chr(k) & Chr(l) & Chr(m) & Chr(i1) & Chr(i2) & _
                Chr(i3) & Chr(i4) & Chr(i5) & Chr(i6) & Chr(n)

            Exit Sub

        End If

    Next: Next: Next: Next: Next: Next
    Next: Next: Next: Next: Next: Next
End Sub
```

## Add multiple checkboxes attached to cells and change colour of cells 

```
Sub AddCheckBoxes()
  On Error Resume Next
    Dim c As Range, myRange As Range
    Set myRange = Selection
    For Each c In myRange.Cells
        ActiveSheet.CheckBoxes.Add(c.Left, c.Top, c.Width, c.Height).Select
            With Selection
                .LinkedCell = c.Address
                .Characters.Text = ""
                .Name = c.Address
            End With
            c.Select
            With Selection
                .FormatConditions.Delete
                .FormatConditions.Add Type:=xlExpression, _
                    Formula1:="=" & c.Address & "=TRUE"
                .FormatConditions(1).Font.ColorIndex = 3 'change for other color when ticked 4 is green, 3 is red, 8 is cyan
                .FormatConditions(1).Interior.ColorIndex = 3 'change for other color when ticked
                .Font.ColorIndex = 2 'cell background color = White
                .VerticalAlignment = xlCenter
            End With
        Next
        myRange.Select
End Sub
```

- Hints
If you end up with too many checkboxes and make a mess of the worksheet a quick way to delete them (and all other similar type objects) is to go to the VBA Window, Press CTRL+G and type into that window:
```
activesheet.drawingobjects.delete
```

## Centering checkboxes
```
Sub CenterCheckbox()
    Dim xRg As Range
    Dim chkBox As OLEObject
    Dim chkFBox As CheckBox
    On Error Resume Next
    Application.ScreenUpdating = False
    For Each chkBox In ActiveSheet.OLEObjects
        If TypeName(chkBox.Object) = "CheckBox" Then
            Set xRg = chkBox.TopLeftCell
            chkBox.Width = xRg.Width * 2 / 3
            chkBox.Height = xRg.Height
            chkBox.Left = xRg.Left + (xRg.Width - chkBox.Width) / 2
            chkBox.Top = xRg.Top + (xRg.Height - chkBox.Height) / 2
        End If
    Next
    For Each chkFBox In ActiveSheet.CheckBoxes
        Set xRg = chkFBox.TopLeftCell
        chkFBox.Width = xRg.Width * 2 / 3
        chkFBox.Height = xRg.Height
        chkFBox.Left = xRg.Left + (xRg.Width - chkFBox.Width) / 2
        chkFBox.Top = xRg.Top + (xRg.Height - chkFBox.Height) / 2
    Next
    Application.ScreenUpdating = True
End Sub

```

## Populate cells in one sheet with info from another sheet and hide until there is info
In the cells use the formula:
```
=IF(SheetName!$J2="yes",SheetName!A2,"")
```
Right click on the sheet and choose 'view code' and insert:
```
Option Explicit
Private Sub Worksheet_Activate()
    Dim r As Range, c As Range
Set r = Range("a1:a299")
Application.ScreenUpdating = False
For Each c In r
    If Len(c.Text) = 0 Then
        c.EntireRow.Hidden = True
    Else
        c.EntireRow.Hidden = False
    End If
Next c
Application.ScreenUpdating = True
End Sub
```
