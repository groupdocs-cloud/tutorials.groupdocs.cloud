---
title: How to Use the PDF Viewer with GroupDocs.Viewer Cloud API Tutorial
description: Learn how to render documents to PDF format with this comprehensive tutorial for GroupDocs.Viewer Cloud API
url: /basic-usage/pdf-viewer/
---

# Tutorial: How to Use the PDF Viewer with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Render various document formats to PDF using GroupDocs.Viewer Cloud API
- Configure PDF rendering options for optimal output
- Implement PDF viewing functionality in your applications
- Work with the rendered PDF documents

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Viewer Cloud account (get your [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret
- Basic understanding of REST APIs
- Familiarity with your programming language of choice (C#, Java, Python, PHP, Ruby, Node.js, or Go)
- A document you want to convert to PDF (we'll use a sample DOCX in this tutorial)

## PDF Rendering Overview

PDF (Portable Document Format) is one of the most widely used document formats for sharing and viewing documents. Converting documents to PDF offers several advantages:

- Consistency: PDFs maintain their layout and formatting across different devices and platforms
- Security: PDFs can be protected with passwords and permissions
- Compression: PDFs can be optimized for size without significant quality loss
- Universal Support: PDF readers are available on virtually all platforms

GroupDocs.Viewer Cloud API simplifies the process of converting various document formats to PDF, providing a consistent viewing experience for your users.

## Step 1: Upload Your Document to Cloud Storage

Before rendering a document, you need to upload it to GroupDocs.Viewer Cloud storage.

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

## Step 2: Render Document to PDF

Now that your document is in cloud storage, you can render it to PDF format.

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer $JWT" \
-d "{
   'FileInfo': {
         'FilePath': 'SampleFiles/sample.docx'
      },
   'ViewFormat': 'PDF'
}"
```

The API will process your document and return information about the rendered PDF file.

### Understanding the Response

```json
{
  "pages": null,
  "attachments": [],
  "file": {
    "path": "viewer/sample_docx/sample.pdf",
    "downloadUrl": "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample.pdf"
  }
}
```

The response contains:
- Information about the rendered PDF file, including its path and download URL
- Unlike HTML or image formats which return individual pages, PDF rendering returns a single file containing all pages

## Step 3: Download the PDF File

To access the rendered PDF, you can download it using the provided URL:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample.pdf" \
-X GET \
-H "Authorization: Bearer $JWT" \
--output "rendered_document.pdf"
```

## Step 4: Implement in Your Application

Now let's implement this process in a real application using one of our supported SDKs.

### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;
using System.Diagnostics;
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
            var apiInstance = new ViewApi(configuration);

            // Define rendering options
            var viewOptions = new ViewOptions
            {
                FileInfo = new FileInfo
                {
                    FilePath = "SampleFiles/sample.docx"
                },
                ViewFormat = ViewOptions.ViewFormatEnum.PDF
            };

            try
            {
                // Call the API to render the document
                var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
                
                Console.WriteLine("Document rendered to PDF successfully!");
                
                // Print information about the rendered PDF
                Console.WriteLine($"PDF file path: {response.File.Path}");
                Console.WriteLine($"Download URL: {response.File.DownloadUrl}");
                
                // Download the rendered PDF (optional)
                using (var webClient = new System.Net.WebClient())
                {
                    webClient.Headers.Add("Authorization", "Bearer " + configuration.GetToken());
                    webClient.DownloadFile(response.File.DownloadUrl, "rendered_document.pdf");
                    Console.WriteLine("PDF downloaded as rendered_document.pdf");
                }
                
                // Open the PDF in default viewer
                Process.Start(new ProcessStartInfo
                {
                    FileName = "rendered_document.pdf",
                    UseShellExecute = true
                });
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception while calling ViewApi: " + e.Message);
            }
            
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

### Python Example

```python
# Import modules
import os
import webbrowser
import requests
import groupdocs_viewer_cloud

# Get your client ID and client secret from https://dashboard.groupdocs.cloud/
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create API instance
api_instance = groupdocs_viewer_cloud.ViewApi.from_keys(client_id, client_secret)

# Define rendering options
view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "SampleFiles/sample.docx"
view_options.view_format = "PDF"

try:
    # Call the API to render the document
    request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
    response = api_instance.create_view(request)
    
    print("Document rendered to PDF successfully!")
    
    # Print information about the rendered PDF
    print(f"PDF file path: {response.file.path}")
    print(f"Download URL: {response.file.download_url}")
    
    # Download the rendered PDF (optional)
    pdf_response = requests.get(
        response.file.download_url,
        headers={"Authorization": f"Bearer {api_instance.configuration.access_token}"}
    )
    
    if pdf_response.status_code == 200:
        with open("rendered_document.pdf", "wb") as file:
            file.write(pdf_response.content)
            print("PDF downloaded as rendered_document.pdf")
    
    # Open the PDF in default viewer
    pdf_path = os.path.abspath("rendered_document.pdf")
    print(f"Opening {pdf_path} in default PDF viewer")
    webbrowser.open(f'file://{pdf_path}')

except groupdocs_viewer_cloud.ApiException as e:
    print(f"Exception while calling ViewApi: {e}")
```

## Try It Yourself

Now that you've seen how to render documents to PDF using GroupDocs.Viewer Cloud API, try it with your own documents. Experiment with different file types like Excel spreadsheets, PowerPoint presentations, images, or CAD drawings.

### Exercise: Customize PDF Output

Try to modify the examples above to:
1. Render only specific pages of a document to PDF
2. Add security to the PDF output with passwords and permissions
3. Optimize the PDF for reduced file size

## Advanced PDF Features

GroupDocs.Viewer Cloud API offers several advanced PDF rendering features:

### PDF Security

You can add password protection and set permissions to restrict what users can do with the PDF:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    RenderOptions = new PdfOptions
    {
        DocumentOpenPassword = "openPassword",
        PermissionsPassword = "permissionsPassword",
        Permissions = new List<string> { "DenyPrinting", "DenyModification" }
    }
};
```

### PDF Optimization

You can optimize the PDF output for reduced file size by compressing images and other content:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    RenderOptions = new PdfOptions
    {
        PdfOptimizationOptions = new PdfOptimizationOptions
        {
            CompressImages = true,
            ImageQuality = 80
        }
    }
};
```

## Troubleshooting Tips

- Authentication Issues: Ensure your Client ID and Client Secret are correct and that you're generating a fresh JWT token
- File Not Found: Verify that the file path in your request matches the actual path in cloud storage
- Large Files: When working with very large documents, you might need to increase timeout values in your code
- PDF Features: Not all features are supported for all document types - check the API documentation for details

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with the GroupDocs.Viewer Cloud API
- How to upload documents to cloud storage
- How to render documents to PDF format
- How to download and use the rendered PDF
- How to implement PDF rendering in your applications using SDKs
- How to use advanced PDF features like security and optimization

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Have questions about this tutorial? Need help implementing PDF viewing in your application? We welcome your feedback and questions on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
