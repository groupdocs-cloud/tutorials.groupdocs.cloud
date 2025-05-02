---
title: How to Convert PDF Documents with Custom Options using GroupDocs.Conversion Cloud API
description: Learn to convert PDF documents with advanced options for handling annotations, security, and formatting using GroupDocs.Conversion Cloud API
url: /advanced-features/pdf-conversion-options/
weight: 150
---

# Tutorial: Converting PDF Documents with Custom Options

In this tutorial, you'll learn how to convert PDF documents to various formats using GroupDocs.Conversion Cloud API. You'll master specialized PDF conversion options including annotation handling, security credentials, and advanced formatting control.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Convert PDF files to Word, Excel, images, and other formats
- Handle PDF security with password-protected documents
- Control annotation visibility during conversion
- Manage PDF form fields and embedded content
- Implement both storage-based and stream-based PDF conversions
- Troubleshoot common PDF conversion challenges

## Prerequisites

Before starting this tutorial, you need:
1. A [GroupDocs.Conversion Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Development environment with your preferred programming language set up
5. Sample PDF files to test conversion (including some with annotations, form fields, and password protection)

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

### Step 2: Basic PDF to Word Conversion

Let's start with a simple conversion from a PDF file to DOCX format:

#### Try it yourself

Using cURL:

```bash
curl -X POST "https://api.groupdocs.cloud/v2.0/conversion" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: application/json" \
-d "{  
      'FilePath': 'documents/sample.pdf',  
      'Format': 'docx',  
      'OutputPath': 'converted'
    }"
```

Replace `YOUR_JWT_TOKEN` with the actual token received in Step 1.

### Step 3: Converting PDF to Word with Annotation Control

Now let's implement a comprehensive example that converts a PDF to Word while handling annotations:

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
                // Set up conversion from PDF to Word with annotation control
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "documents/annotated.pdf",
                    Format = "docx",
                    // PDF-specific load options
                    LoadOptions = new PdfLoadOptions()
                    {
                        Password = "", // If PDF is password-protected
                        HidePdfAnnotations = true, // Hide annotations in output
                        FlattenAllFields = true,   // Flatten form fields
                        RemoveEmbeddedFiles = true // Remove embedded files for cleaner output
                    },
                    // Word-specific convert options
                    ConvertOptions = new DocxConvertOptions()
                    {
                        FromPage = 1,
                        PagesCount = 0,  // All pages
                        Dpi = 300
                    },
                    OutputPath = "converted"
                };

                // Execute conversion
                List<StoredConvertedResult> response = apiInstance.ConvertDocument(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("PDF document converted successfully to DOCX: " + response[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 4: Converting Password-Protected PDF Documents

For PDFs that require a password to open:

```java
// Java SDK Example
import com.groupdocs.cloud.conversion.api.*;
import com.groupdocs.cloud.conversion.client.ApiException;
import com.groupdocs.cloud.conversion.model.*;
import com.groupdocs.cloud.conversion.model.requests.*;
import java.util.List;

public class ProtectedPdfExample {
    public static void main(String[] args) {
        // Configure API client
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        Configuration configuration = new Configuration(clientId, clientSecret);
        ConvertApi apiInstance = new ConvertApi(configuration);
        
        try {
            // Prepare convert settings for protected PDF
            ConvertSettings settings = new ConvertSettings();
            settings.setFilePath("documents/secured.pdf");
            settings.setFormat("docx");
            
            // Set PDF-specific load options with password
            PdfLoadOptions loadOptions = new PdfLoadOptions();
            loadOptions.setPassword("yourpassword");  // Password to open the PDF
            loadOptions.setFlattenAllFields(true);
            loadOptions.setHidePdfAnnotations(true);
            
            settings.setLoadOptions(loadOptions);
            
            // Configure Word-specific convert options
            WordProcessingConvertOptions convertOptions = new WordProcessingConvertOptions();
            convertOptions.setFromPage(1);
            convertOptions.setPagesCount(0);  // All pages
            convertOptions.setDpi(300);
            
            settings.setConvertOptions(convertOptions);
            settings.setOutputPath("converted");
            
            // Execute conversion
            List<StoredConvertedResult> result = apiInstance.convertDocument(
                new ConvertDocumentRequest(settings));
            
            System.out.println("Protected PDF document converted successfully: " + result.get(0).getUrl());
        } catch (ApiException e) {
            System.err.println("Exception when calling ConvertApi: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Step 5: Converting PDF to Image Format with Page Control

Converting PDF documents to images gives you precise control over rendering:

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
    settings.file_path = "documents/multipage.pdf"
    settings.format = "jpg"
    
    # Configure PDF-specific load options
    load_options = groupdocs_conversion_cloud.PdfLoadOptions()
    load_options.password = ""  # If PDF is protected
    load_options.hide_pdf_annotations = True
    load_options.remove_embedded_files = True
    
    settings.load_options = load_options
    
    # Configure JPG-specific convert options
    convert_options = groupdocs_conversion_cloud.JpegConvertOptions()
    convert_options.from_page = 2       # Start from second page
    convert_options.pages_count = 3     # Convert three pages
    convert_options.dpi = 300           # High resolution
    convert_options.quality = 100       # Maximum quality
    convert_options.grayscale = False   # Color image
    
    settings.convert_options = convert_options
    settings.output_path = "converted"
    
    # Execute conversion
    request = ConvertDocumentRequest(settings)
    result = api_instance.convert_document(request)
    
    print(f"PDF document converted successfully to JPG: {len(result)} images")
    for i, image in enumerate(result):
        print(f" - Page {i+2}: {image.name} ({image.size} bytes)")
except groupdocs_conversion_cloud.ApiException as e:
    print(f"Exception when calling ConvertApi: {e}")
```

### Step 6: Converting PDF to Excel with Table Recognition

PDF documents often contain tabular data that's valuable in spreadsheet format:

```javascript
// Node.js SDK Example
const { ConvertApi, Configuration } = require("groupdocs-conversion-cloud");

// Configure API client
const clientId = "YOUR_CLIENT_ID";
const clientSecret = "YOUR_CLIENT_SECRET";
const config = new Configuration(clientId, clientSecret);
const apiInstance = new ConvertApi(config);

// Prepare conversion settings
const settings = {
    filePath: "documents/financial_report.pdf",
    format: "xlsx",
    loadOptions: {
        // PDF-specific load options
        password: "",  // If PDF is protected
        hidePdfAnnotations: true,
        flattenAllFields: true
    },
    convertOptions: {
        // Excel-specific convert options
        fromPage: 1,
        pagesCount: 0,  // All pages
        zoom: 100
    },
    outputPath: "converted"
};

// Execute conversion
apiInstance.convertDocument({ convertSettings: settings })
    .then((result) => {
        console.log(`PDF document converted successfully to Excel: ${result[0].url}`);
    })
    .catch((error) => {
        console.log(`Error: ${error.message}`);
    });
```

### Step 7: Stream-Based PDF Conversion

For applications that need to process the converted document directly:

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
    filePath: "documents/sample.pdf",
    format: "docx",
    loadOptions: {
        // PDF-specific load options
        hidePdfAnnotations: true,
        flattenAllFields: true
    },
    convertOptions: {
        // DOCX-specific convert options
        dpi: 300
    },
    // Set outputPath to null for stream output
    outputPath: null
};

// Execute conversion
apiInstance.convertDocumentDownload({ convertSettings: settings })
    .then((result) => {
        // Save the stream to a file
        const fileName = "./converted-pdf.docx";
        const writeStream = fs.createWriteStream(fileName);
        
        result.pipe(writeStream);
        
        writeStream.on("finish", () => {
            console.log(`PDF document converted and saved to ${fileName}`);
        });
    })
    .catch((error) => {
        console.log(`Error: ${error.message}`);
    });
```

## PDF-Specific Load Options

When converting PDF documents, you can leverage these specialized options:

| Option | Description | Default | Impact |
|--------|-------------|---------|--------|
| Password | Document open password | null | Required for protected PDFs |
| HidePdfAnnotations | Hide annotations in output | false | Controls annotation visibility |
| FlattenAllFields | Flatten form fields | false | Controls form field appearance |
| RemoveEmbeddedFiles | Remove embedded files | false | Controls embedded content |
| ExtractOCRText | Extract OCR text when available | true | Controls text extraction method |
| EnableLayeredRendering | Enable layer rendering | false | Controls layer handling |

## Troubleshooting Common Issues

### 1. Password and Security Problems

If you encounter issues with protected PDFs:
- Ensure the password is correct and provided in the correct case
- If a document has permission restrictions, you might need owner password
- For highly secured PDFs, some restrictions might prevent certain types of conversion

### 2. Annotation Handling Issues

When dealing with annotated PDFs:
- Use HidePdfAnnotations to control whether annotations appear in output
- If annotations appear unexpectedly, ensure this option is set to true
- For format-specific annotation conversion, test different target formats

### 3. Form Field Preservation

For PDFs with forms:
- Use FlattenAllFields to control whether fields become regular content
- When converting to editable formats, you might need to set this to false
- Test form field handling with different destination formats

### 4. Image Quality Challenges

When converting PDF to images:
- Adjust DPI for better quality (300 DPI is good for most purposes)
- For JPG conversion, set quality to a higher value (90-100)
- For text clarity, enable anti-aliasing if available

### 5. Layout Preservation Issues

For complex layouts:
- PDF to Word/Excel conversion might not preserve all formatting
- Try PDF to PDF/A conversion first for consistent results
- Consider using PDF to image conversion for exact visual fidelity

## What You've Learned

In this tutorial, you've learned:
- How to convert PDF documents to various formats including Word, Excel, and images
- Handling password-protected PDF documents
- Controlling annotation visibility and form field handling
- Managing embedded content during conversion
- Implementing both storage-based and stream-based PDF conversions
- Troubleshooting common PDF conversion challenges

## Further Practice

To reinforce your learning, try these exercises:
1. Create a batch conversion utility that processes multiple PDF files with consistent settings
2. Implement a web form that allows users to upload PDFs and choose conversion options
3. Build a system that extracts tables from PDFs and converts them to Excel with proper formatting
4. Create a PDF processing pipeline that handles annotations differently based on their type

## Additional Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [Live Demo](https://products.groupdocs.app/conversion/family)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.conversion-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [forum](https://forum.groupdocs.cloud/c/conversion/11) for support.
