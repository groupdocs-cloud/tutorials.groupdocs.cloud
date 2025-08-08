---
title: How to Work with Folders in GroupDocs.Parser Cloud Tutorial
url: /storage-operations/working-with-folder/
weight: 2
description: Learn how to create, list, copy, move, and delete folders in cloud storage with this step-by-step GroupDocs.Parser Cloud API tutorial.
---

# Tutorial: How to Work with Folders in GroupDocs.Parser Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- List all files within a specific folder
- Create new folders in cloud storage
- Delete folders from your storage
- Copy folders between storage locations
- Move folders to different paths

## Prerequisites

Before you begin this tutorial, you need:

1. A GroupDocs Cloud account (if you don't have one, [register for free](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret (available from the [Dashboard](https://dashboard.groupdocs.cloud/#/apps))
3. Basic knowledge of REST APIs
4. Familiarity with your preferred programming language (C#, Java, or cURL)

## Introduction

Organizing your documents in cloud storage is essential for efficient document management. GroupDocs.Parser Cloud API provides powerful folder operations to help you structure and manage your documents effectively. In this tutorial, we'll explore how to list files, create folders, and perform various operations on folders in your cloud storage.

## Step 1: Setting Up Your Environment

Before working with folder operations, you need to set up authentication to access the GroupDocs.Parser Cloud API.

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

## Step 2: Listing Files in a Folder

Let's start by learning how to retrieve a list of all files in a specific folder.

### Understanding the API

The Get File Listing API allows you to get a comprehensive list of all files and subfolders within a specified folder in your cloud storage.

### Implementation Example

Here's how to list files in a folder using different methods:

#### Using cURL

```bash
curl -X GET "https://api.groupdocs.cloud/v1.0/parser/storage/folder/parserdocs?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create FolderApi instance
var apiInstance = new FolderApi(configuration);

// Folder path
string folderPath = "parserdocs";
// Storage name
string storageName = "MyStorage";

// Call API to get files list
FilesList response = apiInstance.GetFilesList(new GetFilesListRequest(folderPath, storageName));

// Display the file list
Console.WriteLine($"Files in '{folderPath}':");
foreach (var item in response.Value)
{
    Console.WriteLine($"Name: {item.Name}");
    Console.WriteLine($"Is Folder: {item.IsFolder}");
    Console.WriteLine($"Size: {item.Size} bytes");
    Console.WriteLine($"Path: {item.Path}");
    Console.WriteLine($"Modified Date: {item.ModifiedDate}");
    Console.WriteLine();
}
```

#### Using Java SDK

```java
// Create FolderApi instance
FolderApi apiInstance = new FolderApi(configuration);

// Folder path
String folderPath = "parserdocs";
// Storage name
String storageName = "MyStorage";

// Call API to get files list
GetFilesListRequest request = new GetFilesListRequest(folderPath, storageName);
FilesList response = apiInstance.getFilesList(request);

// Display the file list
System.out.println("Files in '" + folderPath + "':");
for (FileInfo item : response.getValue()) {
    System.out.println("Name: " + item.getName());
    System.out.println("Is Folder: " + item.getIsFolder());
    System.out.println("Size: " + item.getSize() + " bytes");
    System.out.println("Path: " + item.getPath());
    System.out.println("Modified Date: " + item.getModifiedDate());
    System.out.println();
}
```

### Expected Response

```json
{
  "value": [
    {
      "name": "four-pages.docx",
      "isFolder": false,
      "modifiedDate": "2022-03-20T12:35:38+00:00",
      "size": 8651,
      "path": "/parserdocs/four-pages.docx"
    },
    {
      "name": "one-page.docx",
      "isFolder": false,
      "modifiedDate": "2022-03-20T12:17:34+00:00",
      "size": 351348,
      "path": "/parserdocs/one-page.docx"
    },
    {
      "name": "sample.pdf",
      "isFolder": false,
      "modifiedDate": "2022-03-20T12:29:10+00:00",
      "size": 12345,
      "path": "/parserdocs/sample.pdf"
    }
  ]
}
```

### Try it yourself

1. Replace `{access_token}` with your actual token in the cURL example
2. Modify "parserdocs" to a folder path in your storage
3. Run the code and observe the list of files in your folder

## Step 3: Creating a New Folder

Now let's learn how to create a new folder in your cloud storage.

### Implementation Example

#### Using cURL

```bash
curl -X POST "https://api.groupdocs.cloud/v1.0/parser/storage/folder/parserdocs/newfolder?storageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create FolderApi instance
var apiInstance = new FolderApi(configuration);

// Folder path to create
string folderPath = "parserdocs/newfolder";
// Storage name
string storageName = "MyStorage";

// Call API to create folder
apiInstance.CreateFolder(new CreateFolderRequest(folderPath, storageName));

Console.WriteLine($"Folder '{folderPath}' created successfully.");
```

#### Using Java SDK

```java
// Create FolderApi instance
FolderApi apiInstance = new FolderApi(configuration);

// Folder path to create
String folderPath = "parserdocs/newfolder";
// Storage name
String storageName = "MyStorage";

// Call API to create folder
CreateFolderRequest request = new CreateFolderRequest(folderPath, storageName);
apiInstance.createFolder(request);

System.out.println("Folder '" + folderPath + "' created successfully.");
```

### Expected Response

```json
{  
  "code": 200,
  "status": "OK"
}
```

### Learning Checkpoint

Question: Can you create nested folders with a single API call?
Answer: Yes, GroupDocs.Parser Cloud API can create nested folders with a single call by specifying the full path including all subfolder levels (e.g., "folder1/folder2/folder3"). The folders will be created recursively.

## Step 4: Deleting a Folder

Let's learn how to delete a folder from your cloud storage.

### Implementation Example

#### Using cURL

```bash
curl -X DELETE "https://api.groupdocs.cloud/v1.0/parser/storage/folder/parserdocs/newfolder?storageName=MyStorage&recursive=true" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create FolderApi instance
var apiInstance = new FolderApi(configuration);

// Folder path to delete
string folderPath = "parserdocs/newfolder";
// Storage name
string storageName = "MyStorage";
// Delete recursively (true to remove all contents)
bool recursive = true;

// Call API to delete folder
apiInstance.DeleteFolder(new DeleteFolderRequest(folderPath, storageName, recursive));

Console.WriteLine($"Folder '{folderPath}' deleted successfully.");
```

#### Using Java SDK

```java
// Create FolderApi instance
FolderApi apiInstance = new FolderApi(configuration);

// Folder path to delete
String folderPath = "parserdocs/newfolder";
// Storage name
String storageName = "MyStorage";
// Delete recursively (true to remove all contents)
Boolean recursive = true;

// Call API to delete folder
DeleteFolderRequest request = new DeleteFolderRequest(folderPath, storageName, recursive);
apiInstance.deleteFolder(request);

System.out.println("Folder '" + folderPath + "' deleted successfully.");
```

### Expected Response

```json
{  
  "code": 200,
  "status": "OK"
}
```

## Step 5: Copying a Folder

Next, let's learn how to copy a folder to another location in your cloud storage.

### Implementation Example

#### Using cURL

```bash
curl -X PUT "https://api.groupdocs.cloud/v1.0/parser/storage/folder/copy/parserdocs/source?destPath=parserdocs/destination&srcStorageName=MyStorage&destStorageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create FolderApi instance
var apiInstance = new FolderApi(configuration);

// Source folder path
string srcPath = "parserdocs/source";
// Destination folder path
string destPath = "parserdocs/destination";
// Source storage name
string srcStorageName = "MyStorage";
// Destination storage name
string destStorageName = "MyStorage";

// Call API to copy folder
apiInstance.CopyFolder(new CopyFolderRequest(srcPath, destPath, srcStorageName, destStorageName));

Console.WriteLine($"Folder copied from '{srcPath}' to '{destPath}' successfully.");
```

#### Using Java SDK

```java
// Create FolderApi instance
FolderApi apiInstance = new FolderApi(configuration);

// Source folder path
String srcPath = "parserdocs/source";
// Destination folder path
String destPath = "parserdocs/destination";
// Source storage name
String srcStorageName = "MyStorage";
// Destination storage name
String destStorageName = "MyStorage";

// Call API to copy folder
CopyFolderRequest request = new CopyFolderRequest(srcPath, destPath, srcStorageName, destStorageName);
apiInstance.copyFolder(request);

System.out.println("Folder copied from '" + srcPath + "' to '" + destPath + "' successfully.");
```

### Expected Response

```json
{  
  "code": 200,
  "status": "OK"
}
```

## Step 6: Moving a Folder

Finally, let's learn how to move a folder to a different location in your cloud storage.

### Implementation Example

#### Using cURL

```bash
curl -X PUT "https://api.groupdocs.cloud/v1.0/parser/storage/folder/move/parserdocs/source?destPath=parserdocs/newlocation&srcStorageName=MyStorage&destStorageName=MyStorage" \
-H "accept: application/json" \
-H "authorization: Bearer {access_token}"
```

#### Using C# SDK

```csharp
// Create FolderApi instance
var apiInstance = new FolderApi(configuration);

// Source folder path
string srcPath = "parserdocs/source";
// Destination folder path
string destPath = "parserdocs/newlocation";
// Source storage name
string srcStorageName = "MyStorage";
// Destination storage name
string destStorageName = "MyStorage";

// Call API to move folder
apiInstance.MoveFolder(new MoveFolderRequest(srcPath, destPath, srcStorageName, destStorageName));

Console.WriteLine($"Folder moved from '{srcPath}' to '{destPath}' successfully.");
```

#### Using Java SDK

```java
// Create FolderApi instance
FolderApi apiInstance = new FolderApi(configuration);

// Source folder path
String srcPath = "parserdocs/source";
// Destination folder path
String destPath = "parserdocs/newlocation";
// Source storage name
String srcStorageName = "MyStorage";
// Destination storage name
String destStorageName = "MyStorage";

// Call API to move folder
MoveFolderRequest request = new MoveFolderRequest(srcPath, destPath, srcStorageName, destStorageName);
apiInstance.moveFolder(request);

System.out.println("Folder moved from '" + srcPath + "' to '" + destPath + "' successfully.");
```

### Expected Response

```json
{  
  "code": 200,
  "status": "OK"
}
```

## Troubleshooting Tips

If you encounter issues while working with folder operations, consider these common solutions:

1. Path Not Found Errors: Ensure that all folder paths exist before performing operations. Use the Object Exists API to check.
2. Permission Errors: Verify that your account has the necessary permissions for the specified storage.
3. Recursive Delete Issues: When deleting a non-empty folder without setting `recursive=true`, you'll get an error. Always use recursive delete for non-empty folders.

## What You've Learned

In this tutorial, you've learned how to:
- List all files within a specific folder
- Create new folders in cloud storage
- Delete folders from your storage
- Copy folders between storage locations
- Move folders to different paths

These folder operations are essential for organizing and managing your documents efficiently in the cloud.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a folder structure with multiple nested levels
2. Build a simple file browser application using the folder and file APIs
3. Implement a folder synchronization function between two different storage locations

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
