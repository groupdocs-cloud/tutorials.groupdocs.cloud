---
title: Basic Conditional Formatting with Aspose.Cells Cloud API Tutorial
second_title: Learn to Highlight Important Data in Excel Worksheets
description: Step-by-step tutorial to implement conditional formatting rules in Excel using Aspose.Cells Cloud API. Learn to highlight cells based on their values.

url: /spreadsheet-elements/conditional-formatting/
weight: 10
keywords: Excel conditional formatting, Format conditions API, Highlight cells API, Excel data visualization, Conditional formatting tutorial
difficulty: Intermediate
---

# Tutorial: Basic Conditional Formatting

In this tutorial, you'll learn how to apply conditional formatting to Excel worksheets using Aspose.Cells Cloud API. Conditional formatting is a powerful feature that changes the appearance of cells based on specific conditions, making it easier to identify important data, trends, and outliers.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Understand what conditional formatting is and how it works
- Create basic conditional formatting rules
- Apply formatting to cell ranges
- View and manage conditional formatting rules
- Remove conditional formatting when it's no longer needed

## Prerequisites

Before you begin, ensure you have:
- An Aspose Cloud account with an active subscription or trial
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
- Basic understanding of RESTful APIs
- An Excel file with data to practice conditional formatting

## Understanding Conditional Formatting

Conditional formatting allows you to apply formatting to cells that meet specific criteria. For example, you might:
- Highlight cells with values greater than a certain threshold
- Color cells that contain specific text
- Apply data bars or color scales to visually represent values
- Use icon sets to categorize data based on value ranges

### 1. Getting Started: Create a Basic Condition

Let's start by creating a simple conditional formatting rule that highlights cells with values greater than 100.

#### Step 1: Understand the API Endpoints

For conditional formatting, we'll use several endpoints:
- To create a format condition: `PUT /cells/{name}/worksheets/{sheetName}/conditionalFormattings/{index}`
- To add a condition: `PUT /cells/{name}/worksheets/{sheetName}/conditionalFormattings/{index}/condition`
- To get formatting rules: `GET /cells/{name}/worksheets/{sheetName}/conditionalFormattings`

#### Step 2: Create a Format Condition

First, let's add a conditional formatting rule to a worksheet.

##### Using cURL:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/cells/FinancialData.xlsx/worksheets/Sheet1/conditionalFormattings/0?cellArea=A1:C10&type=CellValue&operatorType=GreaterThan&formula1=100" \
-H "Authorization: Bearer <your_access_token>" \
-H "Accept: application/json"
```

> Note: Replace `<your_access_token>` with your actual bearer token obtained from Aspose Cloud.

##### Using Python SDK:

```python
# Tutorial Code Example: Create a Basic Conditional Format
import asposecellscloud
from asposecellscloud.apis.cells_api import CellsApi
from asposecellscloud.models import *
from asposecellscloud.requests import *

# Configure API credentials
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"
api = CellsApi(client_id, client_secret)

# Specify file and formatting details
file_name = "FinancialData.xlsx"
sheet_name = "Sheet1"
index = 0  # First conditional formatting rule
cell_area = "A1:C10"  # Range to apply the formatting
format_type = "CellValue"
operator_type = "GreaterThan"
formula1 = "100"  # The threshold value

# Create the conditional formatting rule
response = api.put_worksheet_format_condition(
    name=file_name,
    sheet_name=sheet_name,
    index=index,
    cell_area=cell_area,
    type=format_type,
    operator_type=operator_type,
    formula1=formula1
)

print(f"Conditional format created: {response.status}")

# Expected output:
# Conditional format created: OK
```

#### Step 3: Add a Style to the Condition

Now let's define how cells that meet our condition should be formatted.

##### Using cURL:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/cells/FinancialData.xlsx/worksheets/Sheet1/conditionalFormattings/0/condition?type=CellValue&operatorType=GreaterThan&formula1=100" \
-H "Authorization: Bearer <your_access_token>" \
-H "Accept: application/json"
```

##### Using C# SDK:

```csharp
// Tutorial Code Example: Add Condition Style
using Aspose.Cells.Cloud.SDK.Api;
using Aspose.Cells.Cloud.SDK.Model;
using System;

namespace AsposeCellsConditionalFormattingExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API credentials
            var cellsApi = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Specify file details
            string fileName = "FinancialData.xlsx";
            string sheetName = "Sheet1";
            int formatIndex = 0;
            
            // Define the condition
            cellsApi.PutWorksheetFormatConditionCondition(
                fileName,
                sheetName,
                formatIndex,
                type: "CellValue",
                operatorType: "GreaterThan",
                formula1: "100"
            );
            
            Console.WriteLine("Condition created successfully!");
            
            // Now get the formatting to verify it was created
            var formattings = cellsApi.GetWorksheetConditionalFormattings(fileName, sheetName);
            
            if (formattings.ConditionalFormattings.ConditionalFormattingList.Count > 0)
            {
                var formatting = formattings.ConditionalFormattings.ConditionalFormattingList[0];
                Console.WriteLine($"Conditional Format created for range: {formatting.Sqref}");
                
                foreach (var condition in formatting.FormatConditions)
                {
                    Console.WriteLine($"  Type: {condition.Type}");
                    Console.WriteLine($"  Operator: {condition.Operator}");
                    Console.WriteLine($"  Formula1: {condition.Formula1}");
                }
            }
        }
    }
}
```

### 2. Creating Different Types of Conditional Formatting

Aspose.Cells Cloud API supports various types of conditional formatting. Let's explore a few common ones.

#### Example: Highlight Cells Containing Specific Text

##### Using Java SDK:

```java
// Tutorial Code Example: Text-Based Conditional Formatting
import com.aspose.cells.cloud.api.CellsApi;
import com.aspose.cells.cloud.model.*;
import com.aspose.cells.cloud.request.*;

public class TextConditionalFormatExample {
    public static void main(String[] args) {
        // Configure API credentials
        CellsApi api = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        // Specify file details
        String fileName = "EmployeeData.xlsx";
        String sheetName = "Sheet1";
        int formatIndex = 1; // Second format rule
        
        try {
            // Create a conditional format for text containing "Manager"
            PutWorksheetFormatConditionRequest request = new PutWorksheetFormatConditionRequest(
                fileName,
                sheetName,
                formatIndex,
                "D2:D100", // Cell area - employee titles column
                "ContainsText", // Type
                "ContainsText", // OperatorType
                "Manager", // Formula1
                null, // Formula2
                null, // folder
                null  // storageName
            );
            
            api.putWorksheetFormatCondition(request);
            System.out.println("Text-based conditional format created!");
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### Example: Add a Color Scale

A color scale applies a gradient based on cell values. Here's how to create a color scale that shows low values in red, medium values in yellow, and high values in green.

##### Using cURL:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/cells/SalesData.xlsx/worksheets/Sheet1/conditionalFormattings/1?cellArea=E2:E20&type=ColorScale" \
-H "Authorization: Bearer <your_access_token>" \
-H "Accept: application/json"
```

### 3. Getting All Conditional Formatting Rules

Let's learn how to retrieve all the conditional formatting rules in a worksheet.

##### Using cURL:

```bash
curl -X GET "https://api.aspose.cloud/v3.0/cells/FinancialData.xlsx/worksheets/Sheet1/conditionalFormattings" \
-H "Authorization: Bearer <your_access_token>" \
-H "Accept: application/json"
```

### 4. Removing Conditional Formatting

Finally, let's see how to remove conditional formatting when it's no longer needed.

#### Removing a Specific Rule:

##### Using cURL:

```bash
curl -X DELETE "https://api.aspose.cloud/v3.0/cells/FinancialData.xlsx/worksheets/Sheet1/conditionalFormattings/0" \
-H "Authorization: Bearer <your_access_token>" \
-H "Accept: application/json"
```

This removes the first conditional formatting rule (index 0).

#### Removing All Rules:

##### Using cURL:

```bash
curl -X DELETE "https://api.aspose.cloud/v3.0/cells/FinancialData.xlsx/worksheets/Sheet1/conditionalFormattings" \
-H "Authorization: Bearer <your_access_token>" \
-H "Accept: application/json"
```

##### Using Python SDK:

```python
# Tutorial Code Example: Remove All Conditional Formatting
import asposecellscloud
from asposecellscloud.apis.cells_api import CellsApi
from asposecellscloud.models import *
from asposecellscloud.requests import *

# Configure API credentials
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"
api = CellsApi(client_id, client_secret)

# Specify file details
file_name = "FinancialData.xlsx"
sheet_name = "Sheet1"

# Remove all conditional formatting
response = api.delete_worksheet_conditional_formattings(
    name=file_name,
    sheet_name=sheet_name
)

print(f"All conditional formatting removed: {response.status}")

# Expected output:
# All conditional formatting removed: OK
```

## Try It Yourself

Now that you've learned the basics of conditional formatting, try these exercises to reinforce your knowledge:

1. Exercise 1: Create a conditional format that highlights cells with values below 50 in red
2. Exercise 2: Create a conditional format that highlights cells containing the text "Urgent" in bold red
3. Exercise 3: Apply a color scale to a range of numerical data
4. Exercise 4: View all conditioŠnal formatting rules in your worksheet, then remove a specific rule

## Troubleshooting Common Issues

### Issue 1: Format Not Applying to Cells
If your conditional formatting doesn't appear to be working:
- Verify that your cell range is correct
- Check that your condition type and operator are compatible
- Make sure your formula values are appropriate for your data

### Issue 2: Can't Remove a Formatting Rule
If you're having trouble removing a rule:
- Double-check the index of the rule you want to remove
- Try retrieving all rules first to confirm the correct index
- As a last resort, remove all rules and recreate the ones you want to keep

## What You've Learned

In this tutorial, you've learned:
- How to create basic conditional formatting rules
- How to apply different types of conditions, including value-based and text-based conditions
- How to view existing conditional formatting rules
- How to remove specific rules or all rules from a worksheet

## Next Tutorial

Ready to explore more advanced conditional formatting techniques? Continue with our next tutorial: [Advanced Formatting Rules](/tutorials/spreadsheet-elements/conditional-formattings/advanced/), where you'll learn about icon sets, data bars, and complex formatting conditions.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cells/)
- [Documentation](https://docs.aspose.cloud/cells/)
- [Live Demo](https://products.aspose.app/cells/family)
- [API Reference](https://reference.aspose.cloud/cells/)
- [Blog](https://blog.aspose.cloud/category/cells/)
- [Free Support](https://forum.aspose.cloud/c/cells/7)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/cells/7)