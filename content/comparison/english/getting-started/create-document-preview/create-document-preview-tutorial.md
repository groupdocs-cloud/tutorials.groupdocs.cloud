---
id: "create-document-preview-tutorial"
url: /getting-started/create-document-preview"
title: "Tutorial: How to Create Document Previews with GroupDocs.Comparison Cloud"
productName: "GroupDocs.Comparison Cloud"
weight: 5
description: "Learn how to generate visual previews of documents before and after comparison using GroupDocs.Comparison Cloud API in this step-by-step tutorial"
keywords: "document preview tutorial, generate document images, preview comparison results, groupdocs cloud api tutorial"
toc: True
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
- A GroupDocs.Comparison Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps))
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