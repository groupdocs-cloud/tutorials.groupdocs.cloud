---
title: How to Adjust Comparison Sensitivity Tutorial
description: Learn to fine-tune document comparison precision by adjusting sensitivity settings in GroupDocs.Comparison Cloud API with this step-by-step tutorial
weight: 70
url: /advanced-features/compare-sensitivity/
---

# Tutorial: How to Adjust Comparison Sensitivity

## Learning Objectives

In this tutorial, you'll learn how to:
- Adjust comparison sensitivity to balance speed and accuracy
- Understand how sensitivity values affect comparison results
- Configure the optimal sensitivity for different document types and comparison needs

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Source and target documents ready for comparison in your cloud storage

## The Practical Scenario

Imagine you're developing a contract review application where users need to compare different versions of legal documents. Some users need quick comparisons to spot major changes, while others require thorough analysis that identifies even the smallest differences. By understanding and implementing comparison sensitivity settings, you can provide both options in your application.

## Understanding Comparison Sensitivity

GroupDocs.Comparison Cloud allows you to adjust the comparison sensitivity to achieve an optimal balance between comparison speed and accuracy. The sensitivity value ranges from 0 to 100:

- Minimal Value (0): Provides the fastest comparison speed but may produce lower quality results. With this setting, if there is at least one common letter in two compared words, these words will not be treated as fully inserted and deleted.

- Default Value (75): This balanced setting means comparison occurs when the percentage of deleted or inserted elements in relation to all elements does not exceed 75%.

- Maximum Value (100): Comparison occurs at any length of a common sub-sequence of two compared objects. This provides the highest quality but slowest comparison speed.

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

## Step 3: Compare Documents with Adjusted Sensitivity

Let's compare the documents with maximum sensitivity (100) to ensure we catch every difference:

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
    'SensitivityOfComparison': 100
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

## Step 4: Experiment with Different Sensitivity Values

To understand how sensitivity affects your specific documents, try comparing them with different sensitivity values:

1. Low Sensitivity (25): Will identify only major changes and provide faster results
2. Medium Sensitivity (50): A balanced approach suitable for most comparisons
3. High Sensitivity (75): The default setting, good for detailed comparisons
4. Maximum Sensitivity (100): Will identify every possible difference, but may be slower

For each test, modify the "SensitivityOfComparison" value in your API request and compare the results.

## Try It Yourself

Now it's your turn to implement adjustable comparison sensitivity in your preferred programming language. Below are examples in various languages to help you get started.

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
        SensitivityOfComparison = 100
    },
    OutputPath = "output/result.docx"
};

var request = new ComparisonsRequest(options);
var response = apiInstance.Comparisons(request);

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

Settings settings = new Settings();
settings.setSensitivityOfComparison(100);

ComparisonOptions options = new ComparisonOptions();
options.setSourceFile(sourceFileInfo);
options.addTargetFilesItem(targetFileInfo);
options.setSettings(settings);
options.setOutputPath("output/result.docx");

ComparisonsRequest request = new ComparisonsRequest(options);
Link response = apiInstance.comparisons(request);

System.out.println("Output file link: " + response.getHref());
```

## Common Issues and Troubleshooting

1. Too Many Differences: If the comparison result shows too many minor differences that aren't relevant to your needs, try reducing the sensitivity value.

2. Missing Differences: If the comparison doesn't identify changes you know exist, increase the sensitivity value to catch more subtle differences.

3. Performance Issues: High sensitivity values (especially 100) may cause slower performance with very large documents. Consider using a lower sensitivity value for initial quick comparisons and higher values for detailed analysis.

4. Format-Specific Behavior: Different document formats might respond differently to sensitivity settings. Test with your specific document types to find the optimal settings.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What is the range of possible sensitivity values?
2. How does a higher sensitivity value affect comparison quality and speed?
3. What sensitivity value would you use for a quick overview of major changes?
4. What is the default sensitivity value in GroupDocs.Comparison Cloud?

## What You've Learned

In this tutorial, you've learned how to:
- Adjust comparison sensitivity to balance speed and accuracy
- Understand how sensitivity values affect comparison results
- Configure the optimal sensitivity for different document types and comparison needs

## Further Practice

To reinforce your learning, try these exercises:
1. Compare the same pair of documents with sensitivity values of 25, 50, 75, and 100, and observe the differences in the results
2. Create a test with documents that have minor spelling changes to see how different sensitivity values affect their detection
3. Develop a function that automatically selects the appropriate sensitivity based on document size and user requirements

## Next Steps

Now that you've learned to adjust comparison sensitivity, check out these related tutorials:
- [Tutorial: How to Customize Change Styles](/advanced-features/customize-changes-styles/)
- [Tutorial: How to Get List of Changes](/advanced-features/get-list-of-changes/)
- [Tutorial: How to Get Change Coordinates](/advanced-features/get-changes-coordinates/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

