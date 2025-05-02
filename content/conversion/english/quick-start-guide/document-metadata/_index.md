---
title: How to Get Document Metadata with GroupDocs.Conversion Cloud Tutorial
weight: 4
description: Learn how to retrieve and analyze document metadata using GroupDocs.Conversion Cloud API in this step-by-step tutorial for developers.
url: /quick-start-guide/document-metadata/
---

# Tutorial: How to Get Document Metadata with GroupDocs.Conversion Cloud

## Learning Objectives

In this tutorial, you'll learn:
- What document metadata is and why it's important
- How to retrieve metadata from documents stored in the cloud
- How to interpret and use metadata information
- How to apply this knowledge in real-world scenarios

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Conversion Cloud account (get a [free trial](https://dashboard.groupdocs.cloud/#/apps) if needed)
- Your Client ID and Client Secret credentials
- Basic understanding of REST APIs
- Familiarity with your preferred programming language (C#, Java, PHP, etc.)
- Completed our [Basic Document Conversion tutorial](/quick-start-guide/convert-document/) (recommended)

## Why Access Document Metadata?

Document metadata provides valuable information about a file, including:
- Author and creation information
- Page count and dimensions
- Security settings (password protection)
- File size and format details
- Timestamps and version information

This information is essential for:
- Building document management systems
- Pre-conversion validation
- File cataloging and searching
- Document filtering and organization
- Security assessment

## Tutorial Steps

### Step 1: Understanding Document Metadata

Document metadata is information about a document that isn't part of the content itself. Different document types have various metadata properties, but common ones include:

- File type/format
- Number of pages
- Document dimensions
- Creation and modification dates
- Author information
- File size
- Security settings

The GroupDocs.Conversion Cloud API allows you to retrieve this information easily from documents stored in your cloud storage.

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
var infoApi = new InfoApi(configuration);
```

### Step 3: Retrieve Document Metadata

Now, let's retrieve metadata from a document in cloud storage:

#### Using cURL

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Get document metadata
curl -X GET "https://api.groupdocs.cloud/v2.0/conversion/info?FilePath=words/four-pages.docx" \
-H "accept: application/json" \
-H "authorization: Bearer [Access Token]"
```

#### Using C# SDK

```csharp
try
{
    // Create document metadata request
    var request = new GetDocumentMetadataRequest
    {
        FilePath = "WordProcessing/four-pages.docx",
        StorageName = "MyStorage" // Optional, default storage is used if not specified
    };

    // Execute request
    var metadata = infoApi.GetDocumentMetadata(request);

    // Display metadata information
    Console.WriteLine($"File Type: {metadata.FileType}");
    Console.WriteLine($"Page Count: {metadata.PageCount}");
    Console.WriteLine($"File Size: {metadata.Size} bytes");
    Console.WriteLine($"Created Date: {metadata.CreatedDate}");
    Console.WriteLine($"Modified Date: {metadata.ModifiedDate}");
    Console.WriteLine($"Is Password Protected: {metadata.IsPasswordProtected}");
    
    // Display additional information if available
    if (metadata.Width > 0 && metadata.Height > 0)
    {
        Console.WriteLine($"Dimensions: {metadata.Width} x {metadata.Height}");
    }
    
    if (!string.IsNullOrEmpty(metadata.Author))
    {
        Console.WriteLine($"Author: {metadata.Author}");
    }
    
    if (!string.IsNullOrEmpty(metadata.Title))
    {
        Console.WriteLine($"Title: {metadata.Title}");
    }
}
catch (Exception e)
{
    Console.WriteLine("Error retrieving document metadata: " + e.Message);
}
```

### Step 4: Working with Password-Protected Documents

If you need to retrieve metadata from a password-protected document, you'll need to provide the password:

#### Using C# SDK

```csharp
try
{
    // Create document metadata request with password
    var request = new GetDocumentMetadataRequest
    {
        FilePath = "WordProcessing/password-protected.docx",
        StorageName = "MyStorage",
        Password = "your-document-password" // Add the document password
    };

    // Execute request
    var metadata = infoApi.GetDocumentMetadata(request);

    // Display metadata
    Console.WriteLine($"Successfully accessed password-protected document");
    Console.WriteLine($"File Type: {metadata.FileType}");
    Console.WriteLine($"Page Count: {metadata.PageCount}");
    // ... other metadata properties
}
catch (Exception e)
{
    Console.WriteLine("Error retrieving document metadata: " + e.Message);
}
```

### Step 5: Practical Applications

Now let's look at some practical applications of document metadata:

#### Checking if Conversion is Needed

```csharp
/// <summary>
/// Determines if a document needs to be converted based on its type
/// </summary>
bool NeedsConversion(string filePath, string targetFormat, string storageName = null)
{
    try
    {
        // Get metadata
        var request = new GetDocumentMetadataRequest
        {
            FilePath = filePath,
            StorageName = storageName
        };
        
        var metadata = infoApi.GetDocumentMetadata(request);
        
        // Check if already in target format
        return !metadata.FileType.Equals(targetFormat, StringComparison.OrdinalIgnoreCase);
    }
    catch (Exception e)
    {
        Console.WriteLine("Error checking document: " + e.Message);
        throw;
    }
}

// Usage example
if (NeedsConversion("documents/sample.docx", "docx"))
{
    Console.WriteLine("Document already in DOCX format, no conversion needed.");
}
else
{
    Console.WriteLine("Document needs conversion.");
    // Perform conversion
}
```

#### Pre-conversion Document Validation

```csharp
/// <summary>
/// Validates a document before conversion
/// </summary>
bool ValidateDocumentForConversion(string filePath, string storageName = null)
{
    try
    {
        // Get metadata
        var request = new GetDocumentMetadataRequest
        {
            FilePath = filePath,
            StorageName = storageName
        };
        
        var metadata = infoApi.GetDocumentMetadata(request);
        
        // Perform validation checks
        bool isValid = true;
        
        // Check if file is too large (e.g., over 10MB)
        if (metadata.Size > 10 * 1024 * 1024)
        {
            Console.WriteLine("Warning: File is very large, conversion may take longer.");
            // isValid = false; // Uncomment to fail validation for large files
        }
        
        // Check if file has too many pages (e.g., over 100 pages)
        if (metadata.PageCount > 100)
        {
            Console.WriteLine("Warning: Document has many pages, conversion may take longer.");
            // isValid = false; // Uncomment to fail validation for many pages
        }
        
        // Check if file is password protected
        if (metadata.IsPasswordProtected)
        {
            Console.WriteLine("Error: File is password protected. Please provide a password.");
            isValid = false;
        }
        
        return isValid;
    }
    catch (Exception e)
    {
        Console.WriteLine("Error validating document: " + e.Message);
        return false;
    }
}

// Usage example
if (ValidateDocumentForConversion("documents/large-document.pdf"))
{
    Console.WriteLine("Document passed validation, proceeding with conversion.");
    // Proceed with conversion
}
else
{
    Console.WriteLine("Document failed validation, conversion aborted.");
}
```

### Step 6: Complete Implementation Example

Here's a complete example that demonstrates retrieving and using document metadata in C#:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-conversion-cloud/groupdocs-conversion-cloud-dotnet-samples
using System;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace DocumentMetadataTutorial
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
            var infoApi = new InfoApi(configuration);
            var storageApi = new StorageApi(configuration);
            var convertApi = new ConvertApi(configuration);

            try
            {
                // Define document path in cloud storage
                string filePath = "WordProcessing/four-pages.docx";
                
                Console.WriteLine("Retrieving document metadata...");
                
                // Create metadata request
                var request = new GetDocumentMetadataRequest
                {
                    FilePath = filePath,
                    StorageName = "MyStorage" // Optional, default storage is used if not specified
                };
                
                // Execute request
                var metadata = infoApi.GetDocumentMetadata(request);
                
                // Display metadata information
                Console.WriteLine("\nDocument Metadata:");
                Console.WriteLine($"---------------------------");
                Console.WriteLine($"File Type: {metadata.FileType}");
                Console.WriteLine($"Page Count: {metadata.PageCount}");
                Console.WriteLine($"File Size: {metadata.Size} bytes");
                Console.WriteLine($"Created Date: {metadata.CreatedDate}");
                Console.WriteLine($"Modified Date: {metadata.ModifiedDate}");
                Console.WriteLine($"Is Password Protected: {metadata.IsPasswordProtected}");
                
                // Display additional information if available
                if (metadata.Width > 0 && metadata.Height > 0)
                {
                    Console.WriteLine($"Dimensions: {metadata.Width} x {metadata.Height}");
                }
                
                if (!string.IsNullOrEmpty(metadata.Author))
                {
                    Console.WriteLine($"Author: {metadata.Author}");
                }
                
                if (!string.IsNullOrEmpty(metadata.Title))
                {
                    Console.WriteLine($"Title: {metadata.Title}");
                }
                
                // Demonstrate practical use of metadata
                Console.WriteLine("\nPractical application example:");
                
                // Check if PDF conversion is needed based on file type
                if (metadata.FileType.Equals("pdf", StringComparison.OrdinalIgnoreCase))
                {
                    Console.WriteLine("Document is already in PDF format, no conversion needed.");
                }
                else
                {
                    Console.WriteLine("Document is not in PDF format, conversion would be required.");
                    
                    // Check if document meets conversion requirements
                    if (metadata.IsPasswordProtected)
                    {
                        Console.WriteLine("Warning: Document is password protected. Password required for conversion.");
                    }
                    
                    if (metadata.PageCount > 50)
                    {
                        Console.WriteLine("Note: Document has many pages, conversion may take longer.");
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();
        }
    }
}
```

## SDK Examples for Other Languages

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-conversion-cloud/groupdocs-conversion-cloud-java-samples
InfoApi apiInstance = new InfoApi(Utils.AppSID, Utils.AppKey);
try {
    GetDocumentMetadataRequest request = new GetDocumentMetadataRequest();
    request.setStorageName(Utils.MYStorage);
    request.setFilePath("conversions/sample.docx");
    
    DocumentMetadata response = apiInstance.getDocumentMetadata(request);

    System.out.println("File Type: " + response.getFileType());
    System.out.println("Page Count: " + response.getPageCount());
    System.out.println("Size: " + response.getSize() + " bytes");
    System.out.println("Created Date: " + response.getCreatedDate());
} catch (ApiException e) {
    System.err.println("Exception while calling InfoApi:");
    e.printStackTrace();
}
```

### PHP Example

```php
<?php
include(dirname(__DIR__) . '\CommonUtils.php');

try {
    $infoApi = CommonUtils::GetInfoApiInstance();

    $request = new GroupDocs\Conversion\Model\Requests\GetDocumentMetadataRequest();
    $request->setStorageName(CommonUtils::$MyStorage);
    $request->setFilePath("conversions/sample.docx");
        
    $metadata = $infoApi->getDocumentMetadata($request);

    echo "File Type: " . $metadata->getFileType() . "<br/>";
    echo "Page Count: " . $metadata->getPageCount() . "<br/>";
    echo "Size: " . $metadata->getSize() . " bytes<br/>";
    echo "Is Password Protected: " . ($metadata->getIsPasswordProtected() ? "Yes" : "No") . "<br/>";
} catch (Exception $e) {
    echo "Something went wrong: " . $e->getMessage();
}
?>
```

### Python Example

```python
# Import modules
import groupdocs_conversion_cloud
from Common_Utilities.Utils import Common_Utilities

# Create instance of the API
api = Common_Utilities.Get_InfoApi_Instance()

try:
    request = groupdocs_conversion_cloud.GetDocumentMetadataRequest()
    request.storage_name = Common_Utilities.myStorage
    request.file_path = "conversions/sample.docx"
    
    metadata = api.get_document_metadata(request)

    print("Document Information:")
    print(f"File Type: {metadata.file_type}")
    print(f"Page Count: {metadata.page_count}")
    print(f"Size: {metadata.size} bytes")
    print(f"Created Date: {metadata.created_date}")
    print(f"Is Password Protected: {metadata.is_password_protected}")
    
except groupdocs_conversion_cloud.ApiException as e:
    print("Exception while calling API: {0}".format(e.message))
```

## Troubleshooting Tips

- File Not Found Errors: Ensure the file path in cloud storage is correct and the file exists.
- Authentication Issues: Verify your Client ID and Client Secret are correct.
- Permission Errors: Make sure your account has access to the specified storage.
- Password Required: If you get an "invalid password" error, the document is password-protected. Provide the correct password in your request.

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve metadata from documents stored in GroupDocs cloud storage
- Access key document properties like file type, page count, and security settings
- Handle password-protected documents
- Apply metadata information in practical scenarios like pre-conversion validation

## Further Practice

To reinforce your learning, try these exercises:
1. Create a document inventory tool that scans a folder in cloud storage and reports metadata for all documents
2. Build a simple web interface that displays document metadata for uploaded files
3. Implement a document filter that categorizes files by type, size, or page count based on metadata

## Next Steps

Congratulations! You've completed our Quick Start Guide tutorial series for GroupDocs.Conversion Cloud API. You now have the skills to:
- Check supported document formats
- Convert documents using cloud storage
- Perform direct document conversions
- Retrieve and utilize document metadata

You're now ready to implement these features in your own applications!

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

We'd love to hear your feedback on this tutorial series! Please feel free to ask questions or suggest improvements on our [support forum](https://forum.groupdocs.cloud/c/conversion/11).
