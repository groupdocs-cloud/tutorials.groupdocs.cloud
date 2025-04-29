---
title: How to Get Supported File Formats with GroupDocs.Viewer Cloud API Tutorial
url: /basic-usage/get-supported-file-formats/
description: Learn how to retrieve the complete list of file formats supported by GroupDocs.Viewer Cloud API in this beginner-friendly tutorial

---

# Tutorial: How to Get Supported File Formats with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve the complete list of file formats supported by GroupDocs.Viewer Cloud API
- Understand how to check format compatibility programmatically
- Implement format validation in your applications
- Filter formats by type and category

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Viewer Cloud account (get your [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret
- Basic understanding of REST APIs
- Familiarity with your programming language of choice (C#, Java, Python, PHP, Ruby, Node.js, or Go)

## Why Check Supported Formats?

Checking supported file formats is an important step in building robust document viewing applications. Understanding which file types can be processed by GroupDocs.Viewer Cloud API helps you:

- Validate user uploads: Prevent users from uploading unsupported file types
- Plan format conversions: Determine if intermediate conversion steps are needed
- Display format options: Show users which file types they can work with
- Handle errors gracefully: Provide meaningful error messages for unsupported formats
- Optimize user experience: Set appropriate expectations for your application's capabilities

GroupDocs.Viewer Cloud API supports over 50 file formats, including popular document types, images, CAD drawings, and more.

## Step 1: Authenticate with the API

Before retrieving supported formats, you need to authenticate with the API:

```bash
# Get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Store JWT in a variable for reuse
JWT="YOUR_JWT_TOKEN"
```

## Step 2: Retrieve Supported File Formats

Now, let's use the API to retrieve the list of supported formats:

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/viewer/formats" \
-H "accept: application/json" \
-H "authorization: Bearer $JWT"
```

### Understanding the Response

The API returns a comprehensive list of supported formats:

```json
{
  "formats": [
    {
      "extension": ".pdf",
      "fileFormat": "Portable Document Format"
    },
    {
      "extension": ".doc",
      "fileFormat": "Microsoft Word"
    },
    {
      "extension": ".docx",
      "fileFormat": "Microsoft Word"
    },
    {
      "extension": ".xls",
      "fileFormat": "Microsoft Excel"
    },
    {
      "extension": ".xlsx",
      "fileFormat": "Microsoft Excel"
    },
    // ... more formats
  ]
}
```

Each format entry includes:
- extension: The file extension (e.g., ".pdf", ".docx")
- fileFormat: The human-readable format name (e.g., "Microsoft Word", "Portable Document Format")

## Step 3: Implement in Your Application

Now let's implement format retrieval in a real application using one of our supported SDKs.

### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using System;
using System.Collections.Generic;
using System.Linq;

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

            try
            {
                // Call the API to get supported file formats
                var response = apiInstance.GetSupportedFileFormats();
                
                Console.WriteLine($"Total supported formats: {response.Formats.Count}\n");
                
                // Display all supported formats
                Console.WriteLine("All supported formats:");
                foreach (var format in response.Formats)
                {
                    Console.WriteLine($"  {format.FileFormat} ({format.Extension})");
                }
                
                // Group formats by category
                var formatsByCategory = response.Formats
                    .GroupBy(f => f.FileFormat)
                    .OrderBy(g => g.Key);
                
                Console.WriteLine("\nFormats by category:");
                foreach (var category in formatsByCategory)
                {
                    var extensions = string.Join(", ", category.Select(f => f.Extension));
                    Console.WriteLine($"  {category.Key}: {extensions}");
                }
                
                // Get Microsoft Office formats
                var officeFormats = response.Formats
                    .Where(f => f.FileFormat.StartsWith("Microsoft"))
                    .ToList();
                
                Console.WriteLine("\nMicrosoft Office formats:");
                foreach (var format in officeFormats)
                {
                    Console.WriteLine($"  {format.FileFormat} ({format.Extension})");
                }
                
                // Check if a specific format is supported
                string formatToCheck = ".docx";
                bool isSupported = response.Formats.Any(f => f.Extension.Equals(formatToCheck, StringComparison.OrdinalIgnoreCase));
                
                Console.WriteLine($"\nIs {formatToCheck} supported? {isSupported}");
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
from collections import defaultdict

# Get your client ID and client secret from https://dashboard.groupdocs.cloud/
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create API instance
api_instance = groupdocs_viewer_cloud.InfoApi.from_keys(client_id, client_secret)

try:
    # Call the API to get supported file formats
    response = api_instance.get_supported_file_formats()
    
    print(f"Total supported formats: {len(response.formats)}\n")
    
    # Display all supported formats
    print("All supported formats:")
    for format_info in response.formats:
        print(f"  {format_info.file_format} ({format_info.extension})")
    
    # Group formats by category
    formats_by_category = defaultdict(list)
    for format_info in response.formats:
        formats_by_category[format_info.file_format].append(format_info.extension)
    
    print("\nFormats by category:")
    for category, extensions in sorted(formats_by_category.items()):
        print(f"  {category}: {', '.join(extensions)}")
    
    # Get image formats
    image_formats = [f for f in response.formats if f.file_format == "Image files"]
    
    print("\nImage formats:")
    for format_info in image_formats:
        print(f"  {format_info.extension}")
    
    # Check if a specific format is supported
    format_to_check = ".pptx"
    is_supported = any(f.extension.lower() == format_to_check.lower() for f in response.formats)
    
    print(f"\nIs {format_to_check} supported? {is_supported}")

except groupdocs_viewer_cloud.ApiException as e:
    print(f"Exception while calling InfoApi: {e}")
```

## Step 4: Creating a Format Validator

Let's create a utility function that validates file formats for your application:

### C# Format Validator

```csharp
public class FormatValidator
{
    private List<string> _supportedExtensions;
    
    public FormatValidator(InfoApi api)
    {
        // Retrieve supported formats and store extensions
        var formats = api.GetSupportedFileFormats();
        _supportedExtensions = formats.Formats
            .Select(f => f.Extension.ToLowerInvariant())
            .ToList();
    }
    
    public bool IsFormatSupported(string filePath)
    {
        // Extract file extension
        string extension = System.IO.Path.GetExtension(filePath).ToLowerInvariant();
        
        // Check if extension is in supported list
        return _supportedExtensions.Contains(extension);
    }
    
    public List<string> GetAllSupportedExtensions()
    {
        return _supportedExtensions;
    }
    
    public List<string> FilterByCategory(string categoryName)
    {
        // This would require storing the category information as well
        // For simplicity, we'll return all extensions
        return _supportedExtensions;
    }
}

// Usage:
var validator = new FormatValidator(apiInstance);
bool canProcess = validator.IsFormatSupported("document.docx");
```

### Python Format Validator

```python
class FormatValidator:
    def __init__(self, api):
        # Retrieve supported formats and store extensions
        formats = api.get_supported_file_formats()
        self.supported_extensions = [f.extension.lower() for f in formats.formats]
        
        # Create a dictionary for formats by category
        self.formats_by_category = {}
        for format_info in formats.formats:
            category = format_info.file_format
            if category not in self.formats_by_category:
                self.formats_by_category[category] = []
            self.formats_by_category[category].append(format_info.extension.lower())
    
    def is_format_supported(self, file_path):
        # Extract file extension
        import os
        extension = os.path.splitext(file_path)[1].lower()
        
        # Check if extension is in supported list
        return extension in self.supported_extensions
    
    def get_all_supported_extensions(self):
        return self.supported_extensions
    
    def filter_by_category(self, category_name):
        return self.formats_by_category.get(category_name, [])

# Usage:
validator = FormatValidator(api_instance)
can_process = validator.is_format_supported("document.docx")
```

## Step 5: Building a Format Selection Component

Let's create a simple HTML component that displays supported formats to users:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Supported File Formats</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        h1, h2 {
            color: #333;
        }
        .format-container {
            margin-bottom: 30px;
        }
        .format-categories {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 20px;
        }
        .category-button {
            padding: 8px 15px;
            background-color: #f0f0f0;
            border: 1px solid #ddd;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
        }
        .category-button.active {
            background-color: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }
        .formats-list {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 20px;
        }
        .format-badge {
            padding: 6px 12px;
            background-color: #e9e9e9;
            border-radius: 20px;
            font-size: 14px;
        }
        .upload-section {
            margin-top: 30px;
            padding: 20px;
            border: 2px dashed #ccc;
            border-radius: 8px;
            text-align: center;
        }
        .file-input {
            display: none;
        }
        .upload-button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        .format-message {
            margin-top: 15px;
            padding: 10px;
            border-radius: 4px;
            display: none;
        }
        .success {
            background-color: #dff0d8;
            color: #3c763d;
        }
        .error {
            background-color: #f2dede;
            color: #a94442;
        }
    </style>
</head>
<body>
    <h1>Supported File Formats</h1>
    
    <div class="format-container">
        <h2>Select Format Category</h2>
        
        <div class="format-categories">
            <button class="category-button active" data-category="all">All Formats</button>
            <button class="category-button" data-category="Microsoft Word">Word Documents</button>
            <button class="category-button" data-category="Microsoft Excel">Excel Spreadsheets</button>
            <button class="category-button" data-category="Microsoft PowerPoint">PowerPoint Presentations</button>
            <button class="category-button" data-category="Image files">Images</button>
            <button class="category-button" data-category="Portable Document Format">PDF</button>
            <button class="category-button" data-category="Microsoft Visio">Visio Diagrams</button>
            <button class="category-button" data-category="OpenDocument Formats">OpenDocument</button>
        </div>
        
        <h3>Supported Formats</h3>
        <div class="formats-list" id="formatsList">
            <!-- Format badges will be added here dynamically -->
        </div>
    </div>
    
    <div class="upload-section">
        <h2>Check Your File</h2>
        <p>Upload a file to check if it's supported by our viewer</p>
        
        <input type="file" id="fileInput" class="file-input">
        <button class="upload-button" id="uploadButton">Select File</button>
        
        <div class="format-message" id="formatMessage"></div>
    </div>
    
    <script>
        // This would typically be loaded from your backend API
        const supportedFormats = [
            { extension: ".pdf", fileFormat: "Portable Document Format" },
            { extension: ".docx", fileFormat: "Microsoft Word" },
            { extension: ".doc", fileFormat: "Microsoft Word" },
            { extension: ".xlsx", fileFormat: "Microsoft Excel" },
            { extension: ".xls", fileFormat: "Microsoft Excel" },
            { extension: ".pptx", fileFormat: "Microsoft PowerPoint" },
            { extension: ".ppt", fileFormat: "Microsoft PowerPoint" },
            { extension: ".jpg", fileFormat: "Image files" },
            { extension: ".jpeg", fileFormat: "Image files" },
            { extension: ".png", fileFormat: "Image files" },
            { extension: ".gif", fileFormat: "Image files" },
            { extension: ".bmp", fileFormat: "Image files" },
            { extension: ".tiff", fileFormat: "Image files" },
            { extension: ".vsdx", fileFormat: "Microsoft Visio" },
            { extension: ".vsd", fileFormat: "Microsoft Visio" },
            { extension: ".odt", fileFormat: "OpenDocument Formats" },
            { extension: ".ods", fileFormat: "OpenDocument Formats" },
            { extension: ".odp", fileFormat: "OpenDocument Formats" }
            // Add more formats as needed
        ];
        
        document.addEventListener('DOMContentLoaded', function() {
            const formatsList = document.getElementById('formatsList');
            const categoryButtons = document.querySelectorAll('.category-button');
            const fileInput = document.getElementById('fileInput');
            const uploadButton = document.getElementById('uploadButton');
            const formatMessage = document.getElementById('formatMessage');
            
            // Initial population of all formats
            displayFormats('all');
            
            // Category button click handlers
            categoryButtons.forEach(button => {
                button.addEventListener('click', function() {
                    // Update active button
                    categoryButtons.forEach(btn => btn.classList.remove('active'));
                    this.classList.add('active');
                    
                    // Display formats for the selected category
                    const category = this.getAttribute('data-category');
                    displayFormats(category);
                });
            });
            
            // Upload button click handler
            uploadButton.addEventListener('click', function() {
                fileInput.click();
            });
            
            // File input change handler
            fileInput.addEventListener('change', function() {
                if (this.files && this.files[0]) {
                    const file = this.files[0];
                    const fileName = file.name;
                    const fileExtension = '.' + fileName.split('.').pop().toLowerCase();
                    
                    // Check if the file format is supported
                    const isSupported = supportedFormats.some(format => 
                        format.extension.toLowerCase() === fileExtension
                    );
                    
                    // Display appropriate message
                    formatMessage.style.display = 'block';
                    if (isSupported) {
                        formatMessage.className = 'format-message success';
                        formatMessage.textContent = `✅ Great! The file format "${fileExtension}" is supported.`;
                    } else {
                        formatMessage.className = 'format-message error';
                        formatMessage.textContent = `❌ Sorry, the file format "${fileExtension}" is not supported.`;
                    }
                }
            });
            
            // Function to display formats based on selected category
            function displayFormats(category) {
                // Clear current formats
                formatsList.innerHTML = '';
                
                // Filter formats by category
                const filteredFormats = category === 'all' 
                    ? supportedFormats 
                    : supportedFormats.filter(format => format.fileFormat === category);
                
                // Add format badges
                filteredFormats.forEach(format => {
                    const badge = document.createElement('div');
                    badge.className = 'format-badge';
                    badge.textContent = format.extension;
                    formatsList.appendChild(badge);
                });
            }
        });
    </script>
</body>
</html>
```

This HTML page provides a user-friendly interface for:
- Viewing all supported formats
- Filtering formats by category
- Testing if a specific file is supported

## Try It Yourself

Now that you've learned how to retrieve and work with supported file formats, try these exercises:

### Exercise 1: Create a Format Filter

Create a utility that categorizes file formats into groups like "Documents", "Spreadsheets", "Presentations", "Images", etc. based on their descriptions.

### Exercise 2: Build a File Upload Validator

Create a simple web form that validates uploaded files against the list of supported formats and provides immediate feedback to users.

### Exercise 3: Format Statistics

Generate statistics about the supported formats, such as:
- Total number of supported formats
- Number of formats by category
- Most common file extensions

## Practical Applications

Let's explore some practical applications of format information:

### Upload Component with Format Validation

```javascript
function createUploadComponent(supportedFormats) {
    // Create upload component with format validation
    const component = {
        validateFile: function(file) {
            const fileName = file.name;
            const fileExtension = '.' + fileName.split('.').pop().toLowerCase();
            
            return supportedFormats.some(format => 
                format.extension.toLowerCase() === fileExtension
            );
        },
        
        getAcceptAttribute: function() {
            // Generate the "accept" attribute for HTML file inputs
            return supportedFormats.map(format => format.extension).join(',');
        },
        
        getFormatCategories: function() {
            // Get unique categories
            const categories = new Set(supportedFormats.map(format => format.fileFormat));
            return Array.from(categories);
        },
        
        getFileTypeDescription: function(fileName) {
            const fileExtension = '.' + fileName.split('.').pop().toLowerCase();
            const format = supportedFormats.find(format => 
                format.extension.toLowerCase() === fileExtension
            );
            
            return format ? format.fileFormat : 'Unknown format';
        }
    };
    
    return component;
}

// Usage example:
const uploadComponent = createUploadComponent(supportedFormats);
console.log('Accept attribute:', uploadComponent.getAcceptAttribute());
console.log('Is sample.docx supported:', uploadComponent.validateFile({name: 'sample.docx'}));
console.log('File type:', uploadComponent.getFileTypeDescription('sample.xlsx'));
```

### File Format Documentation Generator

```javascript
function generateFormatDocumentation(supportedFormats) {
    // Group formats by category
    const formatsByCategory = {};
    
    supportedFormats.forEach(format => {
        if (!formatsByCategory[format.fileFormat]) {
            formatsByCategory[format.fileFormat] = [];
        }
        
        formatsByCategory[format.fileFormat].push(format.extension);
    });
    
    // Generate documentation HTML
    let html = '<div class="format-documentation">';
    html += '<h2>Supported File Formats</h2>';
    
    for (const category in formatsByCategory) {
        html += `<div class="format-category">`;
        html += `<h3>${category}</h3>`;
        html += `<p>Supported extensions: ${formatsByCategory[category].join(', ')}</p>`;
        html += `</div>`;
    }
    
    html += '</div>';
    
    return html;
}

// Usage example:
const documentationHtml = generateFormatDocumentation(supportedFormats);
document.getElementById('documentation').innerHTML = documentationHtml;
```

## Troubleshooting Tips

- Authentication Issues: Ensure your Client ID and Client Secret are correct and that you're generating a fresh JWT token
- API Changes: The list of supported formats may be updated over time as new formats are added to the API
- Extension vs. Format: Remember that some file formats may share the same extension (e.g., ".xml" can be used for different types of XML-based formats)
- MIME Types: If you're working with web uploads, you might need to map extensions to MIME types

## What You've Learned

In this tutorial, you've learned:
- How to retrieve the complete list of file formats supported by GroupDocs.Viewer Cloud API
- How to implement format validation in your applications
- How to categorize and filter formats by type
- How to build user-friendly interfaces for format selection
- How to create utilities for working with format information

## Next Steps

Ready to put your knowledge into practice? Check out these related tutorials:
- [Document Information Tutorial](/basic-usage/get-document-information/)
- [HTML Viewer Basic Tutorial](/basic-usage/html-viewer/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Have questions about supported file formats? Need help implementing format validation in your application? We welcome your feedback and questions on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).