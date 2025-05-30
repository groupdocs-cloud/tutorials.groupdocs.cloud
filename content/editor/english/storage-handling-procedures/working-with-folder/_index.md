---
title: How to Manage Folders in GroupDocs.Editor Cloud Tutorial
url: /storage-handling-procedures/working-with-folder/
weight: 2
description: Learn how to efficiently manage folders in cloud storage with this step-by-step tutorial for GroupDocs.Editor Cloud API, including listing, creating, and moving folders.
---

# Tutorial: How to Manage Folders in GroupDocs.Editor Cloud

## Introduction

In this tutorial, you'll learn how to perform essential folder management operations using the GroupDocs.Editor Cloud API. Proper folder organization is crucial for efficient document management, especially when working with numerous files in various states of editing.

Learning Objectives:
- List files within specific folders in cloud storage
- Create new folders for document organization
- Delete folders when they're no longer needed
- Copy folder contents between different locations
- Move folders within your cloud storage structure

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Editor Cloud subscription (or [free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST APIs and cloud storage concepts
4. Development environment with your preferred programming language (C#, Java, PHP, Ruby, Node.js, or Python)
5. Corresponding GroupDocs.Editor Cloud SDK installed

## Use Case Scenario

Imagine you're developing a document management system that allows users to organize their documents in folders. You need to implement functionality for browsing folder contents, creating new organizational structures, and reorganizing existing content through copy or move operations.

## Tutorial Steps

### Step 1: Set Up Authentication

Before using any folder operations, you need to authenticate with the GroupDocs.Editor Cloud API:

Try it yourself: Set up authentication in your preferred language

#### Python Setup 
```python
import groupdocs_editor_cloud
from groupdocs_editor_cloud import Configuration

# Create authentication object with your credentials
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
```

#### C# Setup
```csharp
using GroupDocs.Editor.Cloud.Sdk.Api;
using GroupDocs.Editor.Cloud.Sdk.Client;

// Create authentication object
Configuration configuration = new Configuration(clientId: "YOUR_CLIENT_ID", clientSecret: "YOUR_CLIENT_SECRET");
```

### Step 2: Listing Files in a Folder

One of the most common operations is retrieving a list of files from a specific folder. This allows users to browse available documents and understand the folder structure.

#### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/editor/storage/folder/mydocuments?storageName=MyStorage" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create FolderApi instance
var folderApi = new FolderApi(configuration);

// Get files list
var request = new GetFilesListRequest("mydocuments", "MyStorage");
var response = folderApi.GetFilesList(request);

// Print files info
Console.WriteLine($"Files in the folder 'mydocuments':");
foreach (var fileInfo in response.Value)
{
    Console.WriteLine($"Name: {fileInfo.Name}, Size: {fileInfo.Size} bytes");
    Console.WriteLine($"Is Folder: {fileInfo.IsFolder}, Path: {fileInfo.Path}");
    Console.WriteLine($"Modified Date: {fileInfo.ModifiedDate}");
    Console.WriteLine();
}
```

##### Python Example

```python
# Create FolderApi instance
folder_api = groupdocs_editor_cloud.FolderApi.from_config(configuration)

# Get files list
request = groupdocs_editor_cloud.GetFilesListRequest("mydocuments", "MyStorage")
response = folder_api.get_files_list(request)

# Print files info
print(f"Files in the folder 'mydocuments':")
for file_info in response.value:
    print(f"Name: {file_info.name}, Size: {file_info.size} bytes")
    print(f"Is Folder: {file_info.is_folder}, Path: {file_info.path}")
    print(f"Modified Date: {file_info.modified_date}")
    print()
```

##### Java Example

```java
// Create FolderApi instance
FolderApi folderApi = new FolderApi(configuration);

// Get files list
GetFilesListRequest request = new GetFilesListRequest("mydocuments", "MyStorage");
FilesList response = folderApi.getFilesList(request);

// Print files info
System.out.println("Files in the folder 'mydocuments':");
for (FileInfo fileInfo : response.getValue()) {
    System.out.println("Name: " + fileInfo.getName() + ", Size: " + fileInfo.getSize() + " bytes");
    System.out.println("Is Folder: " + fileInfo.getIsFolder() + ", Path: " + fileInfo.getPath());
    System.out.println("Modified Date: " + fileInfo.getModifiedDate());
    System.out.println();
}
```

### Step 3: Creating a New Folder

To organize documents effectively, you often need to create new folders. This operation allows you to create a folder structure that meets your organizational needs.

#### cURL Example

```bash
curl -X POST "https://api.groupdocs.cloud/v1.0/editor/storage/folder/mydocuments/reports?storageName=MyStorage" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create FolderApi instance
var folderApi = new FolderApi(configuration);

// Create folder
var request = new CreateFolderRequest("mydocuments/reports", "MyStorage");
folderApi.CreateFolder(request);

Console.WriteLine("Folder 'reports' created successfully in 'mydocuments'");
```

##### Python Example

```python
# Create FolderApi instance
folder_api = groupdocs_editor_cloud.FolderApi.from_config(configuration)

# Create folder
request = groupdocs_editor_cloud.CreateFolderRequest("mydocuments/reports", "MyStorage")
folder_api.create_folder(request)

print("Folder 'reports' created successfully in 'mydocuments'")
```

### Step 4: Deleting a Folder

When a folder is no longer needed, you can delete it to keep your storage organized. The API allows you to delete folders either recursively (including all contents) or only if they're empty.

#### cURL Example

```bash
curl -X DELETE "https://api.groupdocs.cloud/v1.0/editor/storage/folder/mydocuments/old-reports?storageName=MyStorage&recursive=true" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create FolderApi instance
var folderApi = new FolderApi(configuration);

// Delete folder recursively
var request = new DeleteFolderRequest("mydocuments/old-reports", "MyStorage", true);
folderApi.DeleteFolder(request);

Console.WriteLine("Folder 'old-reports' deleted successfully including all its contents");
```

##### Python Example

```python
# Create FolderApi instance
folder_api = groupdocs_editor_cloud.FolderApi.from_config(configuration)

# Delete folder recursively
request = groupdocs_editor_cloud.DeleteFolderRequest("mydocuments/old-reports", "MyStorage", True)
folder_api.delete_folder(request)

print("Folder 'old-reports' deleted successfully including all its contents")
```

### Step 5: Copying a Folder

Sometimes you need to duplicate a folder structure, perhaps to create a backup or a new version. The copy operation allows you to duplicate folders along with their contents.

#### cURL Example

```bash
curl -X PUT "https://api.groupdocs.cloud/v1.0/editor/storage/folder/copy/mydocuments/reports?destPath=mydocuments/backup/reports&storageName=MyStorage" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create FolderApi instance
var folderApi = new FolderApi(configuration);

// Copy folder
var request = new CopyFolderRequest(
    srcPath: "mydocuments/reports",
    destPath: "mydocuments/backup/reports",
    srcStorageName: "MyStorage",
    destStorageName: "MyStorage"
);
folderApi.CopyFolder(request);

Console.WriteLine("Folder 'reports' copied successfully to 'mydocuments/backup/reports'");
```

##### Python Example

```python
# Create FolderApi instance
folder_api = groupdocs_editor_cloud.FolderApi.from_config(configuration)

# Copy folder
request = groupdocs_editor_cloud.CopyFolderRequest(
    src_path="mydocuments/reports",
    dest_path="mydocuments/backup/reports",
    src_storage_name="MyStorage",
    dest_storage_name="MyStorage"
)
folder_api.copy_folder(request)

print("Folder 'reports' copied successfully to 'mydocuments/backup/reports'")
```

### Step 6: Moving a Folder

The move operation allows you to reorganize your folder structure by relocating folders. This is particularly useful when restructuring your document organization.

#### cURL Example

```bash
curl -X PUT "https://api.groupdocs.cloud/v1.0/editor/storage/folder/move/mydocuments/drafts?destPath=mydocuments/archive/2024/drafts&storageName=MyStorage" -H "accept: application/json" -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### SDK Examples

##### C# Example

```csharp
// Create FolderApi instance
var folderApi = new FolderApi(configuration);

// Move folder
var request = new MoveFolderRequest(
    srcPath: "mydocuments/drafts",
    destPath: "mydocuments/archive/2024/drafts",
    srcStorageName: "MyStorage",
    destStorageName: "MyStorage"
);
folderApi.MoveFolder(request);

Console.WriteLine("Folder 'drafts' moved successfully to 'mydocuments/archive/2024/drafts'");
```

##### Python Example

```python
# Create FolderApi instance
folder_api = groupdocs_editor_cloud.FolderApi.from_config(configuration)

# Move folder
request = groupdocs_editor_cloud.MoveFolderRequest(
    src_path="mydocuments/drafts",
    dest_path="mydocuments/archive/2024/drafts",
    src_storage_name="MyStorage",
    dest_storage_name="MyStorage"
)
folder_api.move_folder(request)

print("Folder 'drafts' moved successfully to 'mydocuments/archive/2024/drafts'")
```

## Troubleshooting Tips

- Path Formatting: Always use forward slashes (`/`) in folder paths and ensure there's no trailing slash.
- Recursive Deletion: Be careful when using recursive deletion as it permanently removes all contents. Always confirm before proceeding.
- Folder Existence: Before copying or moving, verify that the destination path's parent folder already exists.
- Permission Issues: Ensure your account has the necessary permissions for all operations.
- Error Handling: Implement proper error handling to manage cases where folders might not exist or operations fail.

## What You've Learned

In this tutorial, you've learned how to:
- List files within folders to browse the contents
- Create new folders to organize your documents
- Delete folders when they're no longer needed
- Copy folder structures to duplicate document collections
- Move folders to reorganize your storage hierarchy

These folder management skills form the foundation of efficient document organization and will help you build more structured and maintainable document workflows.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a folder browser application that displays the folder hierarchy
2. Implement a folder backup utility that copies folders at scheduled intervals
3. Build a folder cleanup tool that identifies and removes empty folders

## Next Steps

Now that you've mastered folder operations, continue to the next tutorial to learn about [working with files in GroupDocs.Editor Cloud](/storage-handling-procedures/working-with-files/).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/editor/)
- [Documentation](https://docs.groupdocs.cloud/editor/)
- [Live Demo](https://products.groupdocs.app/editor/family)
- [API Reference](https://reference.groupdocs.cloud/editor/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.editor-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/editor/20/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
