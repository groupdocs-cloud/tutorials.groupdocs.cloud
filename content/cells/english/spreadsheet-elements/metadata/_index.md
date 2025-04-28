---
title:  Document Metadata and Properties Complete Tutorial
second_title: Aspose.Cells Cloud API Tutorial

url: /spreadsheet-elements/metadata/
description: Learn to read, add, update, and delete document properties and metadata in Excel files using Aspose.Cells Cloud API in this comprehensive tutorial.
weight: 60
keywords: Excel metadata, document properties, Excel API tutorial, Aspose.Cells tutorial, file metadata management
---

# Document Metadata and Properties: Complete Tutorial

## Prerequisites

Before starting this tutorial, make sure you have:
1. An Aspose Cloud account (sign up for a [free trial](https://dashboard.aspose.cloud/#/apps) if you don't have one)
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
3. Basic understanding of REST APIs
4. Familiarity with your chosen programming language

## Introduction to Document Properties in Excel

Excel files can contain various metadata, also known as document properties. These properties provide information about the file itself, such as its author, creation date, last modified date, and more. Document properties fall into two categories:

1. Built-in properties: Standard properties like Title, Author, Subject, Keywords, Category, etc.
2. Custom properties: User-defined properties that you can create to store specific information.

Using Aspose.Cells Cloud API, you can programmatically manage these document properties in your Excel files without requiring Microsoft Excel to be installed, providing a powerful way to automate metadata management.

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

### 2. Getting All Document Properties

Let's start by retrieving all document properties from an Excel file:

#### Using cURL:

```bash
curl -X GET "$BASE_URL/test.xlsx/documentproperties" \
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
    # Get all document properties
    response = api_instance.cells_properties_get_document_properties(
        name="test.xlsx")
    
    # Print property information
    if response.document_properties and response.document_properties.document_property_list:
        print(f"Document Properties:")
        
        # Print each property
        for prop in response.document_properties.document_property_list:
            print(f"  {prop.name}: {prop.value} (Built-in: {prop.built_in})")
    else:
        print("No document properties found.")
        
except Exception as e:
    print("Exception when calling CellsApi->cells_properties_get_document_properties: %s\n" % e)
```

### 3. Getting a Specific Document Property

To retrieve a specific document property by its name:

#### Using cURL:

```bash
curl -X GET "$BASE_URL/test.xlsx/documentproperties/Author" \
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
            string propertyName = "Author";
            
            try
            {
                // Get the specific document property
                var response = cellsApi.CellsPropertiesGetDocumentProperty(
                    fileName, 
                    propertyName);
                
                // Display property details
                if (response.DocumentProperty != null)
                {
                    Console.WriteLine("Document Property Details:");
                    Console.WriteLine($"Name: {response.DocumentProperty.Name}");
                    Console.WriteLine($"Value: {response.DocumentProperty.Value}");
                    Console.WriteLine($"Built-in: {response.DocumentProperty.BuiltIn}");
                }
                else
                {
                    Console.WriteLine($"Property '{propertyName}' not found.");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### 4. Adding or Updating a Document Property

To add a new document property or update an existing one:

#### Using cURL:

```bash
curl -X PUT "$BASE_URL/test.xlsx/documentproperties/Company" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"Value": "Aspose", "Name": "Company", "BuiltIn": "False"}'
```

#### Using Java SDK:

```java
import com.aspose.cells.cloud.api.CellsApi;
import com.aspose.cells.cloud.model.*;
import com.aspose.cells.cloud.request.*;

public class UpdateDocumentPropertyExample {
    public static void main(String[] args) {
        // Configure API credentials
        CellsApi cellsApi = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        try {
            // Specify parameters
            String fileName = "test.xlsx";
            String propertyName = "Company";
            
            // Create document property object
            DocumentProperty property = new DocumentProperty();
            property.setName(propertyName);
            property.setValue("Aspose");
            property.setBuiltIn("False");
            
            // Create request
            PutDocumentPropertyRequest request = new PutDocumentPropertyRequest(
                fileName,
                propertyName,
                property,
                null, // folder
                null  // storage name
            );
            
            // Add/update the document property
            cellsApi.putDocumentProperty(request);
            
            System.out.println("Document property added/updated successfully.");
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 5. Deleting a Specific Document Property

To delete a specific document property by its name:

#### Using cURL:

```bash
curl -X DELETE "$BASE_URL/test.xlsx/documentproperties/Company" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
-H "Accept: application/json"
```

#### Using Node.js SDK:

```javascript
const { CellsApi } = require('asposecellscloud');

// Configure API credentials
const cellsApi = new CellsApi('YOUR_CLIENT_ID', 'YOUR_CLIENT_SECRET');

// Specify parameters
const fileName = "test.xlsx";
const propertyName = "Company";

// Delete the document property
cellsApi.cellsPropertiesDeleteDocumentProperty(fileName, propertyName)
    .then(() => {
        console.log(`Document property '${propertyName}' deleted successfully.`);
    })
    .catch(error => {
        console.error("Error:", error);
    });
```

### 6. Deleting All Document Properties

To remove all document properties from an Excel file:

#### Using cURL:

```bash
curl -X DELETE "$BASE_URL/test.xlsx/documentproperties" \
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
    
    # Delete all document properties
    response = api_instance.cells_properties_delete_document_properties(
        name=name
    )
    
    print("All document properties deleted successfully.")
    
except Exception as e:
    print("Exception when calling CellsApi->cells_properties_delete_document_properties: %s\n" % e)
```

### 7. Getting Metadata Without Using Storage

Aspose.Cells Cloud API also allows you to get metadata from Excel files without storing them first:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/metadata/get" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-F "file=@test.xlsx"
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
            
            // Specify file path
            string filePath = "test.xlsx";
            
            try
            {
                // Read file content
                var fileStream = File.OpenRead(filePath);
                
                // Get metadata without storage
                var response = cellsApi.CellsMetadataGet(fileStream);
                
                // Display metadata
                if (response != null && response.Count > 0)
                {
                    Console.WriteLine("Document Properties:");
                    foreach (var property in response)
                    {
                        Console.WriteLine($"{property.Name}: {property.Value}");
                    }
                }
                else
                {
                    Console.WriteLine("No metadata found.");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```
### 8. Updating Metadata Without Using Storage

You can also update metadata in Excel files without storing them first:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/metadata/update" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-F "file=@test.xlsx" \
-F 'properties=[{"name":"Author","value":"John Doe"},{"name":"Company","value":"Aspose"},{"name":"Category","value":"Tutorial"}]'
```

#### Using Java SDK:

```java
import com.aspose.cells.cloud.api.CellsApi;
import com.aspose.cells.cloud.model.*;
import com.aspose.cells.cloud.request.*;
import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class UpdateMetadataWithoutStorageExample {
    public static void main(String[] args) {
        // Configure API credentials
        CellsApi cellsApi = new CellsApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        try {
            // Specify file path
            String filePath = "test.xlsx";
            
            // Create properties to update
            List<DocumentProperty> properties = new ArrayList<>();
            
            DocumentProperty author = new DocumentProperty();
            author.setName("Author");
            author.setValue("John Doe");
            properties.add(author);
            
            DocumentProperty company = new DocumentProperty();
            company.setName("Company");
            company.setValue("Aspose");
            properties.add(company);
            
            DocumentProperty category = new DocumentProperty();
            category.setName("Category");
            category.setValue("Tutorial");
            properties.add(category);
            
            // Update metadata without storage
            File result = cellsApi.cellsMetadataUpdate(new File(filePath), properties);
            
            System.out.println("Metadata updated successfully. Updated file: " + result.getAbsolutePath());
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 9. Deleting Metadata Without Using Storage

Similarly, you can delete metadata from Excel files without storing them:

#### Using cURL:

```bash
curl -X POST "$BASE_URL/metadata/delete" \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-F "file=@test.xlsx" \
-F "type=all"
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
    # Specify file path
    file_path = "test.xlsx"
    
    # Delete metadata without storage
    with open(file_path, 'rb') as file:
        response = api_instance.cells_metadata_delete(
            file=file,
            type="all"  # Delete all properties (can also be "BuiltIn" or "Custom")
        )
    
    # Save updated file
    with open("updated_" + file_path, 'wb') as out_file:
        for file_info in response.files:
            out_file.write(file_info.file_content)
    
    print("Metadata deleted successfully. Updated file saved as: updated_" + file_path)
    
except Exception as e:
    print("Exception when calling CellsApi->cells_metadata_delete: %s\n" % e)
```

## Try It Yourself

Now it's time to practice what you've learned:

1. Create a new Excel file or use an existing one
2. Retrieve all document properties and see what's already there
3. Add several custom properties (e.g., Project, Department, Status)
4. Update one of the built-in properties (e.g., Author, Title)
5. Delete a specific property
6. Try the methods that don't require storage to process metadata
7. Finally, delete all properties and verify they're gone

## Troubleshooting Tips

- Error 401 (Unauthorized): Make sure your Client ID and Client Secret are correct and that your token hasn't expired.
- Error 404 (Not Found): Check that your file name and property names are correct.
- Property not being updated: For built-in properties, ensure you're using the exact property name with correct capitalization (e.g., "Author" not "author").
- Custom property not appearing: When creating custom properties, make sure to set BuiltIn to "False".
- Values not appearing as expected: Document property values are stored as strings, so ensure proper conversion if you're working with numbers or dates.

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve all document properties from an Excel file
- Get a specific document property by its name
- Add new document properties or update existing ones
- Delete specific document properties by name
- Remove all document properties from a file
- Work with metadata without using storage (direct file processing)
- Update multiple properties at once

You now have the skills to programmatically manage document metadata in Excel files using the Aspose.Cells Cloud API, allowing you to automate document information management.

## Next Steps

Consider exploring these related tutorials:
- [List Objects and Tables Tutorial](/tutorials/spreadsheet-elements/list-objects/)
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
