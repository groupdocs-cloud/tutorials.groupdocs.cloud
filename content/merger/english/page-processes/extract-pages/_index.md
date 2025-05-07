---
title: How to Extract Pages from Documents Tutorial
weight: 1
url: /page-processes/extract-pages/
description: Learn how to extract specific pages from documents to create new files using GroupDocs.Merger Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Extract Pages from Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Extract specific pages from a document by providing exact page numbers
- Extract pages using a page range with options for even/odd pages
- Work with both unprotected and password-protected documents
- Implement page extraction in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret (available in the [API dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your cloud storage
4. The appropriate SDK installed for your language of choice

## Practical Scenario

Imagine you have a 10-page report and need to extract pages 2, 4, and 7 to create a summary document. Alternatively, you might need to extract all even pages from pages 1-10 for a two-sided printing job. This tutorial will teach you both approaches.

## Method 1: Extract Pages by Exact Page Numbers

This approach allows you to specify exactly which pages you want to extract. This is useful when you need specific, non-sequential pages from your document.

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

### Step 2: Define Your Extraction Request

Now, create a JSON request that specifies:
- The source document path
- The exact page numbers to extract
- The destination path for the new document

### Step 3: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/extract" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 'FilePath': '/WordProcessing/sample-10-pages.docx'},
     'Pages':  [ 2, 4, 7 ],
     'OutputPath': 'output/extract-pages-by-numbers.docx'
 }"
```

#### Try it yourself!

Replace the placeholder values with your actual Client ID, Client Secret, and file paths, then run the command. The API will return a JSON response with the path to your newly created document containing only pages 2, 4, and 7.

### Step 4: Verify the Results

Download the resulting document from your storage and open it. You should see a document with only the three pages you specified.

## Method 2: Extract Pages by Page Range

This approach allows you to extract pages based on a range and filter them by even or odd page numbers.

### Step 1: Define Your Range Extraction Request

Create a JSON request that specifies:
- The source document path
- The start and end page numbers of your range
- The range mode (All, EvenPages, or OddPages)
- The destination path for the new document

### Step 2: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/extract" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 'FilePath': '/WordProcessing/sample-10-pages.docx'},
     'StartPageNumber': 1,
     'EndPageNumber': 10,
     'RangeMode': 'EvenPages',
     'OutputPath': 'output/extract-pages-by-range.docx'
 }"
```

#### Try it yourself!

Run this command with your own JWT token. The resulting document will contain only the even-numbered pages (2, 4, 6, 8, 10) from your source document.

## Working with Password-Protected Documents

If your document is password-protected, you need to include the password in your request:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/extract" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 
         'FilePath': '/WordProcessing/protected-document.docx',
         'Password': 'your_password'
     },
     'Pages':  [ 2, 4, 7 ],
     'OutputPath': 'output/extract-pages-protected.docx'
 }"
```

## SDK Implementation Examples

### C# Example

```csharp
// Extract Pages using C# SDK
string MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new PagesApi(configuration);

var options = new ExtractOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "WordProcessing/sample-10-pages.docx"
    },
    Pages = new List<int?> { 2, 4, 7 },
    OutputPath = "output/extract-pages-by-numbers.docx"
};

var response = apiInstance.Extract(new ExtractRequest(options));
Console.WriteLine("Output file path: " + response.Path);
```

### Java Example

```java
// Extract Pages using Java SDK
String MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
PagesApi apiInstance = new PagesApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("WordProcessing/sample-10-pages.docx");

ExtractOptions options = new ExtractOptions();
options.setFileInfo(fileInfo);
options.setPages(Arrays.asList(2, 4, 7));
options.setOutputPath("output/extract-pages-by-numbers.docx");

ExtractRequest request = new ExtractRequest(options);
DocumentResult response = apiInstance.extract(request);
System.out.println("Output file path: " + response.getPath());
```

### Python Example

```python
# Extract Pages using Python SDK
import groupdocs_merger_cloud

# Get your app_sid and app_key at https://dashboard.groupdocs.cloud
my_client_id = ""
my_client_secret = ""

# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(my_client_id, my_client_secret)
api_instance = groupdocs_merger_cloud.PagesApi(configuration)

file_info = groupdocs_merger_cloud.FileInfo()
file_info.file_path = "WordProcessing/sample-10-pages.docx"

options = groupdocs_merger_cloud.ExtractOptions()
options.file_info = file_info
options.pages = [2, 4, 7]
options.output_path = "output/extract-pages-by-numbers.docx"

request = groupdocs_merger_cloud.ExtractRequest(options)
response = api_instance.extract(request)
print("Output file path: " + response.path)
```

## Troubleshooting Tips

- Error 401: If you receive an authentication error, make sure your JWT token is valid and hasn't expired.
- Error 400: Verify that the file path is correct and the file exists in your storage.
- Empty Result Document: Ensure you've specified valid page numbers that exist in your source document.
- Password Issues: If working with protected documents, check that the provided password is correct.

## What You've Learned

In this tutorial, you've learned how to:
- Extract specific pages from a document by providing exact page numbers
- Extract pages using a range with filtering for even or odd pages
- Handle password-protected documents
- Implement page extraction functionality using various programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Extract the first and last page from a document
2. Extract all odd pages from a 20-page document
3. Combine both methods to extract pages 1, 3, and all even pages between 10-20

## Next Tutorial

Ready to learn more? Continue to the next tutorial: [How to Remove Pages from Documents](/page-processes/remove-pages/) to learn how to permanently delete pages from your documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

If you have any questions about this tutorial, please let us know in the comments below or through our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!
