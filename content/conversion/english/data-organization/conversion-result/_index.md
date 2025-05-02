---
title: Processing Conversion Results Tutorial
url: /data-organization/conversion-result/
weight: 40
description: Learn effective techniques for handling and organizing document conversion output with this hands-on tutorial for GroupDocs.Conversion Cloud API
---

# Tutorial: Processing Conversion Results

## Learning Objectives

In this tutorial, you'll learn:
- What ConversionResult is and how to interpret it
- How to process conversion output efficiently
- Techniques for handling multi-page and multi-format conversions
- Best practices for managing converted documents in cloud storage

## Prerequisites

Before starting this tutorial, you should have:
- A [GroupDocs.Conversion Cloud account](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with JSON data structures
- Completion of the previous tutorials in this series (recommended)
- A development environment set up for your preferred language (examples provided in cURL, Python, Java, and C#)

## What is ConversionResult?

ConversionResult is the data structure returned by the GroupDocs.Conversion Cloud API after a successful conversion operation. It provides critical information about the converted document(s), including:

- File names of the converted documents
- File sizes
- URLs to access the converted documents in cloud storage

The ConversionResult is returned as an array, since a single conversion request can produce multiple output files (e.g., when converting multiple pages of a document to separate image files).

## Understanding the ConversionResult Structure

Let's examine the basic structure of a ConversionResult:

```json
[
  {
    "name": "converted-document.pdf",
    "size": 17958,
    "url": "converted/converted-document.pdf"
  },
  {
    "name": "converted-document-page-1.jpg",
    "size": 45327,
    "url": "converted/converted-document-page-1.jpg"
  },
  {
    "name": "converted-document-page-2.jpg",
    "size": 39481,
    "url": "converted/converted-document-page-2.jpg"
  }
]
```

Each item in the array contains:

| Field | Description |
|-------|-------------|
| name | The name of the converted file |
| size | The size of the file in bytes |
| url | The relative path to the file in your cloud storage |

## Step 1: Basic Processing of ConversionResult

Let's start with a simple example of processing a ConversionResult:

```python
# Tutorial Code Example: Basic ConversionResult Processing
# Assuming 'result' contains the ConversionResult array

# Print information about each converted file
for file in result:
    print(f"File: {file['name']}")
    print(f"Size: {file['size']} bytes")
    print(f"URL: {file['url']}")
    print("---")
    
# Calculate total size of all converted files
total_size = sum(file['size'] for file in result)
print(f"Total conversion size: {total_size} bytes")
print(f"Number of files created: {len(result)}")
```

### Try it yourself

Write code to process a ConversionResult and categorize files by their extension:

```python
# Exercise: Categorize ConversionResult files by extension
# Your code here:
def categorize_by_extension(result):
    extensions = {}
    
    for file in result:
        # Extract extension from filename
        name_parts = file['name'].split('.')
        if len(name_parts) > 1:
            ext = name_parts[-1].lower()
            if ext not in extensions:
                extensions[ext] = []
            extensions[ext].append(file)
    
    # Print summary
    for ext, files in extensions.items():
        print(f"Extension .{ext}: {len(files)} files, {sum(f['size'] for f in files)} bytes total")
        
    return extensions

# Usage
file_categories = categorize_by_extension(result)
```

## Step 2: Working with File Paths and URLs

The `url` field in ConversionResult provides the relative path to the file in your cloud storage. To access or download these files, you'll need to understand how to work with these URLs.

```python
# Tutorial Code Example: Working with ConversionResult URLs
import requests

# Assuming 'result' contains the ConversionResult array
# and 'access_token' contains your API access token

base_url = "https://api.groupdocs.cloud/v2.0/conversion/storage/file/"

def download_converted_file(file_info, destination_folder="downloads/"):
    """Download a converted file using its URL from the ConversionResult"""
    import os
    
    # Create destination folder if it doesn't exist
    if not os.path.exists(destination_folder):
        os.makedirs(destination_folder)
    
    # Construct full API URL
    file_url = base_url + file_info["url"]
    
    # Send request to download file
    headers = {"Authorization": f"Bearer {access_token}"}
    response = requests.get(file_url, headers=headers, stream=True)
    
    if response.status_code == 200:
        # Save file to local destination
        local_path = os.path.join(destination_folder, file_info["name"])
        with open(local_path, "wb") as f:
            for chunk in response.iter_content(chunk_size=8192):
                f.write(chunk)
        print(f"Downloaded: {file_info['name']} to {local_path}")
        return local_path
    else:
        print(f"Failed to download {file_info['name']}: {response.status_code}")
        return None

# Download all converted files
for file in result:
    download_converted_file(file)
```

### Try it yourself

Write a function to download only the largest file from a ConversionResult:

```python
# Exercise: Download only the largest file from ConversionResult
# Your code here:
def download_largest_file(result, destination_folder="downloads/"):
    """Find and download only the largest file from the ConversionResult"""
    if not result:
        print("No files to download")
        return None
    
    # Find the largest file
    largest_file = max(result, key=lambda x: x["size"])
    print(f"Largest file is {largest_file['name']} ({largest_file['size']} bytes)")
    
    # Download the file
    return download_converted_file(largest_file, destination_folder)

# Usage
largest_file_path = download_largest_file(result)
```

## Step 3: Handling Multi-Page Conversions

When converting multi-page documents to formats like images, you'll receive multiple files in the ConversionResult (one for each page). Let's see how to handle this scenario:

```python
# Tutorial Code Example: Handling Multi-Page Conversion Results
def organize_pages_by_document(result):
    """Organize conversion results by document and page number"""
    documents = {}
    
    for file in result:
        name = file["name"]
        
        # Check if this is a page file (contains "-page-" in the name)
        if "-page-" in name:
            # Parse document name and page number
            base_name, page_info = name.split("-page-")
            page_num = int(page_info.split(".")[0])
            
            # Add to documents dictionary
            if base_name not in documents:
                documents[base_name] = {"pages": {}}
            
            documents[base_name]["pages"][page_num] = file
        else:
            # This is a main document file
            doc_name = name.split(".")[0]
            if doc_name not in documents:
                documents[doc_name] = {"main_file": file, "pages": {}}
            else:
                documents[doc_name]["main_file"] = file
    
    return documents

# Usage example
organized_results = organize_pages_by_document(result)

# Process each document's pages in order
for doc_name, doc_info in organized_results.items():
    print(f"Document: {doc_name}")
    
    if "main_file" in doc_info:
        print(f"Main file: {doc_info['main_file']['name']} ({doc_info['main_file']['size']} bytes)")
    
    print("Pages:")
    # Sort pages by number
    sorted_pages = sorted(doc_info["pages"].items())
    for page_num, page_file in sorted_pages:
        print(f"  Page {page_num}: {page_file['name']} ({page_file['size']} bytes)")
```

This code organizes conversion results by document and page number, making it easier to process multi-page conversions systematically.

### Batch Processing Large Documents

For very large documents, you might need to process pages in batches:

```python
# Tutorial Code Example: Batch Processing Large Documents
def batch_process_pages(organized_results, batch_size=10, process_func=None):
    """Process document pages in batches"""
    if process_func is None:
        # Default processing function just prints info
        process_func = lambda page_files: print(f"Processing batch of {len(page_files)} pages")
    
    for doc_name, doc_info in organized_results.items():
        print(f"Processing document: {doc_name}")
        
        # Get all pages sorted by page number
        all_pages = [page for _, page in sorted(doc_info["pages"].items())]
        total_pages = len(all_pages)
        
        # Process pages in batches
        for i in range(0, total_pages, batch_size):
            batch = all_pages[i:i+batch_size]
            print(f"Processing batch {i//batch_size + 1}, pages {i+1}-{min(i+batch_size, total_pages)}")
            process_func(batch)
            
# Example usage with a custom processing function
def my_batch_processor(page_files):
    # Process a batch of page files
    print(f"Processing {len(page_files)} pages")
    total_batch_size = sum(p["size"] for p in page_files)
    print(f"Total batch size: {total_batch_size} bytes")
    # Add your processing logic here...

batch_process_pages(organized_results, batch_size=5, process_func=my_batch_processor)
```

## Step 4: Implementing Complete Solutions

Let's see how to implement ConversionResult processing with the API in different languages:

### Python Example

```python
# Tutorial Code Example: Complete ConversionResult Processing in Python
import groupdocs_conversion_cloud
import os

# Configure the API client
configuration = groupdocs_conversion_cloud.Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)
api_client = groupdocs_conversion_cloud.ApiClient(configuration)
convert_api = groupdocs_conversion_cloud.ConvertApi(api_client)
file_api = groupdocs_conversion_cloud.FileApi(api_client)

# Create conversion settings
settings = groupdocs_conversion_cloud.ConvertSettings()
settings.file_path = "documents/multipage.docx"
settings.format = "jpeg"
settings.output_path = "converted/"

# Execute conversion
result = convert_api.convert(settings)
print(f"Conversion completed: {len(result)} files generated")

# Process and download results
download_folder = "downloads/"
if not os.path.exists(download_folder):
    os.makedirs(download_folder)

# Organize results by page
pages = {}
for file_info in result:
    print(f"File: {file_info.name}, Size: {file_info.size} bytes, URL: {file_info.url}")
    
    # Extract page number from filename
    if "-page-" in file_info.name:
        page_num = int(file_info.name.split("-page-")[1].split(".")[0])
        pages[page_num] = file_info
    
    # Download the file
    request = groupdocs_conversion_cloud.DownloadFileRequest(file_info.url)
    response = file_api.download_file(request)
    
    # Save to local file
    output_path = os.path.join(download_folder, file_info.name)
    with open(output_path, 'wb') as f:
        f.write(response)
    
    print(f"Downloaded to {output_path}")

# Process pages in order
for page_num in sorted(pages.keys()):
    page_file = pages[page_num]
    print(f"Processing page {page_num}: {page_file.name}")
    # Add your page processing logic here...
```

### C# Example

```csharp
// Tutorial Code Example: Complete ConversionResult Processing in C#
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using GroupDocs.Conversion.Cloud.Sdk;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

// Configure the API client
var configuration = new Configuration 
{
    ClientId = "YOUR_CLIENT_ID",
    ClientSecret = "YOUR_CLIENT_SECRET"
};

var convertApi = new ConvertApi(configuration);
var fileApi = new FileApi(configuration);

// Create conversion settings
var settings = new ConvertSettings 
{
    FilePath = "documents/multipage.docx",
    Format = "jpeg",
    OutputPath = "converted/"
};

// Execute conversion
var result = convertApi.Convert(new ConvertRequest(settings));
Console.WriteLine($"Conversion completed: {result.Count} files generated");

// Process and download results
string downloadFolder = "downloads";
if (!Directory.Exists(downloadFolder))
{
    Directory.CreateDirectory(downloadFolder);
}

// Organize results by page
var pages = new Dictionary<int, StoredConvertedResult>();
foreach (var fileInfo in result)
{
    Console.WriteLine($"File: {fileInfo.Name}, Size: {fileInfo.Size} bytes, URL: {fileInfo.Url}");
    
    // Extract page number from filename
    if (fileInfo.Name.Contains("-page-"))
    {
        string[] parts = fileInfo.Name.Split(new[] { "-page-" }, StringSplitOptions.None);
        string pageNumStr = parts[1].Split('.')[0];
        if (int.TryParse(pageNumStr, out int pageNum))
        {
            pages[pageNum] = fileInfo;
        }
    }
    
    // Download the file
    var downloadRequest = new DownloadFileRequest(fileInfo.Url);
    var response = fileApi.DownloadFile(downloadRequest);
    
    // Save to local file
    string outputPath = Path.Combine(downloadFolder, fileInfo.Name);
    using (var fileStream = File.Create(outputPath))
    {
        response.CopyTo(fileStream);
    }
    
    Console.WriteLine($"Downloaded to {outputPath}");
}

// Process pages in order
foreach (var pageNum in pages.Keys.OrderBy(k => k))
{
    var pageFile = pages[pageNum];
    Console.WriteLine($"Processing page {pageNum}: {pageFile.Name}");
    // Add your page processing logic here...
}
```

### Java Example

```java
// Tutorial Code Example: Complete ConversionResult Processing in Java
import com.groupdocs.conversion.cloud.api.ConvertApi;
import com.groupdocs.conversion.cloud.api.FileApi;
import com.groupdocs.conversion.cloud.api.model.*;
import com.groupdocs.conversion.cloud.api.model.requests.*;
import java.io.*;
import java.nio.file.*;
import java.util.*;
import java.util.stream.Collectors;

// Configure the API client
Configuration configuration = new Configuration();
configuration.setClientId("YOUR_CLIENT_ID");
configuration.setClientSecret("YOUR_CLIENT_SECRET");

ConvertApi convertApi = new ConvertApi(configuration);
FileApi fileApi = new FileApi(configuration);

// Create conversion settings
ConvertSettings settings = new ConvertSettings();
settings.setFilePath("documents/multipage.docx");
settings.setFormat("jpeg");
settings.setOutputPath("converted/");

try {
    // Execute conversion
    List<StoredConvertedResult> result = convertApi.convert(new ConvertRequest(settings));
    System.out.println("Conversion completed: " + result.size() + " files generated");
    
    // Process and download results
    String downloadFolder = "downloads";
    Files.createDirectories(Paths.get(downloadFolder));
    
    // Organize results by page
    Map<Integer, StoredConvertedResult> pages = new HashMap<>();
    for (StoredConvertedResult fileInfo : result) {
        System.out.println("File: " + fileInfo.getName() + 
                           ", Size: " + fileInfo.getSize() + " bytes, URL: " + fileInfo.getUrl());
        
        // Extract page number from filename
        if (fileInfo.getName().contains("-page-")) {
            String[] parts = fileInfo.getName().split("-page-");
            String pageNumStr = parts[1].split("\\.")[0];
            try {
                int pageNum = Integer.parseInt(pageNumStr);
                pages.put(pageNum, fileInfo);
            } catch (NumberFormatException e) {
                System.out.println("Could not parse page number: " + pageNumStr);
            }
        }
        
        // Download the file
        DownloadFileRequest downloadRequest = new DownloadFileRequest(fileInfo.getUrl());
        byte[] response = fileApi.downloadFile(downloadRequest);
        
        // Save to local file
        String outputPath = Paths.get(downloadFolder, fileInfo.getName()).toString();
        Files.write(Paths.get(outputPath), response);
        
        System.out.println("Downloaded to " + outputPath);
    }
    
    // Process pages in order
    List<Integer> sortedPageNums = pages.keySet().stream().sorted().collect(Collectors.toList());
    for (Integer pageNum : sortedPageNums) {
        StoredConvertedResult pageFile = pages.get(pageNum);
        System.out.println("Processing page " + pageNum + ": " + pageFile.getName());
        // Add your page processing logic here...
    }
    
} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
    e.printStackTrace();
}
```

## Tips and Best Practices

### 1. Optimize Cloud Storage Management

Manage your cloud storage efficiently:
- Delete temporary conversion files once downloaded and processed
- Organize converted files in a structured folder hierarchy
- Set up automated cleanup for old converted files
- Monitor storage usage to avoid quota limits

```python
# Tutorial Code Example: Cleaning Up Temporary Files
def cleanup_converted_files(result, storage_api):
    """Delete converted files from cloud storage after processing"""
    for file in result:
        try:
            request = DeleteFileRequest(file["url"])
            storage_api.delete_file(request)
            print(f"Deleted {file['name']} from cloud storage")
        except Exception as e:
            print(f"Error deleting {file['name']}: {e}")
```

### 2. Implement Reliable Error Handling

Always implement proper error handling for API operations:
- Check for API connection errors
- Verify file existence before downloading
- Implement retries for transient errors
- Log detailed error information for troubleshooting

### 3. Use Batch Processing for Large Documents

For large documents with many pages:
- Process pages in batches to manage memory efficiently
- Implement progress tracking for long-running conversions
- Consider parallel processing for faster operations

### 4. Track Conversion Metadata

Maintain metadata about your conversions:
- Record document names, page counts, and file sizes
- Track conversion timestamps for auditing
- Store relationships between original and converted files

## Troubleshooting Common Issues

### Issue 1: Missing Files in ConversionResult
If files are missing from your ConversionResult:
- Verify the document exists and is accessible
- Check if page selection options limited the conversion
- Ensure the document has content (not blank or corrupted)

### Issue 2: Access Denied When Downloading
If you receive access denied errors:
- Verify your access token is valid and not expired
- Check permissions for the storage and files
- Ensure the file paths are correctly formatted

### Issue 3: File Size Discrepancies
If downloaded file sizes don't match expected sizes:
- Ensure you're reading the file in binary mode
- Check for network interruptions during download
- Verify no content encoding/compression is applied

## What You've Learned

In this tutorial, you've learned:
- The structure and purpose of ConversionResult in the GroupDocs.Conversion Cloud API
- How to process and download converted files
- Techniques for organizing and managing multi-page conversions
- Best practices for working with converted documents in cloud storage
- Implementation examples in multiple programming languages

## Further Practice

To reinforce your understanding:
1. Create a utility that downloads and organizes converted files by document type and date
2. Implement a system that tracks conversion statistics (file sizes, page counts, etc.)
3. Build a simple web interface to view and download converted documents
4. Create a batch processor for converting and organizing large document collections

## Next Steps

Ready to advance your skills? Move on to our next tutorial: [Tutorial: Implementing Format-Specific Conversion Options](/data-organization/format-specific-options/) to learn more about optimizing conversions for particular document formats.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [Live Demo](https://products.groupdocs.app/conversion/family)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.conversion-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback

Have questions about working with ConversionResult? Need clarification on any part of this tutorial? We'd love to hear from you! Please visit our [support forum](https://forum.groupdocs.cloud/c/conversion/11) with any questions or feedback.