---
title: How to Generate Document Page Previews Tutorial
url: /document-management/generate-document-pages-preview/
weight: 45
description: Learn how to create image previews of document pages using GroupDocs.Merger Cloud API in this comprehensive developer tutorial.
---

# Tutorial: How to Generate Document Page Previews

## Introduction

In this tutorial, you'll learn how to generate image previews (thumbnails) of document pages using GroupDocs.Merger Cloud API. Page preview generation is a valuable feature for document management systems, allowing users to quickly visualize documents without opening them in their native applications. This functionality is particularly useful for creating document galleries, previewing search results, or implementing document viewers in web and mobile applications.

GroupDocs.Merger Cloud provides a flexible API for generating high-quality image previews of various document formats, with control over page selection, image dimensions, and output format.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Generate image previews of document pages
- Select specific pages or page ranges for preview generation
- Configure preview image dimensions and format
- Implement preview generation in different programming languages
- Handle password-protected documents during preview operations

## Prerequisites

Before starting this tutorial, make sure you have:
- A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic knowledge of RESTful APIs
- Sample documents to generate previews for (PDF, DOCX, etc.)

## Use Case Scenario

Let's consider a practical scenario: You're developing a document management system that needs to display thumbnails of documents in its web interface. Users upload various document types (PDF, Word, Excel, etc.), and your application needs to generate preview images of these documents to display in the UI, allowing users to quickly identify and locate their files visually.

## Understanding Preview Generation

GroupDocs.Merger Cloud allows you to generate previews with several configuration options:

1. Page Selection: Generate previews for specific pages or page ranges
2. Filtering: Apply filters to include only even or odd pages
3. Image Dimensions: Control the width and height of the generated preview images
4. Image Format: Choose from formats like JPG, PNG, or BMP for the output images

## Implementation Steps

### Step 1: Authenticate with the API

First, authenticate with the GroupDocs.Merger Cloud API by obtaining a JWT token using your Client ID and Client Secret.

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

### Step 2: Prepare Your Preview Generation Request

To generate page previews, create a JSON request specifying:
- The document to generate previews from
- Pages to preview (specific pages or a range)
- Optional dimensions and format for the preview images
- Output path pattern for the generated images
- Password (if the document is protected)

Here's a basic request structure for generating previews of specific pages:

```json
{
    "FileInfo": { 
        "FilePath": "WordProcessing/four-pages.docx" 
    },
    "OutputPath": "/Output/preview-page",
    "Pages": [1, 3],
    "Format": "Png"
}
```

### Step 3: Generate Previews Using the API

Now let's execute the preview generation operation using the API:

#### cURL Example

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/preview" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'WordProcessing/four-pages.docx' },
    'OutputPath': '/Output/preview-page',
    'Pages': [1, 3],
    'Format': 'Png'
 }"
```

The API will respond with:

```json
{
  "documents": [
    {
      "path": "\\Output\\preview-page_1.png"
    },
    {
      "path": "\\Output\\preview-page_3.png"
    }
  ]
}
```

### Step 4: Retrieve and Use the Preview Images

After successful preview generation, you can download or use the preview images from the output paths provided in the response.

## Try It Yourself

Let's implement preview generation in different programming languages. First, make sure your document is uploaded to your GroupDocs Cloud storage, then use the appropriate SDK for your language.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new DocumentApi(configuration);

try
{
    // Configure preview options
    var options = new PreviewOptions
    {
        FileInfo = new FileInfo
        {
            FilePath = "WordProcessing/four-pages.docx"
        },
        OutputPath = "Output/preview-page",
        Pages = new List<int> { 1, 3 },
        Format = PreviewOptions.FormatEnum.Png,
        Width = 800,   // Optional: specify width in pixels
        Height = 600   // Optional: specify height in pixels
    };

    // Execute preview operation
    var request = new PreviewRequest(options);
    var response = apiInstance.Preview(request);

    Console.WriteLine("Number of preview images: " + response.Documents.Count);
    foreach (var doc in response.Documents)
    {
        Console.WriteLine("Preview image path: " + doc.Path);
    }
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling API: " + e.Message);
}
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-java-samples
String MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
DocumentApi apiInstance = new DocumentApi(configuration);

try {
    // Configure file info
    FileInfo fileInfo = new FileInfo();   
    fileInfo.setFilePath("WordProcessing/four-pages.docx");
    
    // Configure preview options
    PreviewOptions options = new PreviewOptions();
    options.setFileInfo(fileInfo);
    options.setPages(Arrays.asList(1, 3));
    options.setFormat(FormatEnum.PNG);
    options.setWidth(800);  // Optional: specify width in pixels
    options.setHeight(600); // Optional: specify height in pixels
    options.setOutputPath("Output/preview-page");

    // Execute preview operation
    PreviewRequest request = new PreviewRequest(options);
    PreviewResult response = apiInstance.preview(request);

    System.out.println("Number of preview images: " + response.getDocuments().size());
    for(DocumentResult doc : response.getDocuments()) {
        System.out.println("Preview image path: " + doc.getPath());
    }
} catch (ApiException e) {
    System.err.println("Exception while calling API:");
    e.printStackTrace();
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-merger_cloud-cloud/groupdocs-merger_cloud-cloud-python-samples
from groupdocs_merger_cloud import *
import groupdocs_merger_cloud

client_id = "YOUR_CLIENT_ID" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

# Create instance of the API
document_api = groupdocs_merger_cloud.DocumentApi.from_keys(client_id, client_secret)

# Configure file info
file_info = groupdocs_merger_cloud.FileInfo("WordProcessing/four-pages.docx")

# Configure preview options
options = groupdocs_merger_cloud.PreviewOptions()
options.file_info = file_info
options.pages = [1, 3]
options.format = "Png"  # Use "Png", "Jpg", or "Bmp"
options.width = 800     # Optional: specify width in pixels
options.height = 600    # Optional: specify height in pixels
options.output_path = "Output/preview-page"

# Execute preview operation
result = document_api.preview(groupdocs_merger_cloud.PreviewRequest(options))

print(f"Number of preview images: {len(result.documents)}")
for doc in result.documents:
    print(f"Preview image path: {doc.path}")
```

## Advanced Preview Generation Options

Let's explore some advanced options for generating document page previews:

### Generating Previews of a Page Range

Instead of specifying individual pages, you can generate previews for a range of pages using the `StartPageNumber` and `EndPageNumber` properties:

```json
{
    "FileInfo": { 
        "FilePath": "WordProcessing/ten-pages.docx" 
    },
    "StartPageNumber": 2,
    "EndPageNumber": 5,
    "OutputPath": "/Output/preview-range"
}
```

This will generate previews for pages 2 through 5 of the document.

### Filtering Pages with RangeMode

You can filter pages within a range to include only even or odd pages using the `RangeMode` property:

```json
{
    "FileInfo": { 
        "FilePath": "WordProcessing/ten-pages.docx" 
    },
    "StartPageNumber": 1,
    "EndPageNumber": 10,
    "RangeMode": "Odd",
    "OutputPath": "/Output/preview-odd-pages"
}
```

This will generate previews for odd-numbered pages (1, 3, 5, 7, 9) within the specified range.

### Customizing Image Dimensions

You can control the dimensions of the generated preview images with the `Width` and `Height` properties:

```json
{
    "FileInfo": { 
        "FilePath": "WordProcessing/four-pages.docx" 
    },
    "Pages": [1, 2],
    "Width": 1024,
    "Height": 768,
    "OutputPath": "/Output/preview-custom-size"
}
```

The dimensions are specified in pixels. If you specify only one dimension (width or height), the API will maintain the aspect ratio of the document when generating the preview.

### Choosing Different Output Formats

You can generate preview images in different formats using the `Format` property:

```json
{
    "FileInfo": { 
        "FilePath": "WordProcessing/four-pages.docx" 
    },
    "Pages": [1],
    "Format": "Jpg",  // Options: "Jpg", "Png", "Bmp"
    "OutputPath": "/Output/preview-jpg"
}
```

The default format is JPG if not specified.

## Supporting Password-Protected Documents

For password-protected documents, include the password in the `FileInfo` object:

```json
{
    "FileInfo": { 
        "FilePath": "Pdf/protected-document.pdf",
        "Password": "your-password"
    },
    "Pages": [1, 2, 3],
    "OutputPath": "/Output/preview-protected"
}
```

## Supported Document Formats

GroupDocs.Merger Cloud can generate previews for a wide range of document formats, including:
- PDF documents
- Word documents (DOCX, DOC, RTF, etc.)
- Excel spreadsheets (XLSX, XLS, etc.)
- PowerPoint presentations (PPTX, PPT, etc.)
- Images (JPG, PNG, TIFF, etc.)
- And many more

## Common Issues and Troubleshooting

1. Authentication Error: If you receive a 401 Unauthorized error, make sure your Client ID and Client Secret are correct and that your JWT token hasn't expired.

2. File Not Found: Verify that the file path is correct and that the document exists in your cloud storage.

3. Password Protection: For protected documents, ensure you're providing the correct password in the request.

4. Page Number Out of Range: If you specify page numbers that don't exist in the document, you'll receive an error. Always ensure that page numbers are within the valid range of your document.

5. Output File Size: Very large preview images (high resolution) may take longer to generate and result in larger file sizes. Choose appropriate dimensions based on your use case.

## Practical Applications

Here are some common applications for document preview generation:

1. Document Management Systems: Display thumbnails of documents in file browsers or document libraries
2. Content Preview: Show previews of documents in search results or file listings
3. Mobile Applications: Generate lightweight previews for mobile document viewers
4. Email Attachments: Create previews of email attachments without downloading the full documents
5. Document Comparison: Generate previews of different document versions for visual comparison

## What You've Learned

In this tutorial, you've learned how to:
- Generate image previews of document pages
- Select specific pages or page ranges for preview generation
- Apply filters to generate previews of only odd or even pages
- Customize the dimensions and format of preview images
- Implement preview generation in different programming languages
- Handle password-protected documents

## Further Practice

To reinforce your learning, try these exercises:
1. Generate previews of different document formats (PDF, DOCX, XLSX, etc.)
2. Create a thumbnail gallery by generating previews of all pages in a document
3. Implement a document viewer that displays preview images with navigation controls
4. Compare the quality and file size of previews in different formats (JPG, PNG, BMP)

## Next Steps

Ready to explore more document operations? Check out these related tutorials:
- [Tutorial: How to Join Multiple Documents](/document-management/join-multiple-documents/)
- [Tutorial: How to Split a Document](/document-management/split-document/)
- [Tutorial: How to Import Attachments into PDF Documents](/document-management/import-attachment/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [Live Demo](https://products.groupdocs.app/merger/family)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.merger-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to share your feedback or ask questions on our [support forum](https://forum.groupdocs.cloud/c/merger/18/).
    "