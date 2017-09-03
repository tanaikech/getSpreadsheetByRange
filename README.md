# Retrieving Spreadsheet ID from Range using Google Apps Script
This report has been written by [Alexander Ivanov](https://github.com/oshliaer) and [me](https://github.com/tanaikech).

This is a simple way for retrieving Spreadsheet ID from a range. As an application, we introduce the enhanced ``copyTo()`` which can copy from a range to other Spreadsheet.

## 1. Basic Principle
The first sample script is for retrieving spreadsheet ID from a range using Google Apps Script. We sometimes want to retrieve spreadsheet ID from ranges. In such case, we can use this.

The flow is as follows.

- Range
- -> Retrieve Sheet using [``getSheet()``](https://developers.google.com/apps-script/reference/spreadsheet/range#getsheet)
- -> Retrieve Spreadsheet using [``getParent()``](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getParent())
- -> Retrieve spreadsheet ID

~~~javascript
var id = "123456789abcdefg";
var sheet = "Sheet";
var cells = "a1:b10";
var range = SpreadsheetApp.openById(id).getSheetByName(sheet).getRange(cells);

var id = range.getSheet().getParent().getId();

>>> id ---> 123456789abcdefg
~~~

# 2. Applications
We considered about applying this to practical scenes.

## 1. Enhanced copyTo()
It can be used as the enhanced ``copyTo()``.

~~~javascript
// Source
var range = "a1:b5";
var ss = SpreadsheetApp.getActiveSpreadsheet();
var srcrange = ss.getActiveSheet().getRange(range);

// Destination
var range = "c1:d5";
var dstid = "### file id ###";
var dst = "### sheet name ###";
var dstrange = SpreadsheetApp.openById(dstid).getSheetByName(dst).getRange(range);
~~~

**For above script, ``srcrange.copyTo(dstrange);`` returns error. Because ``copyTo()`` cannot be used for copying data to other spreadsheet.**

So we propose a following simple script. This script can copy data from Spreadsheet A to Spreadsheet B using the range. Namely, a range can be copied to other Spreadsheet.

### Script
~~~javascript
function copyToo(srcrange, dstrange) {
  var dstSS = dstrange.getSheet().getParent();
  var copiedsheet = srcrange.getSheet().copyTo(dstSS);
  copiedsheet.getRange(srcrange.getA1Notation()).copyTo(dstrange);
  dstSS.deleteSheet(copiedsheet);
}
~~~

If this was useful for you, we are glad.
