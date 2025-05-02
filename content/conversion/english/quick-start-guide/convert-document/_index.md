---
title: How to Convert Documents with GroupDocs.Conversion Cloud Tutorial
weight: 2
description: Learn step-by-step how to convert documents between various formats using GroupDocs.Conversion Cloud API in this developer tutorial.
url: /quick-start-guide/convert-document/
---

# Tutorial: How to Convert Documents with GroupDocs.Conversion Cloud

## Learning Objectives

In this tutorial, you'll learn:
- How to upload files to GroupDocs cloud storage
- How to convert documents from one format to another using cloud storage
- How to download the converted document
- How to work with conversion options for different file formats

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Conversion Cloud account (get a [free trial](https://dashboard.groupdocs.cloud/#/apps) if needed)
- Your Client ID and Client Secret credentials
- Basic understanding of REST APIs
- Familiarity with your preferred programming language (C#, Java, PHP, etc.)
- Completed our [Supported Formats tutorial](/quick-start-guide/supported-formats/) (recommended)

## Why Use Document Conversion?

Document conversion is essential for modern applications as it enables:
- Standardization of document formats across an organization
- Creation of archivable versions of documents (like PDF/A)
- Improved document sharing and compatibility between different systems
- Data extraction from proprietary formats for analysis

## Tutorial Steps

### Step 1: Understanding the Conversion Process

The GroupDocs.Conversion Cloud API document conversion process involves three main steps:
1. Upload the input document to cloud storage
2. Convert the document to the desired format
3. Download the converted document (optional if using output path)

Let's implement each of these steps.

### Step 2: Set Up Your Environment

First, let's set up our development environment:

#### Try it yourself:

1. Create a new project in your preferred IDE
2. Install the appropriate GroupDocs.Conversion Cloud SDK for your language
3. Set up your authentication credentials:

```csharp
// C# example
string MyClientSecret = ""; // Get from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);

// Create necessary API instances
var convertApi = new ConvertApi(configuration);
var storageApi = new StorageApi(configuration); // For uploading files
```

### Step 3: Upload a File to Cloud Storage

Before conversion, we need to upload our file to the GroupDocs cloud storage:

#### Using cURL

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Upload file
curl -v "https://api.groupdocs.cloud/v2.0/conversion/storage/file/WordProcessing/four-pages.docx" \
-X PUT \
-H "Content-Type: application/octet-stream" \
-H "Accept: application/json" \
-H "Authorization: Bearer <jwt token>" \
--data-binary @"/path/to/local/four-pages.docx"
```

#### Using C# SDK

```csharp
using System.IO;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

// Upload file to cloud storage
var uploadRequest = new UploadFileRequest("WordProcessing/four-pages.docx", 
    File.OpenRead("/path/to/local/four-pages.docx"), Common.MyStorage);
storageApi.UploadFile(uploadRequest);

Console.WriteLine("File uploaded successfully!");
```

### Step 4: Convert the Document

Now, let's convert the uploaded document:

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v2.0/conversion/conversion" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer <jwt token>" \
-d "{
        'FilePath': 'WordProcessing/four-pages.docx',
        'Format': 'pdf',
        'OutputPath': 'Output'
    }"
```

#### Using C# SDK

```csharp
// Prepare convert settings
var settings = new ConvertSettings
{
    FilePath = "WordProcessing/four-pages.docx",
    Format = "pdf",
    OutputPath = "converted"
};

// Convert to specified format
var response = convertApi.ConvertDocument(new ConvertDocumentRequest(settings));

Console.WriteLine("Document converted successfully!");
foreach (var file in response)
{
    Console.WriteLine($"File: {file.Name}, Size: {file.Size}, Path: {file.Path}");
}
```

### Step 5: Download the Converted Document

After conversion, you can download the converted document:

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v2.0/conversion/storage/file/Output/four-pages.pdf" \
-X GET \
-H "Accept: application/octet-stream" \
-H "Authorization: Bearer <jwt token>" \
--output "/path/to/local/four-pages.pdf"
```

#### Using C# SDK

```csharp
// Download converted file
var downloadRequest = new DownloadFileRequest("converted/four-pages.pdf", Common.MyStorage);
var stream = storageApi.DownloadFile(downloadRequest);

// Save to local file
using (var fileStream = File.Create("/path/to/save/four-pages.pdf"))
{
    stream.CopyTo(fileStream);
}

Console.WriteLine("File downloaded successfully!");
```

### Step 6: Working with Conversion Options

Different formats have specific conversion options. Let's look at how to use PDF conversion options:

#### Using C# SDK

```csharp
// Prepare convert settings with PDF options
var settings = new ConvertSettings
{
    FilePath = "WordProcessing/four-pages.docx",
    Format = "pdf",
    OutputPath = "converted-with-options",
    ConvertOptions = new PdfConvertOptions
    {
        CenterWindow = true,
        CompressImages = false,
        DisplayDocTitle = true,
        Dpi = 1024,
        FitWindow = false,
        FromPage = 1,
        Grayscale = false,
        ImageQuality = 100,
        Linearize = false,
        MarginTop = 5,
        MarginLeft = 5,
        Password = "password", // Set password for the resulting PDF
        UnembedFonts = true,
        RemoveUnusedStreams = true,
        RemoveUnusedObjects = true,
        RemovePdfaCompliance = false
    }
};

// Convert to PDF with options
var response = convertApi.ConvertDocument(new ConvertDocumentRequest(settings));

Console.WriteLine("Document converted with options successfully!");
```

### Step 7: Converting Password-Protected Documents

If you need to convert a password-protected document, you must provide the password in the loading options:

#### Using C# SDK

```csharp
// Prepare convert settings for password-protected document
var settings = new ConvertSettings
{
    FilePath = "WordProcessing/password-protected.docx",
    Format = "pdf",
    OutputPath = "converted-protected",
    LoadOptions = new DocxLoadOptions
    {
        Password = "your-document-password" // Password to open the protected document
    }
};

// Convert the password-protected document
var response = convertApi.ConvertDocument(new ConvertDocumentRequest(settings));

Console.WriteLine("Password-protected document converted successfully!");
```

## Complete Implementation Example

Here's a complete example combining all the steps above in C#:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-conversion-cloud/groupdocs-conversion-cloud-dotnet-samples
using System;
using System.IO;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace DocumentConversionTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get your client ID and client secret from https://dashboard.groupdocs.cloud
            string MyClientSecret = "YOUR_CLIENT_SECRET";
            string MyClientId = "YOUR_CLIENT_ID";

            // Create API instances
            var configuration = new Configuration(MyClientId, MyClientSecret);
            var convertApi = new ConvertApi(configuration);
            var storageApi = new StorageApi(configuration);

            try
            {
                // Step 1: Upload file to cloud storage
                string localFilePath = @"C:\Documents\sample.docx";
                string cloudFilePath = "WordProcessing/sample.docx";
                
                Console.WriteLine("Uploading file...");
                using (var stream = File.OpenRead(localFilePath))
                {
                    var uploadRequest = new UploadFileRequest(cloudFilePath, stream);
                    storageApi.UploadFile(uploadRequest);
                }
                Console.WriteLine("File uploaded successfully!");

                // Step 2: Convert the document
                Console.WriteLine("Converting document...");
                var settings = new ConvertSettings
                {
                    FilePath = cloudFilePath,
                    Format = "pdf",
                    OutputPath = "converted"
                };

                var response = convertApi.ConvertDocument(new ConvertDocumentRequest(settings));
                Console.WriteLine("Document converted successfully!");

                // Step 3: Download the converted document
                Console.WriteLine("Downloading converted document...");
                string convertedFilePath = response[0].Path;
                string localSavePath = @"C:\Documents\sample_converted.pdf";
                
                var downloadRequest = new DownloadFileRequest(convertedFilePath);
                var stream = storageApi.DownloadFile(downloadRequest);

                using (var fileStream = File.Create(localSavePath))
                {
                    stream.CopyTo(fileStream);
                }
                Console.WriteLine($"File downloaded successfully to {localSavePath}!");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }

            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```

## Troubleshooting Tips

- File Not Found Errors: Ensure the file path in cloud storage is correct. Remember that files are case-sensitive.
- Authentication Issues: Make sure your Client ID and Client Secret are correct and that you're generating a valid JWT token.
- Format Not Supported: Check if the conversion from your source format to the target format is supported using the formats API.
- Password Issues: If converting a protected document, ensure the password is correct.

## What You've Learned

In this tutorial, you've learned how to:
- Upload documents to GroupDocs cloud storage
- Convert documents from one format to another
- Download converted documents
- Work with conversion options for different formats
- Convert password-protected documents

## Further Practice

To reinforce your learning, try these exercises:
1. Create a batch conversion utility that converts multiple files in a folder
2. Build a simple web form that allows users to upload a document and choose the target format
3. Experiment with different conversion options for various formats (PDF, DOCX, XLSX, etc.)

## Next Tutorial

Continue your learning journey with our next tutorial: [Tutorial: How to Convert Documents Without Storage](/quick-start-guide/convert-direct/) to learn a more streamlined approach for document conversion.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

We'd love to hear your feedback on this tutorial! Please feel free to ask questions or suggest improvements on our [support forum](https://forum.groupdocs.cloud/c/conversion/11).
