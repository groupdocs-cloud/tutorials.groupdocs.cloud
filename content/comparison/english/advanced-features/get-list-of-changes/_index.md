---
title: How to Get a List of Changes from Comparisons Tutorial
description: Learn to programmatically retrieve and process all changes between documents using GroupDocs.Comparison Cloud API in this step-by-step tutorial
weight: 100
url: /advanced-features/get-list-of-changes/
---

# Tutorial: How to Get a List of Changes from Comparisons

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve a comprehensive list of all changes between source and target documents
- Understand the structure and properties of change information objects
- Process and analyze comparison results programmatically

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Source and target documents ready for comparison in your cloud storage

## The Practical Scenario

Imagine you're developing a contract management system where legal teams need to track and review all changes between different versions of a document. While a visual comparison with highlighted changes is useful for manual review, programmatically accessing a structured list of changes enables you to:

1. Build automated reports of all modifications
2. Implement custom approval workflows based on the type and extent of changes
3. Create statistics and analytics on document revisions
4. Integrate with other systems that need to process document changes

GroupDocs.Comparison Cloud API provides a dedicated endpoint to retrieve a comprehensive list of all changes between documents, allowing you to implement these advanced features.

## Understanding Change Information Objects

Before we start implementing, let's understand what information is available in the change objects returned by the API:

- ID: A unique identifier for each change
- Type: The type of change (Inserted, Deleted, Modified, etc.)
- Text: The affected text content
- TargetText: The text in the target document (for context)
- Authors: Who made the change (if available in the document)
- StyleChangeInfo: Information about style changes
- PageInfo: Information about the page where the change occurs
- Box: Coordinates of the change within the page (when requested)

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

Now, let's retrieve the list of all changes between the source and target documents:

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

The API will return a JSON array containing all identified changes between the documents. Here's a sample of what the response might look like:

```json
[
  {
    "id": 0,
    "comparisonAction": "None",
    "type": "Inserted",
    "text": "lol",
    "targetText": "Latin (i/ˈlætɪn/, /ˈlætɪn/; Latin: lingua latīna, IPA: [ˈlɪŋɡʷa laˈtiːna]) is a classical language, originally spoken inLatium, Italy, which belongs to the Italic branch of the Indo-European languages.[3] The Latin alphabet is derived from the Etruscan and Greek alphabetslol.",
    "authors": [
      "GroupDocs"
    ],
    "styleChangeInfo": [],
    "pageInfo": {
      "width": 0,
      "height": 0,
      "pageNumber": 0
    },
    "box": {
      "height": 0,
      "width": 0,
      "x": 0,
      "y": 0
    }
  },
  // More changes...
]
```

## Step 4: Get Changes with Coordinates

If you need spatial information about where each change appears on the page, you can request coordinate data by setting the `CalculateComponentCoordinates` option to `true`:

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
  ],
  'Settings': {
    'CalculateComponentCoordinates': true
  }
}"
```

With this option enabled, each change in the response will include precise coordinates in the `box` property and page dimensions in the `pageInfo` property:

```json
"pageInfo": {
  "width": 612,
  "height": 792,
  "pageNumber": 1
},
"box": {
  "height": 16.0934353,
  "width": 14.0021219,
  "x": 388.503021,
  "y": 115.730377
}
```

These coordinates can be useful for creating custom visualizations or overlaying change information on document previews.

## Try It Yourself

Now it's your turn to implement change detection and analysis in your preferred programming language. Below are examples in various languages to help you get started.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);

var apiInstance = new CompareApi(configuration);
var options = new ComparisonOptions
{
    SourceFile = new FileInfo
    {
        FilePath = "source_files/word/source.docx"
    },
    TargetFiles = new List<FileInfo> {
        new FileInfo {
            FilePath = "target_files/word/target.docx"
        }
    }
};

var request = new PostChangesRequest(options);
var changes = apiInstance.PostChanges(request);

// Process the changes
Console.WriteLine($"Total changes found: {changes.Count}");
foreach (var change in changes.Take(5)) // Display first 5 changes
{
    Console.WriteLine($"Change ID: {change.Id}");
    Console.WriteLine($"Type: {change.Type}");
    Console.WriteLine($"Text: {change.Text}");
    Console.WriteLine("---------------------");
}
```

### Node.js Example

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-node-samples
global.comparison_cloud = require("groupdocs-comparison-cloud");

global.clientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
global.clientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

global.compareApi = comparison_cloud.CompareApi.fromKeys(clientId, clientSecret);

try {
 let source = new comparison_cloud.FileInfo();
 source.filePath = "source_files/word/source.docx";
 let target = new comparison_cloud.FileInfo();
 target.filePath = "target_files/word/target.docx";  
 let options = new comparison_cloud.ComparisonOptions();
 options.sourceFile = source;
 options.targetFiles = [target];

 let request = new comparison_cloud.PostChangesRequest(options);  
 let changes = await compareApi.postChanges(request);

 console.log("Total changes found: " + changes.length);
 
 // Display the first 5 changes
 for (let i = 0; i < Math.min(5, changes.length); i++) {
  const change = changes[i];
  console.log("Change Type: " + change.type);
  console.log("Text: " + change.text);
  console.log("---------------------");
 }
} catch (error) {
 console.log(error.message);
}
```

## Common Issues and Troubleshooting

1. Large Documents: When working with very large documents, the list of changes might be extensive. Consider implementing pagination or filtering in your application to handle large result sets efficiently.

2. Complex Formatting: Documents with complex formatting might generate many style-related changes. If you're only interested in content changes, you might need to filter the results to focus on insertions, deletions, and modifications.

3. Performance Considerations: Getting the list of changes requires the API to perform a full comparison. For large documents, this operation might take some time. Consider implementing asynchronous processing for better user experience.

4. Coordinate Calculation: Enabling the `CalculateComponentCoordinates` option provides valuable spatial information but might slightly increase processing time. Only enable this option when you need the coordinate data.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What API endpoint is used to retrieve the list of changes?
2. What types of changes can be detected by GroupDocs.Comparison Cloud?
3. How can you get the spatial coordinates of changes within the document pages?
4. What property would you use to identify who made a particular change?

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve a comprehensive list of all changes between source and target documents
- Understand the structure and properties of change information objects
- Request spatial coordinates for more detailed change information
- Process and analyze comparison results programmatically

## Further Practice

To reinforce your learning, try these exercises:
1. Create a function that categorizes changes by type (insertions, deletions, modifications)
2. Implement a change filtering system based on author information
3. Develop a simple reporting tool that generates a summary of all changes
4. Create a visualization that maps changes to their locations in the document

## Next Steps

Now that you've learned to retrieve and process the list of changes, check out these related tutorials:
- [Tutorial: How to Get Change Coordinates](/advanced-features/get-changes-coordinates/)
- [Tutorial: How to Accept or Reject Document Changes](/advanced-features/accept-reject-document-changes/)
- [Tutorial: How to Customize Change Styles](/advanced-features/customize-changes-styles/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
