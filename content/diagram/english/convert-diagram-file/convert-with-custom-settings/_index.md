---
title: Convert Diagram Files with Custom Settings Tutorial
url: /convert-diagram-file/convert-with-custom-settings/
weight: 30
description: Learn how to convert diagram files with custom quality and format settings using Aspose.Diagram Cloud API in this developer tutorial.
---

# Tutorial: Convert Diagram Files with Custom Settings

## Learning Objectives

In this tutorial, you'll learn how to:
- Apply custom settings when converting diagram files
- Control quality and appearance parameters for different output formats
- Customize image resolution, page size, and other format-specific options
- Implement advanced conversion settings in different programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
- Completed the [basic diagram conversion tutorial](/convert-diagram-file/convert-to-another-format/)
- Familiarity with the [page-specific conversion tutorial](/convert-diagram-file/convert-specific-pages/)
- An Aspose Cloud account with active subscription
- Your Client ID and Client Secret from the dashboard
- A diagram file in your storage (VDX, VSDX, etc.)
- Development environment for your preferred language

## Introduction

When converting diagram files, you often need fine-grained control over how the output appears. Aspose.Diagram Cloud API provides several advanced options to customize the conversion process based on your specific requirements.

In this tutorial, we'll explore how to use the extended parameters of the `POST /diagram/{name}/SaveAs` API to apply custom settings during the conversion process. These settings can significantly improve the quality and appearance of your converted files for different use cases.

## Step 1: Authentication - Getting Access Token

As with previous tutorials, you'll need to authenticate with Aspose Cloud services first:

### Try it yourself:

```bash
curl -v "https://api.aspose.cloud/oauth2/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the access token from the response for use in subsequent API calls.

## Step 2: Understanding Format-Specific Settings

Different output formats support different customization options. Here are some of the key settings you can customize:

### PDF Output Settings
- PdfCompliance: Set PDF compliance level (e.g., PDF/A-1a, PDF/A-1b)
- ImageCompression: Control image compression method
- TextCompression: Specify text compression method
- JpegQuality: Set JPEG quality level for images

### Image Output Settings (PNG, JPEG, TIFF)
- Resolution: Control output resolution (DPI)
- ImageColorMode: Set color mode (e.g., RGB, Grayscale)
- TiffCompression: For TIFF outputs, set compression method

### SVG Output Settings
- SvgFitToViewPort: Whether to fit diagram to viewport
- ExportElementsDrawnOnPage: Control which page elements to export

## Step 3: Convert with Custom PDF Settings

Let's convert a diagram to PDF with custom settings:

### Try it yourself:

```bash
curl -v "https://api.aspose.cloud/v3.0/diagram/diagram_file.vsdx/SaveAs?IsOverwrite=true&newfilename=custom_settings.pdf" \
-X POST \
-d '{
  "Format": "pdf",
  "PdfCompliance": "PdfA1a",
  "JpegQuality": 90,
  "ExportHiddenPage": false
}' \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace:
- `diagram_file.vsdx` with your actual diagram filename
- `YOUR_ACCESS_TOKEN` with the token from Step 1

This example converts the diagram to a PDF/A-1a compliant file with high JPEG quality and excludes hidden pages.

## Step 4: Convert with Custom Image Settings

Now let's convert a diagram to PNG with custom resolution settings:

### Try it yourself:

```bash
curl -v "https://api.aspose.cloud/v3.0/diagram/diagram_file.vsdx/SaveAs?IsOverwrite=true&newfilename=high_res.png" \
-X POST \
-d '{
  "Format": "png",
  "Resolution": 300,
  "ImageColorMode": "Rgb",
  "ExportElementsDrawnOnPage": true
}' \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This example converts the diagram to a high-resolution (300 DPI) PNG image in RGB color mode, including all elements drawn on the page.

## Step 5: Combining Page Selection with Custom Settings

You can combine page selection with custom settings for more precise control:

### Try it yourself:

```bash
curl -v "https://api.aspose.cloud/v3.0/diagram/multipage_diagram.vsdx/SaveAs?IsOverwrite=true&newfilename=custom_pages.pdf" \
-X POST \
-d '{
  "Format": "pdf",
  "PageIndex": 0,
  "PageCount": 2,
  "JpegQuality": 95,
  "ExportHiddenPage": false
}' \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This example converts only the first two pages (pages 0 and 1) with high JPEG quality settings and excludes hidden pages.

## Implementation with SDKs

### C# Example

```csharp
using System;
using System.IO;
using Aspose.Diagram.Cloud.Sdk.Api;
using Aspose.Diagram.Cloud.Sdk.Model;

namespace AsposeCloudDiagramExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get Client ID and Client Secret from https://dashboard.aspose.cloud/
            string clientId = "YOUR_CLIENT_ID";
            string clientSecret = "YOUR_CLIENT_SECRET";
            
            // Create API instance
            var api = new DiagramApi(clientId, clientSecret);
            
            // The file name in storage
            string fileName = "diagram_file.vsdx";
            
            // Convert to PDF with custom settings
            string format = "pdf";
            string newFileName = "custom_settings.pdf";
            bool isOverwrite = true;
            
            var request = new SaveAsRequest(fileName, format, newFileName, isOverwrite)
            {
                // Custom PDF settings
                PdfCompliance = "PdfA1a",
                JpegQuality = 90,
                ExportHiddenPage = false
            };
            
            var result = api.SaveAs(request);
            
            Console.WriteLine("File converted with custom settings successfully!");
            Console.WriteLine($"Source Document: {result.SaveResult.SourceDocument.Href}");
            Console.WriteLine($"Destination Document: {result.SaveResult.DestDocument.Href}");
        }
    }
}
```

### Java Example

```java
import com.aspose.diagram.cloud.api.DiagramApi;
import com.aspose.diagram.cloud.model.*;
import com.aspose.diagram.cloud.model.requests.*;

public class DiagramConvertCustomSettingsExample {
    public static void main(String[] args) {
        // Get Client ID and Client Secret from https://dashboard.aspose.cloud/
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Create API instance
        DiagramApi api = new DiagramApi(clientId, clientSecret);
        
        try {
            // The file name in storage
            String fileName = "diagram_file.vsdx";
            
            // Convert to PNG with custom settings
            String format = "png";
            String newFileName = "high_res.png";
            Boolean isOverwrite = true;
            
            SaveAsRequest request = new SaveAsRequest(fileName, format, newFileName, isOverwrite);
            
            // Custom PNG settings
            request.setResolution(300);
            request.setImageColorMode("Rgb");
            request.setExportElementsDrawnOnPage(true);
            
            SaveAsResponse response = api.saveAs(request);
            
            System.out.println("File converted with custom settings successfully!");
            System.out.println("Source Document: " + response.getSaveResult().getSourceDocument().getHref());
            System.out.println("Destination Document: " + response.getSaveResult().getDestDocument().getHref());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Python Example

```python
import asposediagramcloud
from asposediagramcloud.apis.diagram_api import DiagramApi
from asposediagramcloud.models.requests import SaveAsRequest

# Get Client ID and Client Secret from https://dashboard.aspose.cloud/
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create API instance
api = DiagramApi(client_id, client_secret)

# The file name in storage
file_name = "multipage_diagram.vsdx"

# Convert to PDF with custom settings
format_type = "pdf"
new_file_name = "custom_pages.pdf"
is_overwrite = True

# Execute request with custom settings and page selection
request = SaveAsRequest(file_name, format_type, new_file_name, is_overwrite)
request.page_index = 0
request.page_count = 2
request.jpeg_quality = 95
request.export_hidden_page = False

result = api.save_as(request)

print("File converted with custom settings successfully!")
print(f"Source Document: {result.save_result.source_document.href}")
print(f"Destination Document: {result.save_result.dest_document.href}")
```

### Node.js Example

```javascript
const { DiagramApi } = require('asposediagramcloud');

// Get Client ID and Client Secret from https://dashboard.aspose.cloud/
const clientId = "YOUR_CLIENT_ID";
const clientSecret = "YOUR_CLIENT_SECRET";

// Create API instance
const api = new DiagramApi(clientId, clientSecret);

// The file name in storage
const fileName = "diagram_file.vsdx";

// Convert to PDF with custom settings
const formatType = "pdf";
const newFileName = "custom_settings.pdf";
const isOverwrite = true;

// Create request options object with custom settings
const options = {
    pdfCompliance: "PdfA1a",
    jpegQuality: 90,
    exportHiddenPage: false
};

// Execute request
api.saveAs(fileName, formatType, newFileName, isOverwrite, options)
    .then((result) => {
        console.log("File converted with custom settings successfully!");
        console.log(`Source Document: ${result.body.saveResult.sourceDocument.href}`);
        console.log(`Destination Document: ${result.body.saveResult.destDocument.href}`);
    })
    .catch((error) => {
        console.error("Error:", error);
    });
```

## Troubleshooting Common Issues

### Unsupported Settings
- Issue: Error indicating a setting is not supported for the chosen format
- Solution: Check the API documentation to ensure the settings you're using are compatible with your target format. Different formats support different customization options.

### Quality Issues
- Issue: Output quality not meeting expectations
- Solution: Adjust resolution and quality settings. For images, try increasing the resolution. For PDFs, try adjusting the JPEG quality and compression settings.

### Memory Limitations
- Issue: "Out of memory" or timeout errors when setting very high quality
- Solution: Be careful when setting extremely high resolution or quality values, as they can consume significant resources. Try more moderate values.

## What You've Learned

In this tutorial, you've learned how to:
- Apply custom settings when converting diagram files
- Control format-specific options like resolution, quality, and compliance
- Combine page selection with custom conversion settings
- Implement advanced conversion settings in different programming languages
- Troubleshoot common issues related to custom conversion settings

## Further Practice

To reinforce your learning:
1. Experiment with different quality settings and compare the results
2. Create conversion presets for different types of outputs (e.g., web-optimized, print-quality)
3. Build a utility that allows users to select from predefined conversion profiles
4. Test the impact of different settings on file size and quality

## Helpful Resources

- [Product Page](https://products.aspose.cloud/diagram/)
- [Documentation](https://docs.aspose.cloud/diagram/)
- [Live Demo](https://products.aspose.app/diagram/family)
- [API Reference](https://reference.aspose.cloud/diagram/)
- [Blog](https://blog.aspose.cloud/category/diagram/)
- [Free Support](https://forum.aspose.cloud/c/diagram/27/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

## Feedback

Have questions or feedback about this tutorial? We'd love to hear from you! Please post your questions or comments in our [support forum](https://forum.aspose.cloud/c/diagram/27/).