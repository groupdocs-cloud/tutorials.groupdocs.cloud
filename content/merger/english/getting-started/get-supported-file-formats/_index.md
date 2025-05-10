---
title: How to Get Supported File Formats with GroupDocs.Merger Cloud API Tutorial
weight: 2
url: /getting-started/get-supported-file-formats/
description: Learn how to programmatically retrieve all supported file formats for document merging with GroupDocs.Merger Cloud in this developer tutorial.
---

# Tutorial: How to Get Supported File Formats with GroupDocs.Merger Cloud API

## Learning Objectives

In this tutorial, you'll learn how to retrieve a list of all file formats that are supported by the GroupDocs.Merger Cloud API. This knowledge is critical for determining which document types you can work with before implementing document merging operations in your applications.

## Prerequisites

Before starting this tutorial, ensure you have:

1. A GroupDocs.Merger Cloud account (or [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret from the [GroupDocs Cloud Dashboard](https://dashboard.groupdocs.cloud/)
3. Basic understanding of REST API concepts
4. Development environment for your preferred language (optional for cURL examples)

## Use Case Scenario

Imagine you're developing a document management application that needs to validate file formats before users upload documents. You need to:
- Present users with a list of supported file types
- Validate uploaded files against supported formats
- Filter documents based on format compatibility

This tutorial will show you how to programmatically retrieve all supported formats to implement these features.

## Step 1: Understanding the Supported File Formats API

The GroupDocs.Merger Cloud API provides a dedicated endpoint for retrieving all supported file formats. This REST API returns a list of file extensions along with their corresponding format descriptions.

The API endpoint for this operation is:

```
HTTP GET ~/formats
```

## Step 2: Preparing Your Request

To retrieve supported file formats, you'll need to:

1. Obtain an access token using your Client ID and Client Secret
2. Make a GET request to the formats endpoint

Let's break this down:

### Step 2.1: Get Your Access Token

First, you need to authenticate with the API to receive a JSON Web Token (JWT):

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

The response will contain an access token that you'll use in the next step.

### Step 2.2: Formulate Your File Formats Request

Now, construct a GET request to retrieve supported file formats:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/formats" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with the token you received in Step 2.1.

## Step 3: Understanding the Response

When you execute the request, you'll receive a JSON response like this (abbreviated for clarity):

```json
{
  "formats": [
    {
      "extension": ".doc",
      "fileFormat": "Microsoft Word Document"
    },
    {
      "extension": ".docm",
      "fileFormat": "Word Open XML Macro-Enabled Document"
    },
    {
      "extension": ".docx",
      "fileFormat": "Microsoft Word Open XML Document"
    },
    ...
    {
      "extension": ".xlsx",
      "fileFormat": "Microsoft Excel Open XML Spreadsheet"
    }
  ]
}
```

This response includes an array of format objects, each containing:
- `extension`: The file extension (e.g., ".docx")
- `fileFormat`: A human-readable description of the file format

## Step 4: Implementing with SDK in Your Application

While you can use direct REST API calls as shown above, GroupDocs provides SDKs for various programming languages to simplify integration. Let's see how to get supported file formats using these SDKs.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new InfoApi(configuration);

var response = apiInstance.GetSupportedFileFormats();

Console.WriteLine("Supported File Formats:");
foreach (var format in response.Formats)
{
    Console.WriteLine($"{format.Extension} - {format.FileFormat}");
}

// Group formats by type
Console.WriteLine("\nWord Processing Formats:");
foreach (var format in response.Formats.Where(f => f.FileFormat.Contains("Word") || f.Extension == ".rtf" || f.Extension == ".txt"))
{
    Console.WriteLine($"{format.Extension} - {format.FileFormat}");
}

Console.WriteLine("\nSpreadsheet Formats:");
foreach (var format in response.Formats.Where(f => f.FileFormat.Contains("Excel") || f.Extension == ".csv"))
{
    Console.WriteLine($"{format.Extension} - {format.FileFormat}");
}

Console.WriteLine("\nPresentation Formats:");
foreach (var format in response.Formats.Where(f => f.FileFormat.Contains("PowerPoint")))
{
    Console.WriteLine($"{format.Extension} - {format.FileFormat}");
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-python-samples
import groupdocs_merger_cloud

client_id = "YOUR_CLIENT_ID" # Get your client ID and secret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get your client ID and secret from https://dashboard.groupdocs.cloud
  
# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(client_id, client_secret)
api = groupdocs_merger_cloud.InfoApi(configuration)

# Retrieve supported file formats
result = api.get_supported_file_formats()

# Print all supported formats
print("Supported File Formats:")
for format_info in result.formats:
    print(f"{format_info.extension} - {format_info.file_format}")

# You can also filter formats by type
print("\nDocument Formats:")
doc_formats = [f for f in result.formats if "Word" in f.file_format or "PDF" in f.file_format]
for format_info in doc_formats:
    print(f"{format_info.extension} - {format_info.file_format}")

print("\nSpreadsheet Formats:")
spreadsheet_formats = [f for f in result.formats if "Excel" in f.file_format or f.extension == ".csv"]
for format_info in spreadsheet_formats:
    print(f"{format_info.extension} - {format_info.file_format}")
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-java-samples
import com.groupdocs.cloud.merger.client.*;
import com.groupdocs.cloud.merger.model.*;
import com.groupdocs.cloud.merger.api.InfoApi;

import java.util.List;
import java.util.stream.Collectors;

public class GetSupportedFileFormatsExample {
    public static void main(String[] args) {
        String clientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
        String clientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
        
        // Create instance of the API
        Configuration configuration = new Configuration(clientId, clientSecret);
        InfoApi api = new InfoApi(configuration);
        
        try {
            // Get supported file formats
            FormatsResult result = api.getSupportedFileFormats();
            
            // Display all supported formats
            System.out.println("Supported File Formats:");
            for (Format format : result.getFormats()) {
                System.out.println(format.getExtension() + " - " + format.getFileFormat());
            }
            
            // Filter and group formats by type
            System.out.println("\nDocument Formats:");
            List<Format> docFormats = result.getFormats().stream()
                .filter(f -> f.getFileFormat().contains("Word") || f.getFileFormat().contains("PDF"))
                .collect(Collectors.toList());
            
            for (Format format : docFormats) {
                System.out.println(format.getExtension() + " - " + format.getFileFormat());
            }
            
            System.out.println("\nPresentation Formats:");
            List<Format> presentationFormats = result.getFormats().stream()
                .filter(f -> f.getFileFormat().contains("PowerPoint"))
                .collect(Collectors.toList());
            
            for (Format format : presentationFormats) {
                System.out.println(format.getExtension() + " - " + format.getFileFormat());
            }
            
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Step 5: Implementing Format Validation in Your Application

Now that you can retrieve supported formats, let's implement a practical file validation function:

### JavaScript Example (Node.js)

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-node-samples
const { InfoApi, Configuration } = require("groupdocs-merger-cloud");

const clientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
const clientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

// Create instance of the API
const configuration = new Configuration(clientId, clientSecret);
const api = new InfoApi(configuration);

// Function to validate file extension
async function isFileFormatSupported(fileExtension) {
    try {
        // Ensure extension starts with a dot
        if (!fileExtension.startsWith('.')) {
            fileExtension = '.' + fileExtension;
        }
        
        // Convert to lowercase for comparison
        fileExtension = fileExtension.toLowerCase();
        
        // Get all supported formats
        const result = await api.getSupportedFileFormats();
        
        // Check if the extension exists in supported formats
        const supported = result.formats.some(format => 
            format.extension.toLowerCase() === fileExtension
        );
        
        return supported;
    } catch (error) {
        console.error("Error validating format:", error);
        return false;
    }
}

// Example usage
async function validateFiles() {
    console.log("Validating formats...");
    console.log("Is .docx supported?", await isFileFormatSupported(".docx"));
    console.log("Is .doc supported?", await isFileFormatSupported("doc")); // Note: no dot prefix
    console.log("Is .xyz supported?", await isFileFormatSupported(".xyz")); // Made-up extension
}

validateFiles();
```

## Try It Yourself: Practice Exercises

Now that you understand how to retrieve and use supported file formats, try these exercises to reinforce your learning:

1. Exercise 1: Create a utility function that groups supported formats by category (Word Processing, Spreadsheets, Presentations, etc.) based on file extensions or format descriptions.

2. Exercise 2: Build a simple file upload component that validates files against supported formats before allowing upload.

3. Exercise 3: Create a dropdown menu that displays all supported formats, categorized by document type.

## Troubleshooting Tips

- Authentication errors: Ensure your Client ID and Client Secret are correct and that your account has appropriate permissions.
- Filtering issues: When grouping formats, remember that some format descriptions may not contain the exact keywords you're filtering on. Consider using multiple conditions or partial matches.
- Extension validation: When validating file extensions, ensure you account for case sensitivity and consistently handle the dot prefix.

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve a complete list of file formats supported by GroupDocs.Merger Cloud
- Process and categorize file formats by type
- Implement file format validation in your applications
- Leverage different programming languages for format retrieval and validation

## Next Steps

Now that you can retrieve and validate supported file formats, a logical next step is to learn how to manage files in your cloud storage. Continue to our [Tutorial: Working with Files](/getting-started/working-with-files/) to expand your knowledge.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback

Did you find this tutorial helpful? Do you have questions about supported file formats? Let us know in our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!
