---
title: How to Remove Pages from Documents Tutorial
weight: 2
url: /page-processes/remove-pages/
description: Learn how to permanently remove specific pages from documents using GroupDocs.Merger Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Remove Pages from Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Permanently remove specific pages from a document
- Remove pages using either exact page numbers or page ranges
- Filter page ranges to remove only even or odd pages
- Handle password-protected documents
- Implement page removal in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret (available in the [API dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your cloud storage
4. The appropriate SDK installed for your language of choice

## Practical Scenario

Consider a scenario where you have a 20-page contract document, but pages 5 and 12 contain outdated clauses that need to be removed. Alternatively, you might need to remove all even-numbered pages from a document that contains duplicate information on alternating pages. This tutorial will teach you both approaches.

## Method 1: Remove Pages by Exact Page Numbers

This approach allows you to specify exactly which pages should be removed from the document.

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

### Step 2: Define Your Page Removal Request

Create a JSON request that specifies:
- The source document path
- The exact page numbers to remove
- The destination path for the new document

### Step 3: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/remove" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 'FilePath': '/WordProcessing/four-pages.docx'},
     'Pages':  [ 2, 4 ],
     'OutputPath': 'output/remove-pages.docx'
 }"
```

#### Try it yourself!

Replace the placeholder values with your actual Client ID, Client Secret, and file paths, then run the command. The API will return a JSON response with the path to your newly created document, which will contain all pages except pages 2 and 4.

### Step 4: Verify the Results

Download the resulting document from your storage and open it. You should see that pages 2 and 4 have been removed, and the document now contains only pages 1 and 3 (renumbered as pages 1 and 2).

## Method 2: Remove Pages by Page Range

This approach allows you to remove pages based on a range and filter them by even or odd page numbers.

### Step 1: Define Your Range Removal Request

Create a JSON request that specifies:
- The source document path
- The start and end page numbers of your range
- The range mode (All, EvenPages, or OddPages)
- The destination path for the new document

### Step 2: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/remove" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 'FilePath': '/WordProcessing/sample-20-pages.docx'},
     'StartPageNumber': 1,
     'EndPageNumber': 10,
     'RangeMode': 'EvenPages',
     'OutputPath': 'output/remove-even-pages.docx'
 }"
```

#### Try it yourself!

Run this command with your own JWT token. The resulting document will have all even-numbered pages (2, 4, 6, 8, 10) removed from the range 1-10, keeping only the odd-numbered pages in that range, plus all pages after page 10.

## Working with Password-Protected Documents

If your document is password-protected, you need to include the password in your request:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/remove" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 
         'FilePath': '/WordProcessing/protected-document.docx',
         'Password': 'your_password'
     },
     'Pages':  [ 3, 5 ],
     'OutputPath': 'output/remove-pages-protected.docx'
 }"
```

## SDK Implementation Examples

### C# Example

```csharp
// Remove Pages using C# SDK
string MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new PagesApi(configuration);

var options = new RemoveOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "WordProcessing/four-pages.docx"
    },
    Pages = new List<int?> { 2, 4 },
    OutputPath = "output/remove-pages.docx"
};

var response = apiInstance.Remove(new RemoveRequest(options));
Console.WriteLine("Output file path: " + response.Path);
```

### Java Example

```java
// Remove Pages using Java SDK
String MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
PagesApi apiInstance = new PagesApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("WordProcessing/four-pages.docx");

RemoveOptions options = new RemoveOptions();
options.setFileInfo(fileInfo);
options.setPages(Arrays.asList(2, 4));
options.setOutputPath("output/remove-pages.docx");

RemoveRequest request = new RemoveRequest(options);
DocumentResult response = apiInstance.remove(request);
System.out.println("Output file path: " + response.getPath());
```

### Python Example

```python
# Remove Pages using Python SDK
import groupdocs_merger_cloud

# Get your app_sid and app_key at https://dashboard.groupdocs.cloud
my_client_id = ""
my_client_secret = ""

# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(my_client_id, my_client_secret)
api_instance = groupdocs_merger_cloud.PagesApi(configuration)

file_info = groupdocs_merger_cloud.FileInfo()
file_info.file_path = "WordProcessing/four-pages.docx"

options = groupdocs_merger_cloud.RemoveOptions()
options.file_info = file_info
options.pages = [2, 4]
options.output_path = "output/remove-pages.docx"

request = groupdocs_merger_cloud.RemoveRequest(options)
response = api_instance.remove(request)
print("Output file path: " + response.path)
```

## Troubleshooting Tips

- Error 401: If you receive an authentication error, make sure your JWT token is valid and hasn't expired.
- Error 400: Verify that the file path is correct and the file exists in your storage.
- No Changes in Document: If all pages remain in the document, check that you've specified valid page numbers that exist in your source document.
- Password Issues: If working with protected documents, check that the provided password is correct.
- Empty Document: If you try to remove all pages from a document, the API may return an error. Always leave at least one page in the document.

## What You've Learned

In this tutorial, you've learned how to:
- Remove specific pages from a document by providing exact page numbers
- Remove pages using a range with filtering for even or odd pages
- Handle password-protected documents
- Implement page removal functionality using various programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Remove the first page from a document
2. Remove all odd pages from a specific range in a document
3. Combine both methods to remove pages 1, 3, and all even pages between 10-20

## Next Tutorial

Ready to learn more? Continue to the next tutorial: [How to Swap Pages in Documents](/page-processes/swap-pages/) to learn how to exchange the positions of pages within your documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

If you have any questions about this tutorial, please let us know in the comments below or through our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!
