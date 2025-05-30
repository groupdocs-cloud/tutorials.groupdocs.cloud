---
title: Learn to Work with Storage in GroupDocs.Editor Cloud
url: /storage-handling-procedures/working-with-storage/
weight: 1
description: This tutorial teaches developers how to manage cloud storage operations in GroupDocs.Editor Cloud including checking storage existence and monitoring usage.
---

# Tutorial: Learn to Work with Storage in GroupDocs.Editor Cloud

## Introduction

In this comprehensive tutorial, you'll learn how to perform essential storage operations using the GroupDocs.Editor Cloud API. Understanding cloud storage operations is fundamental when working with document editing services, as proper storage management ensures efficient document workflows.

Learning Objectives:
- Verify the existence of a storage in your account
- Check if a specific file or folder exists in your storage
- Monitor storage space usage
- Retrieve file version information

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Editor Cloud subscription (or [free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST APIs
4. Development environment with the preferred programming language (C#, Java, PHP, Ruby, Node.js, or Python)
5. Corresponding GroupDocs.Editor Cloud SDK installed

## Use Case Scenario

Consider a document management application that needs to perform various checks before processing documents. You need to verify storage availability, check file existence, monitor space usage, and track file versions to ensure smooth operation.

## Tutorial Steps

### Step 1: Set Up Authentication

Before using any storage operations, you need to authenticate with the GroupDocs.Editor Cloud API. All SDKs require your Client ID and Client Secret for this.

#### Python Setup:
```python
import groupdocs_editor_cloud
from groupdocs_editor_cloud import Configuration

# Create authentication object with your credentials
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
```

#### C# Setup:
```csharp
using GroupDocs.Editor.Cloud.Sdk.Api;
using GroupDocs.Editor.Cloud.Sdk.Client;

// Create authentication object
Configuration configuration = new Configuration(clientId: "YOUR_CLIENT_ID", clientSecret: "YOUR_CLIENT_SECRET");
```

### Step 2: Checking Storage Existence

The first operation we'll learn is how to check if a specific storage exists in your account. This is useful when you need to verify that the storage is properly configured before proceeding with other operations.


#### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/editor/storage/MyStorage/exist" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create StorageApi instance
var storageApi = new StorageApi(configuration);

// Check if storage exists
var request = new StorageExistsRequest("MyStorage");
var response = storageApi.StorageExists(request);

// Print result
Console.WriteLine($"Storage exists: {response.Exists}");
```

##### Python Example

```python
# Create StorageApi instance
storage_api = groupdocs_editor_cloud.StorageApi.from_config(configuration)

# Check if storage exists
request = groupdocs_editor_cloud.StorageExistsRequest("MyStorage")
response = storage_api.storage_exists(request)

# Print result
print(f"Storage exists: {response.exists}")
```

##### Java Example

```java
// Create StorageApi instance
StorageApi storageApi = new StorageApi(configuration);

// Check if storage exists
StorageExistsRequest request = new StorageExistsRequest("MyStorage");
StorageExistsResponse response = storageApi.storageExists(request);

// Print result
System.out.println("Storage exists: " + response.getExists());
```

### Step 3: Checking File or Folder Existence

Often, you need to check whether a specific file or folder exists in your storage before performing operations on it. This helps prevent errors and ensures that your code handles missing files properly.

#### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/editor/storage/exist/mydocuments/sample.docx?storageName=MyStorage" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create StorageApi instance
var storageApi = new StorageApi(configuration);

// Check if file exists
var request = new ObjectExistsRequest("mydocuments/sample.docx", "MyStorage");
var response = storageApi.ObjectExists(request);

// Print results
Console.WriteLine($"Object exists: {response.Exists}");
Console.WriteLine($"Is folder: {response.IsFolder}");
```

##### Python Example

```python
# Create StorageApi instance
storage_api = groupdocs_editor_cloud.StorageApi.from_config(configuration)

# Check if file exists
request = groupdocs_editor_cloud.ObjectExistsRequest("mydocuments/sample.docx", "MyStorage")
response = storage_api.object_exists(request)

# Print results
print(f"Object exists: {response.exists}")
print(f"Is folder: {response.is_folder}")
```

### Step 4: Monitoring Storage Space Usage

To ensure your application has sufficient storage space, you should regularly monitor the storage usage. This helps prevent issues with file uploads and other operations that require storage space.

#### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/editor/storage/disc?storageName=MyStorage" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create StorageApi instance
var storageApi = new StorageApi(configuration);

// Get storage usage info
var request = new GetDiscUsageRequest("MyStorage");
var response = storageApi.GetDiscUsage(request);

// Print results
Console.WriteLine($"Used space: {response.UsedSize} bytes");
Console.WriteLine($"Total space: {response.TotalSize} bytes");
```

##### Python Example

```python
# Create StorageApi instance
storage_api = groupdocs_editor_cloud.StorageApi.from_config(configuration)

# Get storage usage info
request = groupdocs_editor_cloud.GetDiscUsageRequest("MyStorage")
response = storage_api.get_disc_usage(request)

# Print results
print(f"Used space: {response.used_size} bytes")
print(f"Total space: {response.total_size} bytes")
```

### Step 5: Retrieving File Versions

The API allows you to retrieve version information for a specific file. This is useful for tracking changes to files and ensuring you're working with the correct version.

#### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/editor/storage/version/mydocuments/document.docx?storageName=MyStorage" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create StorageApi instance
var storageApi = new StorageApi(configuration);

// Get file versions
var request = new GetFileVersionsRequest("mydocuments/document.docx", "MyStorage");
var response = storageApi.GetFileVersions(request);

// Print versions info
foreach (var version in response.Value)
{
    Console.WriteLine($"Version: {version.VersionId}, IsLatest: {version.IsLatest}");
    Console.WriteLine($"Modified date: {version.ModifiedDate}");
    Console.WriteLine($"Size: {version.Size} bytes");
    Console.WriteLine();
}
```

##### Python Example

```python
# Create StorageApi instance
storage_api = groupdocs_editor_cloud.StorageApi.from_config(configuration)

# Get file versions
request = groupdocs_editor_cloud.GetFileVersionsRequest("mydocuments/document.docx", "MyStorage")
response = storage_api.get_file_versions(request)

# Print versions info
for version in response.value:
    print(f"Version: {version.version_id}, IsLatest: {version.is_latest}")
    print(f"Modified date: {version.modified_date}")
    print(f"Size: {version.size} bytes")
    print()
```

## Troubleshooting Tips

- Authentication Issues: Make sure your Client ID and Client Secret are correct. Check for any whitespace in the credentials.
- Storage Not Found: Verify the storage name is correctly spelled and exists in your account.
- File Path Issues: Always use forward slashes (`/`) in file paths, not backslashes.
- Permission Problems: Ensure your account has the necessary permissions to access the specified storage.

## What You've Learned

In this tutorial, you've learned how to:
- Verify if a specific storage exists in your account
- Check if a file or folder exists in your storage
- Monitor storage space usage to prevent space issues
- Retrieve and examine file version information

These fundamental storage operations form the foundation for more advanced document management workflows and help ensure your applications run smoothly.

## Further Practice

To reinforce your learning, try the following exercises:
1. Create a simple console application that reports storage statistics
2. Build a file existence checker that validates multiple files
3. Implement a storage monitor that alerts when usage exceeds a certain threshold

## Next Steps

Now that you've mastered basic storage operations, proceed to the next tutorial to learn about [folder operations in GroupDocs.Editor Cloud](/storage-handling-procedures/working-with-folder/).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/editor/)
- [Documentation](https://docs.groupdocs.cloud/editor/)
- [Live Demo](https://products.groupdocs.app/editor/family)
- [API Reference](https://reference.groupdocs.cloud/editor/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.editor-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/editor/20/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
