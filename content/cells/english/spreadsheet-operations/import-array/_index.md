---
title: Importing Array Data into Excel with Aspose.Cells Cloud API Tutorial
second_title: Complete Developer Guide

url: /spreadsheet-operations/import-array/
description: Learn how to import various array types (integer, string, double, and multi-dimensional) into Excel worksheets using Aspose.Cells Cloud REST API in this step-by-step tutorial.
weight: 10
keywords: Excel array import, Aspose.Cells tutorial, REST API Excel, import data, integer arrays, data integration
---

# Tutorial: Importing Array Data into Excel with Aspose.Cells Cloud API

## Prerequisites

Before starting this tutorial, make sure you have:
1. An Aspose Cloud account with an active subscription or free trial
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
3. Basic understanding of array data structures
4. An existing Excel file for testing imports (or we'll create one from scratch)

## Introduction to Array Data Import

Arrays are fundamental data structures in programming, representing ordered collections of values. Importing arrays into Excel worksheets allows you to:

- Generate reports with programmatically defined data
- Analyze numerical data using Excel's powerful functions
- Visualize array data through Excel charts and graphs
- Prepare structured data for further processing

The Aspose.Cells Cloud API provides robust functionality for importing various types of arrays with extensive customization options.

## Understanding Array Import Options

When importing arrays into Excel, you need to consider several key options:

### Array Types Supported

Aspose.Cells Cloud supports importing several array types:

- Integer Arrays: For whole numbers (`int[]`)
- Double Arrays: For decimal numbers (`double[]`) 
- String Arrays: For text data (`string[]`)
- 2D Integer Arrays: For tabular integer data (`int[,]`)
- 2D Double Arrays: For tabular decimal data (`double[,]`)
- 2D String Arrays: For tabular text data (`string[,]`)

### Import Configuration Options

Key options for customizing array imports:

- FirstRow: The starting row (0-based index)
- FirstColumn: The starting column (0-based index)
- IsVertical: Whether to arrange 1D arrays vertically (true) or horizontally (false)
- IsInsert: Whether to insert new cells or overwrite existing content
- DestinationWorksheet: The target worksheet name

## Importing Integer Arrays

Let's start with a simple example: importing an integer array into an Excel worksheet.

### Try It Yourself: Using cURL

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/import" \
-X POST \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-H "Authorization: Bearer <your_jwt_token>" \
-F "file=@Book1.xlsx" \
-F 'ImportOption={
    "Data": [1, 2, 4, 8, 16, 32, 64, 128],
    "DestinationWorksheet": "Sheet1",
    "FirstRow": 1,
    "FirstColumn": 2,
    "IsVertical": true,
    "IsInsert": true,
    "ImportDataType": "IntArray"
}'
```

Replace `<your_jwt_token>` with your actual authentication token. This request imports an integer array into Book1.xlsx, starting at cell C2 (row 1, column 2) in a vertical arrangement.

### Implementation in C#

```csharp
// Initialize the API with your client credentials
CellsApi cellsApi = new CellsApi(clientId, clientSecret);

// Prepare import options
var importOptions = new ImportIntArrayOption
{
    Data = new List<int> { 1, 2, 4, 8, 16, 32, 64, 128 },
    DestinationWorksheet = "Sheet1", 
    FirstRow = 1,
    FirstColumn = 2,
    IsVertical = true,
    IsInsert = true,
    ImportDataType = "IntArray"
};

// Prepare the file to import into
var fileStream = File.OpenRead("Book1.xlsx");
var files = new Dictionary<string, Stream> { { "Book1.xlsx", fileStream } };

// Execute the import
var response = cellsApi.PostImport(
    files,
    importOptions
);

// Save the resulting Excel file
using (var outputStream = File.Create("result.xlsx"))
{
    response.File.CopyTo(outputStream);
}

Console.WriteLine("Integer array successfully imported into Excel!");
```

### Implementation in PHP

```php
<?php
require_once(__DIR__ . '/vendor/autoload.php');

// Configure API key authorization
$config = new \Aspose\Cells\Configuration();
$config->setAppSid('your_client_id');
$config->setAppKey('your_client_secret');

$apiInstance = new \Aspose\Cells\Api\CellsApi(
    new GuzzleHttp\Client(),
    $config
);

// Prepare the file to import into
$fileContents = file_get_contents("Book1.xlsx");
$files = array(
    "Book1.xlsx" => $fileContents
);

// Prepare import options
$importOptions = new \Aspose\Cells\Model\ImportIntArrayOption();
$importOptions->setData([1, 2, 4, 8, 16, 32, 64, 128]);
$importOptions->setDestinationWorksheet("Sheet1");
$importOptions->setFirstRow(1);
$importOptions->setFirstColumn(2);
$importOptions->setIsVertical(true);
$importOptions->setIsInsert(true);
$importOptions->setImportDataType("IntArray");

try {
    // Execute the import
    $result = $apiInstance->postImport($files, $importOptions);
    
    // Save the resulting Excel file
    file_put_contents("result.xlsx", $result->getFile());
    
    echo "Integer array successfully imported into Excel!\n";
} catch (Exception $e) {
    echo "Exception when calling CellsApi->postImport: " . $e->getMessage() . "\n";
}
?>
```

## Importing String Arrays

String arrays are perfect for text data like product names, categories, or descriptions.

### Try It Yourself: Using cURL

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/import" \
-X POST \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-H "Authorization: Bearer <your_jwt_token>" \
-F "file=@Book1.xlsx" \
-F 'ImportOption={
    "Data": ["January", "February", "March", "April", "May", "June"],
    "DestinationWorksheet": "Sheet1",
    "FirstRow": 0,
    "FirstColumn": 0,
    "IsVertical": false,
    "IsInsert": true,
    "ImportDataType": "StringArray"
}'
```

### Implementation in C#

```csharp
// Prepare import options for string array
var importOptions = new ImportStringArrayOption
{
    Data = new List<string> { "January", "February", "March", "April", "May", "June" },
    DestinationWorksheet = "Sheet1", 
    FirstRow = 0,
    FirstColumn = 0,
    IsVertical = false,  // Horizontal arrangement
    IsInsert = true,
    ImportDataType = "StringArray"
};

// Execute the import (same file handling as previous example)
var response = cellsApi.PostImport(
    files,
    importOptions
);
```

## Importing Double Arrays

Double arrays are ideal for numerical data with decimal precision, such as prices or measurements.

### Implementation in C#

```csharp
// Prepare import options for double array
var importOptions = new ImportDoubleArrayOption
{
    Data = new List<double> { 1.1, 2.2, 3.3, 4.4, 5.5 },
    DestinationWorksheet = "Sheet1", 
    FirstRow = 2,
    FirstColumn = 0,
    IsVertical = true,
    IsInsert = false,  // Overwrite existing content
    ImportDataType = "DoubleArray"
};

// Execute the import (same file handling as previous examples)
var response = cellsApi.PostImport(
    files,
    importOptions
);
```

## Importing Two-Dimensional Arrays

Two-dimensional arrays are perfect for tabular data with multiple rows and columns.

### Importing 2D Integer Arrays

```csharp
// Prepare import options for 2D integer array
var importOptions = new Import2DimensionIntArrayOption
{
    Data = new int[,] { 
        { 1, 2, 3 },
        { 4, 5, 6 },
        { 7, 8, 9 }
    },
    DestinationWorksheet = "Sheet2",
    FirstRow = 0,
    FirstColumn = 0,
    IsInsert = true,
    ImportDataType = "TwoDimensionIntArray"
};

// Execute the import
var response = cellsApi.PostImport(
    files,
    importOptions
);
```

### Using JSON Format (for cURL and other languages)

When using API clients that require JSON, format 2D arrays like this:

```json
{
    "Data": [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
    ],
    "DestinationWorksheet": "Sheet2",
    "FirstRow": 0,
    "FirstColumn": 0,
    "IsInsert": true,
    "ImportDataType": "TwoDimensionIntArray"
}
```

## Import to Existing Files in Cloud Storage

If your Excel file is already stored in Aspose Cloud Storage, you can import data directly:

### Implementation in C#

```csharp
// Prepare import options
var importOptions = new ImportIntArrayOption
{
    Data = new List<int> { 1, 2, 4, 8, 16, 32 },
    DestinationWorksheet = "Sheet1", 
    FirstRow = 0,
    FirstColumn = 0,
    IsVertical = true,
    IsInsert = true,
    ImportDataType = "IntArray"
};

// Execute the import directly to a file in cloud storage
var response = cellsApi.PostImportData(
    "Book1.xlsx",     // File name in cloud storage
    importOptions,
    null,             // Password (if required)
    "MyFolder",       // Cloud storage folder (optional)
    null              // Storage name (optional)
);

Console.WriteLine("Data imported successfully to file in cloud storage!");
```

## Creating Dynamic Reports with Arrays

Now that you know how to import different array types, let's create a practical example: a monthly sales report.

### Step 1: Prepare Your Excel Template

First, create a simple Excel template with headers:

1. Add a title "Monthly Sales Report" in cell A1
2. Add column headers in row 3: "Month", "Units Sold", "Revenue", "Average Price"

### Step 2: Import Data Arrays

```csharp
// Initialize API
CellsApi cellsApi = new CellsApi(clientId, clientSecret);

// Prepare the template file
var fileStream = File.OpenRead("SalesTemplate.xlsx");
var files = new Dictionary<string, Stream> { { "SalesTemplate.xlsx", fileStream } };

// Prepare month names
var importMonths = new ImportStringArrayOption
{
    Data = new List<string> { "January", "February", "March", "April", "May", "June" },
    DestinationWorksheet = "Sheet1", 
    FirstRow = 3,
    FirstColumn = 0,
    IsVertical = true,
    IsInsert = false,
    ImportDataType = "StringArray"
};

// Import month names
var responseMonths = cellsApi.PostImport(files, importMonths);

// Use the result file for the next import
using (var tempFile = File.Create("temp.xlsx"))
{
    responseMonths.File.CopyTo(tempFile);
}

// Prepare units sold data
var fileStream2 = File.OpenRead("temp.xlsx");
var files2 = new Dictionary<string, Stream> { { "temp.xlsx", fileStream2 } };

var importUnitsSold = new ImportIntArrayOption
{
    Data = new List<int> { 120, 145, 168, 192, 210, 225 },
    DestinationWorksheet = "Sheet1", 
    FirstRow = 3,
    FirstColumn = 1,
    IsVertical = true,
    IsInsert = false,
    ImportDataType = "IntArray"
};

// Import units sold data
var responseUnitsSold = cellsApi.PostImport(files2, importUnitsSold);

// Continue with revenue data and other imports...
```

## Advanced Import Techniques

### Combining Different Array Types

You can import multiple arrays of different types to build complex reports:

```csharp
// Import string array for product names
cellsApi.PostImport(files, stringArrayOptions);

// Import double array for prices
cellsApi.PostImport(files, doubleArrayOptions);

// Import integer array for quantities
cellsApi.PostImport(files, intArrayOptions);
```

### Creating Formulas After Import

After importing data, you might want to add formulas. You can do this using other Aspose.Cells Cloud endpoints:

```csharp
// After importing data, add a formula to calculate totals
var cellsApi = new CellsApi(clientId, clientSecret);

// Set a formula in cell D2 that multiplies B2 and C2
var setFormulaRequest = new StorageCellsApiPutWorksheetCellRequest(
    name: "result.xlsx",
    sheetName: "Sheet1",
    cellName: "D2",
    formula: "=B2*C2",
    folder: "MyFolder"
);

var response = cellsApi.PutWorksheetCell(setFormulaRequest);
```

## Troubleshooting Common Issues

### Issue: Data Not Appearing in the Expected Location
Solution: Double-check your FirstRow and FirstColumn values. Remember they're zero-based indices.

### Issue: Error When Importing Large Arrays
Solution: For very large arrays, consider:
- Splitting into smaller chunks
- Enabling HTTP compression
- Using 2D arrays for better handling of tabular data

### Issue: Formatting Problems with Numeric Data
Solution: For precise formatting control, import as strings with your desired format, or apply cell formatting after import.

## What You've Learned

In this tutorial, you've learned:
- How to import various types of arrays (integer, string, double) into Excel worksheets
- How to work with both one-dimensional and two-dimensional arrays
- How to configure import options like starting position and orientation
- How to import data to files in cloud storage
- How to create dynamic reports using multiple array imports
- Advanced techniques and troubleshooting for array imports

## Further Practice

To reinforce your learning, try these exercises:
1. Create a dynamic sales dashboard by importing multiple arrays of different types
2. Write a script that reads data from a database and imports it as arrays into Excel
3. Build a report generator that combines array imports with formula calculations


## Helpful Resources

- [Product Page](https://products.aspose.cloud/cells/)
- [Documentation](https://docs.aspose.cloud/cells/)
- [API Reference](https://reference.aspose.cloud/cells/)
- [Free Support Forum](https://forum.aspose.cloud/c/cells/7)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/cells/7)
