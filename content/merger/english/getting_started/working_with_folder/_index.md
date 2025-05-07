---
title: How to Work with Folders in GroupDocs.Merger Cloud API Tutorial
weight: 4
url: /getting-started/working-with-folder/
description: Master folder management techniques with GroupDocs.Merger Cloud API - learn to create, list, delete, copy, and move folders in this comprehensive tutorial.
---

# Tutorial: How to Work with Folders in GroupDocs.Merger Cloud API

## Learning Objectives

In this tutorial, you'll learn how to manage folders in your cloud storage using the GroupDocs.Merger Cloud API. You'll master techniques for creating folders, listing their contents, copying, moving, and deleting folders. These skills are essential for organizing documents effectively in your cloud-based applications.

## Prerequisites

Before starting this tutorial, ensure you have:

1. A GroupDocs.Merger Cloud account (or [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret from the [GroupDocs Cloud Dashboard](https://dashboard.groupdocs.cloud/)
3. Basic understanding of REST API concepts
4. Development environment for your preferred language (optional for cURL examples)
5. Completed the [Working with Files tutorial](/getting-started/working-with-files/)


## Use Case Scenario

Consider developing a document management solution where efficient folder organization is critical. You need to:
- Create logical folder structures for different document types
- List folder contents to display in your application's UI
- Copy entire folders for backup purposes
- Move folders to reorganize your document hierarchy
- Delete folders that are no longer needed

This tutorial will show you how to implement these essential folder operations programmatically.

## Step 1: Understanding Folder API Operations

The GroupDocs.Merger Cloud API provides several endpoints for folder management:

- List Files API: Gets a list of all files within a specific folder
- Create Folder API: Creates a new folder in the storage
- Delete Folder API: Removes a folder from the storage
- Copy Folder API: Copies a folder to another location
- Move Folder API: Moves a folder to another location

Let's explore each of these operations in detail.

## Step 2: Listing Files in a Folder

First, let's learn how to retrieve a list of all files within a specific folder. This is useful for displaying folder contents to users in your application.

### API Endpoint

```
GET ~/storage/folder/{path}
```

Where `{path}` is the folder path.

### Request Parameters

| Parameter | Description |
|---|---|
| path | Folder path e.g. /Folder1. Required. |
| storageName | Name of the storage. If not specified, the default storage is used. |

### cURL Example for Listing Files

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/merger/storage/folder/conversiondocs?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### Expected Response

```json
{
  "value": [
    {
      "name": "four-pages.docx",
      "isFolder": false,
      "modifiedDate": "2019-03-20T12:35:38+00:00",
      "size": 8651,
      "path": "/conversiondocs/four-pages.docx"
    },
    {
      "name": "one-page.docx",
      "isFolder": false,
      "modifiedDate": "2019-03-20T12:17:34+00:00",
      "size": 351348,
      "path": "/conversiondocs/one-page.docx"
    },
    {
      "name": "password-protected.docx",
      "isFolder": false,
      "modifiedDate": "2019-03-20T12:35:40+00:00",
      "size": 10240,
      "path": "/conversiondocs/password-protected.docx"
    }
    // ... other files
  ]
}
```

### C# Example

```csharp
// For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
// Create API instance
var folderApi = new FolderApi(configuration);

// Folder path
var folderPath = "conversiondocs";
var storageName = "MyStorage"; // Optional

// Get files list
var response = folderApi.GetFilesList(new GetFilesListRequest(folderPath, storageName));

Console.WriteLine($"Files in folder '{folderPath}':");
foreach (var fileInfo in response.Value)
{
    string entityType = fileInfo.IsFolder ? "Folder" : "File";
    Console.WriteLine($"{entityType}: {fileInfo.Name}");
    Console.WriteLine($"  Path: {fileInfo.Path}");
    Console.WriteLine($"  Size: {fileInfo.Size} bytes");
    Console.WriteLine($"  Modified: {fileInfo.ModifiedDate}");
    Console.WriteLine();
}
```

### Python Example

```python
# For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-python-samples
import groupdocs_merger_cloud
from groupdocs_merger_cloud import FolderApi, GetFilesListRequest

client_id = "YOUR_CLIENT_ID" # Get from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get from https://dashboard.groupdocs.cloud
  
# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(client_id, client_secret)
folder_api = FolderApi(configuration)

# Folder path
folder_path = "conversiondocs"
storage_name = "MyStorage"  # Optional

# Get files list
request = GetFilesListRequest(folder_path, storage_name)
response = folder_api.get_files_list(request)

print(f"Files in folder '{folder_path}':")
for file_info in response.value:
    entity_type = "Folder" if file_info.is_folder else "File"
    print(f"{entity_type}: {file_info.name}")
    print(f"  Path: {file_info.path}")
    print(f"  Size: {file_info.size} bytes")
    print(f"  Modified: {file_info.modified_date}")
    print()
```

## Step 3: Creating New Folders

Now, let's learn how to create new folders in your cloud storage. This is essential for organizing documents into logical categories.

### API Endpoint

```
POST ~/storage/folder/{path}
```

Where `{path}` is the folder path to create.

### Request Parameters

| Parameter | Description |
|---|---|
| path | Target folder's path e.g. Folder1/Folder2/. The folders will be created recursively. Required. |
| storageName | Name of the storage. If not specified, the default storage is used. |

### cURL Example for Creating a Folder

```bash
curl -X POST "https://api.groupdocs.cloud/v1.0/merger/storage/folder/documents/contracts?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### Java Example

```java
// For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-java-samples
import com.groupdocs.cloud.merger.client.*;
import com.groupdocs.cloud.merger.model.requests.*;
import com.groupdocs.cloud.merger.api.FolderApi;

public class CreateFolderExample {
    public static void main(String[] args) {
        String clientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud
        String clientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
        
        // Create instance of the API
        Configuration configuration = new Configuration(clientId, clientSecret);
        FolderApi folderApi = new FolderApi(configuration);
        
        try {
            // Folder path to create
            String folderPath = "documents/contracts";
            String storageName = "MyStorage"; // Optional
            
            // Create folder
            CreateFolderRequest request = new CreateFolderRequest(folderPath, storageName);
            folderApi.createFolder(request);
            
            System.out.println("Folder created successfully: " + folderPath);
            
        } catch (Exception e) {
            System.out.println("Error creating folder: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Step 4: Deleting Folders

Sometimes, you need to remove folders that are no longer necessary. The Delete Folder API allows you to remove a folder and optionally all its contents recursively.

### API Endpoint

```
DELETE ~/storage/folder/{path}
```

Where `{path}` is the folder path to delete.

### Request Parameters

| Parameter | Description |
|---|---|
| path | Folder path e.g. /Folder1. Required. |
| storageName | Name of the storage. If not specified, the default storage is used. |
| recursive | Remove folders, subfolders, and files. Default is false. |

### cURL Example for Deleting a Folder

```bash
curl -X DELETE "https://api.groupdocs.cloud/v1.0/merger/storage/folder/documents/old-contracts?storageName=MyStorage&recursive=true" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### Node.js Example

```javascript
// For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-node-samples
const { FolderApi, Configuration, DeleteFolderRequest } = require("groupdocs-merger-cloud");

const clientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud
const clientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud

// Create instance of the API
const configuration = new Configuration(clientId, clientSecret);
const folderApi = new FolderApi(configuration);

// Folder path to delete
const folderPath = "documents/old-contracts";
const storageName = "MyStorage"; // Optional
const recursive = true; // Set to true to delete subfolders and files

// Delete folder
const request = new DeleteFolderRequest(folderPath, storageName, recursive);
folderApi.deleteFolder(request)
    .then(() => {
        console.log(`Folder deleted successfully: ${folderPath}`);
    })
    .catch(error => {
        console.error("Error deleting folder:", error.message);
    });
```

## Step 5: Copying Folders

The Copy Folder API allows you to duplicate an entire folder structure from one location to another in your cloud storage. This is useful for creating backups or organizing documents without modifying the originals.

### API Endpoint

```
PUT ~/storage/folder/copy/{srcPath}
```

Where `{srcPath}` is the source folder path.

### Request Parameters

| Parameter | Description |
|---|---|
| srcPath | Source folder path e.g. /Folder1. Required. |
| destPath | Destination folder path. Required. |
| srcStorageName | Name of the source storage. If not specified, the default storage is used. |
| destStorageName | Name of the destination storage. If not specified, the default storage is used. |

### cURL Example for Copying a Folder

```bash
curl -X PUT "https://api.groupdocs.cloud/v1.0/merger/storage/folder/copy/documents/contracts?destPath=backup/contracts&srcStorageName=MyStorage&destStorageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### PHP Example

```php
// For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-php-samples
<?php
require_once __DIR__ . '/vendor/autoload.php';

use GroupDocs\Merger\Model;
use GroupDocs\Merger\Model\Requests;

$client_id = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud
$client_secret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud

// Create instance of the API
$configuration = new GroupDocs\Merger\Configuration();
$configuration->setAppSid($client_id);
$configuration->setAppKey($client_secret);
$folderApi = new GroupDocs\Merger\FolderApi($configuration);

// Source and destination paths
$srcPath = "documents/contracts";
$destPath = "backup/contracts";
$srcStorageName = "MyStorage"; // Optional
$destStorageName = "MyStorage"; // Optional

try {
    // Copy folder
    $request = new Requests\CopyFolderRequest($srcPath, $destPath, $srcStorageName, $destStorageName);
    $folderApi->copyFolder($request);
    
    echo "Folder copied successfully from '{$srcPath}' to '{$destPath}'";
} catch (Exception $e) {
    echo "Error copying folder: " . $e->getMessage();
}
```

## Step 6: Moving Folders

The Move Folder API allows you to relocate an entire folder structure from one location to another in your cloud storage. This is useful for reorganizing your document hierarchy.

### API Endpoint

```
PUT ~/storage/folder/move/{srcPath}
```

Where `{srcPath}` is the source folder path.

### Request Parameters

| Parameter | Description |
|---|---|
| srcPath | Source folder path e.g. /Folder1. Required. |
| destPath | Destination folder path. Required. |
| srcStorageName | Name of the source storage. If not specified, the default storage is used. |
| destStorageName | Name of the destination storage. If not specified, the default storage is used. |

### cURL Example for Moving a Folder

```bash
curl -X PUT "https://api.groupdocs.cloud/v1.0/merger/storage/folder/move/documents/old-contracts?destPath=archive/contracts/2023&srcStorageName=MyStorage&destStorageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### Ruby Example

```ruby
# For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-ruby-samples
require 'groupdocs_merger_cloud'

class FolderManager
  def initialize(client_id, client_secret)
    # Create configuration and API instances
    @configuration = GroupDocsMergerCloud::Configuration.new(client_id, client_secret)
    @folder_api = GroupDocsMergerCloud::FolderApi.from_config(@configuration)
    @storage_name = "MyStorage" # Default storage
  end

  def move_folder(src_path, dest_path)
    puts "Moving folder from '#{src_path}' to '#{dest_path}'..."
    
    request = GroupDocsMergerCloud::MoveFolderRequest.new(src_path, dest_path, @storage_name, @storage_name)
    @folder_api.move_folder(request)
    
    puts "✓ Folder moved successfully"
    return true
  end
end

# Usage example
client_id = "YOUR_CLIENT_ID" # Get from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get from https://dashboard.groupdocs.cloud

folder_manager = FolderManager.new(client_id, client_secret)
folder_manager.move_folder("documents/old-contracts", "archive/contracts/2023")
```

## Building a Complete Folder Management System

Now that you've learned each individual folder operation, let's combine them to create a simple folder management system. The following example demonstrates how to implement a basic folder manager:

### Python Example: Folder Management System

```python
# For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-python-samples
import groupdocs_merger_cloud
from groupdocs_merger_cloud import Configuration, FolderApi
from groupdocs_merger_cloud import GetFilesListRequest, CreateFolderRequest
from groupdocs_merger_cloud import DeleteFolderRequest, CopyFolderRequest, MoveFolderRequest

class FolderManager:
    def __init__(self, client_id, client_secret):
        # Create configuration and API instances
        self.configuration = Configuration(client_id, client_secret)
        self.folder_api = FolderApi(self.configuration)
        self.storage_name = "MyStorage"  # Default storage
    
    def list_folder_contents(self, folder_path):
        """List all files and folders within a specified folder"""
        print(f"Listing contents of folder '{folder_path}'...")
        
        request = GetFilesListRequest(folder_path, self.storage_name)
        response = self.folder_api.get_files_list(request)
        
        folders = []
        files = []
        
        # Separate folders and files
        for item in response.value:
            if item.is_folder:
                folders.append(item)
            else:
                files.append(item)
        
        # Print folders first
        print(f"\nFolders ({len(folders)}):")
        for folder in folders:
            print(f"  {folder.name}/")
        
        # Then print files
        print(f"\nFiles ({len(files)}):")
        for file in files:
            print(f"  {file.name} ({file.size} bytes)")
        
        return response.value
    
    def create_folder(self, folder_path):
        """Create a new folder at the specified path"""
        print(f"Creating folder '{folder_path}'...")
        
        request = CreateFolderRequest(folder_path, self.storage_name)
        self.folder_api.create_folder(request)
        
        print(f"✓ Folder created successfully")
        return True
    
    def delete_folder(self, folder_path, recursive=True):
        """Delete a folder at the specified path"""
        print(f"Deleting folder '{folder_path}'...")
        
        request = DeleteFolderRequest(folder_path, self.storage_name, recursive)
        self.folder_api.delete_folder(request)
        
        print(f"✓ Folder deleted successfully")
        return True
    
    def copy_folder(self, src_path, dest_path):
        """Copy a folder from source to destination path"""
        print(f"Copying folder from '{src_path}' to '{dest_path}'...")
        
        request = CopyFolderRequest(src_path, dest_path, self.storage_name, self.storage_name)
        self.folder_api.copy_folder(request)
        
        print(f"✓ Folder copied successfully")
        return True
    
    def move_folder(self, src_path, dest_path):
        """Move a folder from source to destination path"""
        print(f"Moving folder from '{src_path}' to '{dest_path}'...")
        
        request = MoveFolderRequest(src_path, dest_path, self.storage_name, self.storage_name)
        self.folder_api.move_folder(request)
        
        print(f"✓ Folder moved successfully")
        return True

# Example usage
if __name__ == "__main__":
    client_id = "YOUR_CLIENT_ID"  # Get from https://dashboard.groupdocs.cloud
    client_secret = "YOUR_CLIENT_SECRET"  # Get from https://dashboard.groupdocs.cloud
    
    folder_manager = FolderManager(client_id, client_secret)
    
    # Example workflow
    print("FOLDER MANAGEMENT DEMO")
    print("=====================")
    
    # Create a project structure
    folder_manager.create_folder("projects/merger-app")
    folder_manager.create_folder("projects/merger-app/documents")
    folder_manager.create_folder("projects/merger-app/templates")
    
    # List the project structure
    folder_manager.list_folder_contents("projects/merger-app")
    
    # Create a backup
    folder_manager.copy_folder("projects/merger-app", "backups/merger-app-backup")
    
    # Reorganize folders
    folder_manager.move_folder("projects/merger-app/templates", "projects/merger-app/resources/templates")
    
    # Clean up (uncomment when ready to test)
    # folder_manager.delete_folder("projects/merger-app", recursive=True)
    # folder_manager.delete_folder("backups/merger-app-backup", recursive=True)
```

## Try It Yourself: Practice Exercises

Now that you understand how to perform folder operations, try these exercises to reinforce your learning:

1. Exercise 1: Create a script that builds a standard folder hierarchy for a new project (e.g., `/project/docs`, `/project/source`, `/project/resources`).

2. Exercise 2: Implement a function that lists all files recursively through a folder structure, not just the top level.

3. Exercise 3: Build a folder cleanup utility that automatically archives folders that haven't been modified in over 30 days by moving them to an archive location.

## Troubleshooting Tips

- Path Format Issues: Ensure folder paths are properly formatted. Include leading slashes when required by the API.
- Recursive Operation Warnings: Be careful with recursive deletions. Always double-check paths before executing such operations.
- Destination Already Exists: When copying or moving folders, check if the destination already exists to avoid unexpected overwrites.
- Permission Errors: Verify you have appropriate permissions for the storage and folders you're working with.

## What You've Learned

In this tutorial, you've learned how to:
- List files and folders to display folder contents
- Create new folders to organize your documents
- Delete folders that are no longer needed
- Copy folders to create backups or duplicates
- Move folders to reorganize your document hierarchy
- Combine these operations into a comprehensive folder management system

These folder management techniques are essential for maintaining an organized document structure in your cloud applications.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback

Did you find this tutorial helpful? Do you have questions about folder management? Let us know in our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!}) (recommended)
