---
title: Creating Responsive HTML Layout with GroupDocs.Viewer Cloud API Tutorial
description: Learn how to create responsive HTML document views that adapt to different screen sizes using GroupDocs.Viewer Cloud API in this step-by-step tutorial
url: /basic-usage/html-viewer-responsive-layout/
weight: 6
---

# Tutorial: Creating Responsive HTML Layout with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Enable responsive HTML rendering for your documents
- Create mobile-friendly document views that adapt to different screen sizes
- Implement responsive document viewing in your web applications
- Test responsive views across different devices

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Viewer Cloud account (get your [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret
- Basic understanding of HTML and responsive web design
- A document to render (we'll use a sample DOCX document in this tutorial)

## Understanding Responsive Document Viewing

In today's multi-device world, responsive design is essential for providing an optimal viewing experience across a wide range of devices—from desktop computers to smartphones. When rendering documents for web viewing, the same principle applies.

GroupDocs.Viewer Cloud API provides a simple way to make your rendered HTML documents responsive, ensuring they look great on any device without horizontal scrolling or zooming.

### How Responsive HTML Rendering Works

When you enable responsive rendering:
1. The API generates HTML with responsive CSS rules
2. Text and content automatically adjust to the viewport width
3. Images and other elements scale proportionally
4. The layout adapts to different screen sizes

## Step 1: Enable Responsive Layout

To enable responsive rendering, you need to set the `IsResponsive` property to `true` in the HTML rendering options. Let's see how to do this using cURL:

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Store JWT in a variable for reuse
JWT="YOUR_JWT_TOKEN"

# Render document with responsive layout
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer $JWT" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.docx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
     'IsResponsive': true
  }
}"
```

This API call will render your document to HTML with responsive layout enabled.

## Step 2: Examine the Response

After making the API call, you'll receive a response like this:

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
    {
      "number": 3,
      "resources": null,
      "path": "viewer/sample_docx/sample_page_3.html",
      "downloadUrl": "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_3.html"
    }
  ],
  "attachments": [],
  "file": null
}
```

## Step 3: Download and Check the Responsive HTML

Download one of the rendered HTML pages to examine its responsive structure:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_1.html" \
-X GET \
-H "Authorization: Bearer $JWT" \
--output "responsive_page1.html"
```

If you open the downloaded HTML file in a text editor, you'll notice that it includes responsive CSS styles. These styles ensure that the content adapts to different screen sizes.

## Step 4: Implement in Your Application

Now let's implement responsive HTML rendering in a real application using one of our supported SDKs.

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
            var apiInstance = new ViewApi(configuration);

            // Define rendering options with responsive layout enabled
            var viewOptions = new ViewOptions
            {
                FileInfo = new FileInfo
                {
                    FilePath = "SampleFiles/sample.docx"
                },
                ViewFormat = ViewOptions.ViewFormatEnum.HTML,
                RenderOptions = new HtmlOptions
                {
                    IsResponsive = true
                }
            };

            try
            {
                // Call the API to render the document
                var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
                
                Console.WriteLine("Document rendered with responsive layout!");
                
                // Print information about the rendered pages
                foreach (var page in response.Pages)
                {
                    Console.WriteLine($"Page {page.Number}: {page.DownloadUrl}");
                    
                    // Download the rendered HTML (optional)
                    using (var webClient = new System.Net.WebClient())
                    {
                        webClient.Headers.Add("Authorization", "Bearer " + configuration.GetToken());
                        webClient.DownloadFile(page.DownloadUrl, $"responsive_page_{page.Number}.html");
                        Console.WriteLine($"Downloaded responsive_page_{page.Number}.html");
                    }
                }
                
                Console.WriteLine("\nThe HTML files now have responsive layout enabled!");
                Console.WriteLine("Try opening them on different devices or resize your browser window to see the responsive behavior.");
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
import requests
import groupdocs_viewer_cloud

# Get your client ID and client secret from https://dashboard.groupdocs.cloud/
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create API instance
api_instance = groupdocs_viewer_cloud.ViewApi.from_keys(client_id, client_secret)

# Define rendering options with responsive layout enabled
view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "SampleFiles/sample.docx"
view_options.view_format = "HTML"
view_options.render_options = groupdocs_viewer_cloud.HtmlOptions()
view_options.render_options.is_responsive = True

try:
    # Call the API to render the document
    request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
    response = api_instance.create_view(request)
    
    print("Document rendered with responsive layout!")
    
    # Print information about the rendered pages
    for page in response.pages:
        print(f"Page {page.number}: {page.download_url}")
        
        # Download the rendered HTML (optional)
        response = requests.get(
            page.download_url,
            headers={"Authorization": f"Bearer {api_instance.configuration.access_token}"}
        )
        
        if response.status_code == 200:
            with open(f"responsive_page_{page.number}.html", "wb") as file:
                file.write(response.content)
                print(f"Downloaded responsive_page_{page.number}.html")
    
    print("\nThe HTML files now have responsive layout enabled!")
    print("Try opening them on different devices or resize your browser window to see the responsive behavior.")

except groupdocs_viewer_cloud.ApiException as e:
    print(f"Exception while calling ViewApi: {e}")
```

## Step 5: Create a Simple Viewer Application

Now, let's create a simple HTML viewer application that displays your responsive document. This example demonstrates how to embed the responsive HTML in a web page:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Document Viewer</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            font-family: Arial, sans-serif;
        }
        .header {
            background-color: #f4f4f4;
            padding: 10px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .container {
            display: flex;
            height: calc(100% - 60px);
        }
        .sidebar {
            width: 200px;
            background-color: #f8f8f8;
            padding: 10px;
            overflow-y: auto;
        }
        .page-link {
            display: block;
            padding: 8px;
            margin-bottom: 5px;
            background-color: #e9e9e9;
            text-decoration: none;
            color: #333;
            border-radius: 4px;
        }
        .page-link:hover {
            background-color: #dedede;
        }
        .active {
            background-color: #4CAF50;
            color: white;
        }
        .content {
            flex: 1;
            padding: 10px;
            overflow-y: auto;
        }
        iframe {
            width: 100%;
            height: 100%;
            border: none;
        }
        @media (max-width: 768px) {
            .container {
                flex-direction: column;
            }
            .sidebar {
                width: 100%;
                height: auto;
                max-height: 150px;
            }
            .content {
                height: calc(100% - 150px);
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Responsive Document Viewer</h1>
    </div>
    <div class="container">
        <div class="sidebar" id="pageList">
            <!-- Page links will be added here dynamically -->
        </div>
        <div class="content">
            <iframe id="documentFrame" src=""></iframe>
        </div>
    </div>

    <script>
        // Configuration - update these paths with your actual document pages
        const pages = [
            { number: 1, path: 'responsive_page_1.html' },
            { number: 2, path: 'responsive_page_2.html' },
            { number: 3, path: 'responsive_page_3.html' }
        ];

        // Initialize the viewer
        document.addEventListener('DOMContentLoaded', function() {
            const pageList = document.getElementById('pageList');
            const documentFrame = document.getElementById('documentFrame');
            
            // Add page links to the sidebar
            pages.forEach(page => {
                const link = document.createElement('a');
                link.href = '#';
                link.className = 'page-link';
                link.textContent = `Page ${page.number}`;
                link.onclick = function(e) {
                    e.preventDefault();
                    // Update active link
                    document.querySelectorAll('.page-link').forEach(l => l.classList.remove('active'));
                    this.classList.add('active');
                    // Load the page in the iframe
                    documentFrame.src = page.path;
                };
                pageList.appendChild(link);
            });
            
            // Load the first page by default
            if (pages.length > 0) {
                documentFrame.src = pages[0].path;
                pageList.querySelector('.page-link').classList.add('active');
            }
        });
    </script>
</body>
</html>
```

Save this as `viewer.html` in the same directory as your downloaded responsive HTML pages. Open it in a web browser to see how your responsive document looks in a real-world application.

## Try It Yourself

Now that you've learned how to create responsive HTML views, try experimenting with different document types and devices. Here are some exercises to enhance your learning:

### Exercise 1: Test Across Multiple Devices

1. Render a multi-page document with responsive layout enabled
2. Open the rendered HTML pages on different devices (desktop, tablet, smartphone)
3. Observe how the content adapts to different screen sizes

### Exercise 2: Compare Responsive vs. Non-Responsive

1. Render the same document twice - once with responsive layout enabled and once without
2. Compare the HTML structure and CSS of both versions
3. Test both versions on a mobile device and note the differences

## Troubleshooting Tips

- Content Not Adapting: If your content doesn't seem responsive, make sure the `IsResponsive` property is set to `true` in your rendering options
- Text Overflow: Some documents with complex layouts might still have text overflow issues on very small screens - consider adding custom CSS for edge cases
- Images Not Scaling: Ensure that any custom styling you add doesn't override the responsive image settings

## What You've Learned

In this tutorial, you've learned:
- How to enable responsive HTML rendering in GroupDocs.Viewer Cloud API
- The benefits of responsive document viewing for multi-device support
- How to implement responsive document viewing in your applications using SDKs
- How to create a simple viewer application that displays responsive documents
- Best practices for testing responsive document views across devices

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Have questions about responsive document viewing? Need help implementing it in your application? We welcome your feedback and questions on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).