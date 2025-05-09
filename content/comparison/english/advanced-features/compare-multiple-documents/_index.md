---
title: How to Compare Multiple Documents Tutorial
description: Learn to compare more than two documents simultaneously using GroupDocs.Comparison Cloud API with this step-by-step tutorial for developers
weight: 40
url: /advanced-features/compare-multiple-documents/
---

# Tutorial: How to Compare Multiple Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Compare more than two documents simultaneously using GroupDocs.Comparison Cloud API
- Configure specific comparison settings for multiple document comparison
- Generate a comprehensive report highlighting differences across multiple files

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Source and target documents ready for comparison in your cloud storage

## The Practical Scenario

Imagine you're working with a team on a contract document that has gone through multiple revisions. You need to compare the original document with three different revised versions to identify all changes, additions, and deletions across all document versions. Manually checking would be time-consuming and error-prone, but GroupDocs.Comparison Cloud API makes this process straightforward.

## Step 1: Upload Your Documents to Cloud Storage

Before comparing documents, you need to upload them to cloud storage. In this tutorial, we'll work with four documents:
- source.docx (the original document)
- target.docx (first revision)
- target_1.docx (second revision)
- target_2.docx (third revision)

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

## Step 3: Compare Multiple Documents

Now let's compare the source document with all three target documents. We'll customize the appearance of inserted items by changing the font color to red (16711680 in decimal color code):

```bash
curl -v "https://api.groupdocs.cloud/v2.0/comparison/comparisons" \
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
    },
    {
      'FilePath': 'target_files/word/target_1.docx'
    },
    {
      'FilePath': 'target_files/word/target_2.docx'
    }
  ],
  'OutputPath': 'output/result.docx',
  'Settings': {
    'InsertedItemsStyle': {
      'FontColor': '16711680'
    }
  }
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

## Step 4: Download and Review the Result

Use the link from the response to download the resulting document. This file contains all differences between the source document and all three target documents, with inserted content highlighted in red as specified in our settings.

## Try It Yourself

Now it's your turn to implement multiple document comparison in your preferred programming language. Below are examples in various languages to help you get started.

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
        },
        new FileInfo {
            FilePath = "target_files/word/target_1.docx"
        },
        new FileInfo {
            FilePath = "target_files/word/target_2.docx"
        }
    },
    Settings = new Settings {
        InsertedItemsStyle = new ItemsStyle
        {
            FontColor = "16711680"
        }
    },
    OutputPath = "output/result.docx"
};

var request = new ComparisonsRequest(options);
var response = apiInstance.Comparisons(request);

Console.WriteLine("Output file link: " + response.Href);
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
target1 = groupdocs_comparison_cloud.FileInfo()
target1.file_path = "target_files/word/target_1.docx"
target2 = groupdocs_comparison_cloud.FileInfo()
target2.file_path = "target_files/word/target_2.docx"
options = groupdocs_comparison_cloud.ComparisonOptions()
options.source_file = source
options.target_files = [target, target1, target2]
options.output_path = "output/result.docx"
settings = groupdocs_comparison_cloud.Settings()
settings.inserted_items_style = groupdocs_comparison_cloud.ItemsStyle()
settings.inserted_items_style.font_color = "16711680"
options.settings = settings

request = groupdocs_comparison_cloud.ComparisonsRequest(options)
response = api_instance.comparisons(request)

print("Output file link: " + response.href)
```

## Common Issues and Troubleshooting

1. Authentication Error: Ensure your Client ID and Client Secret are correct and that your token hasn't expired.
   
2. File Not Found: Verify the file paths for your source and target documents. The paths should be relative to your cloud storage root.

3. Comparison Takes Too Long: When comparing very large documents or many documents simultaneously, the operation may take longer. Consider implementing asynchronous processing for better user experience.

4. Memory Issues: If you encounter memory-related errors when processing large documents, try optimizing your code or consider upgrading your service plan.

## What You've Learned

In this tutorial, you've learned how to:
- Compare multiple documents simultaneously using GroupDocs.Comparison Cloud API
- Apply custom styling to highlight inserted content
- Process the API response to obtain the resulting document

## Further Practice

To reinforce your learning, try these exercises:
1. Compare documents of different formats (e.g., DOCX, PDF, XLSX)
2. Experiment with different style settings for various change types
3. Implement a solution that allows users to select which documents to compare dynamically

## Next Steps

Now that you've learned to compare multiple documents, check out these related tutorials:

- [Tutorial: How to Customize Change Styles](/advanced-features/customize-changes-styles)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

