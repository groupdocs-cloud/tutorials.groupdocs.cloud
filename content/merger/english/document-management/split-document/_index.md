---
title: How to Split a Document Tutorial
weight: 25
url: /document-management/split-document/
description: Learn how to split documents into multiple files with precision using GroupDocs.Merger Cloud API in this comprehensive developer tutorial.
---

# Tutorial: How to Split a Document

## Introduction

In this tutorial, you'll learn how to split documents into multiple smaller documents using GroupDocs.Merger Cloud API. Document splitting is a powerful feature that allows you to divide large documents into more manageable pieces, extract specific sections, or create separate documents for different chapters or sections.

GroupDocs.Merger Cloud offers flexible document splitting capabilities with multiple approaches:
- Split into single-page documents by specifying exact page numbers
- Split into single-page documents using start/end page ranges
- Apply filters to split only even or odd pages
- Split into multi-page documents at specified breakpoints

## Learning Objectives

By the end of this tutorial, you will be able to:
- Split a document into multiple single-page documents
- Extract specific pages from a document
- Split documents using page ranges and filters
- Create multi-page documents from splitting operations
- Implement document splitting in different programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
- A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic knowledge of RESTful APIs
- Sample multi-page documents to work with

## Use Case Scenario

Let's consider a practical scenario: You have a large annual report document (100+ pages) that needs to be divided into separate chapters for distribution to different departments. Each chapter starts on specific pages within the document (e.g., chapter 1 on page 1, chapter 2 on page 25, chapter 3 on page 42, etc.).

## Implementation Steps

### Step 1: Authenticate with the API

First, authenticate with the GroupDocs.Merger Cloud API by obtaining a JWT token using your Client ID and Client Secret.

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

### Step 2: Prepare Your Document Splitting Request

GroupDocs.Merger Cloud provides different approaches to document splitting. Let's explore each method:

#### Method 1: Split to Single-Page Documents by Exact Page Numbers

This method allows you to extract specific pages into separate single-page documents.

```json
{
   "FileInfo": { "FilePath": "WordProcessing/sample-10-pages.docx" },
   "Pages": [ 3, 6, 8 ],
   "Mode": "Pages",
   "OutputPath": "output/split-to-specific-pages"
}
```

#### Method 2: Split to Single-Page Documents by Page Range

This method creates separate single-page documents for each page within a specified range.

```json
{
   "FileInfo": { "FilePath": "WordProcessing/sample-10-pages.docx" },
   "StartPageNumber": 3,
   "EndPageNumber": 7,
   "Mode": "Pages",
   "OutputPath": "output/split-by-page-range"
}
```

#### Method 3: Split to Single-Page Documents with Filtering

This method creates separate single-page documents for odd or even pages within a specified range.

```json
{
   "FileInfo": { "FilePath": "WordProcessing/sample-10-pages.docx" },
   "StartPageNumber": 3,
   "EndPageNumber": 7,
   "RangeMode": "Odd",
   "Mode": "Pages",
   "OutputPath": "output/split-filtered-pages"
}
```

#### Method 4: Split to Multi-Page Documents

This method creates multiple multi-page documents based on specified breakpoints.

```json
{
   "FileInfo": { "FilePath": "WordProcessing/sample-10-pages.docx" },
   "Pages": [ 3, 6, 8 ],
   "Mode": "Intervals",
   "OutputPath": "output/split-to-multipage"
}
```

### Step 3: Execute Document Splitting Operation

Let's execute the document splitting operation using the API. For this example, we'll use Method 1 to extract specific pages:

#### cURL Example

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/split" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
   'FileInfo': { 'FilePath': 'WordProcessing/sample-10-pages.docx' },
   'Pages': [ 3, 6, 8 ],
   'Mode': 'Pages',
   'OutputPath': 'output/split-to-specific-pages'
}"
```

The API will respond with:

```json
{
  "documents": [
    {
      "path": "output/split-to-specific-pages/sample-10-pages_0.docx"
    },
    {
      "path": "output/split-to-specific-pages/sample-10-pages_1.docx"
    },
    {
      "path": "output/split-to-specific-pages/sample-10-pages_2.docx"
    }
  ]
}
```

## Try It Yourself

Let's implement document splitting in different programming languages. First, make sure your multi-page document is uploaded to your GroupDocs Cloud storage, then use the appropriate SDK for your language.

### C# Example: Splitting to Single Pages by Exact Numbers

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new DocumentApi(configuration);

try
{
    // Configure split options with specific pages
    var options = new SplitOptions
    {
        FileInfo = new FileInfo
        {
            FilePath = "WordProcessing/sample-10-pages.docx"
        },
        Pages = new List<int> { 3, 6, 8 },
        Mode = SplitOptions.ModeEnum.Pages,
        OutputPath = "Output/split-to-specific-pages"
    };

    // Execute split operation
    var request = new SplitRequest(options);
    var response = apiInstance.Split(request);

    Console.WriteLine("Number of created documents: " + response.Documents.Count);
    foreach (var doc in response.Documents)
    {
        Console.WriteLine("Document path: " + doc.Path);
    }
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling API: " + e.Message);
}
```

### Java Example: Splitting to Single Pages by Page Range

```java
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-java-samples
String MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
DocumentApi apiInstance = new DocumentApi(configuration);

try {
    // Configure file info
    FileInfo fileInfo = new FileInfo();   
    fileInfo.setFilePath("WordProcessing/sample-10-pages.docx");
    
    // Configure split options with page range
    SplitOptions options = new SplitOptions();
    options.setFileInfo(fileInfo);
    options.setStartPageNumber(3);
    options.setEndPageNumber(7);
    options.setMode(ModeEnum.PAGES);
    options.setOutputPath("output/split-by-page-range");

    // Execute split operation
    SplitRequest request = new SplitRequest(options);
    SplitResult response = apiInstance.split(request);

    System.out.println("Number of created documents: " + response.getDocuments().size());
    for(DocumentResult doc : response.getDocuments()) {
        System.out.println("Document path: " + doc.getPath());
    }
} catch (ApiException e) {
    System.err.println("Exception while calling API:");
    e.printStackTrace();
}
```

### Python Example: Splitting to Single Pages with Filtering (Odd Pages)

```python
# For complete examples and data files, please go to https://github.com/groupdocs-merger_cloud-cloud/groupdocs-merger_cloud-cloud-python-samples
from groupdocs_merger_cloud import *
import groupdocs_merger_cloud

client_id = "YOUR_CLIENT_ID" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

# Create instance of the API
document_api = groupdocs_merger_cloud.DocumentApi.from_keys(client_id, client_secret)

# Configure file info
file_info = groupdocs_merger_cloud.FileInfo("WordProcessing/sample-10-pages.docx")

# Configure split options with page range and odd filter
options = groupdocs_merger_cloud.SplitOptions()
options.file_info = file_info
options.start_page_number = 3
options.end_page_number = 7
options.range_mode = "Odd"  # Use "Odd" for odd pages, "Even" for even pages
options.mode = "Pages"
options.output_path = "Output/split-filtered-pages"

# Execute split operation
result = document_api.split(groupdocs_merger_cloud.SplitRequest(options))

print(f"Number of created documents: {len(result.documents)}")
for doc in result.documents:
    print(f"Document path: {doc.path}")
```

### C# Example: Splitting to Multi-Page Documents

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new DocumentApi(configuration);

try
{
    // Configure split options for multi-page documents
    var options = new SplitOptions
    {
        FileInfo = new FileInfo
        {
            FilePath = "WordProcessing/sample-10-pages.docx"
        },
        Pages = new List<int> { 3, 6, 8 },
        Mode = SplitOptions.ModeEnum.Intervals,  // Use Intervals mode for multi-page splitting
        OutputPath = "Output/split-to-multipage"
    };

    // Execute split operation
    var request = new SplitRequest(options);
    var response = apiInstance.Split(request);

    Console.WriteLine("Number of created documents: " + response.Documents.Count);
    foreach (var doc in response.Documents)
    {
        Console.WriteLine("Document path: " + doc.Path);
    }
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling API: " + e.Message);
}
```

## Understanding Document Splitting Options

GroupDocs.Merger Cloud provides versatile splitting capabilities through several key parameters:

### Pages Collection vs. Page Range

You have two ways to specify which pages to extract:

- Pages Collection: Provide an array of specific page numbers to extract.
  ```json
  "Pages": [3, 6, 8]
  ```

- Page Range: Define a contiguous range with start and end page numbers.
  ```json
  "StartPageNumber": 3,
  "EndPageNumber": 7
  ```

### Splitting Modes

The `Mode` parameter determines how the document will be split:

- Pages Mode: Creates separate single-page documents for each specified page.
  ```json
  "Mode": "Pages"
  ```

- Intervals Mode: Creates multi-page documents using the specified pages as breakpoints.
  ```json
  "Mode": "Intervals"
  ```

When using Intervals mode with Pages [3, 6, 8], you will get:
- Document 1: Pages 1-2
- Document 2: Pages 3-5
- Document 3: Pages 6-7
- Document 4: Pages 8-10

### Range Filtering

The `RangeMode` parameter allows you to filter pages when using page ranges:

- All: Include all pages in the range (default)
- Odd: Include only odd-numbered pages in the range
- Even: Include only even-numbered pages in the range

```json
"RangeMode": "Odd"
```

## Common Issues and Troubleshooting

1. Authentication Error: If you receive a 401 Unauthorized error, make sure your Client ID and Client Secret are correct and that your JWT token hasn't expired.

2. File Not Found: Verify that the file path is correct and that the document exists in your cloud storage.

3. Invalid Page Numbers: If you specify page numbers that don't exist in the document, you'll receive an error. Always ensure that page numbers are within the valid range of your document.

4. Output Path Issues: Make sure the output path is valid. The API will create any necessary directories.

5. File Format Support: Verify that your document format is supported for splitting operations. Most common formats (PDF, DOCX, PPTX, etc.) are supported.

## What You've Learned

In this tutorial, you've learned how to:
- Split a document into multiple single-page documents
- Extract specific pages from a document
- Create page ranges with filtering options
- Split a document into multi-page documents at specified breakpoints
- Implement document splitting in different programming languages
- Handle the API response to access the extracted documents

## Further Practice

To reinforce your learning, try these exercises:
1. Split a PDF document with more than 20 pages into chapters at pages 5, 10, 15, and 20
2. Extract only even pages from a specific range of a document
3. Create a custom workflow that first splits a document and then joins specific parts back together
4. Split a password-protected document

## Next Steps

Ready to learn more advanced document operations? Check out these related tutorials:
- [Tutorial: How to Join Multiple Documents](/document-management/join-multiple-documents/)
- [Tutorial: How to Generate Document Page Previews](/document-management/generate-document-pages-preview/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [Live Demo](https://products.groupdocs.app/merger/family)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.merger-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to share your feedback or ask questions on our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!