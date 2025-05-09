---
title: How to Get Document Information with GroupDocs.Comparison Cloud Tutorial
weight: 2
description: Learn how to extract document metadata and properties from files before comparison using GroupDocs.Comparison Cloud API in this step-by-step tutorial
url: /getting-started/get-document-information/
---

# Tutorial: How to Get Document Information with GroupDocs.Comparison Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve essential information about documents before comparison
- Extract document properties like format, extension, size, and page count
- Implement document info extraction in different programming languages
- Use this information to prepare for document comparison

## Prerequisites

Before starting this tutorial, you'll need:
- A GroupDocs.Comparison Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- A document uploaded to your GroupDocs Cloud Storage
- Basic familiarity with RESTful APIs and your chosen programming language

## Understanding Document Information

Before comparing documents, it's often necessary to gather information about them. GroupDocs.Comparison Cloud API provides a dedicated endpoint to retrieve important document metadata, including:

- Document file format
- File extension
- File size
- Number of pages

This information is crucial for making informed decisions about document comparison settings.

## Step 1: Upload a Document (If Needed)

Before retrieving document information, make sure you have uploaded a document to your cloud storage. For detailed instructions on file uploads, refer to the [File API documentation](https://docs.groupdocs.cloud/comparison/developer-guide/working-with-file-api/).

## Step 2: Understanding the API Resource

To get document information, we'll use the following GroupDocs.Comparison Cloud REST API resource:

`POST https://api.groupdocs.cloud/v2.0/comparison/info`

This endpoint requires a JSON body with the document's file path.

## Step 3: Implementing with cURL

Let's start with a cURL example to understand the request and response format:

```bash
# First, get the JWT token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Use the token to get document information
curl -v "https://api.groupdocs.cloud/v2.0/comparison/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FilePath': 'source_files/word/source.docx'
}"
```

Replace `YOUR_CLIENT_ID`, `YOUR_CLIENT_SECRET`, and `YOUR_JWT_TOKEN` with your actual credentials.

The response will be a JSON object containing document metadata:

```json
{
  "format": "Microsoft Word Document",
  "extension": ".docx",
  "size": 23059,
  "pageCount": 1
}
```

## Step 4: Implementing with SDKs

Now let's implement the same functionality using various SDKs.

### C# Example

```csharp
using System;
using GroupDocs.Comparison.Cloud.Sdk.Api;
using GroupDocs.Comparison.Cloud.Sdk.Client;
using GroupDocs.Comparison.Cloud.Sdk.Model;
using GroupDocs.Comparison.Cloud.Sdk.Model.Requests;

namespace GetDocumentInfoExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API credentials
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Create InfoApi instance
            var infoApi = new InfoApi(configuration);
            
            // Prepare request
            var fileInfo = new FileInfo
            {
                FilePath = "source_files/word/source.docx"
            };
            var request = new GetDocumentInfoRequest(fileInfo);
            
            try
            {
                // Get document information
                var response = infoApi.GetDocumentInfo(request);
                
                // Display document information
                Console.WriteLine($"Document format: {response.Format}");
                Console.WriteLine($"Extension: {response.Extension}");
                Console.WriteLine($"Size: {response.Size} bytes");
                Console.WriteLine($"Page count: {response.PageCount}");
            }
            catch (Exception e)
            {
                Console.WriteLine($"Error: {e.Message}");
            }
        }
    }
}
```

### Python Example

```python
import groupdocs_comparison_cloud
from groupdocs_comparison_cloud import GetDocumentInfoRequest

# Configure API credentials
configuration = groupdocs_comparison_cloud.Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create InfoApi instance
info_api = groupdocs_comparison_cloud.InfoApi.from_keys(
    configuration.client_id, configuration.client_secret
)

# Prepare request
file_info = groupdocs_comparison_cloud.FileInfo()
file_info.file_path = "source_files/word/source.docx"
request = GetDocumentInfoRequest(file_info)

try:
    # Get document information
    response = info_api.get_document_info(request)
    
    # Display document information
    print(f"Document format: {response.format}")
    print(f"Extension: {response.extension}")
    print(f"Size: {response.size} bytes")
    print(f"Page count: {response.page_count}")
except groupdocs_comparison_cloud.ApiException as e:
    print(f"Error: {e}")
```

### Java Example

```java
import com.groupdocs.cloud.comparison.client.*;
import com.groupdocs.cloud.comparison.model.*;
import com.groupdocs.cloud.comparison.api.InfoApi;
import com.groupdocs.cloud.comparison.model.requests.GetDocumentInfoRequest;

public class GetDocumentInfoExample {
    public static void main(String[] args) {
        // Configure API credentials
        Configuration configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        // Create InfoApi instance
        InfoApi infoApi = new InfoApi(configuration);
        
        // Prepare request
        FileInfo fileInfo = new FileInfo();
        fileInfo.setFilePath("source_files/word/source.docx");
        GetDocumentInfoRequest request = new GetDocumentInfoRequest(fileInfo);
        
        try {
            // Get document information
            InfoResult response = infoApi.getDocumentInfo(request);
            
            // Display document information
            System.out.println("Document format: " + response.getFormat());
            System.out.println("Extension: " + response.getExtension());
            System.out.println("Size: " + response.getSize() + " bytes");
            System.out.println("Page count: " + response.getPageCount());
        } catch (ApiException e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Node.js Example

```javascript
// Load the GroupDocs.Comparison Cloud SDK
const comparison = require("groupdocs-comparison-cloud");

// Configure API credentials
const clientId = "YOUR_CLIENT_ID";
const clientSecret = "YOUR_CLIENT_SECRET";

// Create InfoApi instance
const infoApi = comparison.InfoApi.fromKeys(clientId, clientSecret);

// Prepare request
const fileInfo = new comparison.FileInfo();
fileInfo.filePath = "source_files/word/source.docx";

// Get document information
infoApi.getDocumentInfo(new comparison.GetDocumentInfoRequest(fileInfo))
    .then(response => {
        console.log("Document format:", response.format);
        console.log("Extension:", response.extension);
        console.log("Size:", response.size, "bytes");
        console.log("Page count:", response.pageCount);
    })
    .catch(error => {
        console.log("Error:", error.message);
    });
```

## Step 5: Practical Application

Now that you can retrieve document information, here are some practical uses for this functionality:

1. Validating Documents: Check if a document has the expected properties before comparison
2. User Interface Updates: Show document information to users before they compare documents
3. Progress Estimation: Use page count to estimate comparison time
4. Storage Management: Track document sizes to manage cloud storage usage
5. Format Compatibility Check: Ensure documents being compared are compatible formats

## Try It Yourself

Now it's your turn to practice:

1. Upload different types of documents to your cloud storage
2. Retrieve information for each document using the code examples
3. Create a function that validates documents based on their properties
4. Build a simple interface that displays document information to users

## Troubleshooting Tips

- 404 Not Found: Ensure the document exists at the specified path
- 401 Unauthorized: Verify your Client ID and Client Secret
- File Format Issues: Confirm the document is in a supported format
- Empty Response: Check that the document is not corrupted

## What You've Learned

In this tutorial, you've learned:
- How to retrieve essential document information before comparison
- How to extract document properties like format, extension, size, and page count
- How to implement document information extraction in different programming languages
- How to use this information to prepare for document comparison

## Next Steps

Now that you can retrieve document information, you're ready to move on to actual document comparison. Continue to the next tutorial:
- [Tutorial: How to Compare Documents](/getting-started/compare-documents/)

This will teach you how to compare two documents and identify differences between them.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

