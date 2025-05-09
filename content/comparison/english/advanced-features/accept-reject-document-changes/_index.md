---
title: How to Accept or Reject Document Changes Tutorial
description: Learn to selectively apply or discard specific changes between document versions using GroupDocs.Comparison Cloud API in this step-by-step tutorial
weight: 20
url: /advanced-features/accept-reject-document-changes/
---

# Tutorial: How to Accept or Reject Document Changes

## Learning Objectives

In this tutorial, you'll learn how to:
- Selectively apply or discard specific changes between document versions
- Process a list of changes with accept/reject decisions
- Generate a final document that incorporates only the changes you want

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Source and target documents ready for comparison in your cloud storage

## The Practical Scenario

Imagine you're reviewing a legal contract that has undergone multiple revisions. You need to carefully review each change and decide whether to accept or reject it before creating the final version. Some changes improve the contract's clarity, while others might introduce unwanted terms or conditions.

With GroupDocs.Comparison Cloud API, you can:
1. First compare the documents to identify all changes
2. Review each change individually
3. Mark each change as accepted or rejected
4. Generate a final document that incorporates only the accepted changes

This workflow enables precise control over document revisions in collaborative environments.

## Understanding the Accept/Reject Process

The process involves two main API calls:

1. First, you call the `changes` endpoint to get a list of all changes between the documents
2. Then, for each change, you set a `comparisonAction` value:
   - `Accept` - to include the change in the final document
   - `Reject` - to exclude the change from the final document
3. Finally, you call the `updates` endpoint with your modified changes list to generate the final document

## Step 1: Upload Your Documents to Cloud Storage

Before comparing documents, you need to upload them to cloud storage. For this tutorial, we'll work with:
- source.docx (original document)
- target.docx (modified document)

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

## Step 3: Get the List of Changes

First, retrieve all changes between the documents:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/comparison/changes" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'SourceFile': {
    'FilePath': 'source_files/word/source.docx'
  },
  'TargetFiles': [
    {
      'FilePath': 'target_files/word/target.docx'
    }
  ]
}"
```

The API will return a JSON array containing all detected changes. Each change has an `id` property that you'll use to reference it when accepting or rejecting.

## Step 4: Apply Accept/Reject Decisions

Now, let's apply our decisions to the changes. In this example, we'll accept the first change (id: 0) and reject all others:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/comparison/updates" \
-X PUT \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'SourceFile': {
    'FilePath': 'source_files/word/source.docx'
  },
  'TargetFiles': [
    {
      'FilePath': 'target_files/word/target.docx'
    }
  ],
  'OutputPath': 'output/result.docx',
  'Changes': [
    {
      'id': 0,
      'comparisonAction': 'Accept'
    },
    {
      'id': 1,
      'comparisonAction': 'Reject'
    },
    {
      'id': 2,
      'comparisonAction': 'Reject'
    }
    // Add more changes as needed
  ]
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

Use the link from the response to download the resulting document. This file contains only the changes you accepted (in this case, only the change with id: 0), while all rejected changes are not applied.

## Try It Yourself

Now it's your turn to implement the accept/reject workflow in your preferred programming language. Below are examples in various languages to help you get started.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);

var apiInstance = new CompareApi(configuration);
var options = new UpdatesOptions
{
    SourceFile = new FileInfo {FilePath = "source_files/word/source.docx"},
    TargetFiles = new List<FileInfo> {new FileInfo {FilePath = "target_files/word/target.docx"}},
    OutputPath = "output/result.docx"
};

// First, get all changes
var changes = apiInstance.PostChanges(new PostChangesRequest(options));

// Process the changes - in this example, we'll accept only the first change
foreach (var change in changes)
    change.ComparisonAction = ChangeInfo.ComparisonActionEnum.Reject;

// Accept just the first change
if (changes.Count > 0)
    changes[0].ComparisonAction = ChangeInfo.ComparisonActionEnum.Accept;

// Update the options with our decision
options.Changes = changes;

// Generate the result document
var response = apiInstance.PutChangesDocument(new PutChangesDocumentRequest(options));

Console.WriteLine("Output file link: " + response.Href);
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-java-samples
String MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);

CompareApi apiInstance = new CompareApi(configuration);
FileInfo sourceFileInfo = new FileInfo();
sourceFileInfo.setFilePath("source_files/word/source.docx");
FileInfo targetFileInfo = new FileInfo();
targetFileInfo.setFilePath("target_files/word/target.docx");

UpdatesOptions options = new UpdatesOptions();
options.setSourceFile(sourceFileInfo);
options.addTargetFilesItem(targetFileInfo);
options.setOutputPath("output/result.docx");

// First, get all changes
PostChangesRequest request = new PostChangesRequest(options);
List<ChangeInfo> changes = apiInstance.postChanges(request);

// Process the changes - in this example, we'll accept only the first change
for (ChangeInfo change : changes) {
    change.setComparisonAction(ComparisonActionEnum.REJECT);
}

// Accept just the first change
if (changes.size() > 0) {
    changes.get(0).setComparisonAction(ComparisonActionEnum.ACCEPT);
}

// Update the options with our decision
options.setChanges(changes);

// Generate the result document
Link response = apiInstance.putChangesDocument(new PutChangesDocumentRequest(options));

System.out.println("Output file link: " + response.getHref());
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-python-samples
import groupdocs_comparison_cloud

client_id = "YOUR_CLIENT_ID" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

api_instance = groupdocs_comparison_cloud.CompareApi.from_keys(client_id, client_secret)

source = groupdocs_comparison_cloud.FileInfo()
source.file_path = "source_files/word/source.docx"
target = groupdocs_comparison_cloud.FileInfo()
target.file_path = "target_files/word/target.docx"
options = groupdocs_comparison_cloud.UpdatesOptions()
options.source_file = source
options.target_files = [target]
options.output_path = "output/result.docx"

# First, get all changes
changes = api_instance.post_changes(groupdocs_comparison_cloud.PostChangesRequest(options))

# Process the changes - in this example, we'll accept only the first change
for change in changes:
    change.comparison_action = "Reject"

# Accept just the first change
if len(changes) > 0:
    changes[0].comparison_action = "Accept"

# Update the options with our decision
options.changes = changes

# Generate the result document
response = api_instance.put_changes_document(groupdocs_comparison_cloud.PutChangesDocumentRequest(options))

print("Output file link: " + response.href)
```

## Advanced Usage Scenarios

### Selective Change Filtering

In practice, you might want to be more selective about which changes to accept or reject. For example, you might want to:

1. Accept all changes by a specific author:
```csharp
foreach (var change in changes)
{
    // Set default action to reject
    change.ComparisonAction = ChangeInfo.ComparisonActionEnum.Reject;
    
    // Accept changes made by a specific author
    if (change.Authors != null && change.Authors.Contains("John Doe"))
    {
        change.ComparisonAction = ChangeInfo.ComparisonActionEnum.Accept;
    }
}
```

2. Accept only certain types of changes:
```csharp
foreach (var change in changes)
{
    // Reject by default
    change.ComparisonAction = ChangeInfo.ComparisonActionEnum.Reject;
    
    // Accept only insertions
    if (change.Type == "Inserted")
    {
        change.ComparisonAction = ChangeInfo.ComparisonActionEnum.Accept;
    }
}
```

3. Accept changes based on content:
```csharp
foreach (var change in changes)
{
    // Reject by default
    change.ComparisonAction = ChangeInfo.ComparisonActionEnum.Reject;
    
    // Accept changes containing specific text
    if (change.Text != null && change.Text.Contains("important clause"))
    {
        change.ComparisonAction = ChangeInfo.ComparisonActionEnum.Accept;
    }
}
```

## Common Issues and Troubleshooting

1. Change IDs Mismatch: Ensure that the change IDs you reference in the `updates` call match exactly with the IDs returned by the `changes` call. If you provide an invalid ID, the API will return an error.

2. Missing Changes: If you don't explicitly specify a comparison action for a change, it defaults to "None", which means the change won't be included in the final document. Always set a specific action for each change.

3. Document Format Limitations: Some document formats may have limitations in how changes can be applied. Test your specific document formats to understand any constraints.

4. Large Documents: For documents with a large number of changes, consider processing changes in batches to improve performance and user experience.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What are the possible values for the `comparisonAction` property?
2. What is the default action if you don't specify a `comparisonAction` for a change?
3. What API endpoints are involved in the accept/reject workflow?
4. How can you selectively accept changes based on criteria like author or content?

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve a list of changes between document versions
- Selectively mark changes for acceptance or rejection
- Generate a final document that incorporates only the accepted changes
- Implement advanced filtering for changes based on various criteria

## Further Practice

To reinforce your learning, try these exercises:
1. Create a user interface that displays each change and allows users to accept or reject them one by one
2. Implement a batch processing feature for accepting or rejecting multiple changes based on common criteria
3. Add a preview function that shows how the document will look after applying accept/reject decisions
4. Create a change history log that tracks which changes were accepted or rejected and by whom

## Next Steps

Now that you've learned to accept or reject document changes, check out these related tutorials:
- [Tutorial: How to Get Change Coordinates](/advanced-features/get-changes-coordinates/)
- [Tutorial: How to Customize Change Styles](/advanced-features/customize-changes-styles/)
- [Tutorial: How to Work with Document Revisions](/advanced-features/get-list-of-revisions/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
