---
title: Uploading and Scaling Images with POST Method Tutorial
url: /change-scale-of-an-image/basic-post-method/
weight: 30
description: Learn to upload and scale CAD drawings in a single operation using the POST method with Aspose.CAD Cloud API
---

# Tutorial: Uploading and Scaling Images with POST Method

## Learning Objectives

In this tutorial, you'll learn:
- How to upload and resize a CAD drawing in a single operation
- When to use the POST method instead of the GET method
- How to prepare a multipart/form-data request
- How to implement the POST resize API in different programming languages

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [Basic Image Resizing Using GET Method](/change-scale-of-an-image/basic-get-method/) tutorial
- An Aspose Cloud account with an active subscription or trial
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A CAD drawing (DWG, DXF, etc.) on your local machine
- Postman, cURL, or similar tool for testing API calls (optional)

## Introduction

While the GET method is useful for resizing files already in cloud storage, the POST method allows you to upload and resize a CAD drawing in a single API call. This is particularly useful when:

- You're working with files that aren't already in cloud storage
- You need to process files directly from your application's users
- You want to minimize the number of API calls for better performance

In this tutorial, we'll learn how to use the `POST /cad/resize` endpoint to upload and resize a CAD drawing in one operation.

## Step 1: Understand the POST Resize Endpoint

The POST endpoint for resizing has the following structure:

```
POST /cad/resize?format={format}&newWidth={width}&newHeight={height}
```

Parameters:
- `{format}` - The desired output format (pdf, png, jpg, etc.)
- `{width}` - The target width in pixels
- `{height}` - The target height in pixels

Unlike the GET method, where you specify the file name in the URL, with the POST method, you'll send the file in the request body using multipart/form-data format.

## Step 2: Obtain an Access Token

As with all Aspose.CAD Cloud API calls, you need an access token for authentication:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

## Step 3: Upload and Resize the CAD Drawing

Now, let's upload and resize a local CAD drawing to a width of 800 pixels and a height of 600 pixels, and convert it to PDF format:

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/resize?format=pdf&newWidth=800&newHeight=600" \
-X POST \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-F "drawingData=@/path/to/your/drawing.dxf" \
-o resized_drawing.pdf
```

Notice the key differences from the GET method:
- We're using `/cad/resize` instead of `/cad/{name}/resize`
- We're using the `-F` flag to upload a file as form data
- The parameter name for the file is `drawingData`

## Try It Yourself: Experiment with Different Files and Formats

Let's practice by uploading and resizing different CAD files:

1. Choose a different CAD file from your local machine
2. Modify the previous curl command to use this file:

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/resize?format=png&newWidth=1024&newHeight=768" \
-X POST \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-F "drawingData=@/path/to/your/another_drawing.dwg" \
-o resized_drawing.png
```

3. Try different output formats and dimensions to see how they affect the result

## SDK Implementation Examples

### C# Implementation

Here's how to implement the upload and resize operation using the Aspose.CAD Cloud SDK for .NET:

```csharp
// Tutorial Code Example - CAD Upload and Resize with POST Method in C#
using System;
using System.IO;
using Aspose.CAD.Cloud.Sdk.Api;
using Aspose.CAD.Cloud.Sdk.Model;
using Aspose.CAD.Cloud.Sdk.Model.Requests;

namespace Aspose.CAD.Cloud.Examples
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get your ClientID and ClientSecret from https://dashboard.aspose.cloud/
            string clientId = "YOUR_CLIENT_ID";
            string clientSecret = "YOUR_CLIENT_SECRET";
            
            // Create API instance
            var api = new CadApi(clientId, clientSecret);
            
            // Path to local CAD file
            string localFilePath = @"C:\path\to\your\drawing.dxf";
            
            // Specify output format and dimensions
            var outputFormat = "pdf";
            var newWidth = 800;
            var newHeight = 600;
            
            // Read the file data
            var drawingData = File.ReadAllBytes(localFilePath);
            
            // Create request
            var request = new PostImageResizeRequest(outputFormat, newWidth, newHeight, drawingData);
            
            // Call the API
            try
            {
                using (var response = api.PostImageResize(request))
                {
                    // Save the result to a file
                    using (var output = File.Create("resized_drawing.pdf"))
                    {
                        response.CopyTo(output);
                    }
                    
                    Console.WriteLine("Successfully uploaded and resized the drawing.");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error: " + ex.Message);
            }
        }
    }
}
```

### Python Implementation

Here's how to upload and resize a CAD drawing using the Aspose.CAD Cloud SDK for Python:

```python
# Tutorial Code Example - CAD Upload and Resize with POST Method in Python
import os
import asposecadcloud
from asposecadcloud.apis.cad_api import CadApi
from asposecadcloud.api_client import ApiClient
from asposecadcloud.configuration import Configuration

# Get your client_id and client_secret from https://dashboard.aspose.cloud/
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
api_client = ApiClient(configuration)

# Create API instance
api = CadApi(api_client)

try:
    # Path to local CAD file
    local_file_path = "/path/to/your/drawing.dxf"
    
    # Specify output format and dimensions
    output_format = "pdf"
    new_width = 800
    new_height = 600
    
    # Read the file data
    with open(local_file_path, 'rb') as f:
        drawing_data = f.read()
    
    # Call the API
    response = api.post_image_resize(output_format, new_width, new_height, drawing_data)
    
    # Save the resized drawing
    with open("resized_drawing.pdf", 'wb') as f:
        f.write(response)
    
    print("Successfully uploaded and resized the drawing.")
    
except Exception as e:
    print("Error: " + str(e))
```

### Node.js Implementation

Here's how to upload and resize a CAD drawing using JavaScript:

```javascript
// Tutorial Code Example - CAD Upload and Resize with POST Method in Node.js
const fs = require('fs');
const request = require('request');
const FormData = require('form-data');

// Get your client_id and client_secret from https://dashboard.aspose.cloud/
const clientId = 'YOUR_CLIENT_ID';
const clientSecret = 'YOUR_CLIENT_SECRET';

// Function to get the access token
function getAccessToken() {
    return new Promise((resolve, reject) => {
        request.post({
            url: 'https://api.aspose.cloud/connect/token',
            form: {
                grant_type: 'client_credentials',
                client_id: clientId,
                client_secret: clientSecret
            },
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            }
        }, (error, response, body) => {
            if (error) {
                reject(error);
            } else {
                const data = JSON.parse(body);
                resolve(data.access_token);
            }
        });
    });
}

// Function to upload and resize the CAD drawing
async function uploadAndResizeDrawing() {
    try {
        // Get the access token
        const token = await getAccessToken();
        
        // Path to local CAD file
        const localFilePath = '/path/to/your/drawing.dxf';
        
        // Specify output format and dimensions
        const outputFormat = 'pdf';
        const newWidth = 800;
        const newHeight = 600;
        
        // Create the API URL
        const url = `https://api.aspose.cloud/v3.0/cad/resize?format=${outputFormat}&newWidth=${newWidth}&newHeight=${newHeight}`;
        
        // Create form data
        const formData = {
            drawingData: fs.createReadStream(localFilePath)
        };
        
        // Make the API call
        request.post({
            url: url,
            headers: {
                'Authorization': `Bearer ${token}`
            },
            formData: formData,
            encoding: null  // Important for binary data
        }, (error, response, body) => {
            if (error) {
                console.error('Error:', error);
            } else {
                // Save the resized drawing
                fs.writeFileSync('resized_drawing.pdf', body);
                console.log('Successfully uploaded and resized the drawing.');
            }
        });
    } catch (error) {
        console.error('Error:', error);
    }
}

// Execute the upload and resize operation
uploadAndResizeDrawing();
```

## Comparison: GET vs POST Method

Now that you've learned both methods, let's compare them:

| Feature | GET Method | POST Method |
|---------|------------|-------------|
| File Source | Cloud Storage | Local File System |
| API Endpoint | `/cad/{name}/resize` | `/cad/resize` |
| Number of API Calls | 2 (upload + resize) | 1 (combined) |
| Best For | Files already in cloud storage | Direct file processing |
| File Size Limit | No specific limit for resize | Depends on server configuration |
| Implementation Complexity | Simpler | Slightly more complex |

## Troubleshooting Tips

If you encounter issues while using the POST method for scaling, consider these common solutions:

1. HTTP 413 Error: This means the file is too large. Some servers have upload limits. Try compressing your CAD file or increasing your server's limit.

2. Content-Type Issues: Ensure you're using `multipart/form-data` as the content type.

3. Form Field Name: Make sure you're using `drawingData` as the name of the form field that contains the file.

4. Empty Response: If you receive an empty response, check your file format. Some CAD formats might need additional processing.

5. Authentication Errors: Verify your access token is valid and properly formatted in the Authorization header.

## Learning Checkpoint

Test your understanding of the concepts covered:

1. What is the main difference between the GET and POST methods for resizing?
2. What is the name of the form field used to upload the CAD file in the POST method?
3. When would you choose the POST method over the GET method?
4. How would you specify a new width of 1920 pixels and height of 1080 pixels in the POST method?

## What You've Learned

In this tutorial, you've learned:
- How to upload and resize a CAD drawing in a single operation using the POST method
- When to use the POST method instead of the GET method
- How to prepare a multipart/form-data request for file uploads
- How to implement the POST resize API in C#, Python, and Node.js

## Next Steps

Now that you know the basics of resizing CAD drawings using both GET and POST methods, you're ready to learn more advanced techniques.

Continue to the next tutorial: [Working with Different Output Formats](/change-scale-of-an-image/output-formats/) to learn how to scale CAD drawings and convert them to various formats in a single operation.

## Further Practice

To reinforce your understanding:
- Create a simple application that allows users to upload and resize CAD files
- Compare the performance between using separate upload and resize operations versus the combined POST method
- Try uploading and resizing CAD files of different sizes and formats
- Implement error handling to deal with malformed CAD files

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cad/)
- [Documentation](https://docs.aspose.cloud/cad/)
- [Live Demo](https://products.aspose.app/cad/family)
- [API Reference](https://reference.aspose.cloud/cad/)
- [Blog](https://blog.aspose.cloud/category/cad/)
- [Free Support](https://forum.aspose.cloud/c/cad/28/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Post them on the [Aspose.CAD Cloud Forum](https://forum.aspose.cloud/c/cad/28/) for quick assistance!