---
title: "Get Document Information with GroupDocs.Signature Cloud"
id: "get-document-information-tutorial"
url: /beginner-guide/document-info/
productName: "GroupDocs.Signature Cloud"
weight: 2
description: "Learn to retrieve essential document metadata and properties using GroupDocs.Signature Cloud API in this step-by-step tutorial for developers."
keywords: "signature cloud tutorial, document information API, get document metadata, file properties tutorial, learn GroupDocs API"
toc: True
---

# Tutorial: How to Get Document Information with GroupDocs.Signature Cloud

## Learning Objectives

In this tutorial, you'll learn:
- How to retrieve detailed information about documents before applying signatures
- How to extract metadata, page count, and document dimensions
- How to incorporate document information into your signature workflows

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Signature Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
- Your document files uploaded to GroupDocs cloud storage or accessible via URL
- Basic REST API understanding
- Development environment set up for your chosen SDK

## Why Document Information Matters

Before applying digital signatures to a document, understanding its structure is crucial. The document information API helps you:

- Determine how many pages are available for applying signatures
- Identify document dimensions to properly position signature elements
- Verify document format and properties before processing
- Plan signature placement based on page layout

## Step 1: Setting Up Authentication

All GroupDocs.Signature Cloud API calls require authentication using your client credentials.

1. Retrieve your Client ID and Client Secret from [GroupDocs Dashboard](https://dashboard.groupdocs.cloud/#/apps)
2. Generate an access token using these credentials

Here's how:

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

## Step 2: Preparing Your Document

Before retrieving document information, ensure your document is available either:
- Uploaded to your GroupDocs Cloud Storage, or
- Accessible via a public URL

For this tutorial, we'll use a document already uploaded to cloud storage.

## Step 3: Making the API Request

To get document information, you'll make a POST request to the info endpoint:

### cURL Example

```bash
curl -X POST "https://api.groupdocs.cloud/v2.0/signature/info" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{  \"FileInfo\": {    \"FilePath\": \"documents/sample.docx\",    \"StorageName\": \"MyStorage\",    \"VersionId\": \"\",    \"Password\": \"\"  }}"
```

### Request Parameters Explained

- **FilePath**: Path to your document in cloud storage
- **StorageName**: Name of the storage where your document is located
- **VersionId**: Optional parameter for versioned documents
- **Password**: Optional parameter for password-protected documents

### Try It Yourself

Replace the placeholder values with your actual document path and access token, then run the command.

## Step 4: Understanding the Response

The API returns a JSON object containing detailed document information:

```json
{
  "fileName": "sample.docx",
  "extension": ".docx",
  "fileFormat": "Microsoft Words",
  "size": 324608,
  "dateModified": "2023-05-15T12:57:58.9646989Z",
  "pages": {
    "totalCount": 5,
    "entries": [
      {
        "number": 1,
        "name": null,
        "width": 816,
        "height": 1056,
        "angle": 0,
        "visible": true,
        "rows": null
      },
      // Additional pages...
    ]
  }
}
```

Key information includes:
- **fileName**: The document's name
- **extension**: File extension
- **fileFormat**: Human-readable format description
- **size**: File size in bytes
- **dateModified**: Last modification date
- **pages**: Array of page information including dimensions and count

## Step 5: Implementing in Your Application

Now let's implement this functionality using various SDKs:

### C# Example

```csharp
// Get document information in C#
// Tutorial Code Example
using GroupDocs.Signature.Cloud.Sdk.Api;
using GroupDocs.Signature.Cloud.Sdk.Client;
using GroupDocs.Signature.Cloud.Sdk.Model;

string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new InfoApi(configuration);

// Document information request
var fileInfo = new FileInfo
{
    FilePath = "documents/sample.docx",
    StorageName = "MyStorage",
    // Password = "document_password" // Uncomment if document is password-protected
};

var request = new GetInfoRequest(fileInfo);
var response = apiInstance.GetInfo(request);

// Display document information
Console.WriteLine($"File Name: {response.FileName}");
Console.WriteLine($"File Format: {response.FileFormat}");
Console.WriteLine($"Page Count: {response.Pages.TotalCount}");

// Process page dimensions for signature placement planning
foreach (var page in response.Pages.Entries)
{
    Console.WriteLine($"Page {page.Number}: Width = {page.Width}, Height = {page.Height}");
    
    // Calculate potential signature zones (example)
    var bottomRightZone = new
    {
        X = page.Width - 200,  // 200 pixels from right edge
        Y = page.Height - 100, // 100 pixels from bottom edge
        Width = 180,
        Height = 80
    };
    
    Console.WriteLine($"  Recommended signature zone: X={bottomRightZone.X}, Y={bottomRightZone.Y}, Width={bottomRightZone.Width}, Height={bottomRightZone.Height}");
}
```

### Java Example

```java
// Get document information in Java
// Tutorial Code Example
import com.groupdocs.signature.cloud.api.InfoApi;
import com.groupdocs.signature.cloud.client.Configuration;
import com.groupdocs.signature.cloud.model.FileInfo;
import com.groupdocs.signature.cloud.model.InfoResult;
import com.groupdocs.signature.cloud.model.PageInfo;

String MyClientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
String MyClientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
InfoApi apiInstance = new InfoApi(configuration);

// Document information request
FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("documents/sample.docx");
fileInfo.setStorageName("MyStorage");
// fileInfo.setPassword("document_password"); // Uncomment if document is password-protected

InfoResult response = apiInstance.getInfo(fileInfo);

// Display document information
System.out.println("File Name: " + response.getFileName());
System.out.println("File Format: " + response.getFileFormat());
System.out.println("Page Count: " + response.getPages().getTotalCount());

// Process page dimensions for signature placement planning
for (PageInfo page : response.getPages().getEntries()) {
    System.out.println("Page " + page.getNumber() + ": Width = " + page.getWidth() + ", Height = " + page.getHeight());
    
    // Calculate potential signature zones (example)
    int x = page.getWidth() - 200;  // 200 pixels from right edge
    int y = page.getHeight() - 100; // 100 pixels from bottom edge
    int width = 180;
    int height = 80;
    
    System.out.println("  Recommended signature zone: X=" + x + ", Y=" + y + ", Width=" + width + ", Height=" + height);
}
```

### Python Example

```python
# Get document information in Python
# Tutorial Code Example
from groupdocs_signature_cloud import *
import groupdocs_signature_cloud

client_id = "YOUR_CLIENT_ID"  # Get from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET"  # Get from https://dashboard.groupdocs.cloud

api = groupdocs_signature_cloud.InfoApi.from_keys(client_id, client_secret)

# Document information request
file_info = groupdocs_signature_cloud.FileInfo()
file_info.file_path = "documents/sample.docx"
file_info.storage_name = "MyStorage"
# file_info.password = "document_password"  # Uncomment if document is password-protected

request = groupdocs_signature_cloud.GetInfoRequest(file_info)
response = api.get_info(request)

# Display document information
print(f"File Name: {response.file_name}")
print(f"File Format: {response.file_format}")
print(f"Page Count: {response.pages.total_count}")

# Process page dimensions for signature placement planning
for page in response.pages.entries:
    print(f"Page {page.number}: Width = {page.width}, Height = {page.height}")
    
    # Calculate potential signature zones (example)
    x = page.width - 200  # 200 pixels from right edge
    y = page.height - 100  # 100 pixels from bottom edge
    width = 180
    height = 80
    
    print(f"  Recommended signature zone: X={x}, Y={y}, Width={width}, Height={height}")
```

## Step 6: Using Document Information for Signature Placement

Now that you have document information, let's use it for signature placement planning:

```javascript
// Example signature positioning function
function calculateSignaturePosition(pageInfo, signatureType) {
    let position = {x: 0, y: 0, width: 0, height: 0};
    
    switch(signatureType) {
        case 'digital':
            // Bottom right corner for digital signatures
            position.width = Math.min(200, pageInfo.width * 0.3);
            position.height = Math.min(80, pageInfo.height * 0.1);
            position.x = pageInfo.width - position.width - 20;
            position.y = pageInfo.height - position.height - 20;
            break;
            
        case 'barcode':
            // Top right for barcodes
            position.width = Math.min(150, pageInfo.width * 0.2);
            position.height = Math.min(150, pageInfo.height * 0.15);
            position.x = pageInfo.width - position.width - 20;
            position.y = 20;
            break;
            
        case 'qrcode':
            // Bottom left for QR codes
            position.width = Math.min(150, pageInfo.width * 0.2);
            position.height = position.width; // Square
            position.x = 20;
            position.y = pageInfo.height - position.height - 20;
            break;
            
        default:
            // Default center position
            position.width = Math.min(200, pageInfo.width * 0.3);
            position.height = Math.min(80, pageInfo.height * 0.1);
            position.x = (pageInfo.width - position.width) / 2;
            position.y = (pageInfo.height - position.height) / 2;
    }
    
    return position;
}
```

## Troubleshooting Tips

Common issues you might encounter:

- **"File not found" error**: Double-check your file path and storage name
- **Permission errors**: Ensure your access token has the right permissions
- **Format errors**: If getting incorrect page information, verify the file is not corrupted
- **Password issues**: For protected documents, ensure you're providing the correct password

## What You've Learned

In this tutorial, you've learned:
- How to retrieve detailed document information using GroupDocs.Signature Cloud API
- How to extract page dimensions, counts, and document metadata
- How to use document information for intelligent signature placement
- How to implement document information retrieval in different programming languages

## Further Practice

To reinforce your learning:
1. Create a function that calculates optimal signature positions based on document type
2. Build a document analyzer that recommends different signature types based on document structure
3. Implement document validation that checks if the document meets requirements for signing

## Additional Resources

- [Product Page](https://products.groupdocs.cloud/signature/)
- [Documentation](https://docs.groupdocs.cloud/signature/)
- [Live Demo](https://products.groupdocs.app/signature/family)
- [API Reference](https://reference.groupdocs.cloud/signature/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.signature-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/signature/13/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
