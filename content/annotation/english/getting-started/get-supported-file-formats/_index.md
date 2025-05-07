---
title: How to Get Supported File Formats with GroupDocs.Annotation Cloud API Tutorial
url: /getting-started/get-supported-file-formats/
weight: 1
description: Learn how to retrieve all supported file formats in GroupDocs.Annotation Cloud API in this step-by-step tutorial for developers
---

# Tutorial: How to Get Supported File Formats with GroupDocs.Annotation Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Make API calls to retrieve the list of file formats supported by GroupDocs.Annotation Cloud
- Interpret the format information returned by the API
- Implement this functionality in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Annotation Cloud account (if you don't have one, [get a free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your API keys (AppSID and AppKey) from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. Basic knowledge of REST APIs
4. A development environment set up with your preferred language

## Practical Scenario

Imagine you're developing a document management application that needs to validate file uploads. Before allowing users to upload files for annotation, you want to check if the file format is supported by the API. This tutorial shows you how to retrieve the list of supported file formats to implement this validation.

## Step-by-Step Guide

### 1. Understanding the API Endpoint

GroupDocs.Annotation Cloud API provides a dedicated endpoint to retrieve all supported file formats:

```
GET https://api.groupdocs.cloud/v2.0/annotation/formats
```

This REST API resource will return a comprehensive list of all file formats that can be annotated using the API.

### 2. Making the API Request with cURL

Let's try calling the API using cURL first to understand the request and response structure:

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/annotation/formats" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual JWT token obtained from your AppSID and AppKey.

### 3. Understanding the Response

The API returns a JSON response containing an array of supported file formats. Each format includes:
- `extension`: The file extension (e.g., ".docx")
- `fileFormat`: A description of the format (e.g., "Microsoft Word")

Here's a snippet of what the response looks like:

```json
{
  "formats": [
    {
      "extension": ".doc",
      "fileFormat": "Microsoft Word"
    },
    {
      "extension": ".docx",
      "fileFormat": "Microsoft Word"
    },
    ...
  ]
}
```

### 4. Implementing in Different Programming Languages

Let's implement this API call in various programming languages:

#### C# Implementation

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyAppSid, MyAppKey);
  
var apiInstance = new InfoApi(configuration);
var response = apiInstance.GetSupportedFileFormats();

// Process the formats
foreach (var format in response.Formats)
{
    Console.WriteLine($"Extension: {format.Extension}, Format: {format.FileFormat}");
}
```

#### Python Implementation

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
info_api = groupdocs_annotation_cloud.InfoApi.from_keys(app_sid, app_key)
 
result = info_api.get_supported_file_formats()

# Process the formats
for format_info in result.formats:
    print(f"Extension: {format_info.extension}, Format: {format_info.file_format}")
```

#### Node.js Implementation

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-node-samples
global.annotation_cloud = require("groupdocs-annotation-cloud");
 
global.appSid = "XXXX-XXXX-XXXX-XXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
global.appKey = "XXXXXXXXXXXXXXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
global.infoApi = annotation_cloud.InfoApi.fromKeys(appSid, appKey);
 
let response = await infoApi.getSupportedFileFormats();

// Process the formats
response.formats.forEach(format => {
    console.log(`Extension: ${format.extension}, Format: ${format.fileFormat}`);
});
```

## Try It Yourself

Now that you've seen the examples, it's time to try implementing this in your own application:

1. Get your API credentials from the dashboard
2. Choose your preferred programming language
3. Copy the appropriate code example above
4. Modify it with your credentials
5. Run the code and observe the results

## Troubleshooting Tips

- Authentication Error: Make sure your AppSID and AppKey are correctly set up and that you're generating the JWT token properly.
- Empty Response: If you receive an empty list, check that your account has permissions to access the API.
- Network Errors: Ensure your firewall isn't blocking API requests to the GroupDocs.Annotation Cloud endpoint.

## What You've Learned

In this tutorial, you have learned:
- How to retrieve a list of supported file formats from the GroupDocs.Annotation Cloud API
- How to interpret the format data returned by the API
- How to implement this functionality in C#, Python, and Node.js

## Further Practice

To solidify your understanding:
1. Try filtering the response to only show specific categories (e.g., only Microsoft Word formats)
2. Create a validation function that checks if a given file extension is supported
3. Implement a user-friendly UI component that shows supported formats to end-users

## Next Steps

Ready to learn more? Check out our next tutorial: [How to Get Document Information with GroupDocs.Annotation Cloud API](/getting-started/get-document-information/).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
