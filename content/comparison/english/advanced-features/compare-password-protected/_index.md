---
title: How to Compare Password-Protected Documents Tutorial
description: Learn to securely compare password-protected documents using GroupDocs.Comparison Cloud API with this comprehensive step-by-step tutorial
weight: 60
url: /advanced-features/compare-password-protected/
---

# Tutorial: How to Compare Password-Protected Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Compare documents that are protected with passwords
- Properly provide password credentials in API requests
- Understand security considerations when comparing confidential documents

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Password-protected source and target documents ready for comparison in your cloud storage

## The Practical Scenario

Consider a scenario where you're working with confidential financial reports that are password-protected. You need to compare different versions of these reports to identify changes between them while maintaining the security of the documents. GroupDocs.Comparison Cloud API allows you to compare these password-protected documents without compromising their security.

## Step 1: Upload Your Password-Protected Documents to Cloud Storage

Before comparing documents, you need to upload them to cloud storage. For this tutorial, we'll work with:
- source_protected.docx (protected with password: "1231")
- target_protected.docx (protected with password: "5784")

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

## Step 3: Compare Password-Protected Documents

Now let's compare the password-protected documents. Notice how we include the passwords in the API request:

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

## Download and Review the Result

Use the link from the response to download the resulting document. This file contains all differences between the password-protected source and target documents. Note that the result document is not password-protected by default. If you want to secure the result document, refer to our tutorial on [setting a password for the result document](/advanced-features/set-password-for-resultant-document/).

## Try It Yourself

Now it's your turn to implement password-protected document comparison in your preferred programming language. Below are examples in various languages to help you get started.

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
        }
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
sourceFileInfo.setFilePath("source_files/word/source_protected.docx");
sourceFileInfo.setPassword("1231");
FileInfo targetFileInfo = new FileInfo();
targetFileInfo.setFilePath("target_files/word/target_protected.docx");
targetFileInfo.setPassword("5784");

ComparisonOptions options = new ComparisonOptions();
options.setSourceFile(sourceFileInfo);
options.addTargetFilesItem(targetFileInfo);
options.setOutputPath("output/result.docx");

ComparisonsRequest request = new ComparisonsRequest(options);
Link response = apiInstance.comparisons(request);

System.out.println("Output file link: " + response.getHref());
```

## Common Issues and Troubleshooting

1. Incorrect Password Error: If you provide an incorrect password, you'll receive an error. Double-check your passwords and ensure they match those used to secure the documents.

2. File Not Found: Verify the file paths for your source and target documents. The paths should be relative to your cloud storage root.

3. Password Required: If you attempt to compare a password-protected document without providing a password, the API will return an error indicating that a password is required.

4. Security Considerations: Remember that the passwords are sent in the API request. Always use HTTPS connections to ensure secure transmission of this sensitive information.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What property do you use to provide a password for a document in the API request?
2. Does the result document inherit password protection from the source or target documents?
3. Why is it important to use HTTPS when sending requests with document passwords?

## What You've Learned

In this tutorial, you've learned how to:
- Compare documents that are protected with passwords
- Properly provide password credentials in API requests
- Understand security considerations when comparing confidential documents

## Further Practice

To reinforce your learning, try these exercises:
1. Compare password-protected documents of different formats (e.g., DOCX, PDF, XLSX)
2. Create a workflow that secures the result document with a new password
3. Implement error handling for cases where incorrect passwords are provided

## Next Steps

Now that you've learned to compare password-protected documents, check out these related tutorials:
- [Tutorial: How to Compare Multiple Password-Protected Documents](/advanced-features/compare-multiple-password-protected/)
- [Tutorial: How to Set Password for Result Document](/advanced-features/set-password-for-resultant-document/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

