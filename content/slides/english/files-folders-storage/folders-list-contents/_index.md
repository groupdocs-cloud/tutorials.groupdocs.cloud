---
title: How to List Files and Folders - Aspose.Slides Cloud API Tutorial
description: Learn to retrieve and display the contents of folders in your cloud storage with this comprehensive tutorial using Aspose.Slides Cloud API.
url: /files-folders-storage/folders-list-contents/
weight: 20
---

# How to List Files and Folders

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with valid credentials
- Basic understanding of REST APIs
- Familiarity with your preferred programming language

## Why List Directory Contents?

Being able to list folder contents is essential for:
- Building file browsers and selection interfaces
- Performing bulk operations on multiple files
- Validating folder structures and organization
- Finding specific files within a folder hierarchy
- Generating reports about storage usage and contents

## Step 1: Understanding the Get Files List API

The `GetFilesList` API method allows you to retrieve information about all files and folders within a specified folder.

API Information:

| API | Type | Description | Resource |
| :- | :- | :- | :- |
| /slides/storage/folder/{path} | GET | Retrieves information about all files and folders within a folder | [GetFilesList](https://reference.aspose.cloud/slides/#/Folder/GetFilesList) |

Request Parameters:

| Name | Type | Location | Required | Description |
| :- | :- | :- | :- | :- |
| path | string | path | true | The path to the folder |
| storageName | string | query | false | The name of a storage where the folder is located |

Note: This method only retrieves information about files and folders that are directly within the specified folder and does not include contents of subfolders.

## Step 2: Obtaining an Access Token

First, authenticate with the Aspose.Slides Cloud API to get an access token:

```bash
curl POST "https://api.aspose.cloud/connect/token" \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded"
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

## Step 3: Listing Files and Folders

Let's retrieve a list of all files and folders in the "MyFolder" folder of the "MyStorage" storage:

### cURL Example

```bash
curl -X GET "https://api.aspose.cloud/v3.0/slides/storage/folder/MyFolder?storageName=MyStorage" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

This will return a JSON response with information about all files and folders:

```json
{
  "value": [
    {
      "name": "MyFolder1",
      "isFolder": true,
      "modifiedDate": "0001-01-01T00:00:00",
      "size": 0,
      "path": "/MyFolder/MyFolder1/"
    },
    {
      "name": "MyFolder2",
      "isFolder": true,
      "modifiedDate": "0001-01-01T00:00:00",
      "size": 0,
      "path": "/MyFolder/MyFolder2/"
    },
    {
      "name": "MyPresentation.pptx",
      "isFolder": false,
      "modifiedDate": "2024-02-16T07:05:08+00:00",
      "size": 34259,
      "path": "/MyFolder/MyPresentation.pptx"
    }
  ]
}
```

The response includes the following information for each item:
- `name`: The name of the file or folder
- `isFolder`: Indicates if the item is a folder (true) or a file (false)
- `modifiedDate`: When the item was last modified
- `size`: Size in bytes (0 for folders)
- `path`: Full path to the item

## Step 4: Implementing Folder Listing

Now, let's implement folder listing in different programming languages:

### C# Example

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Model;

class Application
{
    static void Main(string[] args)
    {
        // Initialize the API client with your credentials
        SlidesApi slidesApi = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Define the storage and folder path
        string storageName = "MyStorage";
        string folderName = "MyFolder";
        
        try {
            // Get the list of files and folders
            FilesList filesList = slidesApi.GetFilesList(folderName, storageName);
            
            // Display summary
            int folderCount = filesList.Value.Count(item => item.IsFolder);
            int fileCount = filesList.Value.Count - folderCount;
            
            Console.WriteLine($"Found {folderCount} folders and {fileCount} files in {folderName}:");
            Console.WriteLine();
            
            // Display folders first, then files
            Console.WriteLine("FOLDERS:");
            Console.WriteLine("--------");
            
            foreach (StorageFile item in filesList.Value.Where(i => i.IsFolder))
            {
                Console.WriteLine($"- {item.Name}");
                Console.WriteLine($"  Path: {item.Path}");
                Console.WriteLine();
            }
            
            Console.WriteLine("FILES:");
            Console.WriteLine("------");
            
            foreach (StorageFile item in filesList.Value.Where(i => !i.IsFolder))
            {
                Console.WriteLine($"- {item.Name}");
                Console.WriteLine($"  Size: {FormatBytes(item.Size)}");
                Console.WriteLine($"  Modified: {item.ModifiedDate}");
                Console.WriteLine($"  Path: {item.Path}");
                Console.WriteLine();
            }
            
            // Find the largest file
            if (fileCount > 0)
            {
                StorageFile largestFile = filesList.Value
                    .Where(i => !i.IsFolder)
                    .OrderByDescending(i => i.Size)
                    .First();
                
                Console.WriteLine($"Largest file: {largestFile.Name} ({FormatBytes(largestFile.Size)})");
            }
            
            // Find the most recently modified file
            if (fileCount > 0)
            {
                StorageFile latestFile = filesList.Value
                    .Where(i => !i.IsFolder && i.ModifiedDate != null)
                    .OrderByDescending(i => DateTime.Parse(i.ModifiedDate))
                    .First();
                
                Console.WriteLine($"Most recently modified file: {latestFile.Name} ({latestFile.ModifiedDate})");
            }
        } catch (Exception ex) {
            Console.WriteLine("Error listing folder contents: " + ex.Message);
        }
    }
    
    // Helper method to format bytes into a readable format
    static string FormatBytes(long bytes)
    {
        string[] sizes = { "B", "KB", "MB", "GB", "TB" };
        double formattedSize = bytes;
        int sizeIndex = 0;
        while (formattedSize >= 1024 && sizeIndex < sizes.Length - 1)
        {
            formattedSize /= 1024;
            sizeIndex++;
        }
        return String.Format("{0:0.##} {1}", formattedSize, sizes[sizeIndex]);
    }
}
```

### Python Example

```python
from asposeslidescloud.apis.slides_api import SlidesApi
from datetime import datetime

# Initialize the API client with your credentials
slides_api = SlidesApi(None, "YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET")

# Define the storage and folder path
storage_name = "MyStorage"
folder_name = "MyFolder"

def format_bytes(bytes):
    """Convert bytes to a human-readable format"""
    sizes = ["B", "KB", "MB", "GB", "TB"]
    formatted_size = float(bytes)
    size_index = 0
    while formatted_size >= 1024 and size_index < len(sizes) - 1:
        formatted_size /= 1024
        size_index += 1
    return f"{formatted_size:.2f} {sizes[size_index]}"

def format_date(date_str):
    """Format the date string in a more readable format"""
    try:
        date_obj = datetime.strptime(date_str, "%Y-%m-%dT%H:%M:%S%z")
        return date_obj.strftime("%Y-%m-%d %H:%M:%S")
    except:
        return date_str if date_str else "N/A"

try:
    # Get the list of files and folders
    files_list = slides_api.get_files_list(folder_name, storage_name)
    
    # Display summary
    folder_items = [item for item in files_list.value if item.is_folder]
    file_items = [item for item in files_list.value if not item.is_folder]
    
    print(f"Found {len(folder_items)} folders and {len(file_items)} files in {folder_name}:")
    print()
    
    # Display folders first, then files
    print("FOLDERS:")
    print("--------")
    
    for item in folder_items:
        print(f"- {item.name}")
        print(f"  Path: {item.path}")
        print()
    
    print("FILES:")
    print("------")
    
    for item in file_items:
        print(f"- {item.name}")
        print(f"  Size: {format_bytes(item.size)}")
        print(f"  Modified: {format_date(item.modified_date)}")
        print(f"  Path: {item.path}")
        print()
    
    # Find the largest file
    if file_items:
        largest_file = max(file_items, key=lambda x: x.size)
        print(f"Largest file: {largest_file.name} ({format_bytes(largest_file.size)})")
    
    # Find the most recently modified file
    if file_items:
        # Filter out files with invalid or missing dates
        files_with_dates = [f for f in file_items if f.modified_date]
        if files_with_dates:
            latest_file = max(files_with_dates, key=lambda x: x.modified_date)
            print(f"Most recently modified file: {latest_file.name} ({format_date(latest_file.modified_date)})")
except Exception as e:
    print(f"Error listing folder contents: {str(e)}")
```

### Java Example

```java
import com.aspose.slides.ApiException;
import com.aspose.slides.api.SlidesApi;
import com.aspose.slides.model.FilesList;
import com.aspose.slides.model.StorageFile;

import java.text.DecimalFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Comparator;
import java.util.Date;
import java.util.List;
import java.util.stream.Collectors;

public class Application {
    public static void main(String[] args) {
        // Initialize the API client with your credentials
        SlidesApi slidesApi = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Define the storage and folder path
        String storageName = "MyStorage";
        String folderName = "MyFolder";
        
        try {
            // Get the list of files and folders
            FilesList filesList = slidesApi.getFilesList(folderName, storageName);
            
            if (filesList.getValue() != null) {
                // Display summary
                List<StorageFile> folderItems = filesList.getValue().stream()
                    .filter(item -> item.getIsFolder())
                    .collect(Collectors.toList());
                
                List<StorageFile> fileItems = filesList.getValue().stream()
                    .filter(item -> !item.getIsFolder())
                    .collect(Collectors.toList());
                
                System.out.println("Found " + folderItems.size() + " folders and " 
                    + fileItems.size() + " files in " + folderName + ":");
                System.out.println();
                
                // Display folders first, then files
                System.out.println("FOLDERS:");
                System.out.println("--------");
                
                for (StorageFile item : folderItems) {
                    System.out.println("- " + item.getName());
                    System.out.println("  Path: " + item.getPath());
                    System.out.println();
                }
                
                System.out.println("FILES:");
                System.out.println("------");
                
                for (StorageFile item : fileItems) {
                    System.out.println("- " + item.getName());
                    System.out.println("  Size: " + formatBytes(item.getSize()));
                    System.out.println("  Modified: " + item.getModifiedDate());
                    System.out.println("  Path: " + item.getPath());
                    System.out.println();
                }
                
                // Find the largest file
                if (!fileItems.isEmpty()) {
                    StorageFile largestFile = fileItems.stream()
                        .max(Comparator.comparing(f -> f.getSize()))
                        .orElse(null);
                    
                    if (largestFile != null) {
                        System.out.println("Largest file: " + largestFile.getName() 
                            + " (" + formatBytes(largestFile.getSize()) + ")");
                    }
                }
                
                // Find the most recently modified file
                if (!fileItems.isEmpty()) {
                    // Filter out files with invalid or missing dates
                    List<StorageFile> filesWithDates = fileItems.stream()
                        .filter(f -> f.getModifiedDate() != null && !f.getModifiedDate().isEmpty())
                        .collect(Collectors.toList());
                    
                    if (!filesWithDates.isEmpty()) {
                        StorageFile latestFile = filesWithDates.stream()
                            .max(Comparator.comparing(f -> {
                                try {
                                    return parseDate(f.getModifiedDate());
                                } catch (ParseException e) {
                                    return new Date(0);
                                }
                            }))
                            .orElse(null);
                        
                        if (latestFile != null) {
                            System.out.println("Most recently modified file: " + latestFile.getName() 
                                + " (" + latestFile.getModifiedDate() + ")");
                        }
                    }
                }
            } else {
                System.out.println("No files or folders found in " + folderName);
            }
        } catch (ApiException e) {
            System.out.println("Error listing folder contents: " + e.getMessage());
        }
    }
    
    // Helper method to format bytes into a readable format
    static String formatBytes(long bytes) {
        String[] sizes = { "B", "KB", "MB", "GB", "TB" };
        double formattedSize = bytes;
        int sizeIndex = 0;
        while (formattedSize >= 1024 && sizeIndex < sizes.length - 1) {
            formattedSize /= 1024;
            sizeIndex++;
        }
        return new DecimalFormat("#.##").format(formattedSize) + " " + sizes[sizeIndex];
    }
    
    // Helper method to parse date strings
    static Date parseDate(String dateStr) throws ParseException {
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssXXX");
        return format.parse(dateStr);
    }
}
```

## Try It Yourself

1. Replace the placeholder credentials in the examples with your actual Aspose Cloud API credentials
2. Run the code to list the contents of a folder in your storage
3. Modify the code to display the information in different formats
4. Add additional analysis and filtering functionality based on your needs

## Building a Recursive Folder Browser

The examples above list only the direct contents of a folder. To build a recursive folder browser that displays the contents of all subfolders as well, you would need to:

1. Get the list of items in the parent folder
2. For each subfolder, make another API call to get its contents
3. Process these contents recursively
4. Build a tree structure to represent the complete hierarchy

Here's a simplified example in Python:

```python
def list_folder_recursive(api, folder_path, storage_name, indent=""):
    """List all files and folders recursively"""
    try:
        # Get items in the current folder
        items = api.get_files_list(folder_path, storage_name).value
        
        # Process each item
        for item in items:
            # Print the current item
            item_type = "📁" if item.is_folder else "📄"
            print(f"{indent}{item_type} {item.name}")
            
            # If it's a folder, recursively list its contents
            if item.is_folder:
                # Form the subfolder path
                subfolder_path = item.path.rstrip('/')
                # Recursive call with increased indentation
                list_folder_recursive(api, subfolder_path, storage_name, indent + "  ")
    except Exception as e:
        print(f"{indent}Error listing {folder_path}: {str(e)}")

# Usage
list_folder_recursive(slides_api, "MyFolder", "MyStorage")
```

## Troubleshooting Tips

- Empty Path: Use empty string or `/` to list the root folder
- Path Format: Ensure you use forward slashes (`/`) for path separation
- Non-Existent Folder: Verify that the folder exists before trying to list its contents
- Date Parsing: Be careful when parsing date strings, formats may vary
- Large Folders: For folders with many items, consider implementing pagination

## What You've Learned

In this tutorial, you have learned:
- How to retrieve a list of files and folders within a specific folder
- How to extract and use metadata about each item (name, size, date, etc.)
- How to filter, sort, and analyze directory contents programmatically
- How to identify the largest files and most recently modified items
- How to implement folder listing in C#, Python, and Java

## Further Practice

To reinforce your learning, try these exercises:
1. Create a function that generates a folder size report, including the size of all subfolders
2. Implement a file search utility that finds files matching specific criteria
3. Build a simple command-line folder browser using the API
4. Develop a function to export folder structure to a tree-view format

## Next Steps

Now that you know how to list files and folders, proceed to the next tutorial to learn about [Copying Folders](/files-folders-storage/folders-copy/) in your cloud storage.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [reach out to our support team](https://forum.aspose.cloud/c/slides/15)
