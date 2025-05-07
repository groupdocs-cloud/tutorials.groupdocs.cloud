---
title: How to Join Word Documents Continuously Tutorial
weight: 30
url: /document-management/join-word-continous/
description: Learn how to merge Word documents without page breaks in this step-by-step tutorial using GroupDocs.Merger Cloud API.
---

# Tutorial: How to Join Word Documents Continuously

## Introduction

In this tutorial, you'll learn how to join Word documents continuously, meaning without page breaks between documents, using GroupDocs.Merger Cloud API. This is a specialized joining option that allows you to merge documents seamlessly, as if they were parts of the same document from the beginning.

When merging Word documents using the standard method, each source document typically starts on a new page in the resultant document. With continuous joining, the last page of one document flows directly into the first page of the next document, creating a seamless transition without empty space or page breaks between them.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Understand the difference between standard and continuous document joining
- Implement continuous joining for Word documents
- Configure WordJoinMode parameter for seamless document merging
- Test and validate continuous joining in different programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
- A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic knowledge of RESTful APIs
- Sample Word documents to merge
- Completed the [basic document joining tutorial](/document-management/join-multiple-documents/) (recommended)

## Use Case Scenario

Let's consider a practical scenario: You're building a contract generation system that needs to combine standard clauses with custom sections into a single, seamless document. Each section is stored as a separate Word document, but when combined, they should flow naturally as if they were written as one document, without unnecessary page breaks.

## Understanding Continuous Joining

### Standard vs. Continuous Joining

Standard Joining: By default, when joining Word documents, each source document starts on a new page in the resultant document. This is appropriate for combining complete chapters or independent documents.

Continuous Joining: With continuous joining, the last page of one document flows directly into the first page of the next document without a page break in between. This is ideal for joining partial sections that should be read as a single continuous document.

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

### Step 2: Prepare Your Continuous Join Request

To join Word documents continuously, create a JSON request specifying:
- The files to be joined
- The WordJoinMode parameter set to "Continuous" for the second and subsequent documents
- Output path for the resultant document

Here's a basic request structure:

```json
{
    "JoinItems": [
        {
            "FileInfo": {
                "FilePath": "WordProcessing/four-pages.docx",
                "Password": "password"
            }
        },
        {
            "FileInfo": {
                "FilePath": "WordProcessing/one-page.docx"
            },
            "WordJoinMode": "Continuous"
        }
    ],
    "OutputPath": "output/joined_continuous.docx"
}
```

Note that the `WordJoinMode` parameter is set to "Continuous" only for the second document, as it will be appended to the first document without a page break.

### Step 3: Join Word Documents Continuously Using the API

Now let's execute the continuous document join operation using the API:

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
                'FilePath': 'WordProcessing/four-pages.docx',
                'Password': 'password'
            }
        },
        {
            'FileInfo':
            {
                'FilePath': 'WordProcessing/one-page.docx'
            },
            'WordJoinMode': 'Continuous'
        }
    ],
    'OutputPath': 'output/joined_continuous.docx'
}"
```

The API will respond with:

```json
{
  "path": "output/joined_continuous.docx"
}
```

### Step 4: Retrieve and Verify the Joined Document

After successful joining, you can download the merged document from the output path provided in the response. Open the document to verify that there is no page break between the content of the first and second documents.

## Try It Yourself

Let's implement continuous document joining in different programming languages. First, make sure your Word documents are uploaded to your GroupDocs Cloud storage, then use the appropriate SDK for your language.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new DocumentApi(configuration);

try
{
    // Create first join item (standard)
    var item1 = new JoinItem
    {
        FileInfo = new FileInfo
        {
            FilePath = "WordProcessing/four-pages.docx"
        }
    };

    // Create second join item with continuous mode
    var item2 = new JoinItem
    {
        FileInfo = new FileInfo
        {
            FilePath = "WordProcessing/one-page.docx"
        },
        WordJoinMode = JoinItem.WordJoinModeEnum.Continuous
    };

    // Configure join options
    var options = new JoinOptions
    {
        JoinItems = new List<JoinItem> { item1, item2 },
        OutputPath = "Output/joined_continuous.docx"
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
    // Configure first document (standard joining)
    FileInfo fileInfo1 = new FileInfo();
    fileInfo1.setFilePath("WordProcessing/four-pages.docx");
    JoinItem item1 = new JoinItem();
    item1.setFileInfo(fileInfo1);

    // Configure second document (continuous joining)
    FileInfo fileInfo2 = new FileInfo();
    fileInfo2.setFilePath("WordProcessing/one-page.docx");
    JoinItem item2 = new JoinItem();
    item2.setFileInfo(fileInfo2);
    item2.setWordJoinMode(WordJoinModeEnum.CONTINUOUS);

    // Configure join options
    JoinOptions options = new JoinOptions();
    options.setJoinItems(Arrays.asList(item1, item2));
    options.setOutputPath("output/joined_continuous.docx");

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

# Configure first document (standard joining)
item1 = groupdocs_merger_cloud.JoinItem()
item1.file_info = groupdocs_merger_cloud.FileInfo("WordProcessing/four-pages.docx")

# Configure second document (continuous joining)
item2 = groupdocs_merger_cloud.JoinItem()
item2.file_info = groupdocs_merger_cloud.FileInfo("WordProcessing/one-page.docx")
item2.word_join_mode = "Continuous"

# Configure join options
options = groupdocs_merger_cloud.JoinOptions()
options.join_items = [item1, item2]
options.output_path = "Output/joined_continuous.docx"

# Execute join operation
result = document_api.join(groupdocs_merger_cloud.JoinRequest(options))        

print("Output file path = " + result.path)
```

## Important Considerations

When using continuous joining, keep these points in mind:

1. Compatible Documents Only: Continuous joining works only with Word documents (DOCX, DOC, etc.). It cannot be used with PDF, presentations, or other formats.

2. Formatting Consistency: For the best results, ensure that the documents being joined have consistent formatting (font, styles, etc.) to create a truly seamless appearance.

3. Headers and Footers: Be aware that headers and footers from individual documents may behave unexpectedly when using continuous joining. Test the results to ensure they meet your expectations.

4. Section Breaks: If your source documents contain section breaks, they might affect how the continuous joining works. You may need to clean up the documents beforehand.

5. Document Properties: The properties of the first document (such as author, title, etc.) will be preserved in the resultant document.

## Common Issues and Troubleshooting

1. Authentication Error: If you receive a 401 Unauthorized error, make sure your Client ID and Client Secret are correct and that your JWT token hasn't expired.

2. File Not Found: Verify that the file paths are correct and that the documents exist in your cloud storage.

3. Password Protection: For protected documents, ensure you're providing the correct password in the request.

4. Unsupported Format: If you attempt to use continuous joining with non-Word documents, you may receive an error. Ensure you're using compatible formats.

5. Unexpected Formatting: If the joined document has unexpected formatting or layout issues, try opening and resaving the source documents to ensure they are properly formatted.

## What You've Learned

In this tutorial, you've learned how to:
- Understand the difference between standard and continuous document joining
- Create a request for continuous joining of Word documents
- Configure the WordJoinMode parameter for seamless document merging
- Implement continuous joining in various programming languages
- Handle potential issues and considerations for optimal results

## Further Practice

To reinforce your learning, try these exercises:
1. Join three or more Word documents with continuous joining between some of them
2. Compare the results of standard joining versus continuous joining with the same documents
3. Join documents with different formatting styles and observe how the formatting is handled in the resultant document
4. Create a document template system that combines standard clauses with custom sections seamlessly

## Next Steps

Ready to explore more document operations? Check out these related tutorials:
- [Tutorial: How to Join Multiple Documents](/document-management/join-multiple-documents/)
- [Tutorial: How to Join Images](/document-management/join-images/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [Live Demo](https://products.groupdocs.app/merger/family)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.merger-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to share your feedback or ask questions on our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!
