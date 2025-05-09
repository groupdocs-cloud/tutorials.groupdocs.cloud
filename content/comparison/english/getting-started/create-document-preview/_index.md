---
title: How to Create Document Previews with GroupDocs.Comparison Cloud Tutorial
weight: 5
description: Learn how to generate visual previews of documents before and after comparison using GroupDocs.Comparison Cloud API in this step-by-step tutorial
url: /getting-started/create-document-preview/
---

# Tutorial: How to Create Document Previews with GroupDocs.Comparison Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Generate visual previews of documents as images
- Customize preview options (size, format, quality)
- Implement document preview functionality in different programming languages
- Use previews effectively in document comparison workflows

## Prerequisites

Before starting this tutorial, you'll need:
- A GroupDocs.Comparison Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Documents uploaded to your GroupDocs Cloud Storage
- Basic familiarity with RESTful APIs and your chosen programming language

## Understanding Document Previews

Document previews allow users to visualize documents without opening them in their native applications. In the context of document comparison, previews are particularly valuable for:

- Quickly reviewing documents before comparison
- Displaying comparison results in web applications
- Creating thumbnails for document management interfaces
- Providing mobile-friendly document views

GroupDocs.Comparison Cloud API enables generating high-quality previews of documents as images, with one image per page.

## Step 1: Upload Documents (If Needed)

Before generating previews, make sure you have uploaded the target document to your cloud storage. For detailed instructions on file uploads, refer to the [File API documentation](https://docs.groupdocs.cloud/comparison/developer-guide/working-with-file-api/).

## Step 2: Understanding the API Resource

To create document previews, we'll use the following GroupDocs.Comparison Cloud REST API resource:

`POST https://api.groupdocs.cloud/v2.0/comparison/preview`

This endpoint requires a JSON body with the file information and preview options.

## Step 3: Implementing with cURL

Let's start with a cURL example to understand the request and response format:

```bash
# First, get the JWT token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Use the token to create document preview
curl -v "https://api.groupdocs.cloud/v2.0/comparison/preview" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'sample2.pdf'
  },
  'Format': 'jpeg',
  'OutputFolder': 'Output'
}"
```

Replace `YOUR_CLIENT_ID`, `YOUR_CLIENT_SECRET`, and `YOUR_JWT_TOKEN` with your actual credentials.

The response will provide information about the generated preview files:

```json
[
  {
    "href": "http://api.groupdocs.cloud/v2.0/comparison/storage/file/Output/sample2_1.jpeg",
    "rel": "Output/sample2_1.jpeg",
    "type": "file",
    "title": "sample2_1.jpeg"
  },
  {
    "href": "http://api.groupdocs.cloud/v2.0/comparison/storage/file/Output/sample2_2.jpeg",
    "rel": "Output/sample2_2.jpeg",
    "type": "file",
    "title": "sample2_2.jpeg"
  }
]
```

## Step 4: Implementing with SDKs

Now let's implement document preview functionality using various SDKs.

### C# Example

```csharp
using System;
using GroupDocs.Comparison.Cloud.Sdk.Api;
using GroupDocs.Comparison.Cloud.Sdk.Client;
using GroupDocs.Comparison.Cloud.Sdk.Model;
using GroupDocs.Comparison.Cloud.Sdk.Model.Requests;

namespace CreateDocumentPreviewExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API credentials
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Create PreviewApi instance
            var apiInstance = new PreviewApi(configuration);
            
            // Prepare preview options
            var options = new PreviewOptions
            {
                FileInfo = new FileInfo { FilePath = "source_files/word/source.docx" },
                Format = PreviewOptions.FormatEnum.Png,
                OutputFolder = "output"
            };
            
            // Create preview request
            var request = new PreviewRequest(options);
            
            try
            {
                // Execute preview generation
                var response = apiInstance.Preview(request);
                
                // Display result information
                Console.WriteLine($"Preview generation completed successfully!");
                Console.WriteLine($"Generated {response.Count} preview images:");
                
                foreach (var link in response)
                {
                    Console.WriteLine($" - {link.Title}: {link.Href}");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine($"Error: {e.Message}");
            }
        }
    }
}
```

### Python Example

```python
import groupdocs_comparison_cloud
from groupdocs_comparison_cloud import PreviewOptions, FileInfo

# Configure API credentials
configuration = groupdocs_comparison_cloud.Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create PreviewApi instance
preview_api = groupdocs_comparison_cloud.PreviewApi.from_keys(
    configuration.client_id, configuration.client_secret
)

# Prepare preview options
file_info = FileInfo()
file_info.file_path = "source_files/word/source.docx"

options = PreviewOptions()
options.file_info = file_info
options.format = "jpg"  # Can be jpg, png, etc.
options.output_folder = "output"

# Create preview request
request = groupdocs_comparison_cloud.PreviewRequest(options)

try:
    # Execute preview generation
    response = preview_api.preview(request)
    
    # Display result information
    print("Preview generation completed successfully!")
    print(f"Generated {len(response)} preview images:")
    
    for link in response:
        print(f" - {link.title}: {link.href}")
except groupdocs_comparison_cloud.ApiException as e:
    print(f"Error: {e}")
```

### Java Example

```java
import com.groupdocs.cloud.comparison.client.*;
import com.groupdocs.cloud.comparison.model.*;
import com.groupdocs.cloud.comparison.api.PreviewApi;
import com.groupdocs.cloud.comparison.model.requests.PreviewRequest;
import java.util.List;

public class CreateDocumentPreviewExample {
    public static void main(String[] args) {
        // Configure API credentials
        Configuration configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        // Create PreviewApi instance
        PreviewApi apiInstance = new PreviewApi(configuration);
        
        // Prepare preview options
        FileInfo fileInfo = new FileInfo();
        fileInfo.setFilePath("source_files/word/source.docx");
        
        PreviewOptions options = new PreviewOptions();
        options.setFileInfo(fileInfo);
        options.setFormat(FormatEnum.JPEG);
        options.setOutputFolder("output");
        
        // Create preview request
        PreviewRequest request = new PreviewRequest(options);
        
        try {
            // Execute preview generation
            List<Link> response = apiInstance.preview(request);
            
            // Display result information
            System.out.println("Preview generation completed successfully!");
            System.out.println("Generated " + response.size() + " preview images:");
            
            for (Link link : response) {
                System.out.println(" - " + link.getTitle() + ": " + link.getHref());
            }
        } catch (ApiException e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Node.js Example

```javascript
// Load the GroupDocs.Comparison Cloud SDK
const comparison = require("groupdocs-comparison-cloud");

// Configure API credentials
const clientId = "YOUR_CLIENT_ID";
const clientSecret = "YOUR_CLIENT_SECRET";

// Create PreviewApi instance
const previewApi = comparison.PreviewApi.fromKeys(clientId, clientSecret);

// Prepare preview options
const fileInfo = new comparison.FileInfo();
fileInfo.filePath = "source_files/word/source.docx";

const options = new comparison.PreviewOptions();
options.fileInfo = fileInfo;
options.format = "png";  // Can be png, jpg, etc.
options.outputFolder = "output";

// Create preview request
const request = new comparison.PreviewRequest(options);

// Execute preview generation
previewApi.preview(request)
    .then(response => {
        console.log("Preview generation completed successfully!");
        console.log(`Generated ${response.length} preview images:`);
        
        response.forEach(link => {
            console.log(` - ${link.title}: ${link.href}`);
        });
    })
    .catch(error => {
        console.log("Error:", error.message);
    });
```

## Step 5: Customizing Preview Options

GroupDocs.Comparison Cloud provides several options to customize document previews:

1. Output Format: Choose between JPEG, PNG, and other image formats
2. Width and Height: Specify custom dimensions for preview images
3. Quality: Adjust image quality for JPEG format (affects file size)
4. Page Range: Generate previews for specific pages only

Here's an example with additional options:

```csharp
// Advanced preview options
var options = new PreviewOptions
{
    FileInfo = new FileInfo { FilePath = "source_files/word/source.docx" },
    Format = PreviewOptions.FormatEnum.Jpeg,
    OutputFolder = "output",
    Width = 800,     // Set preview width
    Height = 1000,   // Set preview height
    PageNumbers = new List<int> { 1, 2, 3 }, // Generate previews for specific pages only
    Resolution = 150 // DPI resolution
}
```

## Step 6: Working with Preview Images

After generating preview images, you can:

1. Download the Images: Use the URLs provided in the response to download the preview images
2. Display in Web Applications: Embed the images in web pages or document viewers
3. Create Thumbnails: Use smaller versions of the images as thumbnails in document lists
4. Combine with Comparison: Generate previews of both original and comparison result documents

Here's a simple example of how to download the preview images using C#:

```csharp
// Download preview images
using (var client = new WebClient())
{
    foreach (var link in response)
    {
        // Extract file name from URL
        string fileName = link.Title;
        
        // Download the file
        client.DownloadFile(link.Href, Path.Combine("Downloads", fileName));
        Console.WriteLine($"Downloaded: {fileName}");
    }
}
```

## Step 7: Integrating Previews in Comparison Workflow

Document previews can enhance your document comparison workflow in several ways:

1. Pre-Comparison Verification: Show users previews of documents before comparison to confirm they're comparing the right files
2. Post-Comparison Review: Generate previews of the comparison result for quick visual assessment
3. Side-by-Side Viewing: Display previews of original and modified documents side by side
4. Mobile-Friendly Access: Provide lightweight image previews for mobile users

## Try It Yourself

Now it's your turn to practice:

1. Upload a multi-page document to your cloud storage
2. Generate previews using the code examples
3. Download and view the preview images
4. Experiment with different preview formats and options
5. Create a simple web page to display the document previews

## Practical Applications

Document previews have numerous practical applications:

1. Document Management Systems: Provide visual thumbnails of documents
2. Web-Based Document Viewers: Show document content without requiring native applications
3. Mobile Applications: Deliver lightweight document views on mobile devices
4. Email Attachments: Generate previews of email attachments
5. Content Approval Workflows: Allow quick visual review of documents before approval

## Troubleshooting Tips

- Empty Preview: Ensure the document is not corrupted and is in a supported format
- Low-Quality Images: Increase the resolution or use PNG format for better quality
- Memory Issues: For very large documents, generate previews page by page
- Font Problems: Some fonts may not render correctly in previews
- File Path Issues: Verify the document exists at the specified path in your storage

## What You've Learned

In this tutorial, you've learned:
- How to generate visual previews of documents as images
- How to customize preview options for different needs
- How to implement document preview functionality in different programming languages
- How to use previews effectively in document comparison workflows
- Practical applications for document previews

## Next Steps

Having completed all the tutorials in this series, you now have a solid foundation in using GroupDocs.Comparison Cloud API for document comparison tasks. To further enhance your skills, you might want to explore:

- Advanced comparison options and settings
- Comparing specific document types (Excel, PowerPoint, etc.)
- Implementing document comparison in your applications
- Automating document comparison workflows

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
