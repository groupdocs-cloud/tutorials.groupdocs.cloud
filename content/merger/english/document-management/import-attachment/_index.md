---
title: How to Import Attachments into PDF Documents Tutorial
url: /document-management/import-attachment/
weight: 40
description: Learn how to add file attachments to PDF documents using GroupDocs.Merger Cloud API in this step-by-step developer tutorial.
---

# Tutorial: How to Import Attachments into PDF Documents

## Introduction

In this tutorial, you'll learn how to import file attachments into PDF documents using GroupDocs.Merger Cloud API. PDF attachments are a powerful feature that allows you to embed and distribute supplementary files along with your PDF document. This capability is particularly useful for including reference materials, source data, or related documents that provide additional context to the main PDF content.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Understand the concept of PDF attachments and their benefits
- Import file attachments into PDF documents using GroupDocs.Merger Cloud
- Implement attachment import operations in different programming languages
- Handle password-protected PDF documents during attachment operations

## Prerequisites

Before starting this tutorial, make sure you have:
- A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic knowledge of RESTful APIs
- A PDF document to add attachments to
- Files to attach (text, images, documents, etc.)

## Use Case Scenario

Let's consider a practical scenario: You're developing a contract management system that needs to include supporting documents along with the main contract PDF. For example, a sales contract might need to include specification sheets, terms and conditions, and other legal documents as attachments rather than merging them all into a single document. This way, users can easily access these supplementary materials while keeping the main contract clean and focused.

## Understanding PDF Attachments

PDF attachments are files embedded within a PDF document. They can be of any file type (documents, images, spreadsheets, etc.) and travel with the PDF document. Key benefits include:

1. File Integrity: Attached files maintain their original format and can be extracted and opened in their native applications.
2. Document Consolidation: Multiple related files can be distributed as a single package.
3. Reduced File Size: Compared to merging all content into a single PDF, attachments can be more space-efficient.
4. Enhanced Referencing: Readers can easily access supplementary materials without breaking the flow of the main document.

## Implementation Steps

### Step 1: Authenticate with the API

First, authenticate with the GroupDocs.Merger Cloud API by obtaining a JWT token using your Client ID and Client Secret.

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

### Step 2: Upload Files to Cloud Storage

Before importing attachments, ensure that both your PDF document and the files you want to attach are uploaded to your GroupDocs Cloud storage.

### Step 3: Prepare Your Import Attachment Request

To import attachments into a PDF document, create a JSON request specifying:
- The PDF document to add attachments to
- The file paths of the attachments to import
- Output path for the resultant PDF document
- Password (if the PDF is protected)

Here's a basic request structure for importing attachments:

```json
{
    "FileInfo": {
        "FilePath": "Pdf/one-page-password.pdf",
        "Password": "password"
    },
    "Attachments": [ "Txt/document.txt" ],
    "OutputPath": "Output/with-attachment.pdf"
}
```

### Step 4: Import Attachments Using the API

Now let's execute the attachment import operation using the API:

#### cURL Example

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/import" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo':
    {
        'FilePath': 'Pdf/one-page-password.pdf',
        'Password': 'password'
    },    
    'Attachments': [ 'Txt/document.txt' ],
    'OutputPath': 'Output/with-attachment.pdf'
}"
```

The API will respond with:

```json
{
  "path": "Output/with-attachment.pdf"
}
```

### Step 5: Retrieve and Verify the PDF with Attachments

After successful importing, you can download the PDF with attachments from the output path provided in the response. Open the PDF in a compatible reader to verify that the attachments are present and accessible.

## Try It Yourself

Let's implement attachment importing in different programming languages. First, make sure your PDF document and attachment files are uploaded to your GroupDocs Cloud storage, then use the appropriate SDK for your language.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new DocumentApi(configuration);

try
{
    // Configure import options
    var options = new ImportOptions
    {
        FileInfo = new FileInfo
        {
            FilePath = "Pdf/one-page-password.pdf",
            Password = "password" // If your PDF is password-protected
        },
        Attachments = new List<string> { "Txt/document.txt" },
        OutputPath = "Output/with-attachment.pdf"
    };

    // Execute import operation
    var request = new ImportRequest(options);
    var response = apiInstance.Import(request);

    Console.WriteLine("Output file path: " + response.Path);
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling API: " + e.Message);
}
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-java-samples
String MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
DocumentApi apiInstance = new DocumentApi(configuration);

try {
    // Configure import options
    ImportOptions options = new ImportOptions();
    FileInfo fileInfo = new FileInfo();   
    fileInfo.setFilePath("Pdf/one-page-password.pdf");
    fileInfo.setPassword("password");   // If your PDF is password-protected
    options.setFileInfo(fileInfo);   
    options.addAttachmentsItem("Txt/document.txt");
    options.setOutputPath("output/with-attachment.pdf");

    // Execute import operation
    CallImportRequest request = new CallImportRequest(options);
    DocumentResult response = apiInstance.callImport(request);

    System.out.println("Output file path: " + response.getPath());
} catch (ApiException e) {
    System.err.println("Exception while calling API:");
    e.printStackTrace();
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-merger_cloud-cloud/groupdocs-merger_cloud-cloud-python-samples
from groupdocs_merger_cloud import *
import groupdocs_merger_cloud

client_id = "YOUR_CLIENT_ID" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

# Create instance of the API
document_api = groupdocs_merger_cloud.DocumentApi.from_keys(client_id, client_secret)

# Configure import options
options = groupdocs_merger_cloud.ImportOptions()
options.file_info = groupdocs_merger_cloud.FileInfo("Pdf/one-page-password.pdf", None, None, "password")
options.attachments = ["Txt/document.txt"]
options.output_path = "Output/with-attachment.pdf"

# Execute import operation
result = document_api.call_import(groupdocs_merger_cloud.CallImportRequest(options))

print("Output file path = " + result.path)
```

## Adding Multiple Attachments

You can add multiple attachments to a PDF document in a single operation by including additional file paths in the `Attachments` array:

```json
"Attachments": [
    "Txt/document1.txt", 
    "Spreadsheets/data.xlsx", 
    "Images/diagram.png"
]
```

This allows you to attach different types of files to the same PDF document.

## Supported Attachment File Types

You can attach virtually any file type to a PDF document, including:
- Text documents (.txt, .doc, .docx, etc.)
- Spreadsheets (.xls, .xlsx, etc.)
- Presentations (.ppt, .pptx, etc.)
- Images (.jpg, .png, .gif, etc.)
- Archives (.zip, .rar, etc.)
- And many more

The file type doesn't affect the attachment process, as files are embedded in their original format.

## Common Issues and Troubleshooting

1. Authentication Error: If you receive a 401 Unauthorized error, make sure your Client ID and Client Secret are correct and that your JWT token hasn't expired.

2. File Not Found: Verify that the file paths are correct and that both the PDF document and attachment files exist in your cloud storage.

3. Password Protection: For protected PDF documents, ensure you're providing the correct password in the request.

4. Large Attachments: Be mindful of the size of attachments. Very large attachments will increase the overall PDF file size significantly, which might impact sharing and downloading.

5. PDF Version Compatibility: Some very old PDF readers might not properly support or display attachments. For best results, ensure your users have modern PDF readers.

## What You've Learned

In this tutorial, you've learned how to:
- Understand the concept and benefits of PDF attachments
- Add file attachments to PDF documents using GroupDocs.Merger Cloud
- Configure attachment import operations with various options
- Implement attachment importing in different programming languages
- Handle password-protected PDF documents

## Further Practice

To reinforce your learning, try these exercises:
1. Add multiple attachments of different file types to a PDF document
2. Create a workflow that combines document joining and attachment operations
3. Build a simple application that allows users to select files to attach to a PDF
4. Compare the file size difference between merging documents versus attaching them

## Next Steps

Ready to explore more document operations? Check out these related tutorials:
- [Tutorial: How to Join Multiple Documents](/document-management/join-multiple-documents/)
- [Tutorial: How to Split a Document](/document-management/split-document/)
- [Tutorial: How to Generate Document Page Previews](/document-management/generate-document-pages-preview/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [Live Demo](https://products.groupdocs.app/merger/family)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.merger-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to share your feedback or ask questions on our [support forum](https://forum.groupdocs.cloud/c/merger/18/).
