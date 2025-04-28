---
title: Managing Shapes in Excel with Aspose.Cells Cloud API Tutorial
second_title: Learn to Add, Manipulate, and Export Shapes Programmatically

url: /spreadsheet-elements/shapes/
weight: 40
description: This step-by-step tutorial teaches you how to add, modify, delete, and export shapes in Excel spreadsheets using Aspose.Cells Cloud API.
keywords: Excel shapes tutorial, add shapes API, delete shapes tutorial, export shapes to image, manipulate Excel shapes, Aspose.Cells Cloud tutorial, AutoShapes Excel API
---

# Tutorial: Managing Shapes in Excel with Aspose.Cells Cloud API

## Introduction

Welcome to this comprehensive tutorial on managing shapes in Excel using Aspose.Cells Cloud API. Shapes add visual elements to Excel spreadsheets and can include various objects like arrows, circles, rectangles, lines, flowchart symbols, and more. These shapes help in creating diagrams, annotations, or enhancing the overall visual appeal of your spreadsheets.

In this tutorial, you'll learn how to programmatically add, modify, delete, and export shapes in Excel spreadsheets using Aspose.Cells Cloud API.

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account (sign up for a [free trial](https://dashboard.aspose.cloud/#/apps) if you don't have one)
2. Your Client ID and Client Secret from the Aspose Cloud dashboard
3. Basic knowledge of REST APIs and HTTP concepts
4. Familiarity with Excel concepts and shapes
5. A development environment or API testing tool (like Postman, cURL, etc.)
6. Completed the [Worksheets tutorial](/tutorials/spreadsheet-elements/worksheets/) (recommended but not required)

## Tutorial Steps

### Step 1: Authentication with Aspose.Cells Cloud API

Before working with shapes, you need to authenticate with the Aspose.Cells Cloud API to get an access token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the access token from the response for use in subsequent API calls.

### Step 2: Get All Shapes in a Worksheet

Let's start by retrieving information about all shapes in an existing worksheet:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This API returns information about all shapes in the specified worksheet.

#### Example Response:

```json
{
  "Shapes": {
    "ShapeList": [
      {
        "link": {
          "Href": "/0",
          "Rel": "self",
          "Type": null,
          "Title": null
        }
      },
      {
        "link": {
          "Href": "/1",
          "Rel": "self",
          "Type": null,
          "Title": null
        }
      }
    ],
    "link": {
      "Href": "http://api.aspose.com/v1.1/cells/sampleShapes.xlsx/worksheets/Sheet1/shapes",
      "Rel": "self",
      "Type": null,
      "Title": null
    }
  },
  "Code": 200,
  "Status": "OK"
}
```

### Step 3: Get Information About a Specific Shape

Now, let's retrieve detailed information about a specific shape:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes/0" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This request retrieves information about the first shape (index 0) in the worksheet.

#### SDK Example (Python):

```python
# Get shape information example - Tutorial Code Example
import requests
import json

# Authentication
auth_url = "https://api.aspose.cloud/connect/token"
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

auth_data = {
    "grant_type": "client_credentials",
    "client_id": client_id,
    "client_secret": client_secret
}

auth_headers = {
    "Content-Type": "application/x-www-form-urlencoded",
    "Accept": "application/json"
}

auth_response = requests.post(auth_url, data=auth_data, headers=auth_headers)
access_token = auth_response.json().get("access_token")

# Get shape information
api_url = "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes/0"
headers = {
    "Content-Type": "application/json",
    "Accept": "application/json",
    "Authorization": f"Bearer {access_token}"
}

response = requests.get(api_url, headers=headers)
result = response.json()

# Display shape information
if response.status_code == 200:
    print("Shape information retrieved successfully!")
    
    shape = result.get("Shape", {})
    print("\nShape Details:")
    print(f"Name: {shape.get('Name')}")
    print(f"Type: {shape.get('AutoShapeType')}")
    print(f"Position: Row {shape.get('UpperLeftRow')}, Column {shape.get('UpperLeftColumn')}")
    print(f"Width: {shape.get('Width')} points")
    print(f"Height: {shape.get('Height')} points")
    
    if shape.get("Text"):
        print(f"Text: {shape.get('Text')}")
        
    print(f"Is Group: {shape.get('IsGroup', False)}")
    print(f"Is Hidden: {shape.get('IsHidden', False)}")
    
else:
    print(f"Error: {response.status_code}")
    print(result)
```

### Step 4: Add a Shape to a Worksheet

Now let's learn how to add a new shape to a worksheet:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes?DrawingType=arc&upperLeftRow=1&upperLeftColumn=1&top=1&left=1&width=100&height=100" \
-X PUT \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This example adds an arc shape at position (1,1) with width and height of 100 points.

#### Supported Shape Types

Aspose.Cells Cloud API supports various shape types, including:
- arc
- button
- chart
- checkbox
- comment
- group
- line
- oval
- picture
- polygon
- rectangle
- textbox
- And many more...

#### SDK Example (C#):

```csharp
// Add a shape example - Tutorial Code Example
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;

namespace AsposeCellsTutorials
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // Authentication
            var client = new HttpClient();
            var authRequest = new HttpRequestMessage(HttpMethod.Post, "https://api.aspose.cloud/connect/token");
            
            var authContent = new FormUrlEncodedContent(new Dictionary<string, string>
            {
                {"grant_type", "client_credentials"},
                {"client_id", "YOUR_CLIENT_ID"},
                {"client_secret", "YOUR_CLIENT_SECRET"}
            });
            
            authRequest.Content = authContent;
            var authResponse = await client.SendAsync(authRequest);
            var authResult = await authResponse.Content.ReadAsStringAsync();
            
            // Parse access token from JSON response
            // For simplicity, using basic string operations
            var accessToken = authResult.Split("\"access_token\":\"")[1].Split("\"")[0];
            
            // Add a shape
            var apiClient = new HttpClient();
            apiClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
            
            // Parameters for the new shape
            var shapeType = "rectangle";
            var upperLeftRow = 2;
            var upperLeftColumn = 2;
            var top = 20;
            var left = 20;
            var width = 200;
            var height = 100;
            
            var apiUrl = $"https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes?DrawingType={shapeType}&upperLeftRow={upperLeftRow}&upperLeftColumn={upperLeftColumn}&top={top}&left={left}&width={width}&height={height}";
            
            var response = await apiClient.PutAsync(apiUrl, null);
            
            if (response.IsSuccessStatusCode)
            {
                Console.WriteLine("Shape added successfully!");
                Console.WriteLine($"A {shapeType} shape was added at row {upperLeftRow}, column {upperLeftColumn}");
                Console.WriteLine($"Dimensions: {width}x{height} points");
            }
            else
            {
                Console.WriteLine($"Error: {response.StatusCode} - {await response.Content.ReadAsStringAsync()}");
            }
        }
    }
}
```

### Step 5: Update a Shape's Properties

You can modify an existing shape's properties:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes/0" \
-X POST \
-d "{ \"Height\": 150, \"Width\": 300, \"Top\": 50, \"Left\": 100, \"Text\": \"Updated Shape Text\", \"AlternativeText\": \"This is an example shape\" }" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This example updates the size, position, and text of the first shape in the worksheet.

### Step 6: Convert a Shape to an Image

You can export a shape as an image in various formats:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes/0?format=png" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
--output shape.png
```

This example exports the first shape as a PNG image.

#### Supported Image Formats
You can export shapes to various image formats, including:
- png
- jpg
- gif
- bmp
- tiff
- svg

#### SDK Example (Java):

```java
// Export shape to image example - Tutorial Code Example
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.util.Scanner;

public class ExportShapeToImageExample {
    public static void main(String[] args) {
        try {
            // Authentication
            String clientId = "YOUR_CLIENT_ID";
            String clientSecret = "YOUR_CLIENT_SECRET";
            String authUrl = "https://api.aspose.cloud/connect/token";
            
            URL url = new URL(authUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("POST");
            conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
            conn.setRequestProperty("Accept", "application/json");
            conn.setDoOutput(true);
            
            String authData = "grant_type=client_credentials&client_id=" + clientId + "&client_secret=" + clientSecret;
            conn.getOutputStream().write(authData.getBytes(StandardCharsets.UTF_8));
            
            // Read auth response
            Scanner scanner = new Scanner(conn.getInputStream());
            String authResponse = scanner.useDelimiter("\\A").next();
            scanner.close();
            
            // Extract access token (simple approach for demonstration)
            String accessToken = authResponse.split("\"access_token\":\"")[1].split("\"")[0];
            
            // Export shape to image
            String apiUrl = "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes/0?format=png";
            URL exportUrl = new URL(apiUrl);
            HttpURLConnection exportConn = (HttpURLConnection) exportUrl.openConnection();
            exportConn.setRequestMethod("GET");
            exportConn.setRequestProperty("Authorization", "Bearer " + accessToken);
            
            int responseCode = exportConn.getResponseCode();
            if (responseCode == HttpURLConnection.HTTP_OK) {
                System.out.println("Shape exported to image successfully!");
                
                // Save the image to a file
                try (InputStream inputStream = exportConn.getInputStream();
                     FileOutputStream outputStream = new FileOutputStream("shape.png")) {
                    
                    byte[] buffer = new byte[4096];
                    int bytesRead;
                    while ((bytesRead = inputStream.read(buffer)) != -1) {
                        outputStream.write(buffer, 0, bytesRead);
                    }
                }
                
                System.out.println("Image saved as 'shape.png'");
            } else {
                System.out.println("Error: " + responseCode);
                Scanner errorScanner = new Scanner(exportConn.getErrorStream());
                System.out.println(errorScanner.useDelimiter("\\A").next());
                errorScanner.close();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Step 7: Delete a Shape

To remove a specific shape from a worksheet:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes/0" \
-X DELETE \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This example deletes the first shape (index 0) from the worksheet.

### Step 8: Delete All Shapes

You can also delete all shapes from a worksheet at once:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/shapes" \
-X DELETE \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This example removes all shapes from the specified worksheet.

### Step 9: Working with AutoShapes

AutoShapes are predefined shapes like stars, arrows, and flowchart symbols. You can access specific AutoShapes:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/autoshapes/0" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This example retrieves information about the first AutoShape in the worksheet.

### Step 10: Export AutoShapes to Image Formats

Similar to regular shapes, you can export AutoShapes to various image formats:

```bash
curl -v "https://api.aspose.cloud/v3.0/cells/Book1.xlsx/worksheets/Sheet1/autoshapes/0?format=png" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
--output autoshape.png
```

This example exports the first AutoShape as a PNG image.

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve information about shapes in a worksheet
- Add different types of shapes to a worksheet
- Modify shape properties like size, position, and text
- Convert shapes to images in various formats
- Delete individual shapes or all shapes from a worksheet
- Work with AutoShapes

## Troubleshooting Tips

- Shape Indices: Shape indices are zero-based and may change if shapes are added or removed. Always verify the current shapes before performing operations.
- Drawing Types: Make sure to specify valid drawing types when adding shapes. Invalid types will result in errors.
- Shape Positioning: When placing shapes, consider both the cell reference (row/column) and pixel positioning (top/left) for precise placement.
- Export Formats: When exporting shapes to images, ensure you specify a supported format.
- API Rate Limiting: If you receive 429 (Too Many Requests) responses, you might have exceeded your API quota.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a flowchart using various shapes and connectors
2. Build a dashboard with interactive shapes (like buttons or checkboxes)
3. Create an organizational chart using grouped shapes
4. Implement a batch operation to export all shapes as separate image files
5. Create a shape library with predefined shapes for reuse

## Next Tutorial

Ready to continue your learning journey? Check out our tutorial on [Hyperlinks Management](/spreadsheet-elements/hyperlinks/) to learn how to add and manipulate hyperlinks in Excel spreadsheets.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cells/)
- [Documentation](https://docs.aspose.cloud/cells/)
- [Live Demo](https://products.aspose.app/cells/family)
- [API Reference](https://reference.aspose.cloud/cells/)
- [Blog](https://blog.aspose.cloud/category/cells/)
- [Free Support](https://forum.aspose.cloud/c/cells/7)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/cells/7)
