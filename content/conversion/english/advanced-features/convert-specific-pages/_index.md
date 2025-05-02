---
title: How to Convert Specific Pages from Documents Tutorial
description: Learn how to convert selected pages from documents using GroupDocs.Conversion Cloud API in this step-by-step developer tutorial.
url: /advanced-features/convert-specific-pages/
weight: 50
---

# Tutorial: How to Convert Specific Pages from Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Selectively convert individual pages from multi-page documents
- Specify non-consecutive pages for conversion
- Implement page selection across different programming languages
- Handle page numbering correctly in your conversion requests

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Conversion Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with your preferred programming language (C#, Java, Python, PHP, Ruby, or Node.js)
- A multi-page document to test the conversion (we'll use a DOCX file in this tutorial)

## Understanding Page Selection in Document Conversion

When working with multi-page documents, you often need to extract or convert only specific pages rather than the entire document. This capability is valuable for:

- Extracting key information from large reports
- Creating previews of selected document pages
- Breaking down large documents into smaller segments
- Focusing on relevant content for targeted processing

In this tutorial, we'll demonstrate how to use GroupDocs.Conversion Cloud API to convert only selected pages from a document.

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

### Step 3: Upload Your Document

Before conversion, upload your document to the cloud storage:

```csharp
// For this step, we're assuming the document is already uploaded
// See the File API documentation for details on uploading files
```

### Step 4: Specify the Pages to Convert

Now let's create a conversion request specifying only certain pages. There are two ways to select pages:

#### Method 1: Using Pages Array (for non-consecutive pages)

```csharp
// Prepare convert settings
var settings = new ConvertSettings
{
    FilePath = "WordProcessing/four-pages.docx",
    Format = "pdf",
    ConvertOptions = new PdfConvertOptions
    {
        Pages = new List<int?> {1, 3} // Convert only pages 1 and 3 (page numbers start from 1)
    },
    OutputPath = "converted/selected-pages.pdf"
};
```

#### Method 2: Using FromPage and PagesCount (for consecutive pages)

```csharp
// Prepare convert settings
var settings = new ConvertSettings
{
    FilePath = "WordProcessing/four-pages.docx",
    Format = "pdf",
    ConvertOptions = new PdfConvertOptions
    {
        FromPage = 2,
        PagesCount = 2 // Convert pages 2 and 3
    },
    OutputPath = "converted/consecutive-pages.pdf"
};
```

### Step 5: Send the Conversion Request

Execute the conversion with your specified settings:

```csharp
// Convert to specified format
var response = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings));
Console.WriteLine("Document converted successfully: " + response[0].Url);
```

### Step 6: Download or Process the Result

After conversion, you can either download the result or process it as needed:

```csharp
// The response contains the URL to access the converted document
// You can download it using the storage API or access it directly via the URL
```

## Complete Code Examples

### Using C#

```csharp
using System;
using System.Collections.Generic;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace GroupDocs.Conversion.Cloud.Examples
{
    class ConvertSpecificPages
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
                // Example 1: Convert non-consecutive pages (1 and 3)
                var settings1 = new ConvertSettings
                {
                    FilePath = "WordProcessing/four-pages.docx",
                    Format = "pdf",
                    ConvertOptions = new PdfConvertOptions
                    {
                        Pages = new List<int?> {1, 3} // Convert only pages 1 and 3
                    },
                    OutputPath = "converted/non-consecutive-pages.pdf"
                };
                
                // Convert to specified format
                var response1 = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings1));
                Console.WriteLine("Non-consecutive pages converted successfully: " + response1[0].Url);
                
                // Example 2: Convert consecutive pages (2 and 3)
                var settings2 = new ConvertSettings
                {
                    FilePath = "WordProcessing/four-pages.docx",
                    Format = "pdf",
                    ConvertOptions = new PdfConvertOptions
                    {
                        FromPage = 2,
                        PagesCount = 2 // Convert pages 2 and 3
                    },
                    OutputPath = "converted/consecutive-pages.pdf"
                };
                
                // Convert to specified format
                var response2 = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings2));
                Console.WriteLine("Consecutive pages converted successfully: " + response2[0].Url);
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
    # Example 1: Convert non-consecutive pages (1 and 3)
    settings1 = groupdocs_conversion_cloud.ConvertSettings()
    settings1.file_path = "WordProcessing/four-pages.docx"
    settings1.format = "pdf"
    
    # Create convert options and specify pages
    convert_options1 = groupdocs_conversion_cloud.PdfConvertOptions()
    convert_options1.pages = [1, 3]  # Convert only pages 1 and 3
    
    settings1.convert_options = convert_options1
    settings1.output_path = "converted/non-consecutive-pages.pdf"
    
    # Convert the document
    result1 = api.convert_document(groupdocs_conversion_cloud.ConvertDocumentRequest(settings1))
    print(f"Non-consecutive pages converted successfully: {result1[0].url}")
    
    # Example 2: Convert consecutive pages (2 and 3)
    settings2 = groupdocs_conversion_cloud.ConvertSettings()
    settings2.file_path = "WordProcessing/four-pages.docx"
    settings2.format = "pdf"
    
    # Create convert options for consecutive pages
    convert_options2 = groupdocs_conversion_cloud.PdfConvertOptions()
    convert_options2.from_page = 2
    convert_options2.pages_count = 2  # Convert pages 2 and 3
    
    settings2.convert_options = convert_options2
    settings2.output_path = "converted/consecutive-pages.pdf"
    
    # Convert the document
    result2 = api.convert_document(groupdocs_conversion_cloud.ConvertDocumentRequest(settings2))
    print(f"Consecutive pages converted successfully: {result2[0].url}")

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
# Now use the token to convert specific pages
curl -v "https://api.groupdocs.cloud/v2.0/conversion" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FilePath': 'WordProcessing/four-pages.docx',
    'Format': 'pdf',
    'ConvertOptions': {
        'Pages': [1, 3]
    },
    'OutputPath': 'converted/specific-pages.pdf'
}"
```

## Troubleshooting Tips

### Page Numbering
Issue: Incorrect pages are being converted.Solution: Remember that page numbering starts from 1, not 0. If you need the first page, use 1 as the page number.

### Invalid Page Numbers
Issue: Error when specifying page numbers that don't exist.Solution: Ensure your page selection doesn't exceed the total number of pages in the document. The API will return an error if you request a non-existent page.

### Format Compatibility
Issue: Some formats might not support page-level operations.Solution: Not all document formats support page-level operations equally. For best results, use common document formats like DOCX, PDF, PPTX, etc.

## Try It Yourself

Now that you've learned how to convert specific pages, try these exercises:

1. Convert the first and last pages of a 10-page document
2. Extract pages 2-5 from a PDF document and convert them to JPG images
3. Try converting specific pages from different document types (PDF, PPTX, etc.)

## What You've Learned

In this tutorial, you've learned how to:
- Selectively convert specific pages from multi-page documents
- Use two different methods for page selection (individual pages and page ranges)
- Implement page selection in different programming languages
- Handle common issues related to page-level conversion

## Next Steps

Now that you can convert specific pages, you might want to explore:
- [Tutorial: How to Add Watermarks During Conversion](/advanced-features/add-watermark/)
- [Tutorial: Converting to Image Formats](/advanced-features/convert-to-image-formats/)
- [Tutorial: How to Use Custom Fonts in Document Conversion](/advanced-features/convert-using-custom-font/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/conversion)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

If you have questions about this tutorial, please feel free to post them in our [support forum](https://forum.groupdocs.cloud/c/conversion).
