---
title: How to Get Document Information with GroupDocs.Annotation Cloud API Tutorial
weight: 2
url: /getting-started/get-document-information/
description: Learn how to extract document metadata and page information using GroupDocs.Annotation Cloud API in this developer tutorial
---

# Tutorial: How to Get Document Information with GroupDocs.Annotation Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve detailed information about a document using the GroupDocs.Annotation Cloud API
- Extract metadata such as file format, size, and modification date
- Get page dimensions for accurate annotation placement
- Implement this functionality in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Annotation Cloud account (if you don't have one, [get a free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your API keys (AppSID and AppKey) from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your storage (or you can use the sample documents provided by the API)
4. Basic knowledge of REST APIs
5. A development environment set up with your preferred language

## Practical Scenario

When building a document annotation application, you often need to know details about the documents before allowing users to add annotations. For example, you need to know the page dimensions to correctly position annotations, or you might want to show document metadata to users. This tutorial shows you how to retrieve this critical information.

## Step-by-Step Guide

### 1. Understanding the API Endpoint

GroupDocs.Annotation Cloud API provides a dedicated endpoint to retrieve document information:

```
POST https://api.groupdocs.cloud/v2.0/annotation/info
```

This endpoint requires you to specify the document path in the request body.

### 2. Making the API Request with cURL

Let's try calling the API using cURL to understand the request and response structure:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{ 'FilePath': 'annotationdocs/one-page.docx'}"
```

Replace `YOUR_ACCESS_TOKEN` with your actual JWT token obtained from your AppSID and AppKey.

### 3. Understanding the Response

The API returns a JSON response containing comprehensive information about the document, including:
- Basic file properties (name, path, extension, format)
- File size and modification date
- Page information including dimensions (width and height)

Here's what a typical response looks like:

```json
{
  "name": "one-page.docx",
  "path": "annotationdocs/one-page.docx",
  "extension": ".docx",
  "fileFormat": "Microsoft Word Open XML format",
  "size": 16646,
  "dateModified": "2021-02-05T05:17:10Z",
  "pages": [
    {
      "number": 1,
      "width": 595,
      "height": 841
    }
  ]
}
```

### 4. Why Page Dimensions Matter

The page dimensions (width and height) are particularly important for annotation. They allow you to:
- Calculate relative positions for annotations
- Ensure annotations appear in the correct location across different devices
- Scale annotations properly when the document is rendered at different sizes

### 5. Implementing in Different Programming Languages

Let's implement this API call in various programming languages:

#### C# Implementation

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyAppSid, MyAppKey);
  
// Create API instance
var apiInstance = new InfoApi(configuration);

// Create request object
var request = new GetInfoRequest
{
    FilePath = "annotationdocs/one-page.docx"
};

// Call API method
var response = apiInstance.GetInfo(request);

// Process the document information
Console.WriteLine($"File: {response.Name}");
Console.WriteLine($"Format: {response.FileFormat}");
Console.WriteLine($"Size: {response.Size} bytes");
Console.WriteLine($"Modified: {response.DateModified}");

// Process page information
foreach (var page in response.Pages)
{
    Console.WriteLine($"Page {page.Number}: Width={page.Width}, Height={page.Height}");
}
```

#### Python Implementation

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
# Create API instance
info_api = groupdocs_annotation_cloud.InfoApi.from_keys(app_sid, app_key)

# Create request object
request = groupdocs_annotation_cloud.GetInfoRequest("annotationdocs/one-page.docx")
 
# Call API method
result = info_api.get_info(request)

# Process the document information
print(f"File: {result.name}")
print(f"Format: {result.file_format}")
print(f"Size: {result.size} bytes")
print(f"Modified: {result.date_modified}")

# Process page information
for page in result.pages:
    print(f"Page {page.number}: Width={page.width}, Height={page.height}")
```

#### Node.js Implementation

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-node-samples
global.annotation_cloud = require("groupdocs-annotation-cloud");
 
global.appSid = "XXXX-XXXX-XXXX-XXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
global.appKey = "XXXXXXXXXXXXXXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
// Create API instance
global.infoApi = annotation_cloud.InfoApi.fromKeys(appSid, appKey);

// Prepare request object
let request = new annotation_cloud.GetInfoRequest();
request.filePath = "annotationdocs/one-page.docx";
 
// Call API method
let result = await infoApi.getInfo(request);

// Process the document information
console.log(`File: ${result.name}`);
console.log(`Format: ${result.fileFormat}`);
console.log(`Size: ${result.size} bytes`);
console.log(`Modified: ${result.dateModified}`);

// Process page information
result.pages.forEach(page => {
    console.log(`Page ${page.number}: Width=${page.width}, Height=${page.height}`);
});
```

## Try It Yourself

Now that you've seen the examples, it's time to try implementing this in your own application:

1. Get your API credentials from the dashboard
2. Upload a document to your storage or use a sample document
3. Choose your preferred programming language
4. Copy the appropriate code example above
5. Modify it with your credentials and document path
6. Run the code and observe the results

## Troubleshooting Tips

- File Not Found Error: Make sure the document path is correct and the file exists in your storage.
- Authentication Error: Verify your AppSID and AppKey are correctly set up.
- Unsupported Format: If you get an error related to format, check if the file format is supported by using the [Get Supported File Formats](/getting-started/get-supported-file-formats/) API.
- Large Files: If you're working with large files, you might need to increase timeout values in your API client.

## What You've Learned

In this tutorial, you have learned:
- How to retrieve document information using the GroupDocs.Annotation Cloud API
- The importance of page dimensions for annotation positioning
- How to extract metadata such as file size and format
- How to implement this functionality in C#, Python, and Node.js

## Further Practice

To solidify your understanding:
1. Try retrieving information for different file types (PDF, images, Excel files)
2. Create a function that validates if a document is suitable for annotation based on its properties
3. Build a simple UI that displays document metadata and previews page dimensions

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
