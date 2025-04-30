---
title: How to Use the Image Viewer with GroupDocs.Viewer Cloud API Tutorial
description: Learn how to render documents to image formats like JPG and PNG with this comprehensive tutorial for GroupDocs.Viewer Cloud API
url: /basic-usage/image-viewer/
weight: 7
---

# Tutorial: How to Use the Image Viewer with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Render documents to image formats (JPG and PNG) using GroupDocs.Viewer Cloud API
- Configure image rendering options for optimal output
- Control image quality, dimensions, and other properties
- Implement image viewing functionality in your applications
- Work with rendered image files

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Viewer Cloud account (get your [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret
- Basic understanding of REST APIs
- Familiarity with your programming language of choice (C#, Java, Python, PHP, Ruby, Node.js, or Go)
- A document you want to convert to images (we'll use a sample DOCX in this tutorial)

## Image Rendering Overview

Converting documents to images (JPG, PNG) provides several advantages in various scenarios:

- Universal Compatibility: Images can be displayed on any device without special viewers
- Static Content: Images represent an exact visual snapshot of your documents
- Simplified Integration: Images are easily integrated into websites and applications
- No Client-Side Rendering: Images don't require client-side processing to display
- Control Over Quality: You can balance image quality against file size

GroupDocs.Viewer Cloud API makes it simple to convert documents to high-quality images with customizable options to meet your specific requirements.

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

## Step 2: Render Document to JPG Images

Now that your document is in cloud storage, you can render it to JPG format.

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
   'ViewFormat': 'JPG'
}"
```

The API will process your document and return information about the rendered image files.

### Understanding the Response

```json
{
  "pages": [
    {
      "number": 1,
      "resources": null,
      "path": "viewer/sample_docx/sample_page_1.jpg",
      "downloadUrl": "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_1.jpg"
    },
    {
      "number": 2,
      "resources": null,
      "path": "viewer/sample_docx/sample_page_2.jpg",
      "downloadUrl": "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_2.jpg"
    },
    {
      "number": 3,
      "resources": null,
      "path": "viewer/sample_docx/sample_page_3.jpg",
      "downloadUrl": "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_3.jpg"
    }
  ],
  "attachments": [],
  "file": null
}
```

The response contains:
- An array of pages with their numbers and download URLs
- Each page of the document is rendered as a separate JPG image
- Paths to access the rendered image files

## Step 3: Download and Use the Image Files

To access the rendered images, you can download each page using the provided URLs:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample_page_1.jpg" \
-X GET \
-H "Authorization: Bearer $JWT" \
--output "page1.jpg"
```

## Step 4: Rendering to PNG Format

If you prefer PNG format (which supports transparency and offers lossless compression), simply change the `ViewFormat` parameter:

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
   'ViewFormat': 'PNG'
}"
```

## Step 5: Customizing Image Output

GroupDocs.Viewer Cloud offers several options to customize the rendered images:

### Adjusting Image Quality

For JPG format, you can control the quality (1-100) to balance image quality against file size:

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
  'ViewFormat': 'JPG',
  'RenderOptions': {
     'JpegQuality': 75
  }
}"
```

### Setting Image Dimensions

You can specify the width and height of the output images:

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
  'ViewFormat': 'JPG',
  'RenderOptions': {
     'Width': 800,
     'Height': 1000
  }
}"
```

## Step 6: Implement in Your Application

Now let's implement image rendering in a real application using one of our supported SDKs.

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
                ViewFormat = ViewOptions.ViewFormatEnum.JPG,
                RenderOptions = new ImageOptions
                {
                    JpegQuality = 90,
                    Width = 800,
                    Height = 1000
                }
            };

            try
            {
                // Call the API to render the document
                var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
                
                Console.WriteLine("Document rendered to images successfully!");
                
                // Create a directory to store the images
                Directory.CreateDirectory("rendered_images");
                
                // Download each rendered image
                foreach (var page in response.Pages)
                {
                    Console.WriteLine($"Page {page.Number}: {page.DownloadUrl}");
                    
                    using (var webClient = new System.Net.WebClient())
                    {
                        webClient.Headers.Add("Authorization", "Bearer " + configuration.GetToken());
                        string outputPath = Path.Combine("rendered_images", $"page_{page.Number}.jpg");
                        webClient.DownloadFile(page.DownloadUrl, outputPath);
                        Console.WriteLine($"Downloaded {outputPath}");
                    }
                }
                
                // Open the directory with rendered images
                Process.Start(new ProcessStartInfo
                {
                    FileName = "rendered_images",
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
view_options.view_format = "JPG"
view_options.render_options = groupdocs_viewer_cloud.ImageOptions()
view_options.render_options.jpeg_quality = 90
view_options.render_options.width = 800
view_options.render_options.height = 1000

try:
    # Call the API to render the document
    request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
    response = api_instance.create_view(request)
    
    print("Document rendered to images successfully!")
    
    # Create a directory to store the images
    os.makedirs("rendered_images", exist_ok=True)
    
    # Download each rendered image
    for page in response.pages:
        print(f"Page {page.number}: {page.download_url}")
        
        image_response = requests.get(
            page.download_url,
            headers={"Authorization": f"Bearer {api_instance.configuration.access_token}"}
        )
        
        if image_response.status_code == 200:
            output_path = os.path.join("rendered_images", f"page_{page.number}.jpg")
            with open(output_path, "wb") as file:
                file.write(image_response.content)
                print(f"Downloaded {output_path}")
    
    print(f"\nAll images have been downloaded to the 'rendered_images' directory.")
    
    # Open the first image in default image viewer
    first_image_path = os.path.abspath(os.path.join("rendered_images", "page_1.jpg"))
    print(f"Opening {first_image_path}")
    
    import webbrowser
    webbrowser.open(f'file://{first_image_path}')

except groupdocs_viewer_cloud.ApiException as e:
    print(f"Exception while calling ViewApi: {e}")
```

## Step 7: Creating a Simple Image Gallery

Let's create a simple HTML gallery to display the rendered images:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document Image Gallery</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .gallery {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
        }
        .thumbnail {
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 10px;
            cursor: pointer;
            transition: transform 0.3s;
            width: 200px;
        }
        .thumbnail:hover {
            transform: scale(1.05);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        .thumbnail img {
            width: 100%;
            height: auto;
            display: block;
        }
        .thumbnail p {
            margin: 10px 0 0;
            text-align: center;
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.9);
        }
        .modal-content {
            position: relative;
            margin: auto;
            display: block;
            max-width: 90%;
            max-height: 90%;
            top: 50%;
            transform: translateY(-50%);
        }
        .close {
            position: absolute;
            top: 15px;
            right: 35px;
            color: #f1f1f1;
            font-size: 40px;
            font-weight: bold;
            cursor: pointer;
        }
        .navigation {
            position: absolute;
            top: 50%;
            width: 100%;
            display: flex;
            justify-content: space-between;
        }
        .nav-btn {
            color: white;
            font-size: 30px;
            font-weight: bold;
            cursor: pointer;
            padding: 15px;
            user-select: none;
        }
    </style>
</head>
<body>
    <h1>Document Image Gallery</h1>
    
    <div class="gallery" id="gallery">
        <!-- Thumbnails will be added here dynamically -->
    </div>
    
    <div id="imageModal" class="modal">
        <span class="close" onclick="closeModal()">&times;</span>
        <img class="modal-content" id="modalImage">
        <div class="navigation">
            <span class="nav-btn" onclick="changeImage(-1)">&#10094;</span>
            <span class="nav-btn" onclick="changeImage(1)">&#10095;</span>
        </div>
    </div>
    
    <script>
        // Configuration - update with your rendered images
        const pages = [
            { number: 1, path: 'rendered_images/page_1.jpg' },
            { number: 2, path: 'rendered_images/page_2.jpg' },
            { number: 3, path: 'rendered_images/page_3.jpg' }
            // Add more pages as needed
        ];
        
        let currentImageIndex = 0;
        
        // Populate the gallery with thumbnails
        document.addEventListener('DOMContentLoaded', function() {
            const gallery = document.getElementById('gallery');
            
            pages.forEach((page, index) => {
                const thumbnail = document.createElement('div');
                thumbnail.className = 'thumbnail';
                thumbnail.innerHTML = `
                    <img src="${page.path}" alt="Page ${page.number}">
                    <p>Page ${page.number}</p>
                `;
                thumbnail.onclick = function() {
                    openModal(index);
                };
                gallery.appendChild(thumbnail);
            });
        });
        
        function openModal(index) {
            const modal = document.getElementById('imageModal');
            const modalImage = document.getElementById('modalImage');
            
            currentImageIndex = index;
            modalImage.src = pages[currentImageIndex].path;
            modal.style.display = 'block';
        }
        
        function closeModal() {
            document.getElementById('imageModal').style.display = 'none';
        }
        
        function changeImage(step) {
            currentImageIndex += step;
            
            // Wrap around to the beginning or end if needed
            if (currentImageIndex >= pages.length) {
                currentImageIndex = 0;
            } else if (currentImageIndex < 0) {
                currentImageIndex = pages.length - 1;
            }
            
            document.getElementById('modalImage').src = pages[currentImageIndex].path;
        }
        
        // Close modal when clicking outside the image
        window.onclick = function(event) {
            const modal = document.getElementById('imageModal');
            if (event.target === modal) {
                closeModal();
            }
        };
        
        // Handle keyboard navigation
        document.addEventListener('keydown', function(event) {
            if (document.getElementById('imageModal').style.display === 'block') {
                if (event.key === 'ArrowLeft') {
                    changeImage(-1);
                } else if (event.key === 'ArrowRight') {
                    changeImage(1);
                } else if (event.key === 'Escape') {
                    closeModal();
                }
            }
        });
    </script>
</body>
</html>
```

Save this as `gallery.html` in the same directory as your downloaded images. Open it in a web browser to see your rendered document pages displayed in a responsive gallery with navigation.

## Try It Yourself

Now that you've learned how to render documents to images using GroupDocs.Viewer Cloud API, try it with your own documents. Experiment with different file types and image rendering options.

### Exercise: Optimize Image Quality vs. Size

Try to find the optimal balance between image quality and file size:
1. Render the same document multiple times with different JPEG quality settings (e.g., 50, 70, 90)
2. Compare the file sizes and visual quality of the resulting images
3. Determine the optimal setting for your specific use case

## Advanced Image Features

GroupDocs.Viewer Cloud API offers several advanced image rendering features:

### Text Extraction with Coordinates

You can extract text with coordinates from the rendered images, which is useful for implementing text selection or search functionality:

```csharp
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
```

### Watermarking

You can add text watermarks to the rendered images:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.JPG,
    RenderOptions = new ImageOptions
    {
        Watermark = new Watermark
        {
            Text = "CONFIDENTIAL",
            Color = "#FF0000",
            Position = Watermark.PositionEnum.Diagonal,
            Size = Watermark.SizeEnum.Full
        }
    }
};
```

## Troubleshooting Tips

- Image Quality Issues: If images appear blurry or low-quality, try increasing the JPEG quality setting or switch to PNG format
- File Size Concerns: For large documents, consider optimizing JPEG quality or rendering only specific pages
- Memory Limitations: When processing very large documents or high-resolution images, be aware of memory usage in your application

## What You've Learned

In this tutorial, you've learned:
- How to render documents to JPG and PNG image formats
- How to customize image properties like quality and dimensions
- How to download and use the rendered images
- How to implement image rendering in your applications using SDKs
- How to create a simple image gallery to display rendered document pages

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Have questions about image rendering? Need help implementing it in your application? We welcome your feedback and questions on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
