---
title: How to Join Documents of Different Formats Tutorial
url: /document-management/join-document-cross-format/
weight: 20
description: Learn to combine documents of different formats like PDF, DOCX, and more into a single file with this step-by-step tutorial for developers.
---

# Tutorial: How to Join Documents of Different Formats

## Introduction

In this tutorial, you'll learn how to join documents of different formats into a single PDF or Word document using GroupDocs.Merger Cloud API. Cross-format document merging is a powerful feature that allows you to combine various document types (like PDF and DOCX) into one unified document without manual conversion steps.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Join documents with different formats (PDF, DOCX, etc.) into a single document
- Handle password-protected documents during cross-format joining
- Implement cross-format joining in different programming languages
- Understand format compatibility and limitations

## Prerequisites

Before starting this tutorial, make sure you have:
- A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic knowledge of RESTful APIs
- Sample documents of different formats (PDF, DOCX, etc.)
- Completed the [basic document joining tutorial](/document-management/join-multiple-documents/) (recommended)

## Use Case Scenario

Let's consider a practical scenario: You're building a report generation system that needs to combine various document types. Your users have content in different formats (PDF reports, Word documents, etc.) that need to be merged into a single comprehensive document for distribution.

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

### Step 2: Prepare Your Cross-Format Join Request

To join documents of different formats, create a JSON request specifying:
- The files to be joined (with their different formats)
- Output path and format for the resultant document
- Any passwords for protected documents

Here's a basic request structure for joining a PDF and a DOCX file:

```json
{
    "JoinItems": [
        {
            "FileInfo": {
                "FilePath": "Pdf/document.pdf",
                "Password": "password"
            }
        },
        {
            "FileInfo": {
                "FilePath": "WordProcessing/document.docx"
            }
        }
    ],
    "OutputPath": "output/joined.pdf"
}
```

### Step 3: Join Different Format Documents Using the API

Now let's execute the cross-format document join operation:

#### cURL Example

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/join" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'JoinItems':
    [
        {
            'FileInfo':
            {
                'FilePath': 'Pdf/one-page-password.pdf',
                'Password': 'password'
            }
        },
        {
            'FileInfo':
            {
                'FilePath': 'WordProcessing/one-page.docx'
            }
        }
    ],
    'OutputPath': 'Output/joined.pdf'
}"
```

The API will respond with:

```json
{
  "path": "Output/joined.pdf"
}
```

### Step 4: Retrieve the Joined Document

After successful joining, you can download or use the merged document from the output path provided in the response.

## Try It Yourself

Let's practice joining documents of different formats in various programming languages. First, ensure your documents are uploaded to your GroupDocs Cloud storage, then use the appropriate SDK for your language.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new DocumentApi(configuration);

try
{
    // Create a join item for PDF document (password-protected)
    var item1 = new JoinItem
    {
        FileInfo = new FileInfo
        {
            FilePath = "Pdf/one-page-password.pdf",
            Password = "password"
        }
    };

    // Create a join item for DOCX document
    var item2 = new JoinItem
    {
        FileInfo = new FileInfo
        {
            FilePath = "WordProcessing/one-page.docx"
        }
    };

    // Configure join options with PDF output format
    var options = new JoinOptions
    {
        JoinItems = new List<JoinItem> { item1, item2 },
        OutputPath = "Output/joined.pdf"
    };

    // Execute join operation
    var request = new JoinRequest(options);
    var response = apiInstance.Join(request);

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
    // Configure first document (PDF with password)
    FileInfo fileInfo1 = new FileInfo();   
    fileInfo1.setFilePath("Pdf/one-page-password.pdf");
    fileInfo1.setPassword("password");
    JoinItem item1 = new JoinItem();
    item1.setFileInfo(fileInfo1);

    // Configure second document (DOCX)
    FileInfo fileInfo2 = new FileInfo();   
    fileInfo2.setFilePath("WordProcessing/one-page.docx");
    JoinItem item2 = new JoinItem();
    item2.setFileInfo(fileInfo2);

    // Configure join options with PDF output
    JoinOptions options = new JoinOptions();
    options.setJoinItems(Arrays.asList(item1, item2));
    options.setOutputPath("output/joined.pdf");

    // Execute join operation
    JoinRequest request = new JoinRequest(options);
    DocumentResult response = apiInstance.join(request);

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

# Configure first document (PDF with password)
item1 = groupdocs_merger_cloud.JoinItem()
item1.file_info = groupdocs_merger_cloud.FileInfo("Pdf/one-page-password.pdf", None, None, "password")

# Configure second document (DOCX)
item2 = groupdocs_merger_cloud.JoinItem()
item2.file_info = groupdocs_merger_cloud.FileInfo("WordProcessing/one-page.docx")

# Configure join options with PDF output
options = groupdocs_merger_cloud.JoinOptions()
options.join_items = [item1, item2]
options.output_path = "Output/joined.pdf"

# Execute join operation
result = document_api.join(groupdocs_merger_cloud.JoinRequest(options))        

print("Output file path = " + result.path)
```

## Format Compatibility and Considerations

When joining documents of different formats, keep in mind the following:

1. Output Format: The output format should be either PDF or a word processing format (DOCX, DOC, etc.).

2. Format Compatibility Matrix:
   - PDF can be combined with: DOCX, DOC, XLS, XLSX, PPT, PPTX, HTML, TXT
   - DOCX can be combined with: PDF, DOC, TXT, HTML, RTF
   - For other combinations, consider converting documents to compatible formats first

3. Content Rendering: Some formatting or advanced features might not translate perfectly across formats. Complex layouts in presentations or spreadsheets may be simplified when converted to PDF or DOCX.

4. File Size: The resultant file may be larger than the sum of the original files due to format conversion overhead.

## Common Issues and Troubleshooting

1. Format Not Supported: If you receive an error about incompatible formats, check the format compatibility matrix above. Some combinations may not be directly supported.

2. Layout Issues: If the layout in the joined document doesn't match the original, consider using format-specific settings or converting documents to the same format before joining.

3. Password Protection: Ensure you're providing the correct password for protected documents.

4. Large Documents: For large documents, the operation may take longer to complete. Consider implementing asynchronous processing for better user experience.

## What You've Learned

In this tutorial, you've learned how to:
- Join documents of different formats into a single PDF or Word document
- Create appropriate requests for cross-format joining operations
- Implement cross-format joining in various programming languages
- Handle password-protected documents in a cross-format scenario
- Consider format compatibility and limitations

## Further Practice

To reinforce your learning, try these exercises:
1. Join a PDF, a DOCX, and a TXT file into a single PDF
2. Join a presentation (PPT/PPTX) with a Word document
3. Compare the results of joining to PDF vs. joining to DOCX

## Next Steps

Ready to learn more document operations? Check out these related tutorials:
- [Tutorial: How to Join Word Documents Continuously](/document-management/join-word-continous/)
- [Tutorial: How to Split a Document](/document-management/split-document/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [Live Demo](https://products.groupdocs.app/merger/family)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.merger-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to share your feedback or ask questions on our [support forum](https://forum.groupdocs.cloud/c/merger/18/).
