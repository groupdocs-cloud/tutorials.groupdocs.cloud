---
title: How to Retrieve Document Information with GroupDocs.Viewer Cloud API Tutorial
url: /basic-usage/get-document-information/
description: Learn how to retrieve comprehensive document information including format, page count, and properties using GroupDocs.Viewer Cloud API in this step-by-step tutorial
weight: 3
---

# Tutorial: How to Retrieve Document Information with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve basic information about documents using GroupDocs.Viewer Cloud API
- Extract document properties like format, page count, and page dimensions
- Get text coordinates for implementing text search and selection
- Access attachment information for documents with embedded files
- Utilize document information in your applications

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Viewer Cloud account (get your [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret
- Basic understanding of REST APIs
- Familiarity with your programming language of choice (C#, Java, Python, PHP, Ruby, Node.js, or Go)
- A document for testing (we'll use a sample DOCX in this tutorial)

## Why Document Information Matters

Retrieving document information is often a crucial first step in document processing workflows. Having accurate metadata allows you to:

- Plan rendering operations: Knowing page count and dimensions helps properly configure viewing options
- Implement pagination: Page information enables efficient navigation in multi-page documents
- Enable text search: Text coordinates make text selection and search possible
- Handle attachments: Awareness of embedded files allows for complete document processing
- Validate compatibility: File format detection ensures the document can be properly processed

GroupDocs.Viewer Cloud API provides a simple and efficient way to extract this valuable information before you begin the rendering process.

## Step 1: Upload Your Document to Cloud Storage

Before retrieving document information, you need to upload a document to GroupDocs.Viewer Cloud storage.

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Store JWT in a variable for reuse
JWT="YOUR_JWT_TOKEN"

# Upload file to storage
curl -v "https://api.groupdocs.cloud/v2.0/viewer/storage/file/SampleFiles/sample.docx" \
-X PUT \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-H "Authorization: Bearer $JWT" \
--data-binary "@/path/to/your/sample.docx"
```

## Step 2: Retrieve Document Information

Now, let's use the API to retrieve comprehensive information about the document:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer $JWT" \
-d "{
      'FileInfo': {
         'FilePath': 'SampleFiles/sample.docx'
      },
      'ViewFormat': 'HTML'
}"
```

### Understanding the Response

The API returns detailed information about the document:

```json
{
  "formatExtension": ".docx",
  "format": "Microsoft Word Open XML Document",
  "pages": [
    {
      "number": 1,
      "width": 595,
      "height": 841,
      "visible": true,
      "lines": []
    },
    {
      "number": 2,
      "width": 595,
      "height": 841,
      "visible": true,
      "lines": []
    },
    {
      "number": 3,
      "width": 595,
      "height": 841,
      "visible": true,
      "lines": []
    }
  ],
  "attachments": [],
  "archiveViewInfo": null,
  "cadViewInfo": null,
  "projectManagementViewInfo": null,
  "outlookViewInfo": null,
  "pdfViewInfo": null
}
```

Key information included in the response:
- Format: The document's format name and extension
- Pages: Details about each page, including number, dimensions, and visibility
- Attachments: List of any embedded files (empty for our example document)
- Specialized Info: Additional format-specific information for special formats (null for our example)

## Step 3: Retrieving Text Coordinates

If you need text coordinates for implementing text selection or search functionality, you can set the `ExtractText` parameter to `true`:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer $JWT" \
-d "{
      'FileInfo': {
         'FilePath': 'SampleFiles/sample.docx'
      },
      'ViewFormat': 'PNG',
      'RenderOptions': {
         'ExtractText': true
      }
}"
```

With text extraction enabled, the response will include detailed text information:

```json
{
  "formatExtension": ".docx",
  "format": "Microsoft Word Open XML Document",
  "pages": [
    {
      "number": 1,
      "width": 595,
      "height": 841,
      "visible": true,
      "lines": [
        {
          "words": [
            {
              "characters": [
                {
                  "x": 229.607,
                  "y": 67.8,
                  "width": 10.674,
                  "height": 19.8,
                  "value": "T"
                },
                // More characters...
              ],
              "x": 229.607,
              "y": 67.8,
              "width": 39.721,
              "height": 19.8,
              "value": "This"
            },
            // More words...
          ],
          "x": 229.607,
          "y": 67.8,
          "width": 136.387,
          "height": 19.8,
          "value": "This is a sample"
        },
        // More lines...
      ]
    },
    // More pages...
  ],
  "attachments": [],
  "archiveViewInfo": null,
  "cadViewInfo": null,
  "projectManagementViewInfo": null,
  "outlookViewInfo": null,
  "pdfViewInfo": null
}
```

This hierarchical structure provides coordinates for:
- Lines of text
- Words within each line
- Individual characters within each word

## Step 4: Working with Document Attachments

For documents that contain attachments (like emails or archives), the API will return information about these embedded files:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer $JWT" \
-d "{
      'FileInfo': {
         'FilePath': 'SampleFiles/with_attachments.msg'
      },
      'ViewFormat': 'HTML'
}"
```

For a document with attachments, the response will include attachment details:

```json
{
  "formatExtension": ".msg",
  "format": "Microsoft Outlook Message",
  "pages": [
    {
      "number": 1,
      "width": 612,
      "height": 792,
      "visible": true,
      "lines": []
    }
  ],
  "attachments": [
    {
      "name": "attachment-image.png",
      "filePath": null
    },
    {
      "name": "attachment-word.doc",
      "filePath": null
    }
  ],
  "archiveViewInfo": null,
  "cadViewInfo": null,
  "projectManagementViewInfo": null,
  "outlookViewInfo": {
    "folders": []
  },
  "pdfViewInfo": null
}
```

## Step 5: Implement in Your Application

Now let's implement document information retrieval in a real application using one of our supported SDKs.

### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;
using System.Collections.Generic;
using System.IO;

namespace GroupDocs.Viewer.Cloud.Tutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get your client ID and client secret from https://dashboard.groupdocs.cloud/
            string MyClientId = "YOUR_CLIENT_ID";
            string MyClientSecret = "YOUR_CLIENT_SECRET";

            // Create API instance
            var configuration = new Configuration(MyClientId, MyClientSecret);
            var apiInstance = new InfoApi(configuration);

            // Define view options
            var viewOptions = new ViewOptions
            {
                FileInfo = new FileInfo
                {
                    FilePath = "SampleFiles/sample.docx"
                },
                ViewFormat = ViewOptions.ViewFormatEnum.HTML
            };

            try
            {
                // Call the API to get document information
                var response = apiInstance.GetInfo(new GetInfoRequest(viewOptions));
                
                Console.WriteLine("Document information retrieved successfully!");
                
                // Display basic document information
                Console.WriteLine($"Format: {response.Format} ({response.FormatExtension})");
                Console.WriteLine($"Total pages: {response.Pages.Count}");
                
                // Display information about each page
                Console.WriteLine("\nPage information:");
                foreach (var page in response.Pages)
                {
                    Console.WriteLine($"  Page {page.Number}: {page.Width}x{page.Height} pixels, Visible: {page.Visible}");
                }
                
                // Display attachment information if any
                if (response.Attachments != null && response.Attachments.Count > 0)
                {
                    Console.WriteLine("\nAttachments:");
                    foreach (var attachment in response.Attachments)
                    {
                        Console.WriteLine($"  {attachment.Name}");
                    }
                }
                else
                {
                    Console.WriteLine("\nNo attachments found.");
                }
                
                // Display format-specific information if available
                if (response.PdfViewInfo != null)
                {
                    Console.WriteLine("\nPDF-specific information available.");
                }
                if (response.ArchiveViewInfo != null)
                {
                    Console.WriteLine("\nArchive-specific information available.");
                }
                if (response.CadViewInfo != null)
                {
                    Console.WriteLine("\nCAD-specific information available.");
                }
                if (response.ProjectManagementViewInfo != null)
                {
                    Console.WriteLine("\nProject Management-specific information available.");
                }
                if (response.OutlookViewInfo != null)
                {
                    Console.WriteLine("\nOutlook-specific information available.");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception while calling InfoApi: " + e.Message);
            }
            
            Console.WriteLine("\nPress any key to exit...");
            Console.ReadKey();
        }
    }
}
```

### Python Example

```python
# Import modules
import groupdocs_viewer_cloud

# Get your client ID and client secret from https://dashboard.groupdocs.cloud/
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create API instance
api_instance = groupdocs_viewer_cloud.InfoApi.from_keys(client_id, client_secret)

# Define view options
view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "SampleFiles/sample.docx"
view_options.view_format = "HTML"

try:
    # Call the API to get document information
    request = groupdocs_viewer_cloud.GetInfoRequest(view_options)
    response = api_instance.get_info(request)
    
    print("Document information retrieved successfully!")
    
    # Display basic document information
    print(f"Format: {response.format} ({response.format_extension})")
    print(f"Total pages: {len(response.pages)}")
    
    # Display information about each page
    print("\nPage information:")
    for page in response.pages:
        print(f"  Page {page.number}: {page.width}x{page.height} pixels, Visible: {page.visible}")
    
    # Display attachment information if any
    if response.attachments and len(response.attachments) > 0:
        print("\nAttachments:")
        for attachment in response.attachments:
            print(f"  {attachment.name}")
    else:
        print("\nNo attachments found.")
    
    # Display format-specific information if available
    if response.pdf_view_info:
        print("\nPDF-specific information available.")
    if response.archive_view_info:
        print("\nArchive-specific information available.")
    if response.cad_view_info:
        print("\nCAD-specific information available.")
    if response.project_management_view_info:
        print("\nProject Management-specific information available.")
    if response.outlook_view_info:
        print("\nOutlook-specific information available.")

except groupdocs_viewer_cloud.ApiException as e:
    print(f"Exception while calling InfoApi: {e}")
```

## Step 6: Extracting Text Coordinates for Selection

If you need to implement text selection functionality, you'll need detailed text coordinates. Let's modify our code to extract text data:

### C# Example with Text Extraction

```csharp
// Define view options with text extraction enabled
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PNG,
    RenderOptions = new ImageOptions
    {
        ExtractText = true
    }
};

try
{
    // Call the API to get document information with text coordinates
    var response = apiInstance.GetInfo(new GetInfoRequest(viewOptions));
    
    Console.WriteLine("Document information with text coordinates retrieved successfully!");
    
    // Process text information for the first page (as an example)
    if (response.Pages.Count > 0 && response.Pages[0].Lines != null && response.Pages[0].Lines.Count > 0)
    {
        Console.WriteLine("\nText content of the first page:");
        
        foreach (var line in response.Pages[0].Lines)
        {
            Console.WriteLine($"Line: \"{line.Value}\"");
            Console.WriteLine($"  Position: X={line.X}, Y={line.Y}, Width={line.Width}, Height={line.Height}");
            
            Console.WriteLine("  Words:");
            foreach (var word in line.Words)
            {
                Console.WriteLine($"    Word: \"{word.Value}\"");
                Console.WriteLine($"      Position: X={word.X}, Y={word.Y}, Width={word.Width}, Height={word.Height}");
                
                if (word.Characters != null && word.Characters.Count > 0)
                {
                    Console.WriteLine("      Characters:");
                    foreach (var character in word.Characters)
                    {
                        Console.WriteLine($"        Char: '{character.Value}' at X={character.X}, Y={character.Y}");
                    }
                }
            }
            Console.WriteLine();
        }
    }
    else
    {
        Console.WriteLine("No text data found in the document.");
    }
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling InfoApi: " + e.Message);
}
```

## Practical Applications

Let's explore some practical applications of document information retrieval:

### Creating a Document Preview

With document information, you can generate a preview that displays basic metadata:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document Preview</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .document-info {
            border: 1px solid #ddd;
            padding: 20px;
            border-radius: 5px;
            margin-bottom: 20px;
        }
        .document-info h2 {
            margin-top: 0;
            color: #333;
        }
        .info-row {
            display: flex;
            margin-bottom: 10px;
        }
        .info-label {
            width: 150px;
            font-weight: bold;
        }
        .pages-list {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 20px;
        }
        .page-thumbnail {
            border: 1px solid #ddd;
            padding: 5px;
            width: 100px;
            height: 150px;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #f9f9f9;
        }
        .attachments-list {
            margin-top: 20px;
        }
        .attachment-item {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
        }
        .attachment-icon {
            margin-right: 10px;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <div class="document-info">
        <h2>Document Information</h2>
        
        <div class="info-row">
            <div class="info-label">Format:</div>
            <div>Microsoft Word Open XML Document (.docx)</div>
        </div>
        
        <div class="info-row">
            <div class="info-label">Total Pages:</div>
            <div>3</div>
        </div>
        
        <div class="info-row">
            <div class="info-label">Page Dimensions:</div>
            <div>595x841 pixels</div>
        </div>
    </div>
    
    <h3>Page Thumbnails</h3>
    <div class="pages-list">
        <div class="page-thumbnail">Page 1</div>
        <div class="page-thumbnail">Page 2</div>
        <div class="page-thumbnail">Page 3</div>
    </div>
    
    <h3>Attachments</h3>
    <div class="attachments-list">
        <div class="attachment-item">
            <div class="attachment-icon">📄</div>
            <div>No attachments found</div>
        </div>
    </div>
</body>
</html>
```

### Planning a Document Viewer UI

You can use document information to intelligently configure your document viewer:

```javascript
// Example pseudo-code for a document viewer application
function configureDocumentViewer(documentInfo) {
    // Set the total number of pages for pagination
    viewer.setTotalPages(documentInfo.pages.length);
    
    // Set the initial zoom based on the page dimensions
    if (documentInfo.pages.length > 0) {
        const firstPage = documentInfo.pages[0];
        const aspectRatio = firstPage.width / firstPage.height;
        viewer.setOptimalZoom(aspectRatio);
    }
    
    // Configure attachments panel if needed
    if (documentInfo.attachments && documentInfo.attachments.length > 0) {
        viewer.showAttachmentsPanel();
        viewer.setAttachments(documentInfo.attachments);
    } else {
        viewer.hideAttachmentsPanel();
    }
    
    // Configure text selection if text coordinates are available
    const hasTextData = documentInfo.pages.some(page => 
        page.lines && page.lines.length > 0
    );
    
    if (hasTextData) {
        viewer.enableTextSelection(documentInfo.pages);
    } else {
        viewer.disableTextSelection();
    }
    
    // Configure special features based on document type
    if (documentInfo.pdfViewInfo) {
        viewer.enablePdfFeatures();
    }
    if (documentInfo.cadViewInfo) {
        viewer.enableCadFeatures();
    }
}
```

## Try It Yourself

Now that you've learned how to retrieve document information using GroupDocs.Viewer Cloud API, try it with different document types:

### Exercise 1: Compare Document Types

1. Retrieve information for different document types (PDF, DOCX, XLSX, PPTX)
2. Compare the structure and properties returned for each format
3. Note any format-specific information returned

### Exercise 2: Implement Text Search

1. Retrieve document information with text coordinates enabled
2. Create a simple search function that highlights matching text based on the coordinates
3. Test with different search terms

## Troubleshooting Tips

- Authentication Issues: Ensure your Client ID and Client Secret are correct and that you're generating a fresh JWT token
- File Not Found: Verify that the file path in your request matches the actual path in cloud storage
- Text Extraction: Text extraction is only available for certain document formats and requires setting the correct view format (PNG or JPG)
- Large Documents: For very large documents, consider retrieving information for specific pages rather than the entire document

## What You've Learned

In this tutorial, you've learned:
- How to retrieve comprehensive document information using GroupDocs.Viewer Cloud API
- How to extract text with coordinates for implementing text selection
- How to access information about document attachments
- How to use document information to plan and configure document viewing experiences
- How to implement these features in your applications using SDKs

## Next Steps

Ready to explore more document viewing capabilities? Check out these related tutorials:
- [HTML Viewer Basic Tutorial](/basic-usage/html-viewer/)
- [Image Viewer Tutorial](/basic-usage/image-viewer/)
- [PDF Viewer Tutorial](/basic-usage/pdf-viewer/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Have questions about retrieving document information? Need help implementing it in your application? We welcome your feedback and questions on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).