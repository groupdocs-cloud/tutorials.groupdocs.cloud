---
title: How to Get a List of Revisions Tutorial
description: Learn to extract and process document revision history using GroupDocs.Comparison Cloud API in this comprehensive step-by-step tutorial
weight: 110
url: /advanced-features/get-list-of-revisions/
---

# Tutorial: How to Get a List of Revisions

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve the complete list of revisions from Word documents
- Access detailed information about each revision
- Process revision information programmatically

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Word documents with revisions (track changes) ready in your cloud storage

## The Practical Scenario

Imagine you're developing a document management system where legal teams need to track the history of changes in contracts. Microsoft Word's "Track Changes" feature is commonly used for collaborative editing, and your application needs to programmatically access this revision history.

With GroupDocs.Comparison Cloud API, you can:
1. Extract all revisions tracked in Word documents
2. View details about each revision (who made it, what type of change, the affected text)
3. Build custom workflows for revision review and approval

## Understanding Revisions in Word Documents

When users enable the "Track Changes" feature in Microsoft Word, the document maintains a record of all modifications made to the content. These tracked changes are known as "revisions" and include:

- Insertions: New content added to the document
- Deletions: Content removed from the document
- Formatting changes: Changes to text styling or formatting
- Moves: Content relocated within the document

Each revision includes metadata such as:
- The author who made the change
- The type of change (insertion, deletion, etc.)
- The affected text content
- The revision's current status (accepted, rejected, or pending)

## Step 1: Upload Your Document to Cloud Storage

Before retrieving revisions, you need to upload a Word document with tracked changes to cloud storage. For this tutorial, we'll work with:
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

Now, let's retrieve all revisions from the document:

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

The API will return a JSON array containing all revisions in the document. Here's a sample response:

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
    "text": "Many students, scholars and members of the \u0013 HYPERLINK \"https://en.wikipedia.org/wiki/Minister_(Christianity)\" \\o \"Minister (Christianity)\" \u0014Christian clergy\u0015 speak Latin fluently, and it is taught in primary, secondary and post-secondary educational institutions around the world.\u0013 HYPERLINK \"https://en.wikipedia.org/wiki/Latin\" \\l \"cite_note-4\" \u0014[4]\u0015\u0013 HYPERLINK \"https://en.wikipedia.org/wiki/Latin\" \\l \"cite_note-5\" \u0014[5]\u0015\r",
    "author": "GroupDocs",
    "type": "Deletion"
  }
]
```

Each revision in the response includes:
- id: A unique identifier for the revision
- action: The current status of the revision (None, Accept, Reject)
- text: The text content affected by the revision
- author: The person who made the revision
- type: The type of revision (Insertion, Deletion, etc.)

## Step 4: Process the Revision Information

Once you have the revision information, you can process it according to your application's needs. Common use cases include:

1. Building a revision history view: Display all revisions with author information and timestamps
2. Implementing a review workflow: Allow users to approve or reject revisions
3. Generating revision statistics: Count revisions by author, type, or other criteria
4. Creating differential reports: Analyze the nature and extent of changes made to the document

## Try It Yourself

Now it's your turn to implement revision retrieval in your preferred programming language. Below are examples in various languages to help you get started.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new ReviewApi(configuration);

var sourceFile = new FileInfo
{
    FilePath = "source_files/word/source_with_revs.docx"
};

var request = new GetRevisionsRequest(sourceFile);

var revisions = apiInstance.GetRevisions(request);
Console.WriteLine("GetListOfRevisions: Revisions count: " + revisions.Count);

// Process the revisions
foreach (var revision in revisions)
{
    Console.WriteLine($"Revision ID: {revision.Id}");
    Console.WriteLine($"Type: {revision.Type}");
    Console.WriteLine($"Author: {revision.Author}");
    Console.WriteLine($"Text: {revision.Text.Substring(0, Math.Min(50, revision.Text.Length))}...");
    Console.WriteLine("---------------------");
}
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

revisions = api_instance.get_revisions(groupdocs_comparison_cloud.GetRevisionsRequest(source))

print(f"Revisions count: {len(revisions)}")

# Process the revisions
for revision in revisions:
    print(f"Revision ID: {revision.id}")
    print(f"Type: {revision.type}")
    print(f"Author: {revision.author}")
    # Show a preview of the text (up to 50 characters)
    text_preview = revision.text[:50] + "..." if len(revision.text) > 50 else revision.text
    print(f"Text: {text_preview}")
    print("---------------------")
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

 let request = new comparison_cloud.GetRevisionsRequest(source);  
 let revisions = await reviewApi.getRevisions(request);

 console.log("Revisions count: " + revisions.length);
 
 // Process the revisions
 for (let i = 0; i < revisions.length; i++) {
   const revision = revisions[i];
   console.log("Revision ID: " + revision.id);
   console.log("Type: " + revision.type);
   console.log("Author: " + revision.author);
   // Show a preview of the text (up to 50 characters)
   const textPreview = revision.text.length > 50 ? 
                       revision.text.substring(0, 50) + "..." : 
                       revision.text;
   console.log("Text: " + textPreview);
   console.log("---------------------");
 }
} catch (error) {
 console.log(error.message);
}
```

## Common Issues and Troubleshooting

1. No Revisions Found: If the API returns an empty array, verify that your document actually contains tracked changes. To test, open the document in Microsoft Word and check if the "Review" tab shows any revisions.

2. Character Encoding: The revision text may contain special characters or control codes used by Word's track changes feature. Handle these appropriately in your application.

3. Large Documents: For documents with a large number of revisions, consider implementing pagination or lazy loading in your UI to improve performance.

4. Unsupported Document Format: The revisions feature is primarily designed for Word documents (.docx, .doc). Other formats may not support revision tracking or may have limited support.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What API endpoint is used to retrieve revisions from a document?
2. What information is included in each revision object?
3. What is the difference between a revision and a change in GroupDocs.Comparison Cloud?
4. What are some common use cases for processing revision information?

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve the complete list of revisions from Word documents
- Access detailed information about each revision, including author, type, and content
- Process revision information programmatically for various use cases

## Further Practice

To reinforce your learning, try these exercises:
1. Create a function that filters revisions by author or type
2. Build a simple UI that displays revisions in a readable format
3. Implement statistics to show the number of revisions by author or type
4. Create a workflow that allows users to batch accept or reject revisions

## Next Steps

Now that you've learned to retrieve document revisions, check out these related tutorials:
- [Tutorial: How to Accept or Reject Revisions](/advanced-features/accept-reject-revisions/)
- [Tutorial: How to Accept All Revisions](/advanced-features/accept-all-revisions/)
- [Tutorial: How to Reject All Revisions](/advanced-features/reject-all-revisions/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

