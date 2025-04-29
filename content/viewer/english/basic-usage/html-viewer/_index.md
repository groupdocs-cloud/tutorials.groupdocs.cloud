---
title: How to Use the HTML Viewer with GroupDocs.Viewer Cloud API Tutorial
description: Learn how to render documents to HTML format with this step-by-step tutorial for GroupDocs.Viewer Cloud API
url: /basic-usage/html-viewer/
---

# Tutorial: How to Use the HTML Viewer with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Render documents to HTML format using GroupDocs.Viewer Cloud API
- Understand the different HTML rendering options available
- Implement HTML viewing in your applications with practical examples
- Access and manipulate rendered HTML documents

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Viewer Cloud account (get your [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret
- Basic understanding of REST APIs
- Familiarity with your programming language of choice (C#, Java, Python, PHP, Ruby, Node.js, or Go)
- A document you want to convert to HTML (we'll use a sample DOCX in this tutorial)

## HTML Rendering Overview

HTML rendering converts your documents to HTML format, making them viewable in any web browser without requiring special plugins or applications. GroupDocs.Viewer Cloud API makes this process seamless with powerful options to customize the output.

### Why Convert Documents to HTML?

Converting documents to HTML offers several advantages:
- Universal Access: HTML is supported by all modern browsers
- Responsive Design: HTML can be styled to adapt to different screen sizes
- Interactive Features: Elements can be made interactive with JavaScript
- Easy Integration: HTML can be easily embedded in web applications

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

## Step 2: Render Document to HTML

Now that your document is in cloud storage, you can render it to HTML format.

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
   'ViewFormat': 'HTML'
}"
```

The API will process your document and return information about the rendered HTML pages.

### Understanding the Response

```json
{
  "pages": [
    {
      "number": 1,
      "resources": null,
      "path": "viewer/sample_docx/sample_page_1.html",
      "downloadUrl": "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_1.html"
    },
    {
      "number": 2,
      "resources": null,
      "path": "viewer/sample_docx/sample_page_2.html",
      "downloadUrl": "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_2.html"
    },
    ...
  ],
  "attachments": [],
  "file": null
}
```

The response contains:
- An array of pages with their numbers and download URLs
- Information about any attachments (if present)
- Paths to access the rendered HTML files

## Step 3: Download and Display the HTML Pages

To access the rendered HTML, you can download each page using the provided URLs:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_1.html" \
-X GET \
-H "Authorization: Bearer $JWT" \
--output "page1.html"
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
using System.Collections.Generic;
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
                ViewFormat = ViewOptions.ViewFormatEnum.HTML
            };

            try
            {
                // Call the API to render the document
                var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
                
                Console.WriteLine("Document rendered successfully!");
                
                // Print information about the rendered pages
                foreach (var page in response.Pages)
                {
                    Console.WriteLine($"Page {page.Number}: {page.DownloadUrl}");
                    
                    // Download the rendered HTML (optional)
                    using (var webClient = new System.Net.WebClient())
                    {
                        webClient.Headers.Add("Authorization", "Bearer " + configuration.GetToken());
                        webClient.DownloadFile(page.DownloadUrl, $"page_{page.Number}.html");
                    }
                }
                
                // Open the first page in default browser
                if (response.Pages.Count > 0)
                {
                    Process.Start(new ProcessStartInfo
                    {
                        FileName = $"page_1.html",
                        UseShellExecute = true
                    });
                }
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
view_options.view_format = "HTML"

try:
    # Call the API to render the document
    request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
    response = api_instance.create_view(request)
    
    print("Document rendered successfully!")
    
    # Print information about the rendered pages
    for page in response.pages:
        print(f"Page {page.number}: {page.download_url}")
        
        # Download the rendered HTML (optional)
        response = requests.get(
            page.download_url,
            headers={"Authorization": f"Bearer {api_instance.configuration.access_token}"}
        )
        
        if response.status_code == 200:
            with open(f"page_{page.number}.html", "wb") as file:
                file.write(response.content)
                print(f"Downloaded page_{page.number}.html")
    
    # Open the first page in default browser
    if response.pages and len(response.pages) > 0:
        first_page_path = os.path.abspath(f"page_1.html")
        print(f"Opening {first_page_path} in browser")
        webbrowser.open(f'file://{first_page_path}')

except groupdocs_viewer_cloud.ApiException as e:
    print(f"Exception while calling ViewApi: {e}")
```

## Try It Yourself

Now that you've seen how to render documents to HTML using GroupDocs.Viewer Cloud API, try it with your own documents. Experiment with different file types like PDFs, Excel spreadsheets, PowerPoint presentations, or images.

### Exercise: Customize HTML Output

Try to modify the examples above to:
1. Render only specific pages of a document
2. Enable responsive layout
3. Exclude certain fonts from the HTML output

## Troubleshooting Tips

- Authentication Issues: Ensure your Client ID and Client Secret are correct and that you're generating a fresh JWT token
- File Not Found: Verify that the file path in your request matches the actual path in cloud storage
- Rendering Errors: Some complex documents may have specific rendering requirements - check the error messages for details

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with the GroupDocs.Viewer Cloud API
- How to upload documents to cloud storage
- How to render documents to HTML format
- How to download and display the rendered HTML pages
- How to implement this functionality in your applications using SDKs

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Have questions about this tutorial? Need help implementing HTML viewing in your application? We welcome your feedback and questions on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
