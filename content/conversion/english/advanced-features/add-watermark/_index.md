---
title: How to Add Watermarks During Document Conversion Tutorial
description: Learn to add text watermarks to documents during conversion with GroupDocs.Conversion Cloud API in this step-by-step developer tutorial.
url: /advanced-features/add-watermark/
weight: 20
---

# Tutorial: How to Add Watermarks During Document Conversion

## Learning Objectives

In this tutorial, you'll learn how to:
- Add customizable text watermarks to documents during the conversion process
- Control watermark appearance including color, size, and positioning
- Set watermark as a background or foreground element
- Implement watermarking across different document formats
- Create different watermark styles for various business needs

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Conversion Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with your preferred programming language (C#, Java, Python, PHP, Ruby, or Node.js)
- A document to test watermarking (we'll use a DOCX file in this tutorial)

## Why Add Watermarks During Conversion?

Watermarks serve multiple purposes in document management:

- Ownership Indication: Displaying company name or copyright information
- Confidentiality: Marking documents as "Confidential" or "Draft"
- Branding: Adding brand elements to distributed documents
- Version Control: Marking document versions or review status
- Security: Discouraging unauthorized sharing or reproduction

Using GroupDocs.Conversion Cloud API, you can add these watermarks dynamically during the conversion process, saving you from having to modify the original documents.

## Implementation Steps

### Step 1: Set Up Your Development Environment

First, ensure you have the GroupDocs.Conversion Cloud SDK installed for your language:
For .NET:
```bash
Install-Package GroupDocs.Conversion-Cloud
```
For Java:
```bash
<dependency>
    <groupId>com.groupdocs</groupId>
    <artifactId>groupdocs-conversion-cloud</artifactId>
    <version>latest-version</version>
</dependency>
```
For Python:
```bash
pip install groupdocs-conversion-cloud
```

### Step 2: Authenticate with the API

To use GroupDocs.Conversion Cloud API, you need to authenticate using your Client ID and Client Secret:

```csharp
// Initialize the API with your credentials
string MyAppKey = "XXXX-XXXX-XXXX-XXXX"; // Your AppKey
string MyAppSid = "XXXX-XXXX-XXXX-XXXX"; // Your AppSID
var configuration = new Configuration(MyAppSid, MyAppKey);
var apiInstance = new ConvertApi(configuration);
```

### Step 3: Configure Watermark Options

Now, let's set up the watermark options for the conversion process:

```csharp
// Create watermark options
var watermark = new WatermarkOptions
{
    Text = "CONFIDENTIAL", // The watermark text
    Color = "Red",         // Color of the watermark text
    Width = 100,           // Width of the watermark
    Height = 100,          // Height of the watermark
    Background = true      // Set as background (true) or foreground (false)
};
```

The watermark options allow you to customize:
- Text: The content of the watermark
- Color: The color name (like Red, Blue) or hex value
- Width/Height: The dimensions of the watermark
- Background: Whether it appears behind (true) or on top of (false) the document content

### Step 4: Create Conversion Settings with Watermark

Now we'll prepare the full conversion settings including the watermark:

```csharp
// Prepare convert settings with watermark
var settings = new ConvertSettings
{
    FilePath = "WordProcessing/sample.docx",
    Format = "pdf",
    ConvertOptions = new PdfConvertOptions
    {
        WatermarkOptions = watermark
    },
    OutputPath = "converted/with-watermark.pdf"
};
```

### Step 5: Execute the Conversion Request

Now, let's execute the conversion with our watermark settings:

```csharp
// Convert document with watermark
var response = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings));
Console.WriteLine("Document converted with watermark: " + response[0].Url);
```

## Complete Code Examples

### Using C#

```csharp
using System;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace GroupDocs.Conversion.Cloud.Examples
{
    class AddWatermarkExample
    {
        public static void Run()
        {
            // Get your credentials from https://dashboard.groupdocs.cloud/applications
            string MyAppKey = "XXXX-XXXX-XXXX-XXXX"; // Your AppKey
            string MyAppSid = "XXXX-XXXX-XXXX-XXXX"; // Your AppSID
            
            var configuration = new Configuration(MyAppSid, MyAppKey);
            
            // Create necessary API instances
            var apiInstance = new ConvertApi(configuration);
            
            try
            {
                // Create watermark options
                var watermark = new WatermarkOptions
                {
                    Text = "CONFIDENTIAL",
                    Color = "Red",
                    Width = 100,
                    Height = 100,
                    Background = true
                };
                
                // Prepare convert settings with watermark
                var settings = new ConvertSettings
                {
                    FilePath = "WordProcessing/sample.docx",
                    Format = "pdf",
                    ConvertOptions = new PdfConvertOptions
                    {
                        WatermarkOptions = watermark
                    },
                    OutputPath = "converted/with-watermark.pdf"
                };
                
                // Convert document with watermark
                var response = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings));
                Console.WriteLine("Document converted with watermark: " + response[0].Url);
                
                // Example 2: Diagonal watermark with different color
                var diagonalWatermark = new WatermarkOptions
                {
                    Text = "DRAFT",
                    Color = "Blue",
                    Width = 150,
                    Height = 150,
                    Background = false // Display on top of content
                };
                
                var settings2 = new ConvertSettings
                {
                    FilePath = "WordProcessing/sample.docx",
                    Format = "pdf",
                    ConvertOptions = new PdfConvertOptions
                    {
                        WatermarkOptions = diagonalWatermark
                    },
                    OutputPath = "converted/with-draft-watermark.pdf"
                };
                
                var response2 = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings2));
                Console.WriteLine("Document converted with draft watermark: " + response2[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception when calling ConvertApi: " + e.Message);
            }
        }
    }
}
```

### Using Python

```python
# Import required modules
import groupdocs_conversion_cloud
import os

# Get your credentials from https://dashboard.groupdocs.cloud/applications
client_id = "XXXX-XXXX-XXXX-XXXX"  # Your Client ID
client_secret = "XXXXXXXXXXXXXXXX"  # Your Client Secret

# Create instance of the API
api = groupdocs_conversion_cloud.ConvertApi.from_keys(client_id, client_secret)

try:
    # Example 1: Create a red "CONFIDENTIAL" watermark
    # Set up the watermark options
    watermark = groupdocs_conversion_cloud.WatermarkOptions()
    watermark.text = "CONFIDENTIAL"
    watermark.color = "Red"
    watermark.width = 100
    watermark.height = 100
    watermark.background = True  # Set as background
    
    # Configure conversion settings
    settings = groupdocs_conversion_cloud.ConvertSettings()
    settings.file_path = "WordProcessing/sample.docx"
    settings.format = "pdf"
    
    # Create convert options with watermark
    convert_options = groupdocs_conversion_cloud.PdfConvertOptions()
    convert_options.watermark_options = watermark
    settings.convert_options = convert_options
    settings.output_path = "converted/with-watermark.pdf"
    
    # Execute conversion
    result = api.convert_document(groupdocs_conversion_cloud.ConvertDocumentRequest(settings))
    print(f"Document converted with CONFIDENTIAL watermark: {result[0].url}")
    
    # Example 2: Create a blue "DRAFT" watermark as foreground
    draft_watermark = groupdocs_conversion_cloud.WatermarkOptions()
    draft_watermark.text = "DRAFT"
    draft_watermark.color = "Blue"
    draft_watermark.width = 150
    draft_watermark.height = 150
    draft_watermark.background = False  # Set as foreground
    
    # Configure conversion settings
    settings2 = groupdocs_conversion_cloud.ConvertSettings()
    settings2.file_path = "WordProcessing/sample.docx"
    settings2.format = "pdf"
    
    # Create convert options with watermark
    convert_options2 = groupdocs_conversion_cloud.PdfConvertOptions()
    convert_options2.watermark_options = draft_watermark
    settings2.convert_options = convert_options2
    settings2.output_path = "converted/with-draft-watermark.pdf"
    
    # Execute conversion
    result2 = api.convert_document(groupdocs_conversion_cloud.ConvertDocumentRequest(settings2))
    print(f"Document converted with DRAFT watermark: {result2[0].url}")

except groupdocs_conversion_cloud.ApiException as e:
    print(f"Exception when calling ConvertApi: {e}")
```

### Using cURL

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Get token from JSON response
# Now use the token to add watermark during conversion
curl -v "https://api.groupdocs.cloud/v2.0/conversion" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FilePath': 'WordProcessing/sample.docx',
    'Format': 'pdf',
    'ConvertOptions': {
        'WatermarkOptions': {
            'Text': 'CONFIDENTIAL',
            'Color': 'Red',
            'Width': 100,
            'Height': 100,
            'Background': true
        }
    },
    'OutputPath': 'converted/with-watermark.pdf'
}"
```

## Watermark Customization Examples

### Example 1: Diagonal Copyright Notice

```csharp
// Create a diagonal copyright watermark
var copyrightWatermark = new WatermarkOptions
{
    Text = "© 2023 My Company - All Rights Reserved",
    Color = "Gray",
    Width = 200,
    Height = 70,
    Background = true
};
```

### Example 2: "DRAFT" Watermark

```csharp
// Create a prominent "DRAFT" watermark
var draftWatermark = new WatermarkOptions
{
    Text = "DRAFT",
    Color = "Red",
    Width = 150,
    Height = 150,
    Background = false // Display on top of content
};
```

### Example 3: Confidential Document Watermark

```csharp
// Create a "CONFIDENTIAL" watermark
var confidentialWatermark = new WatermarkOptions
{
    Text = "CONFIDENTIAL",
    Color = "#FF0000", // Using hex color code
    Width = 100,
    Height = 100,
    Background = true
};
```

## Troubleshooting Tips

### Watermark Not Appearing
Issue: The watermark is not visible in the converted document.Solution: 
- Check if the watermark color contrasts with the document background
- Try setting `Background` to `false` to place the watermark above the content
- Increase the watermark width and height values

### Watermark Positioning
Issue: Watermark is not positioned as expected.Solution: Currently, the API centers the watermark. If you need precise positioning, consider using the GroupDocs.Watermark product instead.

### Format Compatibility
Issue: Watermark doesn't appear in certain output formats.
Solution: Not all formats support watermarking. PDF, image formats, and most document formats work best. Some plain text formats may not support watermarks.

## Try It Yourself

Now that you've learned how to add watermarks, try these exercises:

1. Create a document with a diagonal "SAMPLE" watermark in a semi-transparent gray color
2. Add a copyright notice to the bottom of each page
3. Create a "For Review Only" watermark to appear on top of the content
4. Experiment with different colors and sizes to find the most effective watermark style

## What You've Learned

In this tutorial, you've learned how to:
- Add text watermarks to documents during conversion
- Configure watermark appearance including text, color, size, and positioning
- Choose between background and foreground watermarks
- Implement watermarking in different programming languages
- Apply different watermark styles for various business needs

## Next Steps

Now that you can add watermarks during conversion, you might want to explore:
- [Tutorial: How to Convert Specific Pages](/advanced-features/convert-specific-pages/)
- [Tutorial: Converting to PDF Formats](/advanced-features/convert-to-pdf-formats/)
- [Tutorial: How to Use Custom Fonts in Document Conversion](/advanced-features/convert-using-custom-font/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/conversion)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)


If you have questions about this tutorial, please feel free to post them in our [support forum](https://forum.groupdocs.cloud/c/conversion).
