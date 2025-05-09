---
title: How to Reject All Revisions Tutorial
description: Learn to efficiently reject all document revisions at once using GroupDocs.Comparison Cloud API in this comprehensive step-by-step tutorial
weight: 120
url: /advanced-features/reject-all-revisions/
---

# Tutorial: How to Reject All Revisions

## Learning Objectives

In this tutorial, you'll learn how to:
- Quickly reject all revisions in a Word document with a single API call
- Understand when and why you might need to reject all tracked changes
- Implement efficient document revision management workflows

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Word documents with tracked changes (revisions) ready in your cloud storage

## The Practical Scenario

Consider a scenario where your legal team receives a contract draft with numerous suggested changes tracked through Microsoft Word's "Track Changes" feature. After careful review, you determine that the suggested revisions undermine the contract's intent and need to be discarded entirely. Rather than rejecting each revision individually, you need an efficient way to reject all revisions at once and revert to the original document content.

With GroupDocs.Comparison Cloud API, you can reject all revisions with a single API call, streamlining your document revision management process.

## Understanding Batch Revision Rejection

Similar to the [Accept All Revisions](/advanced-features/accept-all-revisions/) functionality, the GroupDocs.Comparison Cloud API provides a dedicated option for rejecting all revisions at once. Instead of retrieving all revisions and marking each one for rejection, you can set the `RejectAll` parameter to `true` in a single API call.

This approach is particularly useful when:
- You need to revert a document to its original state before revisions
- Multiple suggested changes are not approved
- You want to preserve the original document content while still maintaining a record of rejected suggestions

## Step 1: Upload Your Document to Cloud Storage

Before processing revisions, you need to upload a Word document with tracked changes to cloud storage. For this tutorial, we'll work with:
- source_with_revs.docx (a document with track changes enabled and several revisions)

Refer to the [File API documentation](https://docs.groupdocs.cloud/comparison/developer-guide/working-with-file-api/) for detailed instructions on uploading files.

## Step 2: Obtain Authorization Token

To authorize API requests, you need to obtain a JWT (JSON Web Token) first:

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

The API will return a token in the response. Save this token for use in subsequent API calls.

## Step 3: Reject All Revisions

Now, let's reject all revisions in the document with a single API call:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/comparison/revisions" \
-X PUT \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'SourceFile': {
    'FilePath': 'source_files/word/source_with_revs.docx'
  },
  'RejectAll': true,
  'OutputPath': 'output/result.docx'
}"
```

Notice that we set `RejectAll` to `true`. This tells the API to reject all revisions in the document automatically.

Upon successful execution, the API will return a link to the resulting document:

```json
{
  "href": "https://api.groupdocs.cloud/v2.0/comparison/storage/file/output/result.docx",
  "rel": "output/result.docx",
  "type": "file",
  "title": "result.docx"
}
```

## Step 4: Download and Review the Result

Use the link from the response to download the resulting document. This file contains the original document content with all revisions rejected. The tracked changes have been discarded, reverting the document to its state before those changes were suggested.

## Try It Yourself

Now it's your turn to implement batch revision rejection in your preferred programming language. Below are examples in various languages to help you get started.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new ReviewApi(configuration);

var options = new ApplyRevisionsOptions
{
    SourceFile = new FileInfo {FilePath = "source_files/word/source_with_revs.docx" },
    RejectAll = true,
    OutputPath = "output/result.docx"
};

var response = apiInstance.ApplyRevisions(new ApplyRevisionsRequest(options));

Console.WriteLine("ApplyRevisions: Output file link: " + response.Href);
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-java-samples
String MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
  
ReviewApi apiInstance = new ReviewApi(configuration);

FileInfo sourceFileInfo = new FileInfo();
sourceFileInfo.setFilePath("source_files/word/source_with_revs.docx");

ApplyRevisionsOptions options = new ApplyRevisionsOptions();
options.setSourceFile(sourceFileInfo);
options.setRejectAll(true);
options.setOutputPath("output/result.docx");

Link response = apiInstance.applyRevisions(new ApplyRevisionsRequest(options));

System.out.println("Output file link: " + response.getHref());
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-python-samples
import groupdocs_comparison_cloud

client_id = "YOUR_CLIENT_ID" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

api_instance = groupdocs_comparison_cloud.ReviewApi.from_keys(client_id, client_secret)

source = groupdocs_comparison_cloud.FileInfo()
source.file_path = "source_files/word/source_with_revs.docx"
options = groupdocs_comparison_cloud.ApplyRevisionsOptions()
options.source_file = source
options.reject_all = True
options.output_path = "output/result.docx"

response = api_instance.apply_revisions(groupdocs_comparison_cloud.ApplyRevisionsRequest(options))

print("Output file link: " + response.href)
```

### Node.js Example

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-node-samples
global.comparison_cloud = require("groupdocs-comparison-cloud");

global.clientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
global.clientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

global.reviewApi = comparison_cloud.ReviewApi.fromKeys(clientId, clientSecret);

try {
 let source = new comparison_cloud.FileInfo();
 source.filePath = "source_files/word/source_with_revs.docx";

 let options = new comparison_cloud.ApplyRevisionsOptions();
 options.sourceFile = source;
 options.rejectAll = true;
 options.outputPath = "output/result.docx";
 
 let response = await reviewApi.applyRevisions(new comparison_cloud.ApplyRevisionsRequest(options));
 console.log("Output file link: " + response.href);
} catch (error) {
 console.log(error.message);
}
```

## When to Use Batch Rejection vs. Selective Processing

Understanding when to use batch rejection versus selective processing is important for efficient document workflows:

Use Batch Rejection (RejectAll) when:
- You need to revert a document to its original state
- All suggested changes have been reviewed and deemed unsuitable
- You want to preserve the base document while starting fresh with a new round of revisions
- You're implementing a workflow where certain types of changes are automatically rejected

Use Selective Processing when:
- Some changes are valuable and should be kept
- Different revisions have different levels of approval
- You need to maintain a record of which specific revisions were rejected and why
- You're implementing a complex approval workflow with conditional logic

## Common Issues and Troubleshooting

1. Document Format Limitations: This feature is primarily designed for Word documents (.docx, .doc). Other formats may not support revision tracking or may have limited support.

2. No Revisions Found: If the document doesn't contain any tracked changes, the API will still execute successfully, but no changes will occur in the document.

3. Performance Considerations: For documents with a very large number of revisions, batch rejection is significantly more efficient than processing each revision individually.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What parameter do you set to reject all revisions in a document?
2. How does rejecting all revisions affect the document content?
3. In what scenarios would batch rejection be more appropriate than selective processing?
4. Does batch rejection modify the original document or create a new one?

## What You've Learned

In this tutorial, you've learned how to:
- Quickly reject all revisions in a Word document with a single API call
- Understand when and why you might need to reject all tracked changes
- Implement efficient document revision management workflows

## Further Practice

To reinforce your learning, try these exercises:
1. Create a document processing pipeline that automatically rejects all revisions from certain authors
2. Implement a hybrid approach that automatically rejects certain types of revisions but requires manual review for others
3. Build a simple command-line utility that rejects all revisions in multiple documents at once

## Next Steps

Now that you've learned to reject all revisions at once, check out these related tutorials:
- [Tutorial: How to Accept All Revisions](/advanced-features/accept-all-revisions/)
- [Tutorial: How to Accept or Reject Revisions](/advanced-features/accept-reject-revisions/)
- [Tutorial: How to Get List of Revisions](/advanced-features/get-list-of-revisions/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
