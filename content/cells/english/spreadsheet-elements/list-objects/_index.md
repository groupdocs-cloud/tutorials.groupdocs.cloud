---
title: List Objects and Tables Tutorial
second_title: Aspose.Cells Cloud API Tutorial

url: /spreadsheet-elements/list-objects/
description: Learn to create, retrieve, update, and manipulate tables (list objects) in Excel worksheets using Aspose.Cells Cloud API in this step-by-step tutorial.
weight: 50
keywords: Excel tables, list objects Excel, Excel API tutorial, Aspose.Cells tutorial, data table management
---

# List Objects and Tables Tutorial

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve list objects (tables) from Excel worksheets
- Add new list objects to a worksheet
- Update existing list objects
- Sort data in a list object
- Convert a list object to a regular range
- Remove duplicates from a list object
- Create pivot tables from list objects

## Prerequisites

Before starting this tutorial, make sure you have:
1. An Aspose Cloud account (sign up for a [free trial](https://dashboard.aspose.cloud/#/apps) if you don't have one)
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
3. Basic understanding of REST APIs
4. Familiarity with your chosen programming language
5. An Excel file with structured data (or you can create one during this tutorial)

## Introduction to List Objects in Excel

List objects (commonly known as tables) in Excel provide a structured way to organize and analyze data. They offer features such as filtering, sorting, and automatic formula extension. Tables also have built-in formatting options and can be referenced in formulas by their table name rather than cell references.

Using Aspose.Cells Cloud API, you can programmatically manage list objects in your Excel files without requiring Microsoft Excel to be installed, providing a powerful way to automate data management in your applications.

## Tutorial Steps

### 1. Authentication

First, let's set up authentication to access the Aspose.Cells Cloud API:

```bash
# Base request URL
BASE_URL="https://api.aspose.cloud/v3.0/cells"

# Get JWT token
TOKEN=$(curl -v "https://api.aspose.cloud/connect/token" \
 -X POST \
 -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
 -H "Content-Type: application/x-www-form-urlencoded" \
 | jq -r '.access_token')
```

Remember to replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

### 2. Getting a List Object

Let's start by retrieving a specific list object from a worksheet:

#### Using cURL:

```bash
curl -X GET "$BASE_URL/Book1.xlsx/worksheets/Sheet1/listobjects/0" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json"
```

#### Using Python SDK:

```python
import asposecellscloud
from asposecellscloud.apis.cells_api import CellsApi
from asposecellscloud.models import *
from asposecellscloud.api_client import ApiClient

# Configure API key authorization
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create API client
api_instance = CellsApi(client_id, client_secret)

try:
    # Get a specific list object
    response = api_instance.cells_list_objects_get_worksheet_list_object(
        name="Book1.xlsx", 
        sheet_name="Sheet1",
        list_object_index=0)  # First list object (index 0)
    
    # Print list object information
    if response.list_object:
        print(f"List Object Details:")
        print(f"Name: {response.list_object.display_name}")
        print(f"Range: {response.list_object.start_row},{response.list_object.start_column} to {response.list_object.end_row},{response.list_object.end_column}")
        print(f"Has Headers: {response.list_object.show_header_row}")
        print(f"Style: {response.list_object.table_style_name}")
        
        # Print columns
        if response.list_object.list_columns:
            print("\nColumns:")
            for idx, column in enumerate(response.list_object.list_columns):
                print(f"  Column {idx+1}: {column.name}")
    else:
        print("List object not found.")
        
except Exception as e:
    print("Exception when calling CellsApi->cells_list_objects_get_worksheet_list_object: %s\n" % e)
```

### 3. Adding a List Object

Now, let's add a new list object to a worksheet:

#### Using cURL:

```bash
curl -X PUT "$BASE_URL/Book1.xlsx/worksheets/Sheet1/listobjects?startRow=1&startColumn=1&endRow=10&endColumn=5&hasHeaders=true" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json"
```

#### Using C# SDK:

```csharp
using System;
using Aspose.Cells.Cloud.SDK.Api;
using Aspose.Cells.Cloud.SDK.Model;

namespace AsposeCellsCloudTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API credentials
            var cellsApi = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Specify parameters
            string fileName = "Book1.xlsx";
            string sheetName = "Sheet1";
            int startRow = 1;
            int startColumn = 1;
            int endRow = 10;
            int endColumn = 5;
            bool hasHeaders = true;
            
            try
            {
                // Add the list object
                var response = cellsApi.CellsListObjectsPutWorksheetListObject(
                    fileName, 
                    sheetName, 
                    startRow,
                    startColumn,
                    endRow,
                    endColumn,
                    hasHeaders);
                
                Console.WriteLine("List object added successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### 4. Updating a List Object

To update an existing list object:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/Book1.xlsx/worksheets/Sheet1/listobjects/0" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"DisplayName":"SalesTable", "ShowTotals":true, "TableStyleName":"TableStyleMedium2", "ShowTableStyleColumnStripes":true, "ShowTableStyleRowStripes":true}'
```

#### Using Java SDK:

```java
import com.aspose.cells.cloud.api.CellsApi;
import com.aspose.cells.cloud.model.*;
import com.aspose.cells.cloud.request.*;

public class UpdateListObjectExample {
    public static void main(String[] args) {
        // Configure API credentials
        CellsApi cellsApi = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        try {
            // Specify parameters
            String fileName = "Book1.xlsx";
            String sheetName = "Sheet1";
            Integer listObjectIndex = 0; // First list object
            
            // Create list object with updated properties
            ListObject listObject = new ListObject();
            listObject.setDisplayName("SalesTable");
            listObject.setShowTotals(true);
            listObject.setTableStyleName("TableStyleMedium2");
            listObject.setShowTableStyleColumnStripes(true);
            listObject.setShowTableStyleRowStripes(true);
            
            // Create request
            PostWorksheetListObjectRequest request = new PostWorksheetListObjectRequest(
                fileName,
                sheetName,
                listObjectIndex,
                listObject,
                null, // folder
                null  // storage name
            );
            
            // Update the list object
            cellsApi.postWorksheetListObject(request);
            
            System.out.println("List object updated successfully.");
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 5. Sorting Data in a List Object

To sort data in a list object:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/Book1.xlsx/worksheets/Sheet1/listobjects/0/sort" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"CaseSensitive":true, "HasHeaders":true, "KeyList":[{"Key":1, "SortOrder":"Ascending"}], "SortLeftToRight":false}'
```

#### Using Node.js SDK:

```javascript
const { CellsApi } = require('asposecellscloud');

// Configure API credentials
const cellsApi = new CellsApi('YOUR_CLIENT_ID', 'YOUR_CLIENT_SECRET');

// Specify parameters
const fileName = "Book1.xlsx";
const sheetName = "Sheet1";
const listObjectIndex = 0; // First list object

// Create the data sorter object
const dataSorter = {
    CaseSensitive: true,
    HasHeaders: true,
    KeyList: [
        {
            Key: 1, // Sort by the second column (index 1)
            SortOrder: "Ascending"
        }
    ],
    SortLeftToRight: false
};

// Sort the list object data
cellsApi.cellsListObjectsPostWorksheetListObjectSort(fileName, sheetName, listObjectIndex, dataSorter)
    .then(() => {
        console.log("List object data sorted successfully.");
    })
    .catch(error => {
        console.error("Error:", error);
    });
```

### 6. Converting a List Object to a Range

To convert a list object back to a regular range:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/Book1.xlsx/worksheets/Sheet1/listobjects/0/ConvertToRange" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json"
```

#### Using Python SDK:

```python
import asposecellscloud
from asposecellscloud.apis.cells_api import CellsApi
from asposecellscloud.models import *
from asposecellscloud.api_client import ApiClient

# Configure API key authorization
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create API client
api_instance = CellsApi(client_id, client_secret)

try:
    # Specify parameters
    name = "Book1.xlsx"
    sheet_name = "Sheet1"
    list_object_index = 0  # First list object
    
    # Convert list object to range
    response = api_instance.cells_list_objects_post_worksheet_list_object_convert_to_range(
        name=name,
        sheet_name=sheet_name,
        list_object_index=list_object_index
    )
    
    print("List object converted to range successfully.")
    
except Exception as e:
    print("Exception when calling CellsApi->cells_list_objects_post_worksheet_list_object_convert_to_range: %s\n" % e)
```

### 7. Removing Duplicates from a List Object

To remove duplicate entries from a list object:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/Book1.xlsx/worksheets/Sheet1/listobjects/0/RemoveDuplicates" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json"
```

#### Using C# SDK:

```csharp
using System;
using Aspose.Cells.Cloud.SDK.Api;
using Aspose.Cells.Cloud.SDK.Model;

namespace AsposeCellsCloudTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API credentials
            var cellsApi = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Specify parameters
            string fileName = "Book1.xlsx";
            string sheetName = "Sheet1";
            int listObjectIndex = 0; // First list object
            
            try
            {
                // Remove duplicates from list object
                var response = cellsApi.CellsListObjectsPostWorksheetListObjectRemoveDuplicates(
                    fileName, 
                    sheetName, 
                    listObjectIndex);
                
                Console.WriteLine("Duplicates removed from list object successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### 8. Creating a Pivot Table from a List Object

To create a pivot table from an existing list object:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/Book1.xlsx/worksheets/Sheet1/listobjects/0/SummarizeWithPivotTable?destsheetName=Sheet2" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"Name":"SalesPivot", "DestCellName":"A1", "PivotFieldRows":[0], "PivotFieldColumns":[1], "PivotFieldData":[2]}'
```

#### Using Java SDK:

```java
import com.aspose.cells.cloud.api.CellsApi;
import com.aspose.cells.cloud.model.*;
import com.aspose.cells.cloud.request.*;
import java.util.Arrays;

public class CreatePivotTableFromListObjectExample {
    public static void main(String[] args) {
        // Configure API credentials
        CellsApi cellsApi = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        try {
            // Specify parameters
            String fileName = "Book1.xlsx";
            String sheetName = "Sheet1";
            Integer listObjectIndex = 0; // First list object
            String destSheetName = "Sheet2"; // Destination worksheet
            
            // Create pivot table request
            CreatePivotTableRequest pivotTableRequest = new CreatePivotTableRequest();
            pivotTableRequest.setName("SalesPivot");
            pivotTableRequest.setDestCellName("A1");
            pivotTableRequest.setPivotFieldRows(Arrays.asList(0)); // First column as rows
            pivotTableRequest.setPivotFieldColumns(Arrays.asList(1)); // Second column as columns
            pivotTableRequest.setPivotFieldData(Arrays.asList(2)); // Third column as data
            
            // Create request
            PostWorksheetListObjectSummarizeWithPivotTableRequest request = new PostWorksheetListObjectSummarizeWithPivotTableRequest(
                fileName,
                sheetName,
                listObjectIndex,
                destSheetName,
                pivotTableRequest,
                null, // folder
                null  // storage name
            );
            
            // Create pivot table from list object
            cellsApi.postWorksheetListObjectSummarizeWithPivotTable(request);
            
            System.out.println("Pivot table created from list object successfully.");
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Try It Yourself

Now it's time to practice what you've learned:

1. Create a new Excel file with some sample data (e.g., sales data with columns for Product, Region, Month, Sales Amount)
2. Convert this data into a list object/table
3. Apply formatting and styling to the table
4. Sort the data by different columns
5. Remove any duplicate entries
6. Create a pivot table from the list object to analyze the data
7. Finally, convert the table back to a regular range

## Troubleshooting Tips

- Error 401 (Unauthorized): Make sure your Client ID and Client Secret are correct and that your token hasn't expired.
- Error 404 (Not Found): Check that your file name, worksheet name, and list object index are correct.
- Index out of range errors: Remember that list object indices are zero-based. The first list object has an index of 0.
- Data not appearing in the list object: Ensure that the range you specified (startRow, startColumn, endRow, endColumn) contains valid data.
- Sorting not working as expected: Verify that your KeyList contains valid column indices and that the SortOrder is correctly specified ("Ascending" or "Descending").
- Pivot table creation issues: Make sure the destination worksheet exists and that the PivotFieldRows, PivotFieldColumns, and PivotFieldData indices correspond to valid columns in your list object.

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve list objects (tables) from Excel worksheets
- Add new list objects to a worksheet
- Update properties of existing list objects
- Sort data in a list object
- Convert a list object to a regular range
- Remove duplicates from a list object
- Create pivot tables from list objects

You now have the skills to programmatically manage list objects in Excel files using the Aspose.Cells Cloud API, allowing you to automate data organization and analysis tasks.

## Next Steps

Consider exploring these related tutorials:
- [Document Metadata and Properties: Complete Tutorial](/tutorials/spreadsheet-elements/metadata/)
- [Mastering Pivot Tables with Aspose.Cells Cloud API](/tutorials/spreadsheet-elements/pivot-tables/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cells/)
- [Documentation](https://docs.aspose.cloud/cells/)
- [Live Demo](https://products.aspose.app/cells/family)
- [API Reference](https://reference.aspose.cloud/cells/)
- [Blog](https://blog.aspose.cloud/category/cells/)
- [Free Support](https://forum.aspose.cloud/c/cells/7)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/cells/7)
