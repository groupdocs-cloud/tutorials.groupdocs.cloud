---
title: How to Customize Change Styles Tutorial
description: Learn to customize the appearance of different change types in document comparisons using GroupDocs.Comparison Cloud API in this detailed tutorial
weight: 80
url: /advanced-features/customize-changes-styles/
---

# Tutorial: How to Customize Change Styles

## Learning Objectives

In this tutorial, you'll learn how to:
- Customize the visual appearance of different change types in document comparisons
- Configure styling options for inserted, deleted, and modified content
- Enhance the readability of comparison results through visual differentiation

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Source and target documents ready for comparison in your cloud storage

## The Practical Scenario

Imagine you're preparing a document comparison for a legal review where multiple stakeholders need to quickly identify different types of changes. Default styling might not provide enough visual distinction between insertions, deletions, and modifications. By customizing change styles, you can make the comparison result more intuitive and easier to analyze.

## Understanding Change Style Options

GroupDocs.Comparison Cloud API allows you to customize the appearance of three types of changes:

1. InsertedItemsStyle - Controls the appearance of content that has been added
2. DeletedItemsStyle - Controls the appearance of content that has been removed
3. ChangedItemsStyle - Controls the appearance of content that has been modified

For each change type, you can customize:
- FontColor - The color of the text (using decimal RGB color values)
- HighlightColor - The background color behind the text
- Bold - Whether the text appears in bold
- Italic - Whether the text appears in italic
- Underline - Whether the text is underlined
- StrikeThrough - Whether the text has a line through it

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

## Step 3: Compare Documents with Custom Styles

Now let's compare the documents with custom change styles:

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
    }
  ],
  'OutputPath': 'output/result.docx',
  'Settings': {
    'InsertedItemsStyle': {
      'HighlightColor': '14297642',
      'FontColor': '5102122',
      'Underline': true
    },
    'DeletedItemsStyle': {
      'FontColor': '14166746',
      'Bold': true
    },
    'ChangedItemsStyle': {
      'FontColor': '14320170',
      'Italic': true
    }
  }
}"
```

In this example:
- Inserted content appears with a light blue highlight, green text, and underlined
- Deleted content appears in red text and bold
- Changed content appears in blue text and italic

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

Use the link from the response to download the resulting document. This file contains all differences between the source and target documents, styled according to your custom settings. Review how the different change types appear and adjust the styling if needed for better visibility.

## Understanding Color Values

The API uses decimal color values for both FontColor and HighlightColor. Here are some common colors and their decimal values:

- Red: 16711680 or 14166746 (darker red)
- Green: 65280 or 5102122 (darker green)
- Blue: 255 or 14320170 (darker blue)
- Yellow: 16776960
- Purple: 16711935
- Teal: 65535
- Black: 0
- White: 16777215

## Try It Yourself

Now it's your turn to implement custom change styles in your preferred programming language. Below are examples in various languages to help you get started.

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
    },
    Settings = new Settings {
        InsertedItemsStyle = new ItemsStyle
        {
            HighlightColor = "14297642", // Light blue
            FontColor = "5102122", // Dark green
            Underline = true,
        },
        DeletedItemsStyle = new ItemsStyle
        {
            FontColor = "14166746", // Dark red
            Bold = true,
        },
        ChangedItemsStyle = new ItemsStyle
        {
            FontColor = "14320170", // Dark blue
            Italic = true,
        },
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
options = groupdocs_comparison_cloud.ComparisonOptions()
options.source_file = source
options.target_files = [target]
options.output_path = "output/result.docx"
settings = groupdocs_comparison_cloud.Settings()
settings.inserted_items_style = groupdocs_comparison_cloud.ItemsStyle()
settings.inserted_items_style.highlight_color = "14297642" # Light blue
settings.inserted_items_style.font_color = "5102122" # Dark green
settings.inserted_items_style.underline = True
settings.deleted_items_style = groupdocs_comparison_cloud.ItemsStyle()
settings.deleted_items_style.font_color = "14166746" # Dark red
settings.deleted_items_style.bold = True
settings.changed_items_style = groupdocs_comparison_cloud.ItemsStyle()
settings.changed_items_style.font_color = "14320170" # Dark blue
settings.changed_items_style.italic = True
options.settings = settings

request = groupdocs_comparison_cloud.ComparisonsRequest(options)
response = api_instance.comparisons(request)

print("Output file link: " + response.href)
```

## Common Issues and Troubleshooting

1. Incorrect Color Values: Color values must be provided as strings containing decimal RGB values. If you see unexpected colors, double-check your color values.

2. Style Not Applied: If your custom styles aren't appearing in the result document, ensure that your settings object is properly structured and that all property names match exactly as shown in the examples.

3. Format Compatibility: Some style features may not be fully supported in all document formats. Test your style settings with the specific document formats you're using.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. Which three change types can you style in GroupDocs.Comparison Cloud?
2. What properties can you customize for each change type?
3. How are color values represented in the API?

## What You've Learned

In this tutorial, you've learned how to:
- Customize the visual appearance of different change types in document comparisons
- Configure styling options for inserted, deleted, and modified content
- Enhance the readability of comparison results through visual differentiation

## Further Practice

To reinforce your learning, try these exercises:
1. Create style configurations optimized for different use cases (legal review, editorial review, code review)
2. Experiment with different color combinations to find the most effective visual distinction
3. Create a user interface that allows users to customize styles before comparison

## Next Steps

Now that you've learned to customize change styles, check out these related tutorials:
- [Tutorial: How to Adjust Comparison Sensitivity](/advanced-features/compare-sensitivity/)
- [Tutorial: How to Get List of Changes](/advanced-features/get-list-of-changes/)
- [Tutorial: How to Compare Multiple Documents](/advanced-features/compare-multiple-documents/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

