---
title: How to Set Password for Result Document Tutorial
description: Learn to secure comparison results with password protection using GroupDocs.Comparison Cloud API in this comprehensive step-by-step tutorial
weight: 130
url: /advanced-features/set-password-for-resultant-document/
---

# Tutorial: How to Set Password for Result Document

## Learning Objectives

In this tutorial, you'll learn how to:
- Secure the output of document comparisons with password protection
- Configure password protection settings for result documents
- Implement secure document comparison workflows

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Source and target documents ready for comparison in your cloud storage

## The Practical Scenario

Imagine you're comparing confidential contract documents for a merger and acquisition deal. The comparison results will highlight sensitive changes between contract versions, and this information needs to be secured before sharing with authorized stakeholders. By adding password protection to the result document, you ensure that only authorized individuals can access the comparison results.

With GroupDocs.Comparison Cloud API, you can easily add password protection to the output document as part of your comparison workflow, enhancing document security and confidentiality.

## Understanding Password Protection Options

When using GroupDocs.Comparison Cloud to compare documents, you can specify password protection for the result document using the following settings:

- PasswordSaveOption: Determines how to handle password protection. Values include:
  - None: No password protection (default)
  - User: Apply a user-defined password
  - Source: Use the same password as the source document
  - Target: Use the same password as the target document

- Password: The actual password string to apply when PasswordSaveOption is set to "User"

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

## Step 3: Compare Documents with Password Protection

Now, let's compare the documents and set a password for the result document:

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
    'PasswordSaveOption': 'User',
    'Password': '3333'
  }
}"
```

In this example, we've set:
- `PasswordSaveOption` to `User` - indicating we want to use a custom password
- `Password` to `3333` - our chosen password for the result document

Upon successful execution, the API will return a link to the resulting document:

```json
{
  "href": "https://api.groupdocs.cloud/v2.0/comparison/storage/file/output/result.docx",
  "rel": "output/result.docx",
  "type": "file",
  "title": "result.docx"
}
```

## Step 4: Download and Verify the Result

Use the link from the response to download the resulting document. When you try to open this document in a word processor like Microsoft Word, you'll be prompted to enter the password (`3333` in our example). This confirms that the password protection has been successfully applied.

## Try It Yourself

Now it's your turn to implement password protection for comparison results in your preferred programming language. Below are examples in various languages to help you get started.

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
        PasswordSaveOption = Settings.PasswordSaveOptionEnum.User,
        Password = "3333"
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
settings.setPasswordSaveOption(Settings.PasswordSaveOptionEnum.USER);
settings.setPassword("3333");

ComparisonOptions options = new ComparisonOptions();
options.setSourceFile(sourceFileInfo);
options.addTargetFilesItem(targetFileInfo);
options.setSettings(settings);
options.setOutputPath("output/result.docx");

ComparisonsRequest request = new ComparisonsRequest(options);
Link response = apiInstance.comparisons(request);

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
options = groupdocs_comparison_cloud.ComparisonOptions()
options.source_file = source
options.target_files = [target]
options.output_path = "output/result.docx"
settings = groupdocs_comparison_cloud.Settings()
settings.password_save_option = "User"
settings.password = "3333"
options.settings = settings

request = groupdocs_comparison_cloud.ComparisonsRequest(options)
response = api_instance.comparisons(request)

print("Output file link: " + response.href)
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
 let settings = new comparison_cloud.Settings();
 settings.password = "3333";
 settings.passwordSaveOption = comparison_cloud.Settings.PasswordSaveOptionEnum.User;
 let options = new comparison_cloud.ComparisonOptions();
 options.sourceFile = source;
 options.targetFiles = [target];
 options.outputPath = "output/result.docx";
 options.settings = settings;

 let request = new comparison_cloud.ComparisonsRequest(options);  

 let response = await compareApi.comparisons(request);
 console.log("Output file link: " + response.href);
} catch (error) {
 console.log(error.message);
}
```

## Exploring Other Password Protection Options

Besides setting a custom user password, you can also choose to inherit the password from either the source or target document:

### Using Source Document's Password

```csharp
Settings = new Settings {
    PasswordSaveOption = Settings.PasswordSaveOptionEnum.Source
}
```

This is useful when you want to maintain the same security level as the original document.

### Using Target Document's Password

```csharp
Settings = new Settings {
    PasswordSaveOption = Settings.PasswordSaveOptionEnum.Target
}
```

This can be appropriate when the target document represents a newer version with updated security requirements.

## Implementing Secure Document Workflows

Password protection is just one aspect of secure document workflows. Consider these additional best practices:

1. Use Strong Passwords: Choose complex passwords that are difficult to guess.

2. Secure Password Transmission: Avoid hardcoding passwords in your application code. Instead, use secure methods to transmit passwords, such as environment variables or secure key management services.

3. Temporary Result Files: Configure your workflow to delete result files from cloud storage after they've been downloaded and securely stored in your system.

4. Access Control: Implement proper access controls in your application to ensure only authorized users can initiate document comparisons and access results.

## Common Issues and Troubleshooting

1. Format Compatibility: Not all document formats support password protection. This feature works primarily with Word documents (.docx, .doc), Excel spreadsheets (.xlsx, .xls), and PowerPoint presentations (.pptx, .ppt).

2. Password Recovery: If the password is lost, there is no built-in recovery method. Ensure passwords are securely stored and accessible when needed.

3. API Parameter Validation: The API performs validation on the `PasswordSaveOption` parameter. If an invalid value is provided, an error will be returned.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What are the available options for `PasswordSaveOption`?
2. When would you use `PasswordSaveOption.Source` instead of `PasswordSaveOption.User`?
3. What document formats typically support password protection?
4. How would you implement a workflow where different comparison results require different password strengths?

## What You've Learned

In this tutorial, you've learned how to:
- Secure the output of document comparisons with password protection
- Configure different password protection settings for result documents
- Implement secure document comparison workflows
- Choose the appropriate password source based on your security requirements

## Further Practice

To reinforce your learning, try these exercises:
1. Create a function that generates strong random passwords for comparison results
2. Implement a workflow that uses different password sources based on document type
3. Build a system that securely stores and manages passwords for comparison results
4. Create a batch comparison process that applies password protection to multiple comparison results

## Next Steps

Now that you've learned to secure comparison results with passwords, check out these related tutorials:
- [Tutorial: How to Compare Password-Protected Documents](/advanced-features/compare-password-protected/)
- [Tutorial: How to Compare Multiple Documents](/advanced-features/compare-multiple-documents/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

