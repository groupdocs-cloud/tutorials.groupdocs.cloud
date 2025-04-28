---
title: Learn to Work with Pictures in Excel Files
second_title: Aspose.Cells Cloud API Tutorial

url: /spreadsheet-elements/pictures/
description: Tutorial on how to add, retrieve, update, and delete pictures in Excel worksheets using Aspose.Cells Cloud API.
weight: 10
keywords: Excel pictures, add images to Excel, Excel API tutorial, Aspose.Cells tutorial, REST API pictures
---

# Tutorial: Working with Pictures in Excel Files

## Prerequisites

Before starting this tutorial, make sure you have:
1. An Aspose Cloud account (sign up for a [free trial](https://dashboard.aspose.cloud/#/apps) if you don't have one)
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
3. Basic understanding of REST APIs
4. Familiarity with your chosen programming language
5. An image file ready to use in your examples (e.g., logo.png)

## Introduction to Picture Management

Excel worksheets can contain various types of pictures and images that enhance data visualization and presentation. Using Aspose.Cells Cloud API, you can programmatically manipulate these pictures without requiring Microsoft Excel to be installed. This provides a powerful way to automate image-related operations in your Excel files.

## Tutorial Steps

### 1. Authentication

All API calls require authentication. Let's first set up the authentication:

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

### 2. Getting All Pictures from a Worksheet

Let's start by retrieving all pictures from a specific worksheet:

#### Using cURL:

```bash
curl -X GET "$BASE_URL/test.xlsx/worksheets/Sheet1/pictures" \
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
    # Get all pictures
    response = api_instance.cells_pictures_get_worksheet_pictures(
        name="test.xlsx", 
        sheet_name="Sheet1")
    
    # Print picture count
    print(f"Number of pictures: {len(response.pictures.picture_list)}")
    
    # Print each picture's link
    for idx, pic in enumerate(response.pictures.picture_list):
        print(f"Picture {idx}: {pic.link.href}")
        
except Exception as e:
    print("Exception when calling CellsApi->cells_pictures_get_worksheet_pictures: %s\n" % e)
```

### 3. Getting a Specific Picture

To retrieve a specific picture by its index:

#### Using cURL:

```bash
# Get picture with index 0, converting to PNG format
curl -X GET "$BASE_URL/test.xlsx/worksheets/Sheet1/pictures/0?format=png" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-o "output_picture.png"
```

#### Using C# SDK:

```csharp
using System;
using System.IO;
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
            string fileName = "test.xlsx";
            string sheetName = "Sheet1";
            int pictureIndex = 0; // First picture
            string format = "png"; // Desired output format
            
            try
            {
                // Get the picture
                var response = cellsApi.CellsPicturesGetWorksheetPictureWithFormat(
                    fileName, 
                    sheetName, 
                    pictureIndex, 
                    format);
                
                // Save to file
                using (var fileStream = File.Create("output_picture.png"))
                {
                    response.CopyTo(fileStream);
                }
                
                Console.WriteLine("Picture successfully downloaded.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### 4. Adding a Picture to a Worksheet

Now, let's add a new picture to a worksheet:

#### Using cURL:

```bash
curl -X PUT "$BASE_URL/test.xlsx/worksheets/Sheet1/pictures?picturePath=logo.png&upperLeftRow=5&upperLeftColumn=5" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json"
```

#### Using Java SDK:

```java
import com.aspose.cells.cloud.api.CellsApi;
import com.aspose.cells.cloud.model.*;
import com.aspose.cells.cloud.request.*;

public class AddPictureExample {
    public static void main(String[] args) {
        // Configure API credentials
        CellsApi cellsApi = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        try {
            // Specify parameters
            String fileName = "test.xlsx";
            String sheetName = "Sheet1";
            String picturePath = "logo.png"; // The path to your picture
            Integer upperLeftRow = 5;
            Integer upperLeftColumn = 5;
            
            // Create request
            PutWorksheetAddPictureRequest request = new PutWorksheetAddPictureRequest(
                fileName,
                sheetName,
                null, // picture object
                upperLeftRow,
                upperLeftColumn,
                null, // lower right row
                null, // lower right column
                picturePath,
                null, // folder
                null  // storage name
            );
            
            // Add the picture
            cellsApi.putWorksheetAddPicture(request);
            
            System.out.println("Picture added successfully.");
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 5. Updating an Existing Picture

To update an existing picture's properties:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/test.xlsx/worksheets/Sheet1/pictures/0" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{ "UpperLeftRow": 10, "Top": 0, "UpperLeftColumn": 1, "Left": 0, "LowerRightRow": 0, "Bottom": 0, "LowerRightColumn": 3, "ImageFormat": "jpg", "SourceFullName": "logo.jpg"}'
```

#### Using Node.js SDK:

```javascript
const { CellsApi } = require('asposecellscloud');

// Configure API credentials
const cellsApi = new CellsApi('YOUR_CLIENT_ID', 'YOUR_CLIENT_SECRET');

// Specify parameters
const fileName = "test.xlsx";
const sheetName = "Sheet1";
const pictureIndex = 0; // First picture

// Create the picture object to update
const pictureToUpdate = {
    UpperLeftRow: 10,
    Top: 0,
    UpperLeftColumn: 1, 
    Left: 0,
    LowerRightRow: 0,
    Bottom: 0,
    LowerRightColumn: 3,
    ImageFormat: "jpg",
    SourceFullName: "logo.jpg"
};

// Update the picture
cellsApi.cellsPicturesPostWorksheetPicture(fileName, sheetName, pictureIndex, pictureToUpdate)
    .then(() => {
        console.log("Picture updated successfully.");
    })
    .catch(error => {
        console.error("Error:", error);
    });
```

### 6. Deleting a Specific Picture

To delete a specific picture by its index:

#### Using cURL:

```bash
curl -X DELETE "$BASE_URL/test.xlsx/worksheets/Sheet1/pictures/0" \
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
    name = "test.xlsx"
    sheet_name = "Sheet1"
    picture_index = 0  # First picture
    
    # Delete the picture
    response = api_instance.cells_pictures_delete_worksheet_picture(
        name=name,
        sheet_name=sheet_name,
        picture_index=picture_index
    )
    
    print("Picture deleted successfully.")
    
except Exception as e:
    print("Exception when calling CellsApi->cells_pictures_delete_worksheet_picture: %s\n" % e)
```

### 7. Deleting All Pictures from a Worksheet

To remove all pictures from a specific worksheet:

#### Using cURL:

```bash
curl -X DELETE "$BASE_URL/test.xlsx/worksheets/Sheet1/pictures" \
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
            string fileName = "test.xlsx";
            string sheetName = "Sheet1";
            
            try
            {
                // Delete all pictures
                var response = cellsApi.CellsPicturesDeleteWorksheetPictures(fileName, sheetName);
                
                Console.WriteLine("All pictures deleted successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

## Try It Yourself

Now it's time to practice what you've learned:

1. Create a new Excel file or use an existing one
2. Add multiple images to the file using the Aspose.Cells Cloud API
3. Retrieve the list of pictures and their properties
4. Update one of the pictures (change its position or size)
5. Delete a specific picture
6. Finally, delete all pictures from the worksheet

## Troubleshooting Tips

- Error 401 (Unauthorized): Make sure your Client ID and Client Secret are correct and that your token hasn't expired.
- Error 404 (Not Found): Check that your file name, worksheet name, and picture index are correct.
- Picture not displaying correctly: Verify the image format is supported (PNG, JPG, etc.) and check the positioning parameters (rows and columns).
- Performance issues with large files: When working with large Excel files with many pictures, consider processing pictures in batches.

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve all pictures from an Excel worksheet
- Get a specific picture by its index
- Add new pictures to a worksheet
- Update properties of existing pictures
- Delete specific pictures
- Remove all pictures from a worksheet

You now have the skills to programmatically manage pictures in Excel files using the Aspose.Cells Cloud API.

## Next Steps

Consider exploring these related tutorials:
- [Tutorial: How to Manage Hyperlinks in Excel Worksheets](/spreadsheet-elements/hyperlinks/)
- [Working with OLE Objects: A Comprehensive Tutorial](/spreadsheet-elements/oleobjects/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cells/)
- [Documentation](https://docs.aspose.cloud/cells/)
- [Live Demo](https://products.aspose.app/cells/family)
- [API Reference](https://reference.aspose.cloud/cells/)
- [Blog](https://blog.aspose.cloud/category/cells/)
- [Free Support](https://forum.aspose.cloud/c/cells/7)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/cells/7)
