---
title: Copying Folders with Aspose.Slides Cloud API Tutorial
url: /files-folders-storage/folders-copy/
description: Learn to copy and duplicate folder structures in cloud storage with this step-by-step tutorial using Aspose.Slides Cloud API.
weight: 30
---

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with valid credentials
- Basic understanding of REST APIs
- Familiarity with your preferred programming language
- Completed the previous tutorials on [creating folders](/files-folders-storage/folders/create/) and [listing folder contents](/files-folders-storage/folders/list-contents/)

## Why Copy Folders?

Copying folders is useful for various purposes:
- Creating backups of important presentations
- Duplicating project structures for new initiatives
- Sharing content between different storage locations
- Setting up template folder structures
- Maintaining different versions of presentation collections

## Step 1: Understanding the Copy Folder API

The `CopyFolder` API method allows you to copy an existing folder to a new location.

API Information:

| API | Type | Description | Resource |
| :- | :- | :- | :- |
| /slides/storage/folder/copy/{srcPath} | PUT | Copies an existing folder to a new folder | [CopyFolder](https://reference.aspose.cloud/slides/#/Folder/CopyFolder) |

Request Parameters:

| Name | Type | Location | Required | Description |
| :- | :- | :- | :- | :- |
| srcPath | string | path | true | The path to the folder to be copied |
| destPath | string | query | true | The path to the destination folder |
| srcStorageName | string | query | false | The name of a source storage |
| destStorageName | string | query | false | The name of a destination storage |

## Step 2: Obtaining an Access Token

First, authenticate with the Aspose.Slides Cloud API to get an access token:

```bash
curl POST "https://api.aspose.cloud/connect/token" \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded"
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

## Step 3: Copying a Folder

Let's copy a folder named "MyPresentations" from "MyFolder1" to "MyFolder2" in the default storage:

### cURL Example

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/slides/storage/folder/copy/MyFolder1%2FMyPresentations?destPath=MyFolder2%2FMyPresentations" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

This request copies the folder and all its contents to the new location. If successful, the API will return an empty response with a 200 OK status code.

## Step 4: Implementing Folder Copying

Now, let's implement folder copying in different programming languages:

### C# Example

```csharp
using Aspose.Slides.Cloud.Sdk;
using System;

class Application
{
    static void Main(string[] args)
    {
        // Initialize the API client with your credentials
        SlidesApi slidesApi = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Define the source and destination paths
        string srcFolderPath = "MyFolder1/MyPresentations";
        string dstFolderPath = "MyFolder2/MyPresentations";
        
        try {
            // Verify the source folder exists
            var objectExist = slidesApi.ObjectExists(srcFolderPath);
            
            if (!objectExist.Exists || !objectExist.IsFolder)
            {
                Console.WriteLine($"Source folder '{srcFolderPath}' does not exist or is not a folder.");
                return;
            }
            
            // Check if destination already exists
            var destExists = slidesApi.ObjectExists(dstFolderPath);
            
            if (destExists.Exists)
            {
                Console.WriteLine($"Warning: Destination folder '{dstFolderPath}' already exists.");
                // Here you could ask for confirmation to proceed, or choose a different path
            }
            
            Console.WriteLine($"Copying folder from '{srcFolderPath}' to '{dstFolderPath}'...");
            
            // Copy the folder
            slidesApi.CopyFolder(srcFolderPath, dstFolderPath);
            
            Console.WriteLine("Folder copied successfully!");
            
            // Optionally verify the copy was successful by listing contents of both folders
            Console.WriteLine("\nOriginal folder contents:");
            var srcFiles = slidesApi.GetFilesList(srcFolderPath);
            foreach (var item in srcFiles.Value)
            {
                Console.WriteLine($"- {item.Name} ({(item.IsFolder ? "Folder" : "File")})");
            }
            
            Console.WriteLine("\nCopied folder contents:");
            var dstFiles = slidesApi.GetFilesList(dstFolderPath);
            foreach (var item in dstFiles.Value)
            {
                Console.WriteLine($"- {item.Name} ({(item.IsFolder ? "Folder" : "File")})");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error copying folder: " + ex.Message);
        }
    }
}
```

## Try It Yourself

1. Replace the placeholder credentials in the examples with your actual Aspose Cloud API credentials
2. Run the code to copy a folder in your storage
3. Verify the copy operation by checking the contents of both folders
4. Try copying folders between different storages

## Practical Scenarios for Folder Copying

### 1. Creating Backup Copies

```csharp
// Example of creating a dated backup copy of a presentation folder
string srcFolder = "Projects/ClientPresentation";
string timestamp = DateTime.Now.ToString("yyyyMMdd_HHmmss");
string backupFolder = $"Backups/ClientPresentation_{timestamp}";

slidesApi.CopyFolder(srcFolder, backupFolder);
```

### 2. Setting Up Template Structures

```python
# Example of copying a template structure for a new project
template_folder = "Templates/StandardProject"
new_project_folder = "Projects/NewClient2024"

slides_api.copy_folder(template_folder, new_project_folder)
```

### 3. Cross-Storage Copying

```java
// Example of copying between different storages
String srcFolder = "Templates/PresentationTemplates";
String dstFolder = "Templates/PresentationTemplates";
String srcStorage = "DevelopmentStorage";
String dstStorage = "ProductionStorage";

slidesApi.copyFolder(srcFolder, dstFolder, srcStorage, dstStorage);
```

## Troubleshooting Tips

- Path Format: Ensure you use forward slashes (`/`) for path separation
- Source Verification: Always verify that the source folder exists before copying
- Destination Checking: Check if the destination already exists to avoid unintended overwrites
- Cross-Storage Permissions: When copying between different storages, ensure you have proper permissions for both
- Large Folders: For folders with many files or large presentations, the operation may take longer to complete

## What You've Learned

In this tutorial, you have learned:
- How to copy folders and their contents to new locations
- How to verify folder existence before copying
- How to implement folder copying in C#, Python, and Java
- How to verify the success of copy operations
- Practical scenarios for folder copying in presentation management

## Further Practice

To reinforce your learning, try these exercises:
1. Create a utility that makes regular backups of important presentation folders
2. Implement a template system that copies base folders for new projects
3. Build a folder comparison tool that shows differences between original and copied folders
4. Develop a migration script that copies folders between different storage providers

## Next Steps

Now that you know how to copy folders, proceed to the next tutorial to learn about [Moving Folders](/files-folders-storage/folders-move/) in your cloud storage.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [reach out to our support team](https://forum.aspose.cloud/c/slides/15)
