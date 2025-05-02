---
title: Learn to Convert to Any Format with GroupDocs.Conversion Cloud API
description: Step-by-step tutorial on implementing universal document format conversions using GroupDocs.Conversion Cloud API
url: /advanced-features/convert-to-any-format/
weight: 180
---

# Tutorial: Converting to Any Format with GroupDocs.Conversion Cloud

In this tutorial, you'll learn how to implement universal document conversion capabilities using GroupDocs.Conversion Cloud API. By the end, you'll be able to convert documents between multiple formats with customized settings and handle both storage-based and stream-based outputs.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Implement basic document conversion between any supported formats
- Configure format-specific conversion options
- Process conversion results as storage URLs and as direct streams
- Handle conversion of password-protected documents
- Troubleshoot common conversion issues

## Prerequisites

Before starting this tutorial, you need:
1. A [GroupDocs.Conversion Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Development environment with your preferred programming language set up
5. Sample documents to test conversion (we'll use a Word document in this tutorial)

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

If using an SDK, authentication is handled by configuring the client:

```csharp
// C# SDK example
string clientId = "YOUR_CLIENT_ID";
string clientSecret = "YOUR_CLIENT_SECRET";
var configuration = new Configuration(clientId, clientSecret);
var apiInstance = new ConvertApi(configuration);
```

### Step 2: Setting Up Basic Conversion Parameters

Let's create a basic conversion request to convert a DOCX file to PDF:

#### Try it yourself

Using cURL:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/conversion" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
      'FilePath': 'documents/sample.docx',
      'Format': 'pdf',
      'OutputPath': 'converted'
    }"
```

Don't forget to replace `YOUR_JWT_TOKEN` with the actual token received in Step 1.

### Step 3: Implementing Storage-Based Conversion

Now let's implement a complete example that converts a document and saves the result to storage:

#### Converting from Word to PDF

```csharp
using System;
using System.Collections.Generic;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace ConversionTutorial
{
    class ConvertToAnyFormat
    {
        public static void ConvertDocumentToStorage()
        {
            // Configure the API client
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            var apiInstance = new ConvertApi(configuration);

            try
            {
                // Specify conversion settings
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "documents/sample.docx",
                    Format = "pdf",
                    OutputPath = "converted"
                };

                // Convert to specified format
                List<StoredConvertedResult> response = apiInstance.ConvertDocument(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("Document converted successfully: " + response[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 4: Implementing Stream-Based Conversion

For applications that need to process the converted file directly rather than saving it to storage:

```csharp
using System;
using System.IO;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace ConversionTutorial
{
    class ConvertToAnyFormat
    {
        public static void ConvertDocumentToStream()
        {
            // Configure the API client
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            var apiInstance = new ConvertApi(configuration);

            try
            {
                // Specify conversion settings - note OutputPath is null for stream output
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "documents/sample.docx",
                    Format = "pdf",
                    // OutputPath = null indicates stream output
                    OutputPath = null
                };

                // Convert and get result as a stream
                Stream response = apiInstance.ConvertDocumentDownload(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("Document converted successfully, stream size: " + 
                    response.Length.ToString() + " bytes");
                
                // Process the stream as needed, for example, save to local file
                using (var fileStream = File.Create("local-result.pdf"))
                {
                    response.CopyTo(fileStream);
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 5: Advanced Conversion with Format-Specific Options

Different target formats support specific conversion options. Here's how to use them:

```csharp
using System;
using System.Collections.Generic;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace ConversionTutorial
{
    class ConvertToAnyFormat
    {
        public static void ConvertWithAdvancedOptions()
        {
            // Configure the API client
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            var apiInstance = new ConvertApi(configuration);

            try
            {
                // Convert from DOCX to XLSX with specific options for Excel
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "documents/sample.docx",
                    Format = "xlsx",
                    ConvertOptions = new XlsxConvertOptions()
                    {
                        FromPage = 1,
                        PagesCount = 5,
                        Zoom = 150
                    },
                    OutputPath = "converted"
                };

                // Convert to specified format
                List<StoredConvertedResult> response = apiInstance.ConvertDocument(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("Document converted successfully: " + response[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 6: Handling Password-Protected Documents

For password-protected documents, you need to provide the password in the load options:

```csharp
using System;
using System.Collections.Generic;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace ConversionTutorial
{
    class ConvertToAnyFormat
    {
        public static void ConvertProtectedDocument()
        {
            // Configure the API client
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            var apiInstance = new ConvertApi(configuration);

            try
            {
                // Convert password-protected DOCX to PDF
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "documents/protected.docx",
                    Format = "pdf",
                    // Specify password in load options
                    LoadOptions = new DocxLoadOptions() 
                    { 
                        Password = "mypassword" 
                    },
                    OutputPath = "converted"
                };

                // Convert to specified format
                List<StoredConvertedResult> response = apiInstance.ConvertDocument(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("Protected document converted successfully: " + response[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

## Troubleshooting Common Issues

### 1. Authentication Errors

If you receive a 401 Unauthorized error, check:
- Your Client ID and Client Secret are correctly entered
- Your JWT token hasn't expired (tokens typically expire after 1 hour)
- You're including the "Bearer " prefix before your token in the Authorization header

### 2. File Not Found Errors

If you get a file not found error:
- Verify the file path is correct and the file exists in your storage
- Check that the storage name parameter matches your actual storage name
- Ensure you've uploaded the file to the storage before attempting conversion

### 3. Conversion Failures

If the conversion fails:
- Verify the source document isn't corrupted
- For protected documents, ensure the correct password is provided
- Check that both source and target formats are supported
- Review the specific error message for additional details

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with GroupDocs.Conversion Cloud API
- Basic document conversion to different formats
- Stream-based and storage-based output handling
- Implementing format-specific conversion options
- Converting password-protected documents
- Troubleshooting common conversion issues

## Further Practice

To reinforce your learning, try these exercises:
1. Convert a PDF document to a Word document (.docx)
2. Implement a batch conversion that processes multiple files
3. Create a simple web form that allows users to upload and convert documents
4. Add error handling that provides user-friendly error messages for conversion issues

## Next Tutorial

Ready to learn more? Continue with our [Tutorial: Converting Documents to Spreadsheet Formats](/advanced-features/convert-to-spreadsheet/) to master specialized conversions to Excel and other spreadsheet formats.

## Additional Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [Live Demo](https://products.groupdocs.app/conversion/family)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.conversion-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [forum](https://forum.groupdocs.cloud/c/conversion/11) for support.
