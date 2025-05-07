---
title: Learn to Work with FileInfo Structure in GroupDocs.Annotation Cloud Tutorial
weight: 1
url: /advanced-data-structures/fileinfo/
description: This tutorial teaches developers how to effectively implement the FileInfo structure to manage document inputs in GroupDocs.Annotation Cloud API
---

# Tutorial: Learn to Work with FileInfo Structure in GroupDocs.Annotation Cloud

## Learning Objectives

In this tutorial, you'll learn:
- What the FileInfo structure is and why it's essential
- How to properly configure the FileInfo parameters
- Implementing FileInfo in different annotation scenarios
- Best practices for handling password-protected documents

## Prerequisites

Before starting this tutorial, ensure you have:
- A GroupDocs.Annotation Cloud account ([get a free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Basic knowledge of REST APIs and your preferred programming language
- Development environment set up for the SDK of your choice (Python, Java, or C#)

## What is FileInfo?

FileInfo is a fundamental data structure in GroupDocs.Annotation Cloud that describes input files for annotation operations. It serves as the foundation for almost all operations, allowing you to specify:

- The location of your document
- Storage information
- Version details
- Password protection parameters

## Step 1: Understanding the FileInfo Structure

The FileInfo structure contains the following key fields:

| Field | Description | Required |
|---|---|---|
| FilePath | The path to your file in storage | Yes |
| StorageName | Name of the storage where the file is located | No (uses default if not specified) |
| VersionId | Version identifier for the document | No |
| Password | Password for protected documents | No (required only for protected files) |

Let's examine a basic FileInfo JSON example:

```json
{
    "FilePath": "documents/sample.pdf",
    "StorageName": "MyStorage",
    "VersionId": "1.0",
    "Password": "p@ssw0rd"
}
```

## Step 2: Creating a Basic FileInfo Instance

Let's start implementing the FileInfo structure in different programming languages.

### Using cURL

<script src="https://gist.github.com/groupdocs-annotation-cloud/tutorial-fileinfo-basic-curl.js"></script>

### Using Python SDK

<script src="https://gist.github.com/groupdocs-annotation-cloud/tutorial-fileinfo-basic-python.js"></script>

### Using Java SDK

<script src="https://gist.github.com/groupdocs-annotation-cloud/tutorial-fileinfo-basic-java.js"></script>

### Using C# SDK

<script src="https://gist.github.com/groupdocs-annotation-cloud/tutorial-fileinfo-basic-csharp.js"></script>

## Step 3: Working with Password-Protected Documents

When dealing with password-protected documents, proper configuration of the FileInfo structure is crucial.

### Try it yourself

1. Create a FileInfo instance for a password-protected PDF document
2. Implement error handling for incorrect passwords
3. Test your implementation with both correct and incorrect passwords

<script src="https://gist.github.com/groupdocs-annotation-cloud/tutorial-fileinfo-password-protected.js"></script>

## Step 4: Advanced FileInfo Usage Scenarios

### Using FileInfo with Different Storage Providers

GroupDocs.Annotation Cloud supports various storage options. Here's how to configure FileInfo for different storage providers:

<script src="https://gist.github.com/groupdocs-annotation-cloud/tutorial-fileinfo-storage-providers.js"></script>

### Troubleshooting Common Issues

When working with FileInfo, you might encounter these common issues:

1. File Not Found Errors
   - Ensure the FilePath is correct and the file exists in the specified location
   - Check storage permissions and accessibility

2. Password Issues
   - Verify the password is correct for protected documents
   - Ensure special characters are properly encoded

3. Storage Configuration Problems
   - Confirm your storage configuration is properly set up
   - Verify the StorageName parameter matches your configured storage

## What You've Learned

In this tutorial, you've learned:
- The structure and purpose of the FileInfo object
- How to create and configure FileInfo instances
- Working with different storage providers
- Handling password-protected documents
- Troubleshooting common FileInfo implementation issues

## Further Practice

To reinforce your learning, try these exercises:
1. Create a FileInfo structure for a document in Amazon S3 storage
2. Implement a function that validates FileInfo parameters before submission
3. Build a FileInfo generator that handles multiple file types with appropriate configurations

## Next Steps

Now that you've mastered the FileInfo structure, continue your learning journey with our [Tutorial: How to Implement AnnotationInfo in Your Applications](/advanced-data-structures/annotationinfo/).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.annotation-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Post them on our [support forum](https://forum.groupdocs.cloud/c/annotation/10/).
