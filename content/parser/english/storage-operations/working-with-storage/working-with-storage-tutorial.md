---
title: How to Work with Storage in GroupDocs.Parser Cloud Tutorial
url: /storage-operations/working-with-storage/
weight: 1
description: Learn how to check storage existence, manage storage objects, and monitor space usage with this step-by-step GroupDocs.Parser Cloud API tutorial.
---

# Tutorial: How to Work with Storage in GroupDocs.Parser Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Check if a specific cloud storage exists
- Verify the existence of files and folders in your storage
- Monitor storage space usage
- Work with file versions in GroupDocs.Parser Cloud storage

## Prerequisites

Before you begin this tutorial, you need:

1. A GroupDocs Cloud account (if you don't have one, [register for free](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret (available from the [Dashboard](https://dashboard.groupdocs.cloud/#/apps))
3. Basic knowledge of REST APIs
4. Familiarity with your preferred programming language (C#, Java, or cURL)

## Introduction

Managing your cloud storage efficiently is crucial when working with documents in the cloud. GroupDocs.Parser Cloud API provides a set of powerful operations to help you manage your storage resources. In this tutorial, we'll explore how to check storage existence, verify objects in storage, monitor space usage, and work with file versions.

## Step 1: Setting Up Your Environment

Before working with storage operations, you need to set up authentication to access the GroupDocs.Parser Cloud API.

### Try it yourself

Create a new project in your preferred development environment and add the required dependencies:

For C#:
```csharp
// Install via NuGet:
// Install-Package GroupDocs.Parser-Cloud
```

For Java:
```java
// Add to pom.xml:
// <dependency>
//     <groupId>com.groupdocs</groupId>
//     <artifactId>groupdocs-parser-cloud</artifactId>
//     <version>latest-version</version>
// </dependency>
```

Then authenticate with your Client ID and Client Secret:

```csharp
// C# authentication example
string ClientId = "your-client-id";
string ClientSecret = "your-client-secret";
var configuration = new Configuration(ClientId, ClientSecret);
```

## Step 2: Checking Storage Existence

Let's start by learning how to check if a specific cloud storage exists.

### Understanding the API

The Storage Existence API lets you verify if a named storage configuration exists in your GroupDocs Cloud account.

### Implementation Example

Here's how to check storage existence using different methods:

#### Using cURL

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/parser/storage/MyStorage/exist" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create StorageApi instance
var apiInstance = new StorageApi(configuration);

// Storage name
string storageName = "MyStorage";

// Call API to check if storage exists
StorageExist response = apiInstance.StorageExists(new StorageExistsRequest(storageName));

// Check the result
Console.WriteLine("Storage exists: " + response.Exists);
```

#### Expected Response

```json
{
  "exists": true
}
```

### Try it yourself

1. Replace `{access_token}` with your actual token in the cURL example
2. In the C# example, replace "MyStorage" with your storage name
3. Run the code and check if your storage exists

## Step 3: Checking Object Existence

Next, let's learn how to verify if a specific file or folder exists in your storage.

### Implementation Example

#### Using cURL

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/parser/storage/exist/documents/sample.docx?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create StorageApi instance
var apiInstance = new StorageApi(configuration);

// File path
string path = "documents/sample.docx";
// Storage name
string storageName = "MyStorage";

// Call API to check if file exists
ObjectExist response = apiInstance.ObjectExists(new ObjectExistsRequest(path, storageName));

// Check the result
Console.WriteLine($"Object exists: {response.Exists}");
Console.WriteLine($"Is folder: {response.IsFolder}");
```

### Expected Response

```json
{
  "exists": true,
  "isFolder": false
}
```

## Step 4: Checking Storage Space Usage

Monitoring your storage space is important to ensure you have enough resources for your documents.

### Implementation Example

#### Using cURL

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/parser/storage/disc?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create StorageApi instance
var apiInstance = new StorageApi(configuration);

// Storage name
string storageName = "MyStorage";

// Call API to get disc usage
DiscUsage response = apiInstance.GetDiscUsage(new GetDiscUsageRequest(storageName));

// Print disc usage info
Console.WriteLine($"Total space: {response.TotalSize} bytes");
Console.WriteLine($"Used space: {response.UsedSize} bytes");
```

### Expected Response

```json
{
  "usedSize": 31032368,
  "totalSize": 3221225472
}
```

### Learning Checkpoint

Question: Why is monitoring storage space important when working with cloud document processing?
Answer: Monitoring storage space helps you manage resources efficiently, avoid running out of space during document processing operations, and plan for scaling your application as needed.

## Step 5: Working with File Versions

GroupDocs.Parser Cloud allows you to work with different versions of the same file.

### Implementation Example

#### Using cURL

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/parser/storage/version/documents/sample.docx?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create StorageApi instance
var apiInstance = new StorageApi(configuration);

// File path
string path = "documents/sample.docx";
// Storage name
string storageName = "MyStorage";

// Call API to get file versions
FileVersions response = apiInstance.GetFileVersions(new GetFileVersionsRequest(path, storageName));

// Print versions info
foreach (var version in response.Value)
{
    Console.WriteLine($"Version: {version.VersionId}");
    Console.WriteLine($"Is latest: {version.IsLatest}");
    Console.WriteLine($"Name: {version.Name}");
    Console.WriteLine($"Modified date: {version.ModifiedDate}");
    Console.WriteLine($"Size: {version.Size} bytes");
    Console.WriteLine();
}
```

### Expected Response

```json
{
  "value": [
    {
      "versionId": "null",
      "isLatest": true,
      "name": "sample.docx",
      "isFolder": false,
      "modifiedDate": "2022-08-16T19:45:05+00:00",
      "size": 347612,
      "path": "/documents/sample.docx"
    }
  ]
}
```

## Troubleshooting Tips

If you encounter issues while working with storage operations, consider these common solutions:

1. Authentication Errors: Ensure your Client ID and Client Secret are correct and that your access token is valid.
2. 404 Not Found Errors: Check that the storage name or file path is spelled correctly.
3. Permission Issues: Verify that your account has the necessary permissions to access the specified storage.

## What You've Learned

In this tutorial, you've learned how to:
- Check if a specific cloud storage exists
- Verify the existence of files and folders in your storage
- Monitor storage space usage
- Retrieve and work with file versions

These storage operations are foundational for efficiently managing your documents in the cloud with GroupDocs.Parser.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a storage monitoring tool that alerts you when space usage exceeds 80%
2. Build a version history viewer for a specific document
3. Implement a function to verify multiple files exist before processing them

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
