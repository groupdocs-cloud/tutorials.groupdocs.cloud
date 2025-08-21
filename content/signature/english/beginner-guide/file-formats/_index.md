---
title: "Supported File Formats in GroupDocs.Signature Cloud"
id: "get-supported-file-formats-tutorial"
url: /beginner-guide/file-formats/
productName: "GroupDocs.Signature Cloud"
weight: 1
description: "This beginner tutorial teaches you how to retrieve all supported file formats in GroupDocs.Signature Cloud API to validate document compatibility."
keywords: "signature api tutorial, groupdocs.signature cloud formats, supported document formats tutorial, learn file format validation"
toc: True
---

# Tutorial: Learn Supported File Formats in GroupDocs.Signature Cloud

## Learning Objectives

In this tutorial, you'll learn:
- How to query all document formats supported by GroupDocs.Signature Cloud
- How to implement format validation in your applications
- How to interpret and use the format information returned by the API

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Signature Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
- Basic understanding of REST APIs
- Familiarity with your programming language of choice (C#, Java, PHP, etc.)
- Development environment set up for your chosen SDK

## Understanding File Format Support

Before applying signatures to documents, it's essential to verify whether your document formats are supported by the API. GroupDocs.Signature Cloud supports a wide range of document formats including PDF, Microsoft Office documents, images, and more.

### Why This Matters

Checking for format support helps you:
- Validate user uploads before processing
- Provide clear feedback to users about acceptable formats
- Ensure compatibility with your signature workflows

## Step 1: Setting Up Authentication

All GroupDocs.Signature Cloud API calls require authentication using your client credentials.

1. Obtain your Client ID and Client Secret from [GroupDocs Dashboard](https://dashboard.groupdocs.cloud/#/apps)
2. Use these credentials to authenticate your API requests

Here's how to generate an access token:

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

## Step 2: Making the API Request

To get all supported file formats, you'll make a GET request to the formats endpoint:

### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/signature/formats" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Try It Yourself

Replace `YOUR_ACCESS_TOKEN` with the token you received in Step 1, then run the command in your terminal. You should receive a JSON response with all supported formats.

## Step 3: Understanding the Response

The API returns a JSON object containing an array of supported formats. Each format includes:
- **extension**: The file extension (e.g., ".pdf", ".docx")
- **fileFormat**: A human-readable format name (e.g., "Portable Document Format", "Microsoft Word")

Here's a sample of what you'll see in the response:

```json
{
  "formats": [
    {
      "extension": ".pdf",
      "fileFormat": "Portable Document Format"
    },
    {
      "extension": ".docx",
      "fileFormat": "Microsoft Word"
    },
    ...
  ]
}
```

## Step 4: Implementing Format Validation

Now let's implement this in your application using SDK examples:

### C# Example

```csharp
// Get supported file formats in C#
// Tutorial Code Example
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new InfoApi(configuration);

var response = apiInstance.GetSupportedFileFormats();
foreach (var format in response.Formats)
{
    Console.WriteLine($"Extension: {format.Extension}, Format: {format.FileFormat}");
}

// Example of validation function:
bool IsFormatSupported(string fileExtension, IEnumerable<FileFormat> supportedFormats)
{
    // Ensure extension starts with a dot
    if (!fileExtension.StartsWith("."))
        fileExtension = "." + fileExtension;
        
    return supportedFormats.Any(format => format.Extension.Equals(fileExtension, StringComparison.OrdinalIgnoreCase));
}
```

### Java Example

```java
// Get supported file formats in Java
// Tutorial Code Example
String MyClientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
String MyClientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
InfoApi apiInstance = new InfoApi(configuration);

FormatsResult response = apiInstance.getSupportedFileFormats();
for (FileFormat format : response.getFormats()) {
    System.out.println("Extension: " + format.getExtension() + ", Format: " + format.getFileFormat());
}

// Example of validation method:
public boolean isFormatSupported(String fileExtension, List<FileFormat> supportedFormats) {
    // Ensure extension starts with a dot
    if (!fileExtension.startsWith("."))
        fileExtension = "." + fileExtension;
        
    for (FileFormat format : supportedFormats) {
        if (format.getExtension().equalsIgnoreCase(fileExtension)) {
            return true;
        }
    }
    return false;
}
```

### Python Example

```python
# Get supported file formats in Python
# Tutorial Code Example
from groupdocs_signature_cloud import *
import groupdocs_signature_cloud

client_id = "YOUR_CLIENT_ID"  # Get from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET"  # Get from https://dashboard.groupdocs.cloud

api = groupdocs_signature_cloud.InfoApi.from_keys(client_id, client_secret)

response = api.get_supported_file_formats()
for format in response.formats:
    print(f"Extension: {format.extension}, Format: {format.file_format}")

# Example of validation function:
def is_format_supported(file_extension, supported_formats):
    # Ensure extension starts with a dot
    if not file_extension.startswith("."):
        file_extension = "." + file_extension
        
    return any(format.extension.lower() == file_extension.lower() for format in supported_formats)
```

## Step 5: Implementing a Format Checker in Your Application

Now, let's create a practical example of a format checker for file uploads:

```javascript
// Example JavaScript frontend code for format validation
async function validateFileFormat(file) {
    try {
        // Call your backend which accesses the GroupDocs API
        const response = await fetch('/api/supported-formats');
        const data = await response.json();
        
        // Extract file extension from filename
        const extension = '.' + file.name.split('.').pop().toLowerCase();
        
        // Check if extension is in supported formats
        const isSupported = data.formats.some(format => 
            format.extension.toLowerCase() === extension
        );
        
        if (isSupported) {
            return { valid: true, message: "File format is supported" };
        } else {
            return { 
                valid: false, 
                message: `Format ${extension} is not supported. Please upload one of: ${getSupportedExtensionsList(data.formats)}` 
            };
        }
    } catch (error) {
        console.error("Error validating format:", error);
        return { valid: false, message: "Error checking format support" };
    }
}

function getSupportedExtensionsList(formats) {
    // Return the first 5 common formats for display
    return formats
        .slice(0, 5)
        .map(format => format.extension)
        .join(', ') + '...';
}
```

## Troubleshooting Tips

Common issues you might encounter:

- **Authentication errors**: Ensure your access token is valid and not expired
- **Empty formats list**: Check your API connection and ensure your account has proper access
- **Case sensitivity**: Remember that file extensions should be compared case-insensitively

## What You've Learned

In this tutorial, you've learned:
- How to retrieve all document formats supported by GroupDocs.Signature Cloud
- How to implement format validation in different programming languages
- How to create a practical file format validator for user uploads

## Further Practice

To reinforce your learning:
1. Implement a file format filter for a file upload component
2. Create a helper function that groups formats by category (e.g., Microsoft Office, Images)
3. Build a user-friendly error message system that suggests alternative formats

## Additional Resources

- [Product Page](https://products.groupdocs.cloud/signature/)
- [Documentation](https://docs.groupdocs.cloud/signature/)
- [Live Demo](https://products.groupdocs.app/signature/family)
- [API Reference](https://reference.groupdocs.cloud/signature/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.signature-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/signature/13/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
