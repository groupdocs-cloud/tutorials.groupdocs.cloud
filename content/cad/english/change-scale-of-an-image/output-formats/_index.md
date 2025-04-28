---
title: Working with Different Output Formats Tutorial
url: /change-scale-of-an-image/output-formats/
weight: 40
description: Learn to convert CAD drawings to various output formats while scaling using Aspose.CAD Cloud API in this comprehensive tutorial
---

# Tutorial: Working with Different Output Formats

## Learning Objectives

In this tutorial, you'll learn:
- How to convert CAD drawings to different output formats while scaling
- The characteristics and benefits of different output formats
- How to adjust format-specific parameters during conversion
- How to implement format conversion with scaling in various programming languages

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [Uploading and Scaling Images with POST Method](/change-scale-of-an-image/basic-post-method/) tutorial
- An Aspose Cloud account with an active subscription or trial
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A CAD drawing (DWG, DXF, etc.) on your local machine or in cloud storage
- Basic understanding of different file formats (raster vs. vector)

## Introduction

One of the most powerful features of Aspose.CAD Cloud is the ability to convert CAD drawings to different formats while simultaneously scaling them. This capability is essential for:

- Making CAD content accessible to users without CAD software
- Optimizing file sizes for web or mobile applications
- Creating printable documents from CAD drawings
- Archiving CAD content in standard formats

In this tutorial, we'll explore how to use the Aspose.CAD Cloud API to convert CAD drawings to various output formats while also scaling them to your desired dimensions.

## Understanding Available Output Formats

Aspose.CAD Cloud supports conversion to multiple output formats, which can be categorized as follows:

### 1. Raster Image Formats

- PNG - Portable Network Graphics (lossless compression, transparency support)
- JPEG/JPG - Joint Photographic Experts Group (lossy compression, smaller file size)
- TIFF/TIF - Tagged Image File Format (lossless, supports multiple pages)
- BMP - Bitmap (uncompressed, larger file size)
- GIF - Graphics Interchange Format (limited colors, animation support)
- DICOM - Digital Imaging and Communications in Medicine (medical imaging)
- WEBP - Web Picture format (modern web format with good compression)
- JPEG2000 - Advanced JPEG format with better compression and quality

### 2. Vector/Document Formats

- PDF - Portable Document Format (widely used for documents)
- SVG - Scalable Vector Graphics (vector format for web)
- WMF - Windows Metafile (vector format for Windows)
- DXF - Drawing Exchange Format (CAD format)
- DWF - Design Web Format (Autodesk format for sharing designs)

### 3. 3D Model Formats

- FBX - Filmbox (3D model format)
- OBJ - Wavefront Object (3D model format)
- STL - Stereolithography (3D printing format)
- 3DS - 3D Studio (legacy 3D format)
- GLTF/GLB - GL Transmission Format (modern 3D format)
- U3D - Universal 3D (3D format for PDF embedding)

## Choosing the Right Output Format

The best output format depends on your specific requirements:

- For Web Display: PNG, JPEG, SVG, or WebP
- For Printing: PDF or TIFF
- For Editing in Other Applications: DXF, SVG, or WMF
- For 3D Visualization: GLTF, OBJ, or FBX
- For 3D Printing: STL
- For Maximum Quality: TIFF, PNG, or PDF
- For Minimum File Size: JPEG, WebP, or PDF with compression

## Converting to Raster Formats

Let's start by learning how to convert a CAD drawing to common raster formats while scaling.

### Example: Converting to PNG

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/sample.dxf/resize?outputFormat=png&newWidth=800&newHeight=600" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-o resized_drawing.png
```

### Example: Converting to JPEG with Quality Control

When converting to JPEG, you can often control the quality level to balance between file size and image quality. While the basic API doesn't expose this directly, some SDKs allow for more detailed control.

Here's a C# example with quality control:

```csharp
// Tutorial Code Example - CAD to JPEG with Quality Control in C#
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
            var outputFormat = "jpg";
            var newWidth = 800;
            var newHeight = 600;
            
            // Create request with additional options
            var request = new GetImageResizeRequest(name, outputFormat, newWidth, newHeight);
            
            // Call the API
            try
            {
                using (var response = api.GetImageResize(request))
                {
                    // Save the result to a file
                    using (var output = File.Create("resized_drawing.jpg"))
                    {
                        response.CopyTo(output);
                    }
                    
                    Console.WriteLine("Successfully converted and resized the drawing to JPEG.");
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

## Converting to PDF Format

PDF is one of the most popular formats for sharing CAD drawings because it's widely supported and preserves the visual appearance of the drawing.

### Example: Converting to PDF

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/sample.dxf/resize?outputFormat=pdf&newWidth=800&newHeight=600" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-o resized_drawing.pdf
```

### PDF Conversion with Advanced Options

When converting to PDF, you might want to control additional parameters such as:
- Page size
- Margins
- Text extraction options
- Compliance level (PDF/A)

Here's a Python example with some advanced PDF options:

```python
# Tutorial Code Example - CAD to PDF with Advanced Options in Python
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
    
    # Call the API with specific options
    storage_name = ""  # Use default storage
    folder = ""  # Use root folder
    
    response = api.get_image_resize(name, output_format, new_width, new_height, 
                                   storage_name=storage_name, folder=folder)
    
    # Save the resized drawing
    with open("resized_drawing.pdf", 'wb') as f:
        f.write(response)
    
    print("Successfully converted and resized the drawing to PDF.")
    
except Exception as e:
    print("Error: " + str(e))
```

## Converting to SVG Format

SVG is an excellent choice for web applications because it's a vector format that scales well at different resolutions.

### Example: Converting to SVG

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/sample.dxf/resize?outputFormat=svg&newWidth=800&newHeight=600" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-o resized_drawing.svg
```

### Node.js Example for SVG Conversion

```javascript
// Tutorial Code Example - CAD to SVG Conversion in Node.js
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

// Function to convert and resize the CAD drawing to SVG
async function convertToSvg() {
    try {
        // Get the access token
        const token = await getAccessToken();
        
        // Specify the drawing name in storage
        const name = 'sample.dxf';
        
        // Specify output format and dimensions
        const outputFormat = 'svg';
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
                // Save the converted drawing
                fs.writeFileSync('resized_drawing.svg', body);
                console.log('Successfully converted and resized the drawing to SVG.');
            }
        });
    } catch (error) {
        console.error('Error:', error);
    }
}

// Execute the conversion
convertToSvg();
```

## Converting to 3D Formats

For 3D CAD drawings, you might want to convert to formats like OBJ, FBX, or GLTF. These formats are supported depending on the source CAD drawing's characteristics.

### Example: Converting to OBJ

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/sample.dwg/resize?outputFormat=obj&newWidth=800&newHeight=600" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-o resized_drawing.zip
```

Note that OBJ output is typically delivered as a ZIP file containing the OBJ file and its associated materials.

## Try It Yourself: Format Conversion Matrix

Now, let's practice converting a CAD drawing to different formats and compare the results:

1. Choose a CAD drawing from your collection
2. Convert it to at least three different formats (e.g., PNG, PDF, and SVG) using the same dimensions
3. Compare the:
   - File size
   - Visual quality
   - Rendering performance
   - Feature support (text, layers, 3D elements)

## Format-Specific Considerations

### Raster Formats (PNG, JPEG, TIFF, etc.)
- Advantages: Wide compatibility, easy to view, good for screenshots/thumbnails
- Disadvantages: Lose vector data, quality degrades when scaled, larger file sizes at high resolutions
- Best for: Web display, simple sharing, thumbnails

### PDF
- Advantages: Preserves appearance, widely supported, can include text/metadata
- Disadvantages: Less editable than vector formats, can be larger than optimized raster images
- Best for: Documentation, printing, archiving

### Vector Formats (SVG, DXF)
- Advantages: Scalable without quality loss, maintains editability, smaller file sizes
- Disadvantages: Not as widely supported as raster or PDF, complex rendering
- Best for: Web applications, further editing, high-quality scaling

### 3D Formats (OBJ, FBX, GLTF)
- Advantages: Preserves 3D data, enables interactive viewing, supports materials
- Disadvantages: Limited support, may lose CAD-specific data
- Best for: 3D visualization, gaming/VR applications, 3D printing preparation

## Troubleshooting Format Conversion Issues

If you encounter problems when converting to specific formats, consider these solutions:

1. Missing or Incomplete Elements: Some formats may not support all features of the original CAD drawing. Consider using PDF or DXF for maximum fidelity.

2. Distorted Output: Ensure you're maintaining the aspect ratio correctly. If not specified, some converters might stretch or compress the image.

3. Large File Sizes: For web use, consider using formats with good compression like PNG or WebP for raster output, or SVG for vector output.

4. Text Issues: If text appears incorrectly, the format might not support the fonts used. PDF generally handles text well.

5. Color Discrepancies: Different formats handle color spaces differently. Test with small samples first if color accuracy is critical.

## Learning Checkpoint

Test your understanding of the concepts covered:

1. Which format would be best for a CAD drawing that needs to be viewed on mobile devices with limited bandwidth?
2. What are the advantages of converting a CAD drawing to SVG instead of PNG?
3. When would you choose PDF over other formats?
4. What special consideration should you keep in mind when converting to OBJ format?

## What You've Learned

In this tutorial, you've learned:
- How to convert CAD drawings to different output formats while scaling
- The characteristics and benefits of various output formats
- How to choose the right format for specific use cases
- How to implement format conversion with scaling in various programming languages

## Next Steps

Now that you understand how to work with different output formats, you're ready to learn more about controlling the scaling process itself.

Continue to the next tutorial: [Proportional vs Non-Proportional Scaling](/change-scale-of-an-image/proportional-scaling/) to learn how to control aspect ratio when scaling your CAD drawings.

## Further Practice

To reinforce your understanding:
- Create a comparison chart of file sizes for the same drawing converted to different formats
- Build a simple application that allows users to select from different output formats
- Test conversion to all supported formats and identify which ones best preserve your specific CAD elements
- Create a batch conversion script that processes multiple CAD files to a selected output format

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cad/)
- [Documentation](https://docs.aspose.cloud/cad/)
- [Live Demo](https://products.aspose.app/cad/family)
- [API Reference](https://reference.aspose.cloud/cad/)
- [Blog](https://blog.aspose.cloud/category/cad/)
- [Free Support](https://forum.aspose.cloud/c/cad/28/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Post them on the [Aspose.CAD Cloud Forum](https://forum.aspose.cloud/c/cad/28/) for quick assistance!
