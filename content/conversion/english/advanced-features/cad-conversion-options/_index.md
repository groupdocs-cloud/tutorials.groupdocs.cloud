---
url: /advanced-features/cad-conversion-options/
title: Converting CAD Documents with Load Options in GroupDocs.Conversion Cloud API Tutorial
description: Learn to convert CAD drawings and models to other formats with specialized options using GroupDocs.Conversion Cloud API
weight: 40
---

# Tutorial: Converting CAD Documents with Load Options

In this tutorial, you'll learn how to convert Computer-Aided Design (CAD) documents to various formats using GroupDocs.Conversion Cloud API. You'll master specialized conversion options for handling technical drawings and 3D models created in CAD applications like AutoCAD.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Convert CAD documents (DWG, DXF, etc.) to other formats like PDF and images
- Customize CAD conversion with specialized load options
- Control drawing dimensions, layouts, and layers
- Implement both storage-based and stream-based CAD conversions
- Troubleshoot common CAD conversion challenges

## Prerequisites

Before starting this tutorial, you need:
1. A [GroupDocs.Conversion Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Development environment with your preferred programming language set up
5. Sample CAD documents to test conversion (we'll use DWG files in this tutorial)

## Implementation Steps

### Step 1: Authentication with GroupDocs.Conversion Cloud API

Before performing any operations, we need to authenticate with the API using your Client ID and Client Secret.

#### Try it yourself

First, let's obtain a JWT access token using cURL:

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Make sure to replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

### Step 2: Basic CAD to PDF Conversion

Let's start with a simple conversion from a DWG file to PDF format:

#### Try it yourself

Using cURL:

```bash
curl -X POST "https://api.groupdocs.cloud/v2.0/conversion" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: application/json" \
-d "{  
      'FilePath': 'cad/sample.dwg',  
      'Format': 'pdf',  
      'OutputPath': 'converted'
    }"
```

Replace `YOUR_JWT_TOKEN` with the actual token received in Step 1.

### Step 3: Converting CAD to PDF with Resolution and Size Options

Now let's implement a comprehensive example that converts a CAD document to PDF with dimension control:

```csharp
// C# SDK Example
using System;
using System.Collections.Generic;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace CadConversionTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API client
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            var apiInstance = new ConvertApi(configuration);

            try
            {
                // Set up conversion from CAD to PDF with options
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "cad/sample.dwg",
                    Format = "pdf",
                    // CAD-specific load options
                    LoadOptions = new CadLoadOptions()
                    {
                        // Set drawing dimensions
                        Width = 1920,    // Width in pixels
                        Height = 1080,   // Height in pixels
                        
                        // Specify layout and layers
                        LayoutName = "Model",  // Use Model space (default layout)
                        
                        // Background color (optional)
                        BackgroundColor = "white"
                    },
                    // PDF-specific convert options
                    ConvertOptions = new PdfConvertOptions()
                    {
                        // PDF settings (if needed)
                        Dpi = 300,  // High resolution for technical drawings
                        Width = 1920,
                        Height = 1080,
                        CenterWindow = true
                    },
                    OutputPath = "converted"
                };

                // Execute conversion
                List<StoredConvertedResult> response = apiInstance.ConvertDocument(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("CAD document converted successfully to PDF: " + response[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 4: Converting CAD to Image Format

Converting CAD documents to image formats is useful for web display or document embedding:

```java
// Java SDK Example
import com.groupdocs.cloud.conversion.api.*;
import com.groupdocs.cloud.conversion.client.ApiException;
import com.groupdocs.cloud.conversion.model.*;
import com.groupdocs.cloud.conversion.model.requests.*;
import java.util.List;

public class CadToImageExample {
    public static void main(String[] args) {
        // Configure API client
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        Configuration configuration = new Configuration(clientId, clientSecret);
        ConvertApi apiInstance = new ConvertApi(configuration);
        
        try {
            // Prepare convert settings for CAD to PNG
            ConvertSettings settings = new ConvertSettings();
            settings.setFilePath("cad/blueprint.dwg");
            settings.setFormat("png");
            
            // Set CAD-specific load options
            CadLoadOptions loadOptions = new CadLoadOptions();
            loadOptions.setWidth(2048);        // Width in pixels
            loadOptions.setHeight(1536);       // Height in pixels
            loadOptions.setLayoutName("Model"); // Use Model space
            loadOptions.setDrawType("AfterRender");  // After render drawing mode
            loadOptions.setBackgroundColor("lightblue");  // Background color
            
            settings.setLoadOptions(loadOptions);
            
            // Configure PNG-specific convert options
            PngConvertOptions convertOptions = new PngConvertOptions();
            convertOptions.setFromPage(1);
            convertOptions.setPagesCount(1);   // Most CAD files have just one page
            convertOptions.setDpi(300);        // High resolution
            convertOptions.setBackgroundColor("lightblue");  // Match background color
            
            settings.setConvertOptions(convertOptions);
            settings.setOutputPath("converted");
            
            // Execute conversion
            List<StoredConvertedResult> result = apiInstance.convertDocument(
                new ConvertDocumentRequest(settings));
            
            System.out.println("CAD document converted successfully to PNG: " + result.get(0).getUrl());
        } catch (ApiException e) {
            System.err.println("Exception when calling ConvertApi: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Step 5: Converting with Layout Selection

CAD files often contain multiple layouts. Here's how to target a specific layout:

```python
# Python SDK Example
import groupdocs_conversion_cloud
from groupdocs_conversion_cloud.models.requests import ConvertDocumentRequest

# Configure API client
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"
api_instance = groupdocs_conversion_cloud.ConvertApi.from_keys(client_id, client_secret)

try:
    # Prepare conversion settings
    settings = groupdocs_conversion_cloud.ConvertSettings()
    settings.file_path = "cad/multilayout.dwg"
    settings.format = "pdf"
    
    # Configure CAD-specific load options
    load_options = groupdocs_conversion_cloud.CadLoadOptions()
    load_options.width = 2000         # Width in pixels
    load_options.height = 1000        # Height in pixels
    load_options.layout_name = "Layout1"  # Specific named layout (not Model space)
    load_options.background_color = "white"
    
    settings.load_options = load_options
    
    # Configure PDF-specific convert options
    convert_options = groupdocs_conversion_cloud.PdfConvertOptions()
    convert_options.dpi = 300
    convert_options.width = 2000
    convert_options.height = 1000
    
    settings.convert_options = convert_options
    settings.output_path = "converted"
    
    # Execute conversion
    request = ConvertDocumentRequest(settings)
    result = api_instance.convert_document(request)
    
    print(f"CAD document converted successfully with layout '{load_options.layout_name}': {result[0].url}")
except groupdocs_conversion_cloud.ApiException as e:
    print(f"Exception when calling ConvertApi: {e}")
```

### Step 6: Stream-Based CAD Conversion

For applications that need to process the converted CAD drawing directly:

```javascript
// Node.js SDK Example
const { ConvertApi, Configuration } = require("groupdocs-conversion-cloud");
const fs = require("fs");

// Configure API client
const clientId = "YOUR_CLIENT_ID";
const clientSecret = "YOUR_CLIENT_SECRET";
const config = new Configuration(clientId, clientSecret);
const apiInstance = new ConvertApi(config);

// Prepare conversion settings
const settings = {
    filePath: "cad/sample.dwg",
    format: "pdf",
    loadOptions: {
        // CAD-specific load options
        width: 1920,
        height: 1080,
        layoutName: "Model",
        backgroundColor: "white"
    },
    convertOptions: {
        // PDF-specific convert options
        dpi: 300,
        width: 1920, 
        height: 1080
    },
    // Set outputPath to null for stream output
    outputPath: null
};

// Execute conversion
apiInstance.convertDocumentDownload({ convertSettings: settings })
    .then((result) => {
        // Save the stream to a file
        const fileName = "./converted-cad.pdf";
        const writeStream = fs.createWriteStream(fileName);
        
        result.pipe(writeStream);
        
        writeStream.on("finish", () => {
            console.log(`CAD document converted and saved to ${fileName}`);
        });
    })
    .catch((error) => {
        console.log(`Error: ${error.message}`);
    });
```

## CAD-Specific Load Options

When converting CAD documents, you can leverage these specialized options:

| Option | Description | Default | Impact |
|--------|-------------|---------|--------|
| Width | Width of the resulting drawing in pixels | Depends on CAD file | Affects output size and clarity |
| Height | Height of the resulting drawing in pixels | Depends on CAD file | Affects output size and clarity |
| LayoutName | Name of the layout to convert | "Model" | Selects specific layout from drawing |
| DrawType | Drawing mode | Auto | Controls rendering technique |
| BackgroundColor | Background color | Black | Affects appearance of transparent areas |
| DrawColor | Drawing color for monochrome conversions | Black | Affects line colors in output |
| Layers | Specific layers to include | All | Controls which layers are visible |

## Troubleshooting Common Issues

### 1. Dimensioning and Scaling Problems

If the drawing dimensions don't look right:
- Adjust Width and Height to maintain the correct aspect ratio
- For technical drawings with fine details, use higher Width/Height values
- Consider using the DrawType option to control how the CAD engine renders the drawing

### 2. Layout Selection Challenges

If you're having trouble with layouts:
- Verify the layout name exists in the original CAD file
- Use "Model" for the model space (default design area)
- For paper space layouts, use the exact name as shown in the CAD program

### 3. Layer Visibility Issues

If certain elements are missing or unwanted:
- Check if the elements are on specific layers
- For selective conversion, specify only the layers you want to include
- For complete drawings, leave the layers option unset to include all layers

### 4. Color and Appearance Problems

If colors don't appear as expected:
- Set the BackgroundColor to match your needs (white for documents, transparent for web)
- For monochrome drawings, use DrawColor to control the line color
- If text or fine lines are hard to see, try adjusting the drawing colors for better contrast

## What You've Learned

In this tutorial, you've learned:
- How to convert CAD documents to PDF and image formats
- Customizing CAD conversion settings for optimal results
- Controlling drawing dimensions, layouts, and layers
- Implementing both storage-based and stream-based CAD conversions
- Troubleshooting common CAD conversion challenges

## Further Practice

To reinforce your learning, try these exercises:
1. Create a CAD viewer that converts drawings to web-friendly formats for display
2. Implement a batch conversion utility that processes multiple CAD files with different layouts
3. Build a service that extracts specific layouts from CAD files based on client requests
4. Create a conversion tool that optimizes CAD drawings for different output purposes (print vs. web)

## Next Tutorial

Ready to learn more? Continue with our [Tutorial: Converting CSV Documents with Load Options](/advanced-features/csv-conversion-options) to master CSV parsing and conversion with customization.

## Additional Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [Live Demo](https://products.groupdocs.app/conversion/family)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.conversion-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [forum](https://forum.groupdocs.cloud/c/conversion/11) for support.
