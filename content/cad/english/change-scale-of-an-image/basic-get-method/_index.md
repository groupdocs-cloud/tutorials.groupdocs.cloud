---
title: Basic Image Resizing Using GET Method Tutorial
url: /change-scale-of-an-image/basic-get-method/
weight: 20
description: Learn to scale CAD drawings using the GET method with Aspose.CAD Cloud API in this step-by-step tutorial
---

# Tutorial: Basic Image Resizing Using GET Method

## Learning Objectives

In this tutorial, you'll learn:
- How to scale an existing CAD drawing using the GET method
- How to specify width and height parameters for resizing
- How to convert to different output formats while scaling
- How to implement the API call in different programming languages

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [Introduction to Image Scaling](/change-scale-of-an-image/introduction/) tutorial
- An Aspose Cloud account with an active subscription or trial
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A CAD drawing (DWG, DXF, etc.) uploaded to your Aspose Cloud Storage
- Postman, cURL, or similar tool for testing API calls (optional)

## Introduction

The GET method for image scaling is ideal when your CAD file is already stored in Aspose Cloud Storage. This approach allows you to resize the drawing by specifying the target dimensions and retrieve the result in your desired format.

In this tutorial, we'll walk through the process of scaling a CAD drawing using the `GET /cad/{name}/resize` endpoint.

## Step 1: Prepare Your CAD File

Before you can resize a CAD drawing, you need to upload it to your Aspose Cloud Storage. If you haven't already uploaded a file, you can do so using the following cURL command:

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/storage/file/sample.dxf" \
-X PUT \
-H "Content-Type: multipart/form-data" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-T "/path/to/your/file.dxf"
```

Replace `YOUR_ACCESS_TOKEN` with the token you obtained in the previous tutorial.

## Step 2: Understand the Resize API Endpoint

The GET endpoint for resizing has the following structure:

```
GET /cad/{name}/resize?outputFormat={format}&newWidth={width}&newHeight={height}
```

Parameters:
- `{name}` - The name of your CAD file in cloud storage
- `{format}` - The desired output format (pdf, png, jpg, etc.)
- `{width}` - The target width in pixels
- `{height}` - The target height in pixels

## Step 3: Obtain an Access Token

As you learned in the previous tutorial, you need an access token to authenticate your API requests. Use the following command to get your token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

## Step 4: Make the Resize API Call

Now, let's scale our CAD drawing to a width of 800 pixels and a height of 600 pixels, and convert it to PDF format:

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/sample.dxf/resize?outputFormat=pdf&newWidth=800&newHeight=600" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-o resized_drawing.pdf
```

This will download the resized drawing as a PDF file named `resized_drawing.pdf`.

## Try It Yourself: Experiment with Different Formats

Now, let's try resizing the same drawing but output it as a PNG image:

1. Modify the previous curl command to change the output format to `png`:

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/sample.dxf/resize?outputFormat=png&newWidth=800&newHeight=600" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-o resized_drawing.png
```

2. Try different dimensions, such as 1024x768 or 1920x1080
3. Experiment with other output formats like `jpg`, `bmp`, or `tiff`

## SDK Implementation Examples

### C# Implementation

Here's how to implement the resize operation using the Aspose.CAD Cloud SDK for .NET:

```csharp
// Tutorial Code Example - CAD Resize with GET Method in C#
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
            
            // The drawing name in storage
            var name = "sample.dxf";
            
            // Specify output format and dimensions
            var outputFormat = "pdf";
            var newWidth = 800;
            var newHeight = 600;
            
            // Create request
            var request = new GetImageResizeRequest(name, outputFormat, newWidth, newHeight);
            
            // Call the API
            try
            {
                using (var response = api.GetImageResize(request))
                {
                    // Save the result to a file
                    using (var output = File.Create("resized_drawing.pdf"))
                    {
                        response.CopyTo(output);
                    }
                    
                    Console.WriteLine("Successfully resized the drawing.");
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

Here's how to resize a CAD drawing using the Aspose.CAD Cloud SDK for Python:

```python
# Tutorial Code Example - CAD Resize with GET Method in Python
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
    # Specify the drawing name in storage
    name = "sample.dxf"
    
    # Specify output format and dimensions
    output_format = "pdf"
    new_width = 800
    new_height = 600
    
    # Call the API
    response = api.get_image_resize(name, output_format, new_width, new_height)
    
    # Save the resized drawing
    with open("resized_drawing.pdf", 'wb') as f:
        f.write(response)
    
    print("Successfully resized the drawing.")
    
except Exception as e:
    print("Error: " + str(e))
```

### Node.js Implementation

Here's how to resize a CAD drawing using JavaScript:

```javascript
// Tutorial Code Example - CAD Resize with GET Method in Node.js
const fs = require('fs');
const request = require('request');

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

// Function to resize the CAD drawing
async function resizeDrawing() {
    try {
        // Get the access token
        const token = await getAccessToken();
        
        // Specify the drawing name in storage
        const name = 'sample.dxf';
        
        // Specify output format and dimensions
        const outputFormat = 'pdf';
        const newWidth = 800;
        const newHeight = 600;
        
        // Create the API URL
        const url = `https://api.aspose.cloud/v3.0/cad/${name}/resize?outputFormat=${outputFormat}&newWidth=${newWidth}&newHeight=${newHeight}`;
        
        // Make the API call
        request.get({
            url: url,
            headers: {
                'Authorization': `Bearer ${token}`
            },
            encoding: null  // Important for binary data
        }, (error, response, body) => {
            if (error) {
                console.error('Error:', error);
            } else {
                // Save the resized drawing
                fs.writeFileSync('resized_drawing.pdf', body);
                console.log('Successfully resized the drawing.');
            }
        });
    } catch (error) {
        console.error('Error:', error);
    }
}

// Execute the resize operation
resizeDrawing();
```

## Troubleshooting Tips

If you encounter issues while using the GET method for scaling, consider these common solutions:

1. Authentication Errors: Ensure your access token is valid and correctly formatted in the Authorization header.

2. File Not Found: Verify that the specified file exists in your Aspose Cloud Storage. Use the storage API to list available files.

3. Parameter Issues: Check that all query parameters are correctly formatted. Width and height must be positive integers.

4. Output Format: Make sure you're using a supported output format. If in doubt, use common formats like PDF, PNG, or JPG.

5. Timeout Errors: For very large CAD files, the operation might time out. Consider using smaller files for testing.

## Learning Checkpoint

Test your understanding of the concepts covered:

1. What are the required parameters for the GET resize endpoint?
2. How do you specify the output format when resizing a CAD drawing?
3. What header is used to pass the authentication token?
4. How would you modify the API call to set a new width of 1024 pixels and height of 768 pixels?

## What You've Learned

In this tutorial, you've learned:
- How to use the GET method to resize CAD drawings stored in Aspose Cloud Storage
- How to specify output dimensions and format
- How to implement the resize operation in C#, Python, and Node.js
- Common troubleshooting tips for the resize API

## Next Steps

Now that you know how to resize CAD drawings that are already in cloud storage, you might want to learn how to upload and resize drawings in a single operation.

Continue to the next tutorial: [Uploading and Scaling Images with POST Method](/change-scale-of-an-image/basic-post-method/) to learn how to scale CAD images by uploading them directly in a POST request.

## Further Practice

To reinforce your understanding:
- Try resizing the same drawing to different dimensions and formats
- Experiment with very large or very small dimensions
- Implement the examples in your preferred programming language
- Create a simple application that allows users to select dimensions for resizing

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cad/)
- [Documentation](https://docs.aspose.cloud/cad/)
- [Live Demo](https://products.aspose.app/cad/family)
- [API Reference](https://reference.aspose.cloud/cad/)
- [Blog](https://blog.aspose.cloud/category/cad/)
- [Free Support](https://forum.aspose.cloud/c/cad/28/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Post them on the [Aspose.CAD Cloud Forum](https://forum.aspose.cloud/c/cad/28/) for quick assistance!
