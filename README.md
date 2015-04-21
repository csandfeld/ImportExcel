PowerShell Import-Excel
-
This PowerShell Module wraps the .NET EPPlus DLL (included). Easily integrate reading and writing Excel spreadsheets into PowerShell, without launching Excel in the background. You can also automate the creation of Pivot Tables and Charts.

Know Issues
-
* Only one pivot table can be added. Investigating how EPPlus saves the target file. 

What's new
-

#### 4/20/2015
* Included and embellished [Claus Nielsen](https://github.com/Claustn) function to take all sheets in an Excel file workbook and create a text file for each `ConvertFrom-ExcelSheet`

#### 4/13/2015
* You can add a title to the Excel "Report" `Title`, `TitleFillPattern`, `TitleBold`, `TitleSize`, `TitleBackgroundColor`
	* Thanks to [Irwin Strachan](http://pshirwin.wordpress.com) for this and other great suggestions, testing and more


#### 4/10/2015
* Renamed `AutoFitColumns` to `AutoSize`
* Implemented `Export-MultipleExcelSheets`
* Implemented `-Password` for a worksheet
* Replaced `-Force` switch with `-NoClobber` switch
* Added examples for `Get-Help`
* If Pivot table is requested, that sheet becomes the tab selected

#### 4/8/2015
* Implemented exporting data to **named sheets** via the -WorkSheename parameter.

Examples
- 
`gsv | Export-Excel .\test.xlsx -WorkSheetname Services`

`dir -file | Export-Excel .\test.xlsx -WorkSheetname Files`

`ps | Export-Excel .\test.xlsx -WorkSheetname Processes -IncludePivotTable -Show -PivotRows Company -PivotData PM`

#### Convert (All or Some) Excel Sheets to Text files
 
Reads each sheet in TestSheets.xlsx and outputs it to the data directory as the sheet name with the extension .txt

    ConvertFrom-ExcelSheet .\TestSheets.xlsx .\data

Reads and outputs sheets like Sheet10 and Sheet20 form TestSheets.xlsx and outputs it to the data directory as the sheet name with the extension .txt
	
	ConvertFrom-ExcelSheet .\TestSheets.xlsx .\data sheet?0

#### Example Adding a Title
You can set the pattern, size and of if the title is bold.

	$p=@{	    
	    Title = "Process Report as of $(Get-Date)" 
	    TitleFillPattern = "LightTrellis"
	    TitleSize = 18
	    TitleBold = $true

		Path  = "$pwd\testExport.xlsx"
	    Show = $true
	    AutoSize = $true
	}
	
	Get-Process |
	    Where Company | Select Company, PM |
	    Export-Excel @p

![image](https://raw.githubusercontent.com/dfinke/ImportExcel/master/images/Title.png)

#### Example Export-MultipleExcelSheets
![image](https://raw.githubusercontent.com/dfinke/ImportExcel/master/images/ExportMultiple.gif)

	$p = Get-Process
	
	$DataToGather = @{
	    PM        = {$p|select company, pm}
	    Handles   = {$p|select company, handles}
	    Services  = {gsv}
	    Files     = {dir -File}
	    Albums    = {(Invoke-RestMethod http://www.dougfinke.com/powershellfordevelopers/albums.js)}
	}
	
	Export-MultipleExcelSheets -Show -AutoSize .\testExport.xlsx $DataToGather



***NOTE*** If the sheet exists when using *-WorkSheetname* parameter, it will be deleted and then added with the new data.

Get-Process Exported to Excel 
-
### Total Physical Memory Grouped By Company
![image](https://raw.githubusercontent.com/dfinke/ImportExcel/master/images/PivotTablesAndCharts.png)

PowerShell Excel EPPlus Video
-
Click on this image to watch the short video.

[![image](http://dougfinke.com/powershellvideos/ExportExcel/ExportExcel_First_Frame.png)](http://dougfinke.com/powershellvideos/ExportExcel/ExportExcel.html)

### Importing data from an Excel spreadsheet

![image](https://raw.githubusercontent.com/dfinke/ImportExcel/master/images/TryImportExcel.gif)

You can also find EPPLus on [Nuget](https://www.nuget.org/packages/EPPlus/).