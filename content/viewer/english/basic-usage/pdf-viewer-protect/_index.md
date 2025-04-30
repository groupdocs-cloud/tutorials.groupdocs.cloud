---
title: How to Secure PDF Documents with GroupDocs.Viewer Cloud API Tutorial
description: Learn how to implement PDF security features including passwords and permissions using GroupDocs.Viewer Cloud API in this step-by-step tutorial
url: /basic-usage/pdf-viewer-protect/
weight: 9
---

# Tutorial: How to Secure PDF Documents with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Implement password protection for PDF documents
- Set document permissions to control what users can do with your PDFs
- Configure different levels of security for different use cases
- Apply these security features using the GroupDocs.Viewer Cloud API

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Viewer Cloud account (get your [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret
- Basic understanding of PDF security concepts
- A document to convert to a secured PDF (we'll use a sample DOCX in this tutorial)

## Understanding PDF Security

PDF security is essential in many business scenarios, such as:
- Sharing confidential documents
- Distributing content with usage restrictions
- Protecting intellectual property
- Complying with data protection regulations

GroupDocs.Viewer Cloud API allows you to implement two primary security features when rendering documents to PDF:

1. Document Open Password - Requires users to enter a password before they can open the document
2. Permissions Password - Allows you to set restrictions on what users can do with the document once opened, such as printing, copying text, or modifying

## Step 1: Setting Up PDF Security

To implement PDF security, you need to configure the appropriate options when rendering a document to PDF. Let's start with a simple cURL example:

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Store JWT in a variable for reuse
JWT="YOUR_JWT_TOKEN"

# Render document with security settings
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer $JWT" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.docx'
  },
  'ViewFormat': 'PDF',
  'RenderOptions': {
     'Permissions': ['DenyModification'],
     'PermissionsPassword': 'p123',
     'DocumentOpenPassword': 'o123'
  }
}"
```

In this example:
- `DocumentOpenPassword` sets a password that users must enter to open the document
- `PermissionsPassword` sets a password for changing permissions
- `Permissions` specifies which actions to restrict (in this case, document modification)

## Step 2: Understanding Available Permissions

GroupDocs.Viewer Cloud API supports the following permission settings:

| Permission Value | Description |
|------------------|-------------|
| `AllowAll` | Allows all operations |
| `DenyPrinting` | Prevents users from printing the document |
| `DenyModification` | Prevents users from modifying the document |
| `DenyContentCopy` | Prevents users from copying content |
| `DenyFormFilling` | Prevents users from filling forms |
| `DenyScreenReaders` | Prevents screen readers from reading the document |
| `DenyAssembly` | Prevents users from assembling the document |
| `DenyDegradedPrinting` | Prevents low-quality printing |

You can combine multiple permissions in your request:

```json
"Permissions": ["DenyPrinting", "DenyModification", "DenyContentCopy"]
```

## Step 3: Download and Test Your Secured PDF

After rendering the document, you'll receive a response like this:

```json
{
  "pages": null,
  "attachments": [],
  "file": {
    "path": "viewer/sample_docx/sample.pdf",
    "downloadUrl": "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample.pdf"
  }
}
```

Download the secured PDF to test the security features:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewer/sample_docx/sample.pdf" \
-X GET \
-H "Authorization: Bearer $JWT" \
--output "secured_document.pdf"
```

Open the PDF in any PDF reader. You should be prompted for the document open password ("o123" in our example). After opening, try to modify the document - you should find that the operation is restricted.

## Step 4: Implement in Your Application

Now let's implement PDF security in a real application using one of our supported SDKs.

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

            // Define rendering options with security settings
            var viewOptions = new ViewOptions
            {
                FileInfo = new FileInfo
                {
                    FilePath = "SampleFiles/sample.docx"
                },
                ViewFormat = ViewOptions.ViewFormatEnum.PDF,
                RenderOptions = new PdfOptions
                {
                    Permissions = new List<string> { "DenyModification", "DenyPrinting" },
                    PermissionsPassword = "p123",
                    DocumentOpenPassword = "o123"
                }
            };

            try
            {
                // Call the API to render the document
                var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
                
                Console.WriteLine("Document rendered with security settings!");
                Console.WriteLine($"PDF file path: {response.File.Path}");
                Console.WriteLine($"Download URL: {response.File.DownloadUrl}");
                
                // Download the secured PDF (optional)
                using (var webClient = new System.Net.WebClient())
                {
                    webClient.Headers.Add("Authorization", "Bearer " + configuration.GetToken());
                    webClient.DownloadFile(response.File.DownloadUrl, "secured_document.pdf");
                    Console.WriteLine("Secured PDF downloaded as secured_document.pdf");
                }
                
                Console.WriteLine("\nSecurity settings applied:");
                Console.WriteLine("- Document open password: o123");
                Console.WriteLine("- Permissions password: p123");
                Console.WriteLine("- Restricted actions: Modification, Printing");
                
                // Open the PDF in default viewer
                Process.Start(new ProcessStartInfo
                {
                    FileName = "secured_document.pdf",
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
import webbrowser
import requests
import groupdocs_viewer_cloud

# Get your client ID and client secret from https://dashboard.groupdocs.cloud/
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create API instance
api_instance = groupdocs_viewer_cloud.ViewApi.from_keys(client_id, client_secret)

# Define rendering options with security settings
view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "SampleFiles/sample.docx"
view_options.view_format = "PDF"
view_options.render_options = groupdocs_viewer_cloud.PdfOptions()
view_options.render_options.permissions = ["DenyModification", "DenyPrinting"]
view_options.render_options.permissions_password = "p123"
view_options.render_options.document_open_password = "o123"

try:
    # Call the API to render the document
    request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
    response = api_instance.create_view(request)
    
    print("Document rendered with security settings!")
    print(f"PDF file path: {response.file.path}")
    print(f"Download URL: {response.file.download_url}")
    
    # Download the secured PDF (optional)
    pdf_response = requests.get(
        response.file.download_url,
        headers={"Authorization": f"Bearer {api_instance.configuration.access_token}"}
    )
    
    if pdf_response.status_code == 200:
        with open("secured_document.pdf", "wb") as file:
            file.write(pdf_response.content)
            print("Secured PDF downloaded as secured_document.pdf")
    
    print("\nSecurity settings applied:")
    print("- Document open password: o123")
    print("- Permissions password: p123")
    print("- Restricted actions: Modification, Printing")
    
    # Open the PDF in default viewer
    pdf_path = os.path.abspath("secured_document.pdf")
    print(f"Opening {pdf_path} in default PDF viewer")
    webbrowser.open(f'file://{pdf_path}')

except groupdocs_viewer_cloud.ApiException as e:
    print(f"Exception while calling ViewApi: {e}")
```

## Common Security Scenarios

Let's explore some common PDF security scenarios and how to implement them:

### Scenario 1: Confidential Document (Open Password Only)

Use this when you want to restrict who can open the document, but allow full access once opened:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/confidential.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    RenderOptions = new PdfOptions
    {
        DocumentOpenPassword = "secret123"
    }
};
```

### Scenario 2: Read-Only Document (No Modifications)

Use this when you want anyone to open the document, but prevent editing:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/contract.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    RenderOptions = new PdfOptions
    {
        Permissions = new List<string> { "DenyModification" },
        PermissionsPassword = "admin123"
    }
};
```

### Scenario 3: Copyright Protected Content (No Copying)

Use this when you want to prevent copying of content:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/copyrighted.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    RenderOptions = new PdfOptions
    {
        Permissions = new List<string> { "DenyContentCopy" },
        PermissionsPassword = "admin123"
    }
};
```

### Scenario 4: Maximum Security (All Restrictions)

Use this when you need maximum security:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/topsecret.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    RenderOptions = new PdfOptions
    {
        DocumentOpenPassword = "open123",
        Permissions = new List<string> { 
            "DenyPrinting", 
            "DenyModification", 
            "DenyContentCopy", 
            "DenyFormFilling", 
            "DenyScreenReaders", 
            "DenyAssembly" 
        },
        PermissionsPassword = "admin456"
    }
};
```

## Try It Yourself

Now that you've learned how to secure PDFs using GroupDocs.Viewer Cloud API, try implementing different security configurations for your own documents.

### Exercise: Create a Document with Custom Security Settings

1. Render a document to PDF with custom security settings based on your requirements
2. Test the PDF in different PDF readers to ensure the security settings work as expected
3. Try implementing different combinations of permission restrictions

## Troubleshooting Tips

- Password Not Working: Ensure you're using the correct password and that it's properly encoded in your API request
- Permissions Not Applied: Check that you're using the correct permission values and that the PDF reader you're using supports the restrictions
- API Errors: Verify that your Client ID and Client Secret are correct and that you have permission to access the document

## What You've Learned

In this tutorial, you've learned:
- How to implement password protection for PDF documents
- How to set document permissions to restrict what users can do with your PDFs
- How to configure different security scenarios for various use cases
- How to implement PDF security in your applications using SDKs

## Next Steps

Ready to explore more PDF functionality? Check out these related tutorial:
- [PDF Viewer Basic Tutorial](/basic-usage/pdf-viewer/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Have questions about PDF security? Need help implementing it in your application? We welcome your feedback and questions on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
