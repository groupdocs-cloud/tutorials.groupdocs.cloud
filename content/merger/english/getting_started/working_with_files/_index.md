---
title: How to Work with Files in GroupDocs.Merger Cloud API Tutorial
weight: 3
url: /getting-started/working-with-files/
description: Learn essential file operations with GroupDocs.Merger Cloud API in this step-by-step tutorial covering uploading, downloading, and managing your documents.
---

# Tutorial: How to Work with Files in GroupDocs.Merger Cloud API

## Learning Objectives

In this tutorial, you'll learn how to perform essential file operations using the GroupDocs.Merger Cloud API. These operations include uploading files to cloud storage, downloading files, copying, moving, and deleting files. Mastering these fundamental file operations is crucial for implementing document processing workflows in your applications.

## Prerequisites

Before starting this tutorial, ensure you have:

1. A GroupDocs.Merger Cloud account (or [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret from the [GroupDocs Cloud Dashboard](https://dashboard.groupdocs.cloud/)
3. Basic understanding of REST API concepts
4. Development environment for your preferred language (optional for cURL examples)
5. Sample files to upload and test with

## Use Case Scenario

Consider building a document management system where users need to:
- Upload documents to cloud storage
- Download documents for viewing or editing
- Copy documents between folders for organization
- Move documents as part of workflow processes
- Delete documents when no longer needed

This tutorial will show you how to implement all these operations programmatically, providing the foundation for a robust document management system.

## Step 1: Understanding File API Operations

The GroupDocs.Merger Cloud API provides several endpoints for file management:

- Download File API: Download files from cloud storage
- Upload File API: Upload files to cloud storage
- Delete File API: Remove files from cloud storage
- Copy File API: Copy files between locations in cloud storage
- Move File API: Move files between locations in cloud storage

These APIs form the foundation of document management in your applications.

## Step 2: Downloading Files from Cloud Storage

Let's start by learning how to download files from your cloud storage. This is often the first step when you need to process or display a document to users.

### API Endpoint

```
GET ~/storage/file/{path}
```

Where `{path}` is the path of the file including the filename and extension.

### Request Parameters

| Parameter | Description |
|---|---|
| path | Path of the file including filename and extension. Required. |
| storageName | Name of the storage. If not specified, the default storage is used. |
| versionId | File version ID (for storages that support versioning). |

### cURL Example for Downloading a File

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/merger/storage/file/one-page.docx?storageName=MyStorage" \
-H "accept: multipart/form-data" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### Step-by-Step Implementation

1. Authentication: Obtain your access token using Client ID and Client Secret
2. Construct the URL: Form the correct path to your file
3. Send the Request: Make a GET request to the file endpoint
4. Process the Response: Save the downloaded file locally

Let's implement file download using different programming languages.

### C# Example

```csharp
// For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
// Create API instance
var fileApi = new FileApi(configuration);

// File name and path
var filePath = "one-page.docx";
var storageName = "MyStorage";

// Download file
var response = fileApi.DownloadFile(new DownloadFileRequest(filePath, storageName));

// Save to local file
using (var fileStream = File.Create("downloaded-document.docx"))
{
    response.CopyTo(fileStream);
}

Console.WriteLine("File downloaded successfully to 'downloaded-document.docx'");
```

### Python Example

```python
# For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-python-samples
import groupdocs_merger_cloud
from groupdocs_merger_cloud import FileApi, DownloadFileRequest

client_id = "YOUR_CLIENT_ID" # Get from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get from https://dashboard.groupdocs.cloud
  
# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(client_id, client_secret)
file_api = FileApi(configuration)

# File name in storage
file_path = "one-page.docx"
storage_name = "MyStorage"  # Optional: if not specified, default storage is used

# Download file
request = DownloadFileRequest(file_path, storage_name)
response = file_api.download_file(request)

# Save to local file
with open("downloaded-document.docx", 'wb') as f:
    for chunk in response:
        f.write(chunk)

print("File downloaded successfully to 'downloaded-document.docx'")
```

## Step 3: Uploading Files to Cloud Storage

Now, let's learn how to upload files to your cloud storage. This is essential when you need to add new documents to your system.

### API Endpoint

```
POST ~/storage/file/{path}
```

Where `{path}` is the destination path including filename and extension.

### Request Parameters

| Parameter | Description |
|---|---|
| path | Path in the storage where the file should be uploaded. Required. |
| storageName | Name of the storage. If not specified, the default storage is used. |
| File | File content to be uploaded. |

### cURL Example for Uploading a File

```bash
curl -X POST "https://api.groupdocs.cloud/v1.0/merger/storage/file/uploaded-document.docx?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: multipart/form-data" \
-F "fileContent=@local-document.docx"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### Step-by-Step Implementation

1. Authentication: Obtain your access token
2. Prepare the File: Read the local file you want to upload
3. Construct the Request: Form the correct path and parameters
4. Send the Request: Upload the file content
5. Verify the Response: Check that the upload was successful

Let's implement file upload using different programming languages.

### C# Example

```csharp
// For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
// Create API instance
var fileApi = new FileApi(configuration);

// Source file on local disk
var localFilePath = "C:\\Temp\\sample-document.docx";
var fileStream = File.OpenRead(localFilePath);

// Destination path in cloud storage
var uploadPath = "uploaded-document.docx";
var storageName = "MyStorage"; // Optional

// Upload file
var request = new UploadFileRequest(uploadPath, fileStream, storageName);
var response = fileApi.UploadFile(request);

if (response.Uploaded.Count > 0)
{
    Console.WriteLine($"File uploaded successfully: {response.Uploaded[0]}");
}
else if (response.Errors != null && response.Errors.Count > 0)
{
    Console.WriteLine($"Error uploading file: {response.Errors[0].Message}");
}
```

### Python Example

```python
# For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-python-samples
import groupdocs_merger_cloud
from groupdocs_merger_cloud import FileApi, UploadFileRequest

client_id = "YOUR_CLIENT_ID" # Get from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get from https://dashboard.groupdocs.cloud
  
# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(client_id, client_secret)
file_api = FileApi(configuration)

# Source file on local disk
local_file_path = "sample-document.docx"

# Destination path in cloud storage
upload_path = "uploaded-document.docx"
storage_name = "MyStorage"  # Optional

# Upload file
with open(local_file_path, 'rb') as file_stream:
    request = UploadFileRequest(upload_path, file_stream, storage_name)
    response = file_api.upload_file(request)

    if len(response.uploaded) > 0:
        print(f"File uploaded successfully: {response.uploaded[0]}")
    elif response.errors and len(response.errors) > 0:
        print(f"Error uploading file: {response.errors[0].message}")
```

## Step 4: Deleting Files from Cloud Storage

Now let's learn how to delete files from your cloud storage. This operation is important for managing storage space and removing documents that are no longer needed.

### API Endpoint

```
DELETE ~/storage/file/{path}
```

Where `{path}` is the path of the file to delete.

### Request Parameters

| Parameter | Description |
|---|---|
| path | Path of the file to delete. Required. |
| storageName | Name of the storage. If not specified, the default storage is used. |
| versionId | File version ID (for storages supporting versioning). |

### cURL Example for Deleting a File

```bash
curl -X DELETE "https://api.groupdocs.cloud/v1.0/merger/storage/file/document-to-delete.docx?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### Java Example

```java
// For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-java-samples
import com.groupdocs.cloud.merger.client.*;
import com.groupdocs.cloud.merger.model.requests.*;
import com.groupdocs.cloud.merger.api.FileApi;

public class DeleteFileExample {
    public static void main(String[] args) {
        String clientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud
        String clientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud
        
        // Create instance of the API
        Configuration configuration = new Configuration(clientId, clientSecret);
        FileApi fileApi = new FileApi(configuration);
        
        try {
            // File to delete
            String filePath = "document-to-delete.docx";
            String storageName = "MyStorage"; // Optional
            
            // Delete file
            DeleteFileRequest request = new DeleteFileRequest(filePath, storageName, null);
            fileApi.deleteFile(request);
            
            System.out.println("File deleted successfully: " + filePath);
            
        } catch (Exception e) {
            System.out.println("Error deleting file: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Step 5: Copying Files in Cloud Storage

Copying files within your cloud storage allows you to duplicate documents without downloading and re-uploading them. This is useful for creating backup copies or organizing files.

### API Endpoint

```
PUT ~/storage/file/copy/{srcPath}
```

Where `{srcPath}` is the source file path.

### Request Parameters

| Parameter | Description |
|---|---|
| srcPath | Source file path e.g. /Folder1/file.ext. Required. |
| destPath | Destination file path. Required. |
| srcStorageName | Source storage name. If not specified, the default storage is used. |
| destStorageName | Destination storage name. If not specified, the default storage is used. |
| versionId | Source file version ID (for storages supporting versioning). |

### cURL Example for Copying a File

```bash
curl -X PUT "https://api.groupdocs.cloud/v1.0/merger/storage/file/copy/original-document.docx?destPath=copied-document.docx&srcStorageName=MyStorage&destStorageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token.

### Node.js Example

```javascript
// For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-node-samples
const { FileApi, Configuration, CopyFileRequest } = require("groupdocs-merger-cloud");

const clientId = "YOUR_CLIENT_ID"; // Get from https://dashboard.groupdocs.cloud
const clientSecret = "YOUR_CLIENT_SECRET"; // Get from https://dashboard.groupdocs.cloud

// Create instance of the API
const configuration = new Configuration(clientId, clientSecret);
const fileApi = new FileApi(configuration);

// Source and destination paths
const srcPath = "original-document.docx";
const destPath = "copied-document.docx";
const srcStorageName = "MyStorage"; // Optional
const destStorageName = "MyStorage"; // Optional

// Copy file
const request = new CopyFileRequest(srcPath, destPath, srcStorageName, destStorageName);
fileApi.copyFile(request)
    .then(() => {
        console.log(`File copied successfully from '${srcPath}' to '${destPath}'`);
    })
    .catch(error => {
        console.error("Error copying file:", error.message);
    });
```

## Step 6: Moving Files in Cloud Storage

Moving files allows you to relocate documents between folders in your cloud storage without downloading and re-uploading them. This is useful for organizing documents or implementing workflow processes.

### API Endpoint

```
PUT ~/storage/file/move/{srcPath}
```

Where `{srcPath}` is the source file path.

### Request Parameters

| Parameter | Description |
|---|---|
| srcPath | Source file path e.g. /Folder1/file.ext. Required. |
| destPath | Destination file path. Required. |
| srcStorageName | Source storage name. If not specified, the default storage is used. |
| destStorageName | Destination storage name. If not specified, the default storage is used. |
| versionId | Source file version ID (for storages supporting versioning). |

### cURL Example for Moving a File

```bash
curl -X PUT "https://api.groupdocs.cloud/v1.0/merger/storage/file/move/source-location/document.docx?destPath=destination-location/document.docx&srcStorageName=MyStorage&destStorageName=MyStorage" \
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
$fileApi = new GroupDocs\Merger\FileApi($configuration);

// Source and destination paths
$srcPath = "source-location/document.docx";
$destPath = "destination-location/document.docx";
$srcStorageName = "MyStorage"; // Optional
$destStorageName = "MyStorage"; // Optional

try {
    // Move file
    $request = new Requests\MoveFileRequest($srcPath, $destPath, $srcStorageName, $destStorageName);
    $fileApi->moveFile($request);
    
    echo "File moved successfully from '{$srcPath}' to '{$destPath}'";
} catch (Exception $e) {
    echo "Error moving file: " . $e->getMessage();
}
```

## Building a Complete File Management System

Now that you've learned each individual file operation, let's combine them to create a simple file management system. The following example demonstrates how to implement a basic file manager:

### Ruby Example: Simple File Manager

```ruby
# For complete examples, please visit: https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-ruby-samples
require 'groupdocs_merger_cloud'

class SimpleFileManager
  def initialize(client_id, client_secret)
    # Create configuration and API instances
    @configuration = GroupDocsMergerCloud::Configuration.new(client_id, client_secret)
    @file_api = GroupDocsMergerCloud::FileApi.from_config(@configuration)
    @storage_name = "MyStorage" # Default storage
  end

  def upload_file(local_path, cloud_path)
    puts "Uploading file '#{local_path}' to '#{cloud_path}'..."
    
    File.open(local_path, 'rb') do |file|
      request = GroupDocsMergerCloud::UploadFileRequest.new(cloud_path, file, @storage_name)
      result = @file_api.upload_file(request)
      
      if result.uploaded.length > 0
        puts "✓ File uploaded successfully: #{result.uploaded[0]}"
        return true
      else
        puts "✗ Upload failed"
        return false
      end
    end
  end

  def download_file(cloud_path, local_path)
    puts "Downloading file '#{cloud_path}' to '#{local_path}'..."
    
    request = GroupDocsMergerCloud::DownloadFileRequest.new(cloud_path, @storage_name)
    response = @file_api.download_file(request)
    
    File.open(local_path, 'wb') do |file|
      file.write(response)
    end
    
    puts "✓ File downloaded successfully"
    return true
  end

  def copy_file(src_path, dest_path)
    puts "Copying file from '#{src_path}' to '#{dest_path}'..."
    
    request = GroupDocsMergerCloud::CopyFileRequest.new(src_path, dest_path, @storage_name, @storage_name)
    @file_api.copy_file(request)
    
    puts "✓ File copied successfully"
    return true
  end

  def move_file(src_path, dest_path)
    puts "Moving file from '#{src_path}' to '#{dest_path}'..."
    
    request = GroupDocsMergerCloud::MoveFileRequest.new(src_path, dest_path, @storage_name, @storage_name)
    @file_api.move_file(request)
    
    puts "✓ File moved successfully"
    return true
  end

  def delete_file(cloud_path)
    puts "Deleting file '#{cloud_path}'..."
    
    request = GroupDocsMergerCloud::DeleteFileRequest.new(cloud_path, @storage_name)
    @file_api.delete_file(request)
    
    puts "✓ File deleted successfully"
    return true
  end
end

# Usage example
client_id = "YOUR_CLIENT_ID" # Get from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get from https://dashboard.groupdocs.cloud

file_manager = SimpleFileManager.new(client_id, client_secret)

# Example workflow
file_manager.upload_file("./local-document.docx", "document.docx")
file_manager.copy_file("document.docx", "document-backup.docx")
file_manager.move_file("document.docx", "archive/document.docx")
file_manager.download_file("archive/document.docx", "./downloaded-document.docx")
file_manager.delete_file("document-backup.docx")
```

## Try It Yourself: Practice Exercises

Now that you understand how to perform file operations, try these exercises to reinforce your learning:

1. Exercise 1: Create a script that recursively uploads all files from a local folder to your cloud storage, preserving the folder structure.

2. Exercise 2: Implement a "sync" function that compares local and cloud folders and ensures they contain the same files, uploading or downloading as necessary.

3. Exercise 3: Build a simple file backup system that creates timestamped copies of important documents on a regular schedule.

## Troubleshooting Tips

- Authentication errors: Ensure your Client ID and Client Secret are correct and your access token is valid.
- File not found errors: Verify file paths are correct and include proper URL encoding for special characters.
- Permission errors: Check that you have the appropriate permissions for the storage and files you're trying to access.
- Storage quota errors: Ensure you have sufficient storage space when uploading files.

## What You've Learned

In this tutorial, you've learned how to:
- Download files from your cloud storage
- Upload files to your cloud storage
- Delete files that are no longer needed
- Copy files between locations without re-uploading
- Move files to organize your document storage
- Combine these operations into a simple file management system

These fundamental file operations form the basis for more complex document processing workflows and are essential for building robust document management applications.

## Next Steps

Now that you know how to manage individual files, a logical next step is to learn how to work with folders to better organize your documents. Continue to our [Tutorial: Working with Folders](/getting-started/working-with-folder/) to expand your knowledge.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback

Did you find this tutorial helpful? Do you have questions about file operations? Let us know in our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!
