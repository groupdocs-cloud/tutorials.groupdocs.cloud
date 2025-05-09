---
id: "get-supported-file-formats-tutorial"
url: /getting-started/get-supported-file-formats"
title: "Tutorial: How to Get Supported File Formats in GroupDocs.Comparison Cloud"
productName: "GroupDocs.Comparison Cloud"
weight: 1
description: "Learn how to retrieve all supported file formats for document comparison using GroupDocs.Comparison Cloud API in this beginner-friendly tutorial"
keywords: "comparison api tutorial, get supported formats, document comparison tutorial, groupdocs cloud api"
toc: True
---

# Tutorial: How to Get Supported File Formats in GroupDocs.Comparison Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Make API calls to retrieve all supported file formats for document comparison
- Implement this functionality in different programming languages
- Process and display the list of supported formats in your application

## Prerequisites

Before starting this tutorial, you'll need:
- A GroupDocs.Comparison Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret credentials
- Basic familiarity with RESTful APIs and your chosen programming language

## Understanding the API

GroupDocs.Comparison Cloud API allows you to compare documents of various formats. Before comparing documents, it's important to know which file formats are supported. The API provides a dedicated endpoint to retrieve this information.

## Step 1: Understanding the Resource

To get the list of supported file formats, we'll use the following GroupDocs.Comparison Cloud REST API resource:

`GET https://api.groupdocs.cloud/v2.0/comparison/formats`

This endpoint returns a complete list of file formats that can be compared using the API.

## Step 2: Implementing with cURL

Let's start with a simple cURL example to understand the request and response format.

```bash
# First, get the JWT token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Use the token to get supported formats
curl -v "https://api.groupdocs.cloud/v2.0/comparison/formats" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

Replace `YOUR_CLIENT_ID`, `YOUR_CLIENT_SECRET`, and `YOUR_JWT_TOKEN` with your actual credentials.

The response will be a JSON object containing an array of supported formats with their extensions and descriptions:

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
    ...
  ]
}
```

## Step 3: Implementing with SDK

Now let's implement the same functionality using various SDKs.

### C# Example

```csharp
using System;
using GroupDocs.Comparison.Cloud.Sdk.Api;
using GroupDocs.Comparison.Cloud.Sdk.Client;

namespace GetSupportedFormatsExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API credentials
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Create InfoApi instance
            var infoApi = new InfoApi(configuration);

            try
            {
                // Get supported file formats
                var formats = infoApi.GetSupportedFileFormats();
                
                Console.WriteLine("Supported formats:");
                // Display each supported format
                foreach (var format in formats.Formats)
                {
                    Console.WriteLine($"{format.FileFormat}: {format.Extension}");
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
from groupdocs_comparison_cloud import Configuration

# Configure API credentials
configuration = groupdocs_comparison_cloud.Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create InfoApi instance
api = groupdocs_comparison_cloud.InfoApi.from_config(configuration)

try:
    # Retrieve supported file formats
    response = api.get_supported_file_formats()
    
    # Display the supported formats
    print("Supported formats:")
    for format_info in response.formats:
        print(f"{format_info.file_format}: {format_info.extension}")
except groupdocs_comparison_cloud.ApiException as e:
    print(f"Error: {e}")
```

### Java Example

```java
import com.groupdocs.cloud.comparison.client.*;
import com.groupdocs.cloud.comparison.model.*;
import com.groupdocs.cloud.comparison.api.InfoApi;

public class GetSupportedFormatsExample {
    public static void main(String[] args) {
        // Configure API credentials
        Configuration configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        // Create InfoApi instance
        InfoApi infoApi = new InfoApi(configuration);
        
        try {
            // Retrieve supported file formats
            FormatsResult response = infoApi.getSupportedFileFormats();
            
            // Display the supported formats
            System.out.println("Supported formats:");
            for (Format format : response.getFormats()) {
                System.out.println(format.getFileFormat() + ": " + format.getExtension());
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

// Create InfoApi instance
const infoApi = comparison.InfoApi.fromKeys(clientId, clientSecret);

// Retrieve supported file formats
infoApi.getSupportedFileFormats()
    .then(result => {
        console.log("Supported formats:");
        result.formats.forEach(format => {
            console.log(`${format.fileFormat}: ${format.extension}`);
        });
    })
    .catch(error => {
        console.log("Error:", error.message);
    });
```

## Step 4: Analyzing the Results

After running any of these examples, you'll receive a list of supported file formats. Let's analyze what information we get:

1. File Extension: The file extension (e.g., `.pdf`, `.docx`)
2. File Format: The name of the document format (e.g., "Portable Document Format", "Microsoft Word")

This information is crucial for:
- Validating input files before attempting comparison
- Building user interfaces that show supported formats
- Filtering documents in your application based on comparison capability

## Try It Yourself

Now it's your turn to practice:

1. Implement the code example in your preferred language
2. Modify the code to filter formats by category (e.g., only Microsoft Office formats)
3. Create a simple UI that displays the supported formats to users

## Troubleshooting Tips

- 401 Unauthorized: Ensure your Client ID and Client Secret are correct
- API Connection Issues: Check your network connection and firewall settings
- SDK Not Working: Verify you have the latest version of the SDK installed

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with the GroupDocs.Comparison Cloud API
- How to retrieve a list of supported file formats
- How to implement this functionality in different programming languages
- How to process and use the format information in your applications

## Next Steps

Now that you know which formats are supported, you can proceed to the next tutorial:
- [Tutorial: How to Get Document Information](/getting-started/get-document-information)

This will teach you how to extract essential information from documents before comparing them.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

