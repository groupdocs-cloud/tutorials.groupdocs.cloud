---
title: How to Swap Pages in Documents Tutorial
weight: 3
url: /page-processes/swap-pages/
description: Learn how to exchange the positions of two pages within a document using GroupDocs.Merger Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Swap Pages in Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Exchange the positions of two pages within a document
- Work with both unprotected and password-protected documents
- Implement page swapping in multiple programming languages
- Verify the results of page swapping operations

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret (available in the [API dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your cloud storage
4. The appropriate SDK installed for your language of choice

## Practical Scenario

Imagine you've created a document but later discover that two pages are in the wrong order. For example, in a four-page document, pages 2 and 4 need to be swapped because they contain related information that should be presented in a different sequence. This tutorial will teach you how to quickly fix this issue without having to recreate the document.

## Step-by-Step Guide to Swapping Pages

### Step 1: Obtain Your JWT Token

Before making any API request, you need to authenticate with the GroupDocs.Merger Cloud API:

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

The response will include your JWT token that you'll use in subsequent requests.

### Step 2: Define Your Page Swap Request

Create a JSON request that specifies:
- The source document path
- The first page number to swap
- The second page number to swap
- The destination path for the new document

### Step 3: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/swap" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': { 'FilePath': 'words/four-pages.docx'},
  'FirstPageNumber': 2,
  'SecondPageNumber': 4,
  'OutputPath': 'output/swap-pages.docx'
 }"
```

#### Try it yourself!

Replace the placeholder values with your actual Client ID, Client Secret, and file paths, then run the command. The API will return a JSON response with the path to your newly created document, which will have pages 2 and 4 exchanged.

### Step 4: Verify the Results

Download the resulting document from your storage and open it. You should see that the content that was previously on page 2 is now on page 4, and vice versa. The content of pages 1 and 3 remains unchanged.

## Working with Password-Protected Documents

If your document is password-protected, you need to include the password in your request:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/swap" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': { 
      'FilePath': 'words/protected-document.docx',
      'Password': 'your_password'
  },
  'FirstPageNumber': 2,
  'SecondPageNumber': 4,
  'OutputPath': 'output/swap-pages-protected.docx'
 }"
```

## SDK Implementation Examples

### C# Example

```csharp
// Swap Pages using C# SDK
string MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new PagesApi(configuration);

var options = new SwapOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "words/four-pages.docx"
    },
    FirstPageNumber = 2,
    SecondPageNumber = 4,
    OutputPath = "output/swap-pages.docx"
};

var response = apiInstance.Swap(new SwapRequest(options));
Console.WriteLine("Output file path: " + response.Path);
```

### Java Example

```java
// Swap Pages using Java SDK
String MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
PagesApi apiInstance = new PagesApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("words/four-pages.docx");

SwapOptions options = new SwapOptions();
options.setFileInfo(fileInfo);
options.setFirstPageNumber(2);
options.setSecondPageNumber(4);
options.setOutputPath("output/swap-pages.docx");

SwapRequest request = new SwapRequest(options);
DocumentResult response = apiInstance.swap(request);
System.out.println("Output file path: " + response.getPath());
```

### Python Example

```python
# Swap Pages using Python SDK
import groupdocs_merger_cloud

# Get your app_sid and app_key at https://dashboard.groupdocs.cloud
my_client_id = ""
my_client_secret = ""

# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(my_client_id, my_client_secret)
api_instance = groupdocs_merger_cloud.PagesApi(configuration)

file_info = groupdocs_merger_cloud.FileInfo()
file_info.file_path = "words/four-pages.docx"

options = groupdocs_merger_cloud.SwapOptions()
options.file_info = file_info
options.first_page_number = 2
options.second_page_number = 4
options.output_path = "output/swap-pages.docx"

request = groupdocs_merger_cloud.SwapRequest(options)
response = api_instance.swap(request)
print("Output file path: " + response.path)
```
## Troubleshooting Tips

- Error 401: If you receive an authentication error, make sure your JWT token is valid and hasn't expired.
- Error 400: Verify that the file path is correct and the file exists in your storage.
- Invalid Page Numbers: Ensure that both page numbers exist in the source document. If you try to swap with a non-existent page, the API will return an error.
- Password Issues: If working with protected documents, check that the provided password is correct.
- Same Page Numbers: If you specify the same page number for both first and second page, the API may return an error or create an identical document with no changes.

## What You've Learned

In this tutorial, you've learned how to:
- Exchange the positions of two pages within a document
- Work with both regular and password-protected documents
- Implement page swapping functionality using various programming languages
- Verify the successful completion of the page swap operation

## Further Practice

To reinforce your learning, try these exercises:
1. Swap the first and last pages of a document
2. Swap multiple pairs of pages in sequence (e.g., swap pages 1-2, then swap pages 3-4 in the resulting document)
3. Create a program that reverses the order of all pages in a document by performing multiple swap operations

## Next Tutorial

Ready to learn more? Continue to the next tutorial: [How to Move Pages in Documents](/page-processes/move-pages) to learn how to change a page's position by moving it to a new location within your document.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

If you have any questions about this tutorial, please let us know in the comments below or through our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!