---
title: Managing Folders in GroupDocs.Viewer Cloud Tutorial
description: Learn how to manage folders in GroupDocs.Viewer Cloud with this step-by-step tutorial covering folder creation, listing, copying, moving, and deletion.
url: /developer-guide/working-with-folder/
weight: 30
---

# Tutorial: Managing Folders in GroupDocs.Viewer Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- List files in a specific folder
- Create new folders in cloud storage
- Delete folders from cloud storage
- Copy folders within cloud storage
- Move folders within cloud storage

## Prerequisites

Before you begin this tutorial, you need:
- A GroupDocs Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
- Client ID and Client Secret credentials
- Basic understanding of REST APIs and your preferred programming language
- Development environment with the respective GroupDocs.Viewer Cloud SDK installed

## Introduction

Organizing documents effectively is crucial for any document management system. GroupDocs.Viewer Cloud provides comprehensive APIs to help you manage folders within your cloud storage.

In this tutorial, we'll explore a practical scenario: You're developing a document portal that needs to organize documents by department, project, and document type. You'll need to create folder structures, list files within folders, and perform operations like copying, moving, and deleting folders.

Let's learn how to implement these folder management operations using the GroupDocs.Viewer Cloud API.

## 1. List Files in a Specific Folder

The first operation we'll learn is listing files in a folder, which is essential for browsing document collections.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the folder path you want to list
3. Optionally specify the storage name
4. Execute the list request
5. Process the returned file list

### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/viewer/storage/folder/viewerdocs?storageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's see how to implement file listing using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class ListFilesExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FolderApi(configuration);

            try
            {
                // Create list files request
                var request = new GetFilesListRequest(
                    "viewerdocs",   // Folder path
                    Common.MyStorage // Storage name
                );

                // Execute the request
                var response = apiInstance.GetFilesList(request);
                
                // Process the results
                Console.WriteLine($"Files found: {response.Value.Count}");
                
                // Print file details
                foreach (var file in response.Value)
                {
                    Console.WriteLine($"File: {file.Name}, Size: {file.Size}, Path: {file.Path}");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error listing files: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.*;
import com.groupdocs.cloud.viewer.model.requests.*;

public class ListFilesExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FolderApi apiInstance = new FolderApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create list files request
            GetFilesListRequest request = new GetFilesListRequest(
                "viewerdocs",   // Folder path
                Common.MyStorage // Storage name
            );
            
            // Execute the request
            FilesList response = apiInstance.getFilesList(request);
            
            // Process the results
            System.out.println("Files found: " + response.getValue().size());
            
            // Print file details
            for (StorageFile file : response.getValue()) {
                System.out.println("File: " + file.getName() + 
                                  ", Size: " + file.getSize() + 
                                  ", Path: " + file.getPath());
            }
        } catch (ApiException e) {
            System.err.println("Error listing files: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import GetFilesListRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FolderApi(configuration)

try:
    # Create list files request
    request = GetFilesListRequest(
        "viewerdocs",     # Folder path
        Common.my_storage  # Storage name
    )
    
    # Execute the request
    response = api_instance.get_files_list(request)
    
    # Process the results
    print(f"Files found: {len(response.value)}")
    
    # Print file details
    for file in response.value:
        print(f"File: {file.name}, Size: {file.size}, Path: {file.path}")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error listing files: {e.message}")
```

### Try It Yourself

1. Replace the folder path with a folder in your storage
2. Run the code and verify that the list is returned successfully
3. Examine the properties of each file in the list
4. Try listing an empty folder or a non-existent folder to see how errors are handled

## 2. Create New Folders in Cloud Storage

Next, let's learn how to create new folders in your cloud storage, which is essential for organizing your documents.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the path of the folder you want to create
3. Optionally specify the storage name
4. Execute the create folder request
5. Verify the folder was created successfully

### cURL Example

```bash
curl -X POST "https://api.groupdocs.cloud/v2.0/viewer/storage/folder/viewerdocs/new-folder?storageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's implement folder creation using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class CreateFolderExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FolderApi(configuration);

            try
            {
                // Create folder request
                var request = new CreateFolderRequest(
                    "viewerdocs/new-folder", // Folder path to create
                    Common.MyStorage         // Storage name
                );

                // Execute the request
                apiInstance.CreateFolder(request);
                Console.WriteLine("Folder created successfully: viewerdocs/new-folder");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error creating folder: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.requests.*;

public class CreateFolderExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FolderApi apiInstance = new FolderApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create folder request
            CreateFolderRequest request = new CreateFolderRequest(
                "viewerdocs/new-folder", // Folder path to create
                Common.MyStorage         // Storage name
            );
            
            // Execute the request
            apiInstance.createFolder(request);
            System.out.println("Folder created successfully: viewerdocs/new-folder");
        } catch (ApiException e) {
            System.err.println("Error creating folder: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import CreateFolderRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FolderApi(configuration)

try:
    # Create folder request
    request = CreateFolderRequest(
        "viewerdocs/new-folder",  # Folder path to create
        Common.my_storage         # Storage name
    )
    
    # Execute the request
    api_instance.create_folder(request)
    
    print("Folder created successfully: viewerdocs/new-folder")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error creating folder: {e.message}")
```

### Try It Yourself

1. Replace the folder path with a new folder path in your storage
2. Run the code and verify that the folder was created successfully
3. Check your cloud storage dashboard to confirm the folder appears
4. Try creating nested folders by using paths like "viewerdocs/parent/child"

## 3. Delete Folders from Cloud Storage

Now, let's learn how to delete folders from your cloud storage, which is necessary for cleaning up unused resources.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the path of the folder you want to delete
3. Decide whether to delete recursively (including all contents)
4. Optionally specify the storage name
5. Execute the delete folder request
6. Verify the folder was deleted successfully

### cURL Example

```bash
curl -X DELETE "https://api.groupdocs.cloud/v2.0/viewer/storage/folder/viewerdocs/new-folder?recursive=true&storageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's implement folder deletion using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class DeleteFolderExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FolderApi(configuration);

            try
            {
                // Create delete folder request
                var request = new DeleteFolderRequest(
                    "viewerdocs/new-folder", // Folder path to delete
                    Common.MyStorage,        // Storage name
                    true                     // Delete recursively
                );

                // Execute the request
                apiInstance.DeleteFolder(request);
                Console.WriteLine("Folder deleted successfully: viewerdocs/new-folder");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error deleting folder: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.requests.*;

public class DeleteFolderExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FolderApi apiInstance = new FolderApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create delete folder request
            DeleteFolderRequest request = new DeleteFolderRequest(
                "viewerdocs/new-folder", // Folder path to delete
                Common.MyStorage,        // Storage name
                true                     // Delete recursively
            );
            
            // Execute the request
            apiInstance.deleteFolder(request);
            System.out.println("Folder deleted successfully: viewerdocs/new-folder");
        } catch (ApiException e) {
            System.err.println("Error deleting folder: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import DeleteFolderRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FolderApi(configuration)

try:
    # Create delete folder request
    request = DeleteFolderRequest(
        "viewerdocs/new-folder",  # Folder path to delete
        Common.my_storage,        # Storage name
        True                      # Delete recursively
    )
    
    # Execute the request
    api_instance.delete_folder(request)
    
    print("Folder deleted successfully: viewerdocs/new-folder")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error deleting folder: {e.message}")
```

### Try It Yourself

1. First, make sure you have a folder you can delete in your storage
2. Replace the folder path with that folder
3. Run the code and verify that the deletion was successful
4. Check your cloud storage dashboard to confirm the folder no longer appears
5. Try setting recursive to false when deleting a non-empty folder to see how the API handles it

## 4. Copy Folders within Cloud Storage

Let's explore how to copy folders within your cloud storage, which is useful for duplicating folder structures.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the source folder path
3. Specify the destination folder path
4. Optionally specify the source and destination storage names
5. Execute the copy folder request
6. Verify the folder was successfully copied

### cURL Example

```bash
curl -X PUT "https://api.groupdocs.cloud/v2.0/viewer/storage/folder/copy/viewerdocs/source-folder?destPath=viewerdocs/destination-folder&srcStorageName=MyStorage&destStorageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's implement folder copying using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class CopyFolderExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FolderApi(configuration);

            try
            {
                // Create copy folder request
                var request = new CopyFolderRequest(
                    "viewerdocs/source-folder",      // Source folder path
                    "viewerdocs/destination-folder", // Destination folder path
                    Common.MyStorage,                // Source storage name
                    Common.MyStorage                 // Destination storage name
                );

                // Execute the request
                apiInstance.CopyFolder(request);
                Console.WriteLine("Folder copied successfully from viewerdocs/source-folder to viewerdocs/destination-folder");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error copying folder: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.requests.*;

public class CopyFolderExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FolderApi apiInstance = new FolderApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create copy folder request
            CopyFolderRequest request = new CopyFolderRequest(
                "viewerdocs/source-folder",      // Source folder path
                "viewerdocs/destination-folder", // Destination folder path
                Common.MyStorage,                // Source storage name
                Common.MyStorage                 // Destination storage name
            );
            
            // Execute the request
            apiInstance.copyFolder(request);
            System.out.println("Folder copied successfully from viewerdocs/source-folder to viewerdocs/destination-folder");
        } catch (ApiException e) {
            System.err.println("Error copying folder: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import CopyFolderRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FolderApi(configuration)

try:
    # Create copy folder request
    request = CopyFolderRequest(
        "viewerdocs/source-folder",       # Source folder path
        "viewerdocs/destination-folder",  # Destination folder path
        Common.my_storage,                # Source storage name
        Common.my_storage                 # Destination storage name
    )
    
    # Execute the request
    api_instance.copy_folder(request)
    
    print("Folder copied successfully from viewerdocs/source-folder to viewerdocs/destination-folder")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error copying folder: {e.message}")
```

### Try It Yourself

1. First, make sure you have a source folder in your storage
2. Replace the source and destination paths with your preferred paths
3. Run the code and verify that the copy was successful
4. Check your cloud storage dashboard to confirm both folders appear with identical contents

## 5. Move Folders within Cloud Storage

Finally, let's learn how to move folders within your cloud storage, which is useful for reorganizing your folder structure.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the source folder path
3. Specify the destination folder path
4. Optionally specify the source and destination storage names
5. Execute the move folder request
6. Verify the folder was successfully moved

### cURL Example

```bash
curl -X PUT "https://api.groupdocs.cloud/v2.0/viewer/storage/folder/move/viewerdocs/source-folder?destPath=viewerdocs/moved-folder&srcStorageName=MyStorage&destStorageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's implement folder moving using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class MoveFolderExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FolderApi(configuration);

            try
            {
                // Create move folder request
                var request = new MoveFolderRequest(
                    "viewerdocs/source-folder", // Source folder path
                    "viewerdocs/moved-folder",  // Destination folder path
                    Common.MyStorage,           // Source storage name
                    Common.MyStorage            // Destination storage name
                );

                // Execute the request
                apiInstance.MoveFolder(request);
                Console.WriteLine("Folder moved successfully from viewerdocs/source-folder to viewerdocs/moved-folder");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error moving folder: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.requests.*;

public class MoveFolderExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FolderApi apiInstance = new FolderApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create move folder request
            MoveFolderRequest request = new MoveFolderRequest(
                "viewerdocs/source-folder", // Source folder path
                "viewerdocs/moved-folder",  // Destination folder path
                Common.MyStorage,           // Source storage name
                Common.MyStorage            // Destination storage name
            );
            
            // Execute the request
            apiInstance.moveFolder(request);
            System.out.println("Folder moved successfully from viewerdocs/source-folder to viewerdocs/moved-folder");
        } catch (ApiException e) {
            System.err.println("Error moving folder: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import MoveFolderRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FolderApi(configuration)

try:
    # Create move folder request
    request = MoveFolderRequest(
        "viewerdocs/source-folder",  # Source folder path
        "viewerdocs/moved-folder",   # Destination folder path
        Common.my_storage,           # Source storage name
        Common.my_storage            # Destination storage name
    )
    
    # Execute the request
    api_instance.move_folder(request)
    
    print("Folder moved successfully from viewerdocs/source-folder to viewerdocs/moved-folder")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error moving folder: {e.message}")
```

### Try It Yourself

1. First, make sure you have a source folder in your storage
2. Replace the source and destination paths with your preferred paths
3. Run the code and verify that the move was successful
4. Check your cloud storage dashboard to confirm the folder appears in its new location and no longer exists at the original location

## Common Issues and Troubleshooting

When working with folder operations, you might encounter these common issues:

1. Authentication Errors
   - Problem: API returns 401 Unauthorized
   - Solution: Check your Client ID and Client Secret; make sure your token hasn't expired

2. Folder Not Found
   - Problem: API returns 404 Not Found when trying to list, delete, copy, or move a folder
   - Solution: Double-check the folder path and storage name; verify the folder exists

3. Folder Already Exists
   - Problem: API returns an error when creating a folder that already exists
   - Solution: Check if the folder exists before creating it, or handle the exception appropriately

4. Permission Issues
   - Problem: API returns 403 Forbidden
   - Solution: Verify your account has the necessary permissions for the requested operation

## What You've Learned

In this tutorial, you've learned how to:
- List files in a specific folder
- Create new folders in your GroupDocs Cloud Storage
- Delete folders from your GroupDocs Cloud Storage
- Copy folders within your GroupDocs Cloud Storage
- Move folders within your GroupDocs Cloud Storage

You now have the essential skills to manage folders for your document viewing applications.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a simple folder browser application that lists and navigates through folders
2. Implement a recursive function that creates a multi-level folder structure
3. Write a utility that synchronizes folders between different storage locations

## Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).