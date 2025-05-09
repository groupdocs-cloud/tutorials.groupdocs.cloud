---
title: Learn to Work with File Info Structure in GroupDocs.Comparison Cloud
description: This tutorial teaches you how to properly configure file information for document comparison operations using GroupDocs.Comparison Cloud API.
weight: 60
url: /advanced-data-structures/fileinfo/
---

# Tutorial: How to Work with FileInfo Structure

In this tutorial, you'll learn how to effectively use the FileInfo data structure in GroupDocs.Comparison Cloud API. By the end, you'll be able to properly configure files for comparison operations including handling password-protected documents and accessing files from different storage locations.

## Learning Objectives

- Configure the basic FileInfo structure for document comparison
- Access files from different storage providers
- Work with password-protected documents
- Specify particular versions of documents for comparison

## Prerequisites

- GroupDocs.Comparison Cloud API credentials
- Basic knowledge of REST API concepts
- A development environment with your preferred language (examples available in cURL, Python, Java, and C#)

## Understanding FileInfo Structure

The FileInfo data structure is fundamental to GroupDocs.Comparison Cloud API as it describes the input files for comparison operations. Let's examine its structure:

```javascript
{
    "FilePath": "string",
    "VersionId": "string",
    "StorageName": "string",
    "Password": "string"
}
```

Key properties include:

| Property | Description |
|---|---|
| FilePath | Path of the file in the cloud storage |
| VersionId | File Version (optional) |
| StorageName | Name of the cloud storage (optional, default storage used if omitted) |
| Password | Password for protected documents |

## Implementing FileInfo in Your Application

### Step 1: Set Up Basic FileInfo Configuration

Let's start with a simple scenario where we need to compare two regular documents from the default storage:

<script src="https://gist.github.com/groupdocs-comparison-cloud/58f5dd43b7c4daabbce789b31e622201.js"></script>

#### Try it yourself
Create a basic FileInfo object pointing to a document in your default storage and print its contents to verify it's correctly configured.

### Step 2: Working with Password-Protected Documents

Many business documents are password-protected. Here's how to configure FileInfo for such documents:

<script src="https://gist.github.com/groupdocs-comparison-cloud/9a23fbe7c87623456789ab31e622202.js"></script>

#### Try it yourself
Create a FileInfo object for a password-protected document in your storage. Try comparing it with another document to verify the password is correctly applied.

### Step 3: Specifying Storage Providers

If you have multiple storage providers configured, you can specify which one to use:

<script src="https://gist.github.com/groupdocs-comparison-cloud/6b78f13e5d6789ab31e622203.js"></script>

#### Troubleshooting Tip
If you receive a "Storage not found" error, verify the storage name is correctly spelled and properly registered in your GroupDocs.Comparison Cloud dashboard.

### Step 4: Working with File Versions

For systems that maintain document versions, you can specify which version to use:

<script src="https://gist.github.com/groupdocs-comparison-cloud/7c23eb67d9ab31e622204.js"></script>

## What You've Learned

In this tutorial, you've learned:
- How to configure the basic FileInfo structure
- How to work with password-protected documents
- How to specify storage providers
- How to work with document versions

This knowledge forms the foundation for all document comparison operations in GroupDocs.Comparison Cloud API.

## Further Practice

Try these exercises to reinforce your learning:
1. Create a program that compares a document with multiple versions of itself
2. Write a function that validates if FileInfo is correctly configured before sending to the API
3. Implement error handling for common FileInfo configuration issues

## Next Tutorial

Ready to learn more? Continue to our [Tutorial: Understanding Format Data Structure](/advanced-data-structures/format/) to learn how GroupDocs.Comparison Cloud API handles different file formats.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
