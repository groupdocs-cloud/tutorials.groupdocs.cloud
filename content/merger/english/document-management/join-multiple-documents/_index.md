---
title: How to Join Multiple Documents Tutorial
weight: 10
url: /document-management/join-multiple-documents/
description: Learn how to combine multiple documents into a single file with GroupDocs.Merger Cloud API in this step-by-step tutorial for developers.
---

# Tutorial: How to Join Multiple Documents

## Introduction

In this tutorial, you'll learn how to join multiple documents into a single resultant file using GroupDocs.Merger Cloud API. Document merging is one of the most common operations in document management systems, and this tutorial will guide you through implementing this functionality in your applications.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Understand the GroupDocs.Merger Join API's capabilities
- Implement document joining functionality in different programming languages
- Handle password-protected documents during the joining process
- Combine different document formats (DOCX, PDF, etc.)

## Prerequisites

Before starting this tutorial, make sure you have:
- A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic knowledge of RESTful APIs
- Sample documents to merge (DOCX, PDF, etc.)

## Use Case Scenario

Let's consider a practical scenario: You're developing a document processing application that needs to combine multiple reports into a single document. Your users want to upload separate files and receive a unified document that contains all content in the correct order.

## Implementation Steps

### Step 1: Authenticate with the API

Before performing any document operations, you need to authenticate with the GroupDocs.Merger Cloud API by obtaining a JWT token using your Client ID and Client Secret.

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

### Step 2: Prepare Your Document Join Request

To join documents, you need to create a JSON request specifying:
- The files to be joined (file paths in storage)
- Output path for the resultant document
- Any passwords for protected documents

Here's a basic request structure:

```json
{
    "JoinItems": [
        {
            "FileInfo": {
                "FilePath": "/WordProcessing/document1.docx"
            }
        },
        {
            "FileInfo": {
                "FilePath": "/WordProcessing/document2.docx"
            }
        }
    ],
    "OutputPath": "output/joined-document.docx"
}
```

### Step 3: Join Documents Using the API

Now let's execute the document join operation using the API:

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
                'FilePath': '/WordProcessing/four-pages.docx',
                'Password': 'password'
            }
        },
        {
            'FileInfo':
            {
                'FilePath': '/WordProcessing/one-page.docx'
            }
        }
    ],
    'OutputPath': 'output/joined.docx'
}"
```

The API will respond with:

```json
{
  "path": "output/joined.docx"
}
```

### Step 4: Retrieve the Joined Document

After successful joining, you can download or use the merged document from the output path provided in the response.

## Try It Yourself

Let's practice joining documents in different programming languages. First, make sure your documents are uploaded to your GroupDocs Cloud storage, then use the appropriate SDK for your language.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new DocumentApi(configuration);

try
{
    // Create a join item for the first document (password-protected)
    var item1 = new JoinItem
    {
        FileInfo = new FileInfo
        {
            FilePath = "WordProcessing/four-pages.docx",
            Password = "password"
        }
    };

    // Create a join item for the second document
    var item2 = new JoinItem
    {
        FileInfo = new FileInfo
        {
            FilePath = "WordProcessing/one-page.docx"
        }
    };

    // Configure join options
    var options = new JoinOptions
    {
        JoinItems = new List<JoinItem> { item1, item2 },
        OutputPath = "Output/joined.docx"
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
    // Configure first document (password-protected)
    FileInfo fileInfo1 = new FileInfo();   
    fileInfo1.setFilePath("WordProcessing/four-pages.docx");
    fileInfo1.setPassword("password");
    JoinItem item1 = new JoinItem();
    item1.setFileInfo(fileInfo1);

    // Configure second document
    FileInfo fileInfo2 = new FileInfo();   
    fileInfo2.setFilePath("WordProcessing/one-page.docx");
    JoinItem item2 = new JoinItem();
    item2.setFileInfo(fileInfo2);

    // Configure join options
    JoinOptions options = new JoinOptions();
    options.setJoinItems(Arrays.asList(item1, item2));
    options.setOutputPath("output/joined.docx");

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

# Configure first document (password-protected)
item1 = groupdocs_merger_cloud.JoinItem()
item1.file_info = groupdocs_merger_cloud.FileInfo("WordProcessing/four-pages.docx", None, None, "password")

# Configure second document
item2 = groupdocs_merger_cloud.JoinItem()
item2.file_info = groupdocs_merger_cloud.FileInfo("WordProcessing/one-page.docx")

# Configure join options
options = groupdocs_merger_cloud.JoinOptions()
options.join_items = [item1, item2]
options.output_path = "Output/joined.docx"

# Execute join operation
result = document_api.join(groupdocs_merger_cloud.JoinRequest(options))        

print("Output file path = " + result.path)
```

## Common Issues and Troubleshooting

1. Authentication Error: If you receive a 401 Unauthorized error, make sure your Client ID and Client Secret are correct and that your JWT token hasn't expired.

2. File Not Found: Verify that the file paths are correct and that the documents exist in your cloud storage.

3. Password Protection: For protected documents, ensure you're providing the correct password in the request.

4. Format Compatibility: When joining different document formats, make sure they are compatible for joining operations. For cross-format joining, refer to our [dedicated tutorial](/document-management/join-document-cross-format/).

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the GroupDocs.Merger Cloud API
- Create a request to join multiple documents
- Handle password-protected documents
- Execute the join operation in various programming languages
- Process the API response to get the merged document

## Further Practice

To reinforce your learning, try these exercises:
1. Join three or more documents of the same format
2. Join documents from different storage locations
3. Join specific pages from documents instead of entire documents

## Next Steps

Ready to learn more advanced document operations? Check out these related tutorials:
- [Tutorial: How to Join Documents of Different Formats](/document-management/join-document-cross-format/)
- [Tutorial: How to Split a Document](/document-management/split-document/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [Live Demo](https://products.groupdocs.app/merger/family)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.merger-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to share your feedback or ask questions on our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!
