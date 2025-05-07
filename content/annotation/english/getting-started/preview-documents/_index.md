---
title: How to Preview Documents with GroupDocs.Annotation Cloud API Tutorial
weight: 2
url: /getting-started/preview-documents/
description: Learn how to generate document previews and thumbnails using GroupDocs.Annotation Cloud API in this step-by-step developer tutorial
---

# Tutorial: How to Preview Documents with GroupDocs.Annotation Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Generate document previews using the GroupDocs.Annotation Cloud API
- Render specific pages or page ranges from documents
- Create thumbnails for document preview
- Configure preview options like image quality and format
- Implement document preview in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Annotation Cloud account (if you don't have one, [get a free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your API keys (AppSID and AppKey) from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your storage
4. Basic knowledge of REST APIs
5. A development environment set up with your preferred language

## Practical Scenario

In a document management system, users typically want to preview documents before opening or annotating them. This preview functionality helps users quickly identify the document they're looking for and see any existing annotations without opening the full document. For example, you might want to:
- Display document thumbnails in a file browser
- Show a preview of the first page in a document list
- Allow users to navigate through document pages before opening
- Generate preview images for presentations or reports

This tutorial shows you how to implement these preview features in your applications.

## Step-by-Step Guide

### 1. Understanding the API Endpoint

GroupDocs.Annotation Cloud API provides an endpoint to generate document previews:

```
POST https://api.groupdocs.cloud/v2.0/annotation/preview
```

This endpoint requires you to specify the document path and preview options in the request body.

### 2. Generating a Preview for a Specific Page

Let's start with a basic example - generating a preview for a specific page of a document:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/preview" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/sample.docx'
  },
  'PageNumber': 1,
  'Format': 'PNG',
  'OutputPath': 'output/preview-page-1.png'
}"
```

This request will:
- Take the document at `annotationdocs/sample.docx`
- Generate a PNG preview of page 1
- Save the result to `output/preview-page-1.png`

### 3. Generating Previews for Multiple Pages

You can also generate previews for multiple pages at once:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/preview" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/sample.docx'
  },
  'PageNumbers': [1, 2, 3],
  'Format': 'JPEG',
  'OutputPath': 'output/preview-page-{0}.jpg'
}"
```

This request will generate JPEG previews for pages 1, 2, and 3, saving them to separate files with the page number in the filename.

### 4. Customizing Preview Options

You can customize the preview by setting various options:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/preview" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/sample.docx'
  },
  'PageNumber': 1,
  'Format': 'JPEG',
  'Width': 800,
  'Height': 1000,
  'Resolution': 96,
  'RenderComments': true,
  'OutputPath': 'output/custom-preview.jpg'
}"
```

This request adds options like width, height, resolution, and whether to render comments in the preview.

### 5. Implementing in C#

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyAppSid, MyAppKey);
  
// Create PreviewApi instance
var apiInstance = new PreviewApi(configuration);

// Create file info object
var fileInfo = new FileInfo
{
    FilePath = "annotationdocs/sample.docx"
};

// Generate preview for a specific page
Console.WriteLine("Generating preview for page 1...");
var previewRequest = new PreviewRequest(fileInfo)
{
    PageNumber = 1,
    Format = "PNG",
    OutputPath = "output/preview-page-1.png"
};

var previewResult = apiInstance.Preview(previewRequest);
Console.WriteLine($"Preview generated successfully. Saved to {previewResult.Path}");

// Generate previews for multiple pages
Console.WriteLine("\nGenerating previews for pages 1, 2, and 3...");
var multiPagePreviewRequest = new PreviewRequest(fileInfo)
{
    PageNumbers = new List<int> { 1, 2, 3 },
    Format = "JPEG",
    OutputPath = "output/preview-page-{0}.jpg"
};

var multiPagePreviewResult = apiInstance.Preview(multiPagePreviewRequest);
Console.WriteLine($"Multiple page previews generated successfully.");
foreach (var path in multiPagePreviewResult.Paths)
{
    Console.WriteLine($"Preview saved to: {path}");
}

// Generate a customized preview
Console.WriteLine("\nGenerating a customized preview...");
var customPreviewRequest = new PreviewRequest(fileInfo)
{
    PageNumber = 1,
    Format = "JPEG",
    Width = 800,
    Height = 1000,
    Resolution = 96,
    RenderComments = true,
    OutputPath = "output/custom-preview.jpg"
};

var customPreviewResult = apiInstance.Preview(customPreviewRequest);
Console.WriteLine($"Customized preview generated successfully. Saved to {customPreviewResult.Path}");
```

### 6. Implementing in Python

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
# Create API instance
preview_api = groupdocs_annotation_cloud.PreviewApi.from_keys(app_sid, app_key)

# Create file info object
file_info = groupdocs_annotation_cloud.FileInfo()
file_info.file_path = "annotationdocs/sample.docx"

# Generate preview for a specific page
print("Generating preview for page 1...")
preview_request = groupdocs_annotation_cloud.PreviewRequest(file_info)
preview_request.page_number = 1
preview_request.format = "PNG"
preview_request.output_path = "output/preview-page-1.png"

preview_result = preview_api.preview(preview_request)
print(f"Preview generated successfully. Saved to {preview_result.path}")

# Generate previews for multiple pages
print("\nGenerating previews for pages 1, 2, and 3...")
multi_page_preview_request = groupdocs_annotation_cloud.PreviewRequest(file_info)
multi_page_preview_request.page_numbers = [1, 2, 3]
multi_page_preview_request.format = "JPEG"
multi_page_preview_request.output_path = "output/preview-page-{0}.jpg"

multi_page_preview_result = preview_api.preview(multi_page_preview_request)
print("Multiple page previews generated successfully.")
for path in multi_page_preview_result.paths:
    print(f"Preview saved to: {path}")

# Generate a customized preview
print("\nGenerating a customized preview...")
custom_preview_request = groupdocs_annotation_cloud.PreviewRequest(file_info)
custom_preview_request.page_number = 1
custom_preview_request.format = "JPEG"
custom_preview_request.width = 800
custom_preview_request.height = 1000
custom_preview_request.resolution = 96
custom_preview_request.render_comments = True
custom_preview_request.output_path = "output/custom-preview.jpg"

custom_preview_result = preview_api.preview(custom_preview_request)
print(f"Customized preview generated successfully. Saved to {custom_preview_result.path}")
```

### 7. Implementing in Node.js

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-node-samples
const { PreviewApi, FileInfo } = require("groupdocs-annotation-cloud");

// Set up authentication credentials
const appSid = "XXXX-XXXX-XXXX-XXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
const appKey = "XXXXXXXXXXXXXXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud

// Initialize API instance
const previewApi = new PreviewApi.fromKeys(appSid, appKey);

// Create file info object
const fileInfo = new FileInfo();
fileInfo.filePath = "annotationdocs/sample.docx";

// Generate preview for a specific page
console.log("Generating preview for page 1...");
const previewRequest = {
    fileInfo: fileInfo,
    pageNumber: 1,
    format: "PNG",
    outputPath: "output/preview-page-1.png"
};

previewApi.preview(previewRequest)
    .then((previewResult) => {
        console.log(`Preview generated successfully. Saved to ${previewResult.path}`);
        
        // Generate previews for multiple pages
        console.log("\nGenerating previews for pages 1, 2, and 3...");
        const multiPagePreviewRequest = {
            fileInfo: fileInfo,
            pageNumbers: [1, 2, 3],
            format: "JPEG",
            outputPath: "output/preview-page-{0}.jpg"
        };
        
        return previewApi.preview(multiPagePreviewRequest);
    })
    .then((multiPagePreviewResult) => {
        console.log("Multiple page previews generated successfully.");
        multiPagePreviewResult.paths.forEach(path => {
            console.log(`Preview saved to: ${path}`);
        });
        
        // Generate a customized preview
        console.log("\nGenerating a customized preview...");
        const customPreviewRequest = {
            fileInfo: fileInfo,
            pageNumber: 1,
            format: "JPEG",
            width: 800,
            height: 1000,
            resolution: 96,
            renderComments: true,
            outputPath: "output/custom-preview.jpg"
        };
        
        return previewApi.preview(customPreviewRequest);
    })
    .then((customPreviewResult) => {
        console.log(`Customized preview generated successfully. Saved to ${customPreviewResult.path}`);
    })
    .catch((error) => {
        console.error("Error:", error.message);
    });
```

## Creating Thumbnails

Thumbnails are smaller versions of document previews, typically used in document lists or galleries. You can generate thumbnails by setting smaller dimensions in the preview request:

```csharp
// Generate thumbnails for all pages
var thumbnailRequest = new PreviewRequest(fileInfo)
{
    Format = "JPEG",
    Width = 150,
    Height = 200,
    OutputPath = "output/thumbnail-{0}.jpg"
};

var thumbnailResult = apiInstance.Preview(thumbnailRequest);
Console.WriteLine($"Thumbnails generated successfully.");
foreach (var path in thumbnailResult.Paths)
{
    Console.WriteLine($"Thumbnail saved to: {path}");
}
```

## Try It Yourself

Now that you've seen the examples, it's time to try implementing document preview in your own application:

1. Get your API credentials from the dashboard
2. Upload a document to your storage or use a sample document
3. Choose your preferred programming language
4. Decide which preview options you want to use
5. Copy the appropriate code example above
6. Modify it with your credentials, document path, and preview options
7. Run the code and verify the generated previews

## Troubleshooting Tips

- Unsupported Format: Make sure the preview format (PNG, JPEG, etc.) is supported by the API.
- Authentication Errors: Verify your AppSID and AppKey are correctly set up.
- File Not Found Errors: Ensure the document path is correct and the file exists in your storage.
- Output Path Issues: Make sure the output folder exists in your storage or use a different path.
- Page Number Issues: If you're getting errors about page numbers, make sure you're not requesting pages that don't exist in the document.
- Image Quality Issues: If the preview quality is too low, try increasing the resolution or quality settings.

## What You've Learned

In this tutorial, you have learned:
- How to generate document previews using the GroupDocs.Annotation Cloud API
- How to render specific pages or page ranges from documents
- How to create thumbnails for document preview
- How to configure preview options like image quality and format
- How to implement document preview in C#, Python, and Node.js

## Further Practice

To solidify your understanding:
1. Try generating previews for different document types (PDF, Word, Excel, etc.)
2. Experiment with different image formats and quality settings
3. Create a function that generates thumbnails for all pages in a document
4. Build a simple UI that displays document previews and allows navigation between pages

## Next Steps

Ready to learn more? Check out our other tutorials on document operations with GroupDocs.Annotation Cloud API.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
