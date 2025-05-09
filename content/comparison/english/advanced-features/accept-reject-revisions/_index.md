---
title: How to Accept or Reject Revisions Tutorial
description: Learn to selectively accept or reject specific revisions in Word documents using GroupDocs.Comparison Cloud API in this comprehensive tutorial
weight: 30
url: /advanced-features/accept-reject-revisions/
---

# Tutorial: How to Accept or Reject Revisions

## Learning Objectives

In this tutorial, you'll learn how to:
- Selectively accept or reject specific revisions in Word documents
- Process a list of revisions with individual accept/reject decisions
- Generate a final document that incorporates your revision decisions

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Word documents with tracked changes (revisions) ready in your cloud storage

## The Practical Scenario

Imagine you're managing a legal team that reviews contract revisions. You've received a Word document with numerous tracked changes from different team members. Before finalizing the document, you need to review each revision and decide whether to accept or reject it based on its merit. Some revisions improve the contract, while others introduce problematic terms or formatting.

With GroupDocs.Comparison Cloud API, you can programmatically:
1. Retrieve all tracked changes (revisions) in the document
2. Selectively mark each revision for acceptance or rejection
3. Generate a final document with only the approved revisions incorporated

This approach allows precise control over document revisions in workflows that require careful review of proposed changes.

## Understanding the Accept/Reject Revisions Process

The process involves three main API calls:

1. First, you call the `revisions` endpoint with a POST request to get a list of all revisions in the document
2. Then, for each revision, you set an `action` value:
   - `Accept` - to include the revision in the final document
   - `Reject` - to exclude the revision from the final document
3. Finally, you call the `revisions` endpoint with a PUT request, including your modified revisions list, to generate the final document

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

## Step 3: Get the List of Revisions

First, retrieve all revisions from the document:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/comparison/revisions" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FilePath': 'source_files/word/source_with_revs.docx'
  }"
```

The API will return a JSON array containing all revisions in the document, which will look similar to this:

```json
[
  {
    "id": 0,
    "action": "None",
    "text": "\rsssssssss",
    "author": "GroupDocs",
    "type": "Insertion"
  },
  {
    "id": 1,
    "action": "None",
    "text": "Many students, scholars and members of the Christian clergy speak Latin fluently...",
    "author": "GroupDocs",
    "type": "Deletion"
  }
]
```

## Step 4: Apply Accept/Reject Decisions

Now, let's apply our decisions to the revisions. In this example, we'll accept specific revisions based on their IDs:

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
  'Revisions': [
    {
      'Id': 0,
      'action': 'Accept'
    },
    {
      'Id': 1,
      'action': 'Accept'
    }
  ],
  'OutputPath': 'output/result.docx'
}"
```

Upon successful execution, the API will return a link to the resulting document:

```json
{
  "href": "https://api.groupdocs.cloud/v2.0/comparison/storage/file/output/result.docx",
  "rel": "output/result.docx",
  "type": "file",
  "title": "result.docx"
}
```

## Step 5: Download and Review the Result

Use the link from the response to download the resulting document. This file contains the original document with revisions accepted or rejected according to your decisions.

## Try It Yourself

Now it's your turn to implement selective revision acceptance in your preferred programming language. Below are examples in various languages to help you get started.

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
    OutputPath = "output/result.docx"
};

// Get all revisions from the document
var revisions = apiInstance.GetRevisions(new GetRevisionsRequest(options.SourceFile));

// In this example, we'll accept all revisions
foreach (var revision in revisions)
    revision.Action = RevisionInfo.ActionEnum.Accept;

// Alternatively, you could be selective:
// revisions[0].Action = RevisionInfo.ActionEnum.Accept;
// revisions[1].Action = RevisionInfo.ActionEnum.Reject;

// Update the options with our decisions
options.Revisions = revisions;

// Generate the result document
var response = apiInstance.ApplyRevisions(new ApplyRevisionsRequest(options));

Console.WriteLine("Output file link: " + response.Href);
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
options.setOutputPath("output/result.docx");

// Get all revisions from the document
GetRevisionsRequest request = new GetRevisionsRequest(sourceFileInfo);
List<RevisionInfo> revisions = apiInstance.getRevisions(request);

// In this example, we'll accept all revisions
for (RevisionInfo revision : revisions) {
    revision.setAction(ActionEnum.ACCEPT);
}

// Alternatively, you could be selective:
// revisions.get(0).setAction(ActionEnum.ACCEPT);
// revisions.get(1).setAction(ActionEnum.REJECT);

// Update the options with our decisions
options.setRevisions(revisions);

// Generate the result document
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
options.output_path = "output/result.docx"

# Get all revisions from the document
revisions = api_instance.get_revisions(groupdocs_comparison_cloud.GetRevisionsRequest(source))

# In this example, we'll accept all revisions
for revision in revisions:
    revision.action = "Accept"

# Alternatively, you could be selective:
# revisions[0].action = "Accept"
# revisions[1].action = "Reject"

# Update the options with our decisions
options.revisions = revisions

# Generate the result document
response = api_instance.apply_revisions(groupdocs_comparison_cloud.ApplyRevisionsRequest(options))

print("Output file link: " + response.href)
```

## Advanced Usage Scenarios

### Selective Revision Filtering

In practice, you'll often want to be more selective about which revisions to accept or reject. Here are some common approaches:

1. Accept or reject by author:
```csharp
foreach (var revision in revisions)
{
    // Set default action to reject
    revision.Action = RevisionInfo.ActionEnum.Reject;
    
    // Accept revisions made by a specific author
    if (revision.Author == "John Doe")
    {
        revision.Action = RevisionInfo.ActionEnum.Accept;
    }
}
```

2. Accept or reject by revision type:
```csharp
foreach (var revision in revisions)
{
    // Reject by default
    revision.Action = RevisionInfo.ActionEnum.Reject;
    
    // Accept only insertions
    if (revision.Type == "Insertion")
    {
        revision.Action = RevisionInfo.ActionEnum.Accept;
    }
}
```

3. Accept or reject based on content:
```csharp
foreach (var revision in revisions)
{
    // Reject by default
    revision.Action = RevisionInfo.ActionEnum.Reject;
    
    // Accept revisions containing specific text
    if (revision.Text != null && revision.Text.Contains("important clause"))
    {
        revision.Action = RevisionInfo.ActionEnum.Accept;
    }
}
```

### Creating a Review Workflow

For a more robust revision review workflow, consider implementing these features:

1. Batch Processing: Allow users to apply rules to accept or reject revisions in batch (e.g., "Accept all insertions")

2. Two-Step Review: Implement a preliminary automatic filtering based on rules, followed by manual review of edge cases

3. Commenting: Add comments to the document explaining why certain revisions were accepted or rejected

4. Revision History: Maintain a log of which revisions were accepted or rejected and by whom

## Common Issues and Troubleshooting

1. Revision IDs Mismatch: Ensure that the revision IDs you reference in the `ApplyRevisions` call match exactly with the IDs returned by the `GetRevisions` call. If you provide an invalid ID, the API will return an error.

2. Missing Action: If you don't explicitly specify an action for a revision, it defaults to "None", which means the revision's status won't be changed. Always set a specific action for each revision you want to process.

3. Document Format Limitations: This feature is primarily designed for Word documents (.docx, .doc). Other formats may not support revision tracking or may have limited support.

4. Large Documents: For documents with a large number of revisions, consider processing revisions in batches to improve performance and user experience.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What are the possible values for the `action` property of a revision?
2. What is the default action if you don't specify an action for a revision?
3. What API endpoints are involved in the accept/reject revisions workflow?
4. How can you selectively accept revisions based on criteria like author or content?

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve a list of revisions from a Word document
- Selectively mark revisions for acceptance or rejection
- Generate a final document that incorporates your revision decisions
- Implement advanced filtering for revisions based on various criteria

## Further Practice

To reinforce your learning, try these exercises:
1. Create a user interface that displays each revision and allows users to accept or reject them one by one
2. Implement a batch processing feature for accepting or rejecting multiple revisions based on common criteria
3. Add a preview function that shows how the document will look after applying accept/reject decisions
4. Create a revision history log that tracks which revisions were accepted or rejected and by whom

## Next Steps

Now that you've learned to selectively accept or reject revisions, check out these related tutorials:
- [Tutorial: How to Accept All Revisions](/advanced-features/accept-all-revisions/)
- [Tutorial: How to Reject All Revisions](/advanced-features/reject-all-revisions/)
- [Tutorial: How to Accept or Reject Document Changes](/advanced-features/accept-reject-document-changes/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
