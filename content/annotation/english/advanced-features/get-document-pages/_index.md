---
title: How to Get Document Pages in GroupDocs.Annotation Cloud Tutorial
description: Learn how to create image previews of document pages using the GroupDocs.Annotation Cloud API in this step-by-step tutorial
weight: 220
url: /advanced-features/get-document-pages/
---

# Tutorial: How to Get Document Pages

## Learning Objectives

In this tutorial, you'll learn how to:
- Generate image representations of document pages
- Access and process document page images
- Implement document preview functionality in different programming languages
- Clean up preview resources when they're no longer needed

## What is Document Page Preview?

The Document Page Preview feature allows you to create image representations (PNG format) of each page in a document. This is useful for building document viewers, implementing annotation UI's, or creating document thumbnails without requiring users to have the original document software installed.

## Prerequisites

Before starting this tutorial, ensure you have:
1. A GroupDocs.Annotation Cloud account (or [get a free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. A development environment for your preferred language
4. A document uploaded to your GroupDocs.Annotation Cloud storage

## Implementation Steps

Let's walk through the process of generating document page previews:

### 1. Authentication

First, authenticate with the GroupDocs.Annotation Cloud API:

```javascript
// Get JWT token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the received JWT token for subsequent API calls.

### 2. Generate Page Previews

Use the `POST /annotation/preview/create` endpoint to generate page images:

```javascript
// cURL example to create document preview images
curl -v "https://api.groupdocs.cloud/v2.0/annotation/preview/create" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{ \"FileInfo\": { \"FilePath\": \"sample-document.docx\" }}"
```

### 3. Process the Response

The API returns information about the generated page images:

```javascript
{
  "totalCount": 3,
  "entries": [
    {
      "number": 0,
      "link": {
        "href": "https://api.groupdocs.cloud/v2.0/annotation/storage/file/sample-document_docx/page_0.png",
        "rel": "self",
        "type": "file",
        "title": "page_0.png"
      }
    },
    {
      "number": 1,
      "link": {
        "href": "https://api.groupdocs.cloud/v2.0/annotation/storage/file/sample-document_docx/page_1.png",
        "rel": "self",
        "type": "file",
        "title": "page_1.png"
      }
    },
    {
      "number": 2,
      "link": {
        "href": "https://api.groupdocs.cloud/v2.0/annotation/storage/file/sample-document_docx/page_2.png",
        "rel": "self",
        "type": "file",
        "title": "page_2.png"
      }
    }
  ]
}
```

### 4. Access the Page Images

Each entry in the response contains a link to the generated PNG image file. You can download these images using the provided URLs.

### 5. Clean Up Resources (Optional)

When you're done with the preview images, you can remove them using the `POST /annotation/preview/remove` endpoint:

```javascript
// cURL example to delete document preview images
curl -v "https://api.groupdocs.cloud/v2.0/annotation/preview/remove" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{ \"FilePath\": \"sample-document.docx\"}"
```

## Try It Yourself

Let's implement document page preview generation in different programming languages.

### C# Example

```csharp
// For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = "YOUR_APP_KEY"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = "YOUR_APP_SID";

var configuration = new Configuration(MyAppSid, MyAppKey);
var apiInstance = new AnnotateApi(configuration);

// Create file info object
var fileInfo = new FileInfo { FilePath = "sample-document.docx" };
var options = new PreviewOptions { FileInfo = fileInfo };

// Generate page previews
var request = new GetPagesRequest(options);
var response = apiInstance.GetPages(request);

Console.WriteLine($"Generated previews for {response.TotalCount} pages:");

// Process the page images
foreach (var entry in response.Entries)
{
    Console.WriteLine($"Page {entry.Number}: {entry.Link.Title} - {entry.Link.Href}");
    
    // Here you could download each image or process it further
    // For example:
    // DownloadImage(entry.Link.Href, $"page_{entry.Number}.png");
}

// Optional: Clean up the preview resources when done
var cleanupRequest = new DeletePagesRequest(fileInfo);
apiInstance.DeletePages(cleanupRequest);
Console.WriteLine("Preview resources cleaned up");

// Example download method (not implemented)
void DownloadImage(string url, string savePath)
{
    // Code to download image from url and save to disk
    Console.WriteLine($"Downloaded {url} to {savePath}");
}
```

### Java Example

```java
// For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-java-samples
String clientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String clientSecret = "YOUR_CLIENT_SECRET";

Configuration configuration = new Configuration(clientId, clientSecret);
AnnotateApi apiInstance = new AnnotateApi(configuration);

// Create file info object
FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("sample-document.docx");

PreviewOptions options = new PreviewOptions();
options.setFileInfo(fileInfo);

// Generate page previews
GetPagesRequest request = new GetPagesRequest(options);
PageImages response = apiInstance.getPages(request);

System.out.println("Generated previews for " + response.getTotalCount() + " pages:");

// Process the page images
for (PageImage entry : response.getEntries()) {
    System.out.println("Page " + entry.getNumber() + ": " + 
        entry.getLink().getTitle() + " - " + entry.getLink().getHref());
    
    // Here you could download each image or process it further
    // For example:
    // downloadImage(entry.getLink().getHref(), "page_" + entry.getNumber() + ".png");
}

// Optional: Clean up the preview resources when done
DeletePagesRequest cleanupRequest = new DeletePagesRequest(fileInfo);
apiInstance.deletePages(cleanupRequest);
System.out.println("Preview resources cleaned up");

// Example download method (not implemented)
void downloadImage(String url, String savePath) {
    // Code to download image from url and save to disk
    System.out.println("Downloaded " + url + " to " + savePath);
}
```

### Python Example

```python
# For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
import requests  # For downloading images

app_sid = "YOUR_APP_SID"  # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "YOUR_APP_KEY"

api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)

# Set up file info for the document
file_info = groupdocs_annotation_cloud.FileInfo()
file_info.file_path = "sample-document.docx"

options = groupdocs_annotation_cloud.PreviewOptions()
options.file_info = file_info

# Generate page previews
request = groupdocs_annotation_cloud.GetPagesRequest(options)
response = api.get_pages(request)

print(f"Generated previews for {response.total_count} pages:")

# Process the page images
for entry in response.entries:
    print(f"Page {entry.number}: {entry.link.title} - {entry.link.href}")
    
    # Here you could download each image or process it further
    # For example:
    # download_image(entry.link.href, f"page_{entry.number}.png")

# Optional: Clean up the preview resources when done
cleanup_request = groupdocs_annotation_cloud.DeletePagesRequest(file_info)
api.delete_pages(cleanup_request)
print("Preview resources cleaned up")

# Example download method
def download_image(url, save_path):
    response = requests.get(url)
    if response.status_code == 200:
        with open(save_path, 'wb') as f:
            f.write(response.content)
        print(f"Downloaded {url} to {save_path}")
    else:
        print(f"Failed to download {url}, status code: {response.status_code}")
```

## Building Document Viewers with Page Previews

Document page previews enable you to build custom document viewers for your applications. Here are some common implementation patterns:

### 1. Basic Document Viewer

A simple document viewer implementation:

```javascript
// Pseudocode for a basic document viewer
function loadDocumentViewer(documentPath) {
    // Generate page previews
    const pages = generatePagePreviews(documentPath);
    
    // Display pages in a container
    const viewerContainer = document.getElementById('viewer-container');
    
    pages.forEach(page => {
        const pageElement = document.createElement('div');
        pageElement.className = 'document-page';
        
        const pageImage = document.createElement('img');
        pageImage.src = page.link.href;
        pageImage.alt = `Page ${page.number + 1}`;
        
        pageElement.appendChild(pageImage);
        viewerContainer.appendChild(pageElement);
    });
}
```

### 2. Lazy-Loading Document Viewer

For documents with many pages, implement lazy loading for better performance:

```javascript
// Pseudocode for a lazy-loading document viewer
function lazyLoadDocumentViewer(documentPath, totalPages) {
    const viewerContainer = document.getElementById('viewer-container');
    let loadedPages = 0;
    const pageHeight = 1000; // Estimated page height in pixels
    
    // Setup container with placeholder pages
    for (let i = 0; i < totalPages; i++) {
        const pageElement = document.createElement('div');
        pageElement.className = 'document-page';
        pageElement.id = `page-${i}`;
        pageElement.style.height = `${pageHeight}px`;
        viewerContainer.appendChild(pageElement);
    }
    
    // Function to load visible pages
    function loadVisiblePages() {
        const viewportTop = window.scrollY;
        const viewportBottom = viewportTop + window.innerHeight;
        
        for (let i = 0; i < totalPages; i++) {
            const pageElement = document.getElementById(`page-${i}`);
            const pageRect = pageElement.getBoundingClientRect();
            
            // If page is visible and not already loaded
            if (pageRect.top < viewportBottom && 
                pageRect.bottom > viewportTop && 
                !pageElement.dataset.loaded) {
                
                // Mark as loaded
                pageElement.dataset.loaded = true;
                
                // Load page image
                const pageImage = document.createElement('img');
                pageImage.src = getPageImageUrl(documentPath, i);
                pageImage.alt = `Page ${i + 1}`;
                pageElement.innerHTML = '';
                pageElement.appendChild(pageImage);
                
                console.log(`Loaded page ${i + 1}`);
            }
        }
    }
    
    // Initial load of visible pages
    loadVisiblePages();
    
    // Load pages when scrolling
    window.addEventListener('scroll', loadVisiblePages);
}
```

### 3. Document Viewer with Annotation Interface

Combine page previews with annotation capability:

```javascript
// Pseudocode for viewer with annotation interface
function annotationEnabledViewer(documentPath) {
    // Generate page previews
    const pages = generatePagePreviews(documentPath);
    
    // Display pages in a container
    const viewerContainer = document.getElementById('viewer-container');
    
    pages.forEach(page => {
        const pageElement = document.createElement('div');
        pageElement.className = 'document-page';
        pageElement.dataset.pageNumber = page.number;
        
        const pageImage = document.createElement('img');
        pageImage.src = page.link.href;
        pageImage.alt = `Page ${page.number + 1}`;
        
        pageElement.appendChild(pageImage);
        viewerContainer.appendChild(pageElement);
        
        // Add annotation layer on top of the page
        const annotationLayer = document.createElement('div');
        annotationLayer.className = 'annotation-layer';
        pageElement.appendChild(annotationLayer);
        
        // Setup annotation events
        annotationLayer.addEventListener('mousedown', startAnnotation);
        annotationLayer.addEventListener('mousemove', updateAnnotation);
        annotationLayer.addEventListener('mouseup', finalizeAnnotation);
    });
}

// Annotation event handlers would be implemented here
```

## Troubleshooting Tips

1. No Pages Generated: Verify that the document format is supported and the file exists
2. Image Quality Issues: The default image format is PNG; consider requesting different formats if needed
3. Large Documents: For documents with many pages, implement pagination or lazy loading
4. Resource Cleanup: Always clean up preview resources when no longer needed to save storage space

## What You've Learned

In this tutorial, you've learned how to:
- Generate image representations of document pages
- Access and process the generated page images
- Implement document preview functionality in different programming languages
- Clean up preview resources when they're no longer needed
- Build different types of document viewers using page previews

## Further Practice

To enhance your understanding of document page previews, try these exercises:
1. Create a complete document viewer that supports navigation, zooming, and rotation
2. Implement a document thumbnail gallery for quick navigation
3. Build a document comparison tool that shows two documents side by side
4. Create a document preview system with caching for improved performance

## Next Steps

Continue your learning journey with these related tutorials:
- [Tutorial: How to Delete Document Pages](/advanced-features/delete-document-pages)
- [Tutorial: How to Add Multiple Annotations](/advanced-features/multiple-annotations)
- [Tutorial: How to Extract Annotations](/advanced-features/extract-annotations)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
