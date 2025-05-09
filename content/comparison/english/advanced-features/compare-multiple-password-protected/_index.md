---
title: How to Compare Multiple Password-Protected Documents Tutorial
description: Learn to compare multiple password-protected documents simultaneously using GroupDocs.Comparison Cloud API with this comprehensive tutorial
weight: 50
url: /advanced-features/compare-multiple-password-protected/
---

# Tutorial: How to Compare Multiple Password-Protected Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Compare multiple documents that are secured with passwords
- Properly provide multiple password credentials in API requests
- Process and understand comparison results from multiple secure documents

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Multiple password-protected documents ready for comparison in your cloud storage

## The Practical Scenario

Imagine you're a legal professional reviewing multiple versions of a confidential contract. You have four password-protected document versions:
- The original contract (source_protected.docx)
- Three revised versions (target_protected.docx, target_1_protected.docx, target_2_protected.docx)

You need to compare all versions simultaneously to identify all changes while maintaining document security. GroupDocs.Comparison Cloud API provides a streamlined way to accomplish this task.

## Step 1: Upload Your Password-Protected Documents to Cloud Storage

Before comparing documents, you need to upload them to cloud storage. For this tutorial, we'll work with:
- source_protected.docx (protected with password: "1231")
- target_protected.docx (protected with password: "5784")
- target_1_protected.docx (protected with password: "5784")
- target_2_protected.docx (protected with password: "5784")

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

## Step 3: Compare Multiple Password-Protected Documents

Now let's compare all four password-protected documents. Notice how we include the passwords for each document in the API request:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/comparison/comparisons" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'SourceFile': {
    'FilePath': 'source_files/word/source_protected.docx',
    'Password': '1231'
  },
  'TargetFiles': [
    {
      'FilePath': 'target_files/word/target_protected.docx',
      'Password': '5784'
    },
    {
      'FilePath': 'target_files/word/target_1_protected.docx',
      'Password': '5784'
    },
    {
      'FilePath': 'target_files/word/target_2_protected.docx',
      'Password': '5784'
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

## Step 4: Download and Review the Result

Use the link from the response to download the resulting document. This file contains all differences between the source document and all three target documents. The comparison result highlights additions, deletions, and modifications across all documents, providing a comprehensive view of all changes.

## Try It Yourself

Now it's your turn to implement multiple password-protected document comparison in your preferred programming language. Below are examples in various languages to help you get started.

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
        FilePath = "source_files/word/source_protected.docx",
        Password = "1231"
    },
    TargetFiles = new List<FileInfo> {
        new FileInfo {
            FilePath = "target_files/word/target_protected.docx",
            Password = "5784"
        },
        new FileInfo {
            FilePath = "target_files/word/target_1_protected.docx",
            Password = "5784"
        },
        new FileInfo {
            FilePath = "target_files/word/target_2_protected.docx",
            Password = "5784"
        }
    },
    OutputPath = "output/result.docx"
};

var request = new ComparisonsRequest(options);
var response = apiInstance.Comparisons(request);

Console.WriteLine("Output file link: " + response.Href);
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
 source.filePath = "source_files/word/source_protected.docx";
 source.password = "1231";
 let target = new comparison_cloud.FileInfo();
 target.filePath = "target_files/word/target_protected.docx";
 target.password = "5784";
 let target1 = new comparison_cloud.FileInfo();
 target1.filePath = "target_files/word/target_1_protected.docx";
 target1.password = "5784";
 let target2 = new comparison_cloud.FileInfo();
 target2.filePath = "target_files/word/target_2_protected.docx";
 target2.password = "5784";
 
 let options = new comparison_cloud.ComparisonOptions();
 options.sourceFile = source;
 options.targetFiles = [target, target1, target2];
 options.outputPath = "output/result.docx";

 let request = new comparison_cloud.ComparisonsRequest(options);  

 let response = await compareApi.comparisons(request);
 console.log("Output file link: " + response.href);
} catch (error) {
 console.log(error.message);
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-python-samples
import groupdocs_comparison_cloud

client_id = "YOUR_CLIENT_ID" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

api_instance = groupdocs_comparison_cloud.CompareApi.from_keys(client_id, client_secret)

source = groupdocs_comparison_cloud.FileInfo()
source.file_path = "source_files/word/source_protected.docx"
source.password = "1231"
target = groupdocs_comparison_cloud.FileInfo()
target.file_path = "target_files/word/target_protected.docx"
target.password = "5784"
target1 = groupdocs_comparison_cloud.FileInfo()
target1.file_path = "target_files/word/target_1_protected.docx"
target1.password = "5784"
target2 = groupdocs_comparison_cloud.FileInfo()
target2.file_path = "target_files/word/target_2_protected.docx"
target2.password = "5784"
options = groupdocs_comparison_cloud.ComparisonOptions()
options.source_file = source
options.target_files = [target, target1, target2]
options.output_path = "output/result.docx"

request = groupdocs_comparison_cloud.ComparisonsRequest(options)
response = api_instance.comparisons(request)

print("Output file link: " + response.href)
```

## Common Issues and Troubleshooting

1. Incorrect Password Errors: If you provide an incorrect password for any document, the comparison will fail. Double-check all passwords before submitting the request.

2. File Not Found: Verify the file paths for all documents. The paths should be relative to your cloud storage root.

3. Handling Multiple Documents: When comparing multiple documents, the API processes each target document against the source. The result contains changes from all target documents, which can be complex to interpret. Consider using styled output (see our [Customize Change Styles](/advanced-features/customize-changes-styles/) tutorial) to differentiate changes from different documents.

4. Performance Considerations: Comparing multiple password-protected documents requires more processing power and may take longer than comparing unprotected documents. Consider implementing asynchronous processing for better user experience.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. How do you specify passwords for multiple target documents in the API request?
2. Does each target document need to have the same password?
3. How can you make the result document more readable when comparing multiple documents?

## What You've Learned

In this tutorial, you've learned how to:
- Compare multiple documents that are secured with passwords
- Properly provide multiple password credentials in API requests
- Process and understand comparison results from multiple secure documents

## Further Practice

To reinforce your learning, try these exercises:
1. Compare multiple password-protected documents of different formats (e.g., DOCX, PDF, XLSX)
2. Implement custom styling to distinguish changes from different target documents
3. Create a workflow that secures the result document with a new password

## Next Steps

Now that you've learned to compare multiple password-protected documents, check out these related tutorials:
- [Tutorial: How to Customize Change Styles](/advanced-features/customize-changes-styles/)
- [Tutorial: How to Set Password for Result Document](/advanced-features/set-password-for-resultant-document/)
- [Tutorial: How to Get List of Changes](/advanced-features/get-list-of-changes/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
