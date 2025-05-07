---
title: How to Get Document Information with GroupDocs.Merger Cloud API Tutorial
weight: 1
url: /getting-started/get-document-information/
description: Learn how to retrieve document metadata and page properties with GroupDocs.Merger Cloud API in this step-by-step tutorial for developers.
---

# Tutorial: How to Get Document Information with GroupDocs.Merger Cloud API

## Learning Objectives

In this tutorial, you'll learn how to use the GroupDocs.Merger Cloud API to retrieve essential information about documents stored in your cloud storage. This foundational skill will help you understand document structure before performing more complex operations.

## Prerequisites

Before starting this tutorial, ensure you have:

1. A GroupDocs.Merger Cloud account (or [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret from the [GroupDocs Cloud Dashboard](https://dashboard.groupdocs.cloud/)
3. At least one document uploaded to your cloud storage
4. Basic understanding of REST API concepts
5. Development environment for your preferred language (optional for cURL examples)

## Use Case Scenario

Imagine you're building a document management system that needs to display document properties to users before they perform merge operations. You need to extract information such as:

- Document file extension
- File size
- File format
- Page dimensions
- Page visibility status

This tutorial will show you how to retrieve this information programmatically.

## Step 1: Understanding the Document Information API

The GroupDocs.Merger Cloud API provides an endpoint that returns basic information about a document and its pages. This REST API accepts the document storage path as an input payload.

Here's what you can retrieve:
- Document properties: File extension, size in bytes, and file format
- Page properties: Width and height in pixels, page number, and visibility status

The API endpoint for this operation is:

```
HTTP POST ~/info
```

## Step 2: Preparing Your Request

To retrieve document information, you'll need to:

1. Obtain an access token using your Client ID and Client Secret
2. Make a POST request to the info endpoint with your document path

Let's break this down:

### Step 2.1: Get Your Access Token

First, you need to authenticate with the API to receive a JSON Web Token (JWT):

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

The response will contain an access token that you'll use in the next step.

### Step 2.2: Formulate Your Document Information Request

Now, construct a POST request to retrieve document information:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{ \"FilePath\": \"/wordprocessing/four-pages.docx\" }"
```

Replace `YOUR_ACCESS_TOKEN` with the token you received in Step 2.1, and adjust the `FilePath` to match your document's location in cloud storage.

## Step 3: Understanding the Response

When you execute the request, you'll receive a JSON response like this:

```json
{
  "extension": ".docx",
  "pages": [
    {
      "width": 612,
      "height": 792,
      "pageNumber": 1,
      "visible": true
    },
    {
      "width": 612,
      "height": 792,
      "pageNumber": 2,
      "visible": true
    },
    {
      "width": 612,
      "height": 792,
      "pageNumber": 3,
      "visible": true
    },
    {
      "width": 612,
      "height": 792,
      "pageNumber": 4,
      "visible": true
    }
  ],
  "size": 8651,
  "fileFormat": "Microsoft Word Open XML Document"
}
```

This response shows:
- The document is a `.docx` file (Microsoft Word Open XML Document)
- It has 4 pages, each 612×792 pixels when converted to an image
- All pages are visible (important for page manipulation operations)
- The file size is 8,651 bytes

## Step 4: Implementing with SDK in Your Application

While you can use direct REST API calls as shown above, GroupDocs provides SDKs for various programming languages to simplify integration. Let's see how to get document information using these SDKs.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new InfoApi(configuration);

var fileInfo = new FileInfo
{
    FilePath = "wordprocessing/four-pages.docx"
};

var request = new GetInfoRequest(fileInfo);

var response = apiInstance.GetInfo(request);

// Document information
Console.WriteLine("Document properties:");
Console.WriteLine($"File format: {response.FileFormat}");
Console.WriteLine($"Extension: {response.Extension}");
Console.WriteLine($"Size in bytes: {response.Size}");

// Page information
Console.WriteLine("\nPage information:");
foreach (var page in response.Pages)
{
    Console.WriteLine($"\nPage #{page.PageNumber}");
    Console.WriteLine($"Width: {page.Width}px");
    Console.WriteLine($"Height: {page.Height}px");
    Console.WriteLine($"Visible: {page.Visible}");
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-python-samples
import groupdocs_merger_cloud

client_id = "YOUR_CLIENT_ID" # Get your client ID and secret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get your client ID and secret from https://dashboard.groupdocs.cloud
  
# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(client_id, client_secret)
api = groupdocs_merger_cloud.InfoApi(configuration)

# Prepare request
file_info = groupdocs_merger_cloud.FileInfo()
file_info.file_path = "wordprocessing/four-pages.docx"

request = groupdocs_merger_cloud.GetInfoRequest(file_info)

# Retrieve document information
result = api.get_info(request)

# Document information
print("Document properties:")
print(f"File format: {result.file_format}")
print(f"Extension: {result.extension}")
print(f"Size in bytes: {result.size}")

# Page information
print("\nPage information:")
for page in result.pages:
    print(f"\nPage #{page.page_number}")
    print(f"Width: {page.width}px")
    print(f"Height: {page.height}px")
    print(f"Visible: {page.visible}")
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-java-samples
import com.groupdocs.cloud.merger.client.*;
import com.groupdocs.cloud.merger.model.*;
import com.groupdocs.cloud.merger.api.InfoApi;

public class GetDocumentInformationExample {
    public static void main(String[] args) {
        String clientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
        String clientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
        
        // Create instance of the API
        Configuration configuration = new Configuration(clientId, clientSecret);
        InfoApi api = new InfoApi(configuration);
        
        try {
            // Prepare request
            FileInfo fileInfo = new FileInfo();
            fileInfo.setFilePath("wordprocessing/four-pages.docx");
            
            GetInfoRequest request = new GetInfoRequest(fileInfo);
            
            // Get document information
            InfoResult result = api.getInfo(request);
            
            // Document information
            System.out.println("Document properties:");
            System.out.println("File format: " + result.getFileFormat());
            System.out.println("Extension: " + result.getExtension());
            System.out.println("Size in bytes: " + result.getSize());
            
            // Page information
            System.out.println("\nPage information:");
            for (PageInfo page : result.getPages()) {
                System.out.println("\nPage #" + page.getPageNumber());
                System.out.println("Width: " + page.getWidth() + "px");
                System.out.println("Height: " + page.getHeight() + "px");
                System.out.println("Visible: " + page.isVisible());
            }
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Try It Yourself: Practice Exercises

Now that you understand how to retrieve document information, try these exercises to reinforce your learning:

1. Exercise 1: Retrieve information for different file types (PDF, PPTX, etc.) and compare the responses. Notice how different document formats may have different page properties.

2. Exercise 2: Create a simple application that displays document thumbnails based on the page dimensions retrieved from the API.

3. Exercise 3: Use the page visibility property to identify and list only visible pages in a document with hidden pages.

## Troubleshooting Tips

- Authentication errors: Ensure your Client ID and Client Secret are correct and that your account has appropriate permissions.
- File not found errors: Verify that the file path in your request matches the actual location in your cloud storage.
- Invalid format errors: Confirm that the document format is supported by GroupDocs.Merger Cloud by using the "Get Supported File Formats" API.

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve comprehensive information about documents stored in the cloud
- Access detailed page properties for document manipulation
- Implement document information retrieval using different programming languages
- Interpret and utilize the API response to understand document structure

## Next Steps

Now that you can retrieve document information, a logical next step is to learn about the file formats supported by GroupDocs.Merger Cloud. Continue to our [Tutorial: Getting Supported File Formats](/getting-started/get-supported-file-formats/) to expand your knowledge.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback

Did you find this tutorial helpful? Do you have questions about retrieving document information? Let us know in our [support forum](https://forum.groupdocs.cloud/c/merger/18/).
