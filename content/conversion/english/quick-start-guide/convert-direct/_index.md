---
title: How to Convert Documents Without Storage in GroupDocs.Conversion Cloud Tutorial
weight: 3
description: Learn how to convert documents directly without using cloud storage in this step-by-step tutorial for GroupDocs.Conversion Cloud API.
url: /quick-start-guide/convert-direct/
---

# Tutorial: How to Convert Documents Without Storage in GroupDocs.Conversion Cloud

## Learning Objectives

In this tutorial, you'll learn:
- How to convert documents directly without using cloud storage
- When and why to use direct conversion
- How to handle conversion options and specialized settings
- How to process the conversion results efficiently

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Conversion Cloud account (get a [free trial](https://dashboard.groupdocs.cloud/#/apps) if needed)
- Your Client ID and Client Secret credentials
- Basic understanding of REST APIs
- Familiarity with your preferred programming language (C#, Java, PHP, etc.)
- Completed our [Basic Document Conversion tutorial](/quick-start-guide/convert-document/) (recommended)

## Why Use Direct Conversion?

Direct conversion (without storage) offers several advantages:
- Simplified workflow: Skip the upload and download steps needed with storage-based conversion
- Reduced latency: Convert documents in a single API call
- Better privacy: Files aren't stored in the cloud
- Reduced storage costs: No need to maintain files in cloud storage
- Ideal for temporary conversions: Perfect for one-time, on-demand conversions

## Tutorial Steps

### Step 1: Understanding Direct Conversion

Direct conversion allows you to convert documents without first uploading them to cloud storage. Instead, you send the file directly in the API request and receive the converted file in the response.

The key differences from storage-based conversion are:
- Uses a different API endpoint (PUT instead of POST)
- Sends the file as form data in the request
- Receives the converted file directly in the response

### Step 2: Set Up Your Environment

Let's start by setting up our development environment:

#### Try it yourself:

1. Create a new project in your preferred IDE
2. Install the appropriate GroupDocs.Conversion Cloud SDK for your language
3. Set up your authentication credentials:

```csharp
// C# example
string MyClientSecret = ""; // Get from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);

// Create necessary API instance
var apiInstance = new ConvertApi(configuration);
```

### Step 3: Convert a Document Directly

Now, let's implement direct conversion:

#### Using cURL

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
  
# Convert document directly
curl -v "https://api.groupdocs.cloud/v2.0/conversion/conversion" \
-X PUT \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-H "Authorization: Bearer <jwt token>" \
--data-binary "@/path/to/file.docx"
```

#### Using C# SDK

```csharp
// Open the file
var fileStream = File.Open("C:\\path\\to\\document.docx", FileMode.Open);

// Create the request
var request = new ConvertDocumentDirectRequest("pdf", fileStream);

// Execute the API call
var response = apiInstance.ConvertDocumentDirect(request);

// Save the result to a file
using (var outputStream = File.Create("C:\\path\\to\\output.pdf"))
{
    response.CopyTo(outputStream);
}

Console.WriteLine("Document converted successfully!");
```

### Step 4: Convert a Password-Protected Document

If your document is password-protected, you need to provide the password using load options:

#### Using C# SDK

```csharp
// Open the file
var fileStream = File.Open("C:\\path\\to\\password-protected.docx", FileMode.Open);

// Create load options with password
var loadOptions = new DocxLoadOptions
{
    Format = "docx",
    Password = "your-document-password"
};

// Create the request with load options
var request = new ConvertDocumentDirectRequest("pdf", fileStream, null, null, loadOptions);

// Execute the API call
var response = apiInstance.ConvertDocumentDirect(request);

// Save the result to a file
using (var outputStream = File.Create("C:\\path\\to\\output.pdf"))
{
    response.CopyTo(outputStream);
}

Console.WriteLine("Password-protected document converted successfully!");
```

### Step 5: Specifying Conversion Options

You can customize the conversion process by specifying conversion options:

#### Using C# SDK

```csharp
// Open the file
var fileStream = File.Open("C:\\path\\to\\document.docx", FileMode.Open);

// Create load options if needed
var loadOptions = new DocxLoadOptions
{
    Format = "docx"
};

// Create PDF conversion options
var convertOptions = new PdfConvertOptions
{
    Dpi = 300,
    Width = 800,
    Height = 1200,
    Password = "set-pdf-password", // Set password for output PDF
    Grayscale = true,
    MarginTop = 20,
    MarginBottom = 20,
    MarginLeft = 20,
    MarginRight = 20
};

// Create the request with all options
var request = new ConvertDocumentDirectRequest("pdf", fileStream, 
    null, null, loadOptions, convertOptions);

// Execute the API call
var response = apiInstance.ConvertDocumentDirect(request);

// Save the result to a file
using (var outputStream = File.Create("C:\\path\\to\\output.pdf"))
{
    response.CopyTo(outputStream);
}

Console.WriteLine("Document converted with options successfully!");
```

### Step 6: Specifying Page Range for Conversion

You can convert specific pages by specifying the starting page and the number of pages:

#### Using C# SDK

```csharp
// Open the file
var fileStream = File.Open("C:\\path\\to\\multipage.docx", FileMode.Open);

// Create the request with page range parameters
// Start from page 2 and convert 3 pages
var request = new ConvertDocumentDirectRequest("pdf", fileStream, 2, 3);

// Execute the API call
var response = apiInstance.ConvertDocumentDirect(request);

// Save the result to a file
using (var outputStream = File.Create("C:\\path\\to\\specific_pages.pdf"))
{
    response.CopyTo(outputStream);
}

Console.WriteLine("Specific pages converted successfully!");
```

### Step 7: Complete Implementation Example

Here's a complete example that demonstrates direct document conversion in C#:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-conversion-cloud/groupdocs-conversion-cloud-dotnet-samples
using System;
using System.IO;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace DirectDocumentConversionTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get your client ID and client secret from https://dashboard.groupdocs.cloud
            string MyClientSecret = "YOUR_CLIENT_SECRET";
            string MyClientId = "YOUR_CLIENT_ID";

            // Create API instance
            var configuration = new Configuration(MyClientId, MyClientSecret);
            var apiInstance = new ConvertApi(configuration);

            try
            {
                // Path to local file
                string filePath = @"C:\Documents\sample.docx";
                
                Console.WriteLine("Converting document directly...");
                
                // Open the file stream
                using (var fileStream = File.OpenRead(filePath))
                {
                    // For password-protected files
                    var loadOptions = new DocxLoadOptions
                    {
                        Format = "docx",
                        Password = "your-password" // Remove if not needed
                    };
                    
                    // Create conversion options
                    var convertOptions = new PdfConvertOptions
                    {
                        Dpi = 300,
                        Grayscale = false
                    };
                    
                    // Create the request
                    var request = new ConvertDocumentDirectRequest(
                        "pdf", 
                        fileStream, 
                        null, // fromPage - null for all pages
                        null, // pagesCount - null for all pages
                        loadOptions, 
                        convertOptions);
                    
                    // Execute conversion
                    var result = apiInstance.ConvertDocumentDirect(request);
                    
                    // Save the result
                    string outputPath = @"C:\Documents\sample_converted.pdf";
                    using (var outputStream = File.Create(outputPath))
                    {
                        result.CopyTo(outputStream);
                    }
                    
                    Console.WriteLine($"Document converted successfully and saved to {outputPath}");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error during conversion: " + e.Message);
            }
            
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```

## SDK Examples for Other Languages

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-conversion-cloud/groupdocs-conversion-cloud-java-samples
String MyClientSecret = ""; // Get from https://dashboard.groupdocs.cloud
String MyClientId = ""; // Get from https://dashboard.groupdocs.cloud
  
Configuration configuration = new Configuration(MyClientId, MyClientSecret);
  
// Create API instance
ConvertApi apiInstance = new ConvertApi(configuration);

File file = new File("examples\\src\\main\\resources\\WordProcessing\\password-protected.docx");
DocxLoadOptions loadOptions = new DocxLoadOptions();
loadOptions.setFormat("docx");
loadOptions.setPassword("password");

ConvertDocumentDirectRequest request = new ConvertDocumentDirectRequest("pdf", file, 1, 0, loadOptions, null); // all pages

File result = apiInstance.convertDocumentDirect(request);

System.out.println("Document converted: " + result.length());
```

### Node.js Example

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-conversion-cloud/groupdocs-conversion-cloud-node-samples
global.conversion_cloud = require("groupdocs-conversion-cloud");

global.clientId = "XXXX-XXXX-XXXX-XXXX"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
global.clientSecret = "XXXXXXXXXXXXXXXX"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
global.convertApi = conversion_cloud.ConvertApi.fromKeys(clientId, clientSecret);

let file = fs.readFileSync('./Resources/WordProcessing/password-protected.docx');
let loadOptions = new conversion_cloud.DocxLoadOptions();
loadOptions.format = "docx";
loadOptions.password = "password";
let request = new conversion_cloud.ConvertDocumentDirectRequest("pdf", file, undefined, undefined, loadOptions);

let result = await convertApi.convertDocumentDirect(request);

console.log("Document converted: " + result.length);
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-conversion-cloud/groupdocs-conversion-cloud-python-samples
import groupdocs_conversion_cloud

client_id = "XXXX-XXXX-XXXX-XXXX" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "XXXXXXXXXXXXXXXX" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
# Create necessary API instances
apiInstance = groupdocs_conversion_cloud.ConvertApi.from_keys(client_id, client_secret)

# Prepare request
load_options = groupdocs_conversion_cloud.DocxLoadOptions()
load_options.format = "docx"
load_options.password = "password"
request = groupdocs_conversion_cloud.ConvertDocumentDirectRequest("pdf", "Resources\\WordProcessing\\password-protected.docx", None, None, load_options)

# Convert
result = apiInstance.convert_document_direct(request)

print("Document converted: " + result)
```

## Troubleshooting Tips

- File Not Found Errors: Ensure the local file path is correct and the file exists.
- Authentication Issues: Verify your Client ID and Client Secret are correct.
- Format Not Supported: Check if the conversion from your source format to the target format is supported.
- Memory Issues: For large files, ensure your application has enough memory allocated.
- Password Errors: For protected documents, double-check the password is correct.

## What You've Learned

In this tutorial, you've learned how to:
- Convert documents directly without using cloud storage
- Work with password-protected documents
- Apply specific conversion options
- Convert selected pages from a document
- Implement direct conversion in various programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Create a simple web application that accepts file uploads and returns converted documents
2. Build a conversion utility that supports batch direct conversion of multiple files
3. Implement a solution that compares the performance of storage-based vs. direct conversion

## Next Tutorial

Continue your learning journey with our next tutorial: [Tutorial: How to Get Document Metadata](/quick-start-guide/document-metadata/) to learn how to retrieve and analyze document metadata.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

We'd love to hear your feedback on this tutorial! Please feel free to ask questions or suggest improvements on our [support forum](https://forum.groupdocs.cloud/c/conversion/11).

