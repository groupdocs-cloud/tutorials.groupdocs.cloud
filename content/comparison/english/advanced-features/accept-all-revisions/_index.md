---
title: How to Accept All Revisions Tutorial
description: Learn to efficiently accept all document revisions at once using GroupDocs.Comparison Cloud API in this step-by-step tutorial
weight: 10
url: /advanced-features/accept-all-revisions/
---

# Tutorial: How to Accept All Revisions

## Learning Objectives

In this tutorial, you'll learn how to:
- Quickly accept all revisions in a Word document with a single API call
- Understand the difference between selective revision processing and batch acceptance
- Implement efficient document finalization workflows

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Word documents with tracked changes (revisions) ready in your cloud storage

## The Practical Scenario

Imagine you're managing the final stage of a document review process. After multiple rounds of collaborative editing with tracked changes, all stakeholders have approved the document and its revisions. Now, you need to finalize the document by accepting all tracked changes at once, rather than reviewing and accepting each revision individually.

With GroupDocs.Comparison Cloud API, you can efficiently accept all revisions with a single API call, streamlining your document finalization process.

## Understanding Batch Revision Acceptance

While the [selective revision acceptance](/advanced-features/accept-reject-revisions/) tutorial showed how to process revisions individually, many workflows require a more efficient way to accept all revisions at once. 

The GroupDocs.Comparison Cloud API provides a dedicated option for this purpose. Instead of retrieving all revisions and marking each one for acceptance, you can set the `AcceptAll` parameter to `true` in a single API call.

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

## Step 3: Accept All Revisions

Now, let's accept all revisions in the document with a single API call:

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
  'AcceptAll': true,
  'OutputPath': 'output/result.docx'
}"
```

Notice that instead of providing a list of revisions with individual actions, we simply set `AcceptAll` to `true`. This tells the API to accept all revisions in the document automatically.

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

Use the link from the response to download the resulting document. This file contains the original document with all revisions accepted. The tracked changes are now permanently incorporated into the document content.

## Try It Yourself

Now it's your turn to implement batch revision acceptance in your preferred programming language. Below are examples in various languages to help you get started.

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
    AcceptAll = true,
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
options.setAcceptAll(true);
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
options.accept_all = True
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
 options.acceptAll = true;
 options.outputPath = "output/result.docx";
 
 let response = await reviewApi.applyRevisions(new comparison_cloud.ApplyRevisionsRequest(options));
 console.log("Output file link: " + response.href);
} catch (error) {
 console.log(error.message);
}
```

## When to Use Batch Acceptance vs. Selective Processing

Understanding when to use batch acceptance versus selective processing is important for efficient document workflows:

Use Batch Acceptance (AcceptAll) when:
- All stakeholders have approved all changes in the document
- You need to quickly finalize a document after the review process is complete
- The document has a large number of minor revisions that don't require individual review
- You're implementing a streamlined workflow where certain document types can be auto-approved

Use Selective Processing when:
- Different stakeholders have different levels of approval authority
- Some revisions require additional review or discussion
- You need to maintain a record of which revisions were accepted or rejected
- You're implementing a complex approval workflow with conditional logic

## Common Issues and Troubleshooting

1. Document Format Limitations: This feature is primarily designed for Word documents (.docx, .doc). Other formats may not support revision tracking or may have limited support.

2. No Revisions Found: If the document doesn't contain any tracked changes, the API will still execute successfully, but no changes will occur in the document.

3. Performance Considerations: For documents with a very large number of revisions, batch acceptance is significantly more efficient than processing each revision individually.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What parameter do you set to accept all revisions in a document?
2. How does batch acceptance differ from selective revision processing?
3. In what scenarios would batch acceptance be more appropriate than selective processing?
4. Does batch acceptance modify the original document or create a new one?

## What You've Learned

In this tutorial, you've learned how to:
- Quickly accept all revisions in a Word document with a single API call
- Understand the difference between selective revision processing and batch acceptance
- Implement efficient document finalization workflows

## Further Practice

To reinforce your learning, try these exercises:
1. Create a document processing pipeline that automatically accepts all revisions after a specified approval date
2. Implement a hybrid approach that automatically accepts certain types of revisions (e.g., formatting) but requires manual review for content changes
3. Build a simple command-line utility that accepts all revisions in multiple documents at once

## Next Steps

Now that you've learned to accept all revisions at once, check out these related tutorials:
- [Tutorial: How to Reject All Revisions](/advanced-features/reject-all-revisions/)
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
