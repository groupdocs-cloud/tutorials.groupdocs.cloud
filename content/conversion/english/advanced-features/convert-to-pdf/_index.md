---
title: How to Convert Documents to PDF Formats with GroupDocs.Conversion Cloud API
description: Learn to convert various document types to PDF and PDF/A formats with security, optimization and formatting options using GroupDocs.Conversion Cloud API
url: /advanced-features/convert-to-pdf/
weight: 200
---

# Tutorial: Converting Documents to PDF Formats

In this tutorial, you'll learn how to convert various document types to PDF and PDF/A formats using GroupDocs.Conversion Cloud API. You'll master PDF conversion with advanced options including security settings, compression, and PDF/A compliance for archival purposes.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Convert documents to standard PDF and PDF/A formats
- Apply security settings including passwords and permissions
- Optimize PDF output with compression and metadata options
- Control PDF appearance with bookmarks and display settings
- Implement both storage-based and stream-based PDF conversions
- Troubleshoot common PDF conversion challenges

## Prerequisites

Before starting this tutorial, you need:
1. A [GroupDocs.Conversion Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Development environment with your preferred programming language set up
5. Sample documents to test conversion (we'll use various formats including Word, Excel, and images)

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

The response will contain a JWT token that needs to be used in subsequent requests.

### Step 2: Basic Document to PDF Conversion

Let's start with a simple conversion from a Word document to PDF format:

#### Try it yourself

Using cURL:

```bash
curl -X POST "https://api.groupdocs.cloud/v2.0/conversion" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: application/json" \
-d "{  
      'FilePath': 'documents/sample.docx',  
      'Format': 'pdf',  
      'OutputPath': 'converted'
    }"
```

Replace `YOUR_JWT_TOKEN` with the actual token received in Step 1.

### Step 3: Converting to PDF with Advanced Options

Now let's implement a comprehensive example that converts a document to PDF with specific PDF conversion options:

```csharp
// C# SDK Example
using System;
using System.Collections.Generic;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace PdfConversionTutorial
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
                // Set up advanced conversion to PDF with options
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "documents/sample.docx",
                    Format = "pdf",
                    // For password-protected documents
                    LoadOptions = new DocxLoadOptions() 
                    { 
                        Password = "",
                        HideWordTrackedChanges = true,
                        DefaultFont = "Arial" 
                    },
                    // PDF-specific convert options
                    ConvertOptions = new PdfConvertOptions()
                    {
                        // Page options
                        FromPage = 1,
                        PagesCount = 5,
                        
                        // Security options
                        Password = "securePassword",       // Set PDF password
                        PermissionsPassword = "ownerPwd",  // Set permissions password
                        Permissions = new List<string>     // Restrict PDF permissions
                        { 
                            "PrintDocument", 
                            "ModifyContent" 
                        },
                        
                        // Appearance options
                        Zoom = 100,
                        Dpi = 300,
                        MarginTop = 10,
                        MarginBottom = 10,
                        MarginLeft = 10,
                        MarginRight = 10,
                        
                        // PDF features
                        BookmarksOutlineLevel = 2,         // Include 2 levels of bookmarks
                        CenterWindow = true,               // Center window when opened
                        DisplayDocTitle = true,            // Display document title
                        FitWindow = true,                  // Fit window to document
                        
                        // Optimization options
                        Grayscale = false,                 // Keep colors
                        RemoveUnusedStreams = true,        // Remove unused content
                        RemoveUnusedObjects = true,        // Remove unused objects
                        CompressImages = true,             // Compress images
                        ImageQuality = 95,                 // Image quality (1-100)
                        UnembedFonts = false               // Keep embedded fonts
                    },
                    OutputPath = "converted"
                };

                // Execute conversion
                List<StoredConvertedResult> response = apiInstance.ConvertDocument(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("Document converted successfully to PDF: " + response[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 4: Converting to PDF/A for Archiving

PDF/A is a specialized format for long-term archiving. Let's convert a document to PDF/A format:

```java
// Java SDK Example
import com.groupdocs.cloud.conversion.api.*;
import com.groupdocs.cloud.conversion.client.ApiException;
import com.groupdocs.cloud.conversion.model.*;
import com.groupdocs.cloud.conversion.model.requests.*;
import java.util.List;

public class PdfAConversionExample {
    public static void main(String[] args) {
        // Configure API client
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        Configuration configuration = new Configuration(clientId, clientSecret);
        ConvertApi apiInstance = new ConvertApi(configuration);
        
        try {
            // Prepare convert settings for PDF/A
            ConvertSettings settings = new ConvertSettings();
            settings.setFilePath("documents/sample.docx");
            settings.setFormat("pdfa_2a");  // Use PDF/A-2a format
            
            // Set Word-specific load options if needed
            WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
            loadOptions.setDefaultFont("Times New Roman");
            settings.setLoadOptions(loadOptions);
            
            // Configure PDF/A-specific convert options
            PdfConvertOptions convertOptions = new PdfConvertOptions();
            convertOptions.setFromPage(1);
            convertOptions.setPagesCount(0);  // Convert all pages
            convertOptions.setDpi(300);       // High resolution
            convertOptions.setWidth(595);     // Standard A4 width in points
            convertOptions.setHeight(842);    // Standard A4 height in points
            
            // Set PDF/A compliance options
            convertOptions.setRemovePdfaCompliance(false);  // Maintain PDF/A compliance
            
            settings.setConvertOptions(convertOptions);
            settings.setOutputPath("converted");
            
            // Execute conversion
            List<StoredConvertedResult> result = apiInstance.convertDocument(
                new ConvertDocumentRequest(settings));
            
            System.out.println("Document converted successfully to PDF/A: " + result.get(0).getUrl());
        } catch (ApiException e) {
            System.err.println("Exception when calling ConvertApi: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Step 5: Creating Secure PDF Documents

Security is often a concern with PDF documents. Here's how to create PDFs with various protection settings:

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
    settings.file_path = "documents/confidential.docx"
    settings.format = "pdf"
    
    # Configure PDF-specific convert options with security settings
    convert_options = groupdocs_conversion_cloud.PdfConvertOptions()
    
    # Document open password - user needs this to open document
    convert_options.password = "userPassword123"
    
    # Owner password for changing permissions
    convert_options.permissions_password = "ownerPassword456"
    
    # Set specific permissions
    convert_options.permissions = [
        "PrintDocument",          # Allow printing
        "ModifyAnnotations",      # Allow annotation editing
        "AssembleDocument"        # Allow document assembly
    ]
    
    # Block these permissions (not explicitly allowed above)
    # - Content copying
    # - Content extraction
    # - Content editing
    # - Printing in high quality
    
    # Optimization and appearance
    convert_options.dpi = 300
    convert_options.center_window = True
    convert_options.fit_window = True
    convert_options.display_doc_title = True
    
    settings.convert_options = convert_options
    settings.output_path = "converted/secure"
    
    # Execute conversion
    request = ConvertDocumentRequest(settings)
    result = api_instance.convert_document(request)
    
    print(f"Secure PDF created successfully: {result[0].url}")
except groupdocs_conversion_cloud.ApiException as e:
    print(f"Exception when calling ConvertApi: {e}")
```

### Step 6: Stream-Based PDF Conversion

For applications that need to process the converted PDF directly:

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
    filePath: "documents/sample.docx",
    format: "pdf",
    loadOptions: {
        // DOCX-specific load options if needed
    },
    convertOptions: {
        // PDF-specific convert options
        fromPage: 1,
        pagesCount: 0,  // All pages
        dpi: 300,
        linearize: true,  // Optimize for web viewing (Fast Web View)
        removeUnusedObjects: true,
        removeUnusedStreams: true,
        compressImages: true,
        imageQuality: 95
    },
    // Set outputPath to null for stream output
    outputPath: null
};

// Execute conversion
apiInstance.convertDocumentDownload({ convertSettings: settings })
    .then((result) => {
        // Save the stream to a file
        const fileName = "./converted-document.pdf";
        const writeStream = fs.createWriteStream(fileName);
        
        result.pipe(writeStream);
        
        writeStream.on("finish", () => {
            console.log(`Document converted and saved to ${fileName}`);
        });
    })
    .catch((error) => {
        console.log(`Error: ${error.message}`);
    });
```

## PDF-Specific Conversion Options

When converting to PDF formats, you can leverage these specialized options:

| Option | Description | Default |
|--------|-------------|---------|
| Password | Document open password | null |
| PermissionsPassword | Password for changing permissions | null |
| Permissions | List of allowed permissions | All |
| PdfFormat | Target PDF format version | "PDF" |
| FromPage | First page number to convert | 1 |
| PagesCount | Number of pages to convert | All pages |
| Dpi | Resolution in dots per inch | 96 |
| Width | Width of the resulting document | Auto |
| Height | Height of the resulting document | Auto |
| BookmarksOutlineLevel | Bookmark outline level to include | 0 (none) |
| CenterWindow | Center window when opened | false |
| CompressImages | Compress images in the PDF | false |
| DisplayDocTitle | Display document title instead of filename | false |
| FitWindow | Fit window to first page | false |
| Grayscale | Convert to grayscale | false |
| RemoveUnusedObjects | Remove unused objects | false |
| RemoveUnusedStreams | Remove unused streams | false |
| UnembedFonts | Remove embedded fonts | false |
| Linearize | Optimize for web viewing | false |
| ImageQuality | Image quality (1-100) | 100 |
| RemovePdfaCompliance | Remove PDF/A compliance | true |

### Available PDF Permissions

When setting the `Permissions` property, you can include any of these values:

- PrintDocument
- ModifyContent
- CopyContent
- ModifyAnnotations
- FillForm
- ExtractContent
- AssembleDocument
- HighQualityPrint

## Troubleshooting Common Issues

### 1. PDF Security Issues

If you encounter problems with PDF security:
- Verify that both password and permissions password are set correctly
- Ensure permissions are properly specified as a list of allowed actions
- Remember that some readers may not support all security features

### 2. PDF/A Compliance Problems

If your PDF/A conversion is rejected by validation tools:
- Set `RemovePdfaCompliance` to false
- Ensure all fonts used in the document can be embedded
- Avoid transparent images or elements
- Consider using PDF/A-2 or PDF/A-3 for more complex documents

### 3. Size and Quality Optimization

If the PDF file size is too large:
- Enable `CompressImages` and reduce `ImageQuality` (75-85 offers good balance)
- Set `RemoveUnusedObjects` and `RemoveUnusedStreams` to true
- Consider using `Grayscale` for documents that don't require color
- Set appropriate DPI (150-300 is sufficient for most purposes)

### 4. Font and Layout Issues

If fonts or layout appear incorrect:
- Specify the `DefaultFont` in load options
- Increase DPI for better text rendering
- If possible, ensure fonts are available in the conversion environment
- For complex layouts, adjust margins or use `FitWindow` option

## What You've Learned

In this tutorial, you've learned:
- How to convert various document formats to PDF and PDF/A
- Implementing PDF security with passwords and permissions
- Optimizing PDF output with compression and metadata options
- Controlling PDF appearance with bookmarks and display settings
- Creating both storage-based and stream-based PDF conversions
- Troubleshooting common PDF conversion challenges

## Further Practice

To reinforce your learning, try these exercises:
1. Create a secure PDF converter that applies different security levels based on content type
2. Implement a batch conversion utility that optimizes multiple documents to PDF with size reduction
3. Build a PDF/A converter specifically for archiving important documents
4. Create a service that adds bookmarks to PDFs based on document headings

## Next Tutorial

Ready to learn more? Continue with our [Tutorial: Converting to HTML Format](/advanced-features/convert-to-html) to master conversion to web-friendly formats with customization options.

## Additional Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [Live Demo](https://products.groupdocs.app/conversion/family)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.conversion-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [forum](https://forum.groupdocs.cloud/c/conversion/11) for support.
