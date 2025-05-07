---
title: How to Move Pages in Documents Tutorial
url: /page-processes/move-pages/
weight: 4
description: Learn how to change a page's position by moving it to a new location within your document using GroupDocs.Merger Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Move Pages in Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Move a page from one position to another within a document
- Work with both unprotected and password-protected documents
- Implement page movement in multiple programming languages
- Understand how page renumbering works when moving pages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret (available in the [API dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your cloud storage
4. The appropriate SDK installed for your language of choice

## Practical Scenario

Imagine you have a document where page 1 contains introductory content that would be better positioned after the table of contents on page 2. Rather than recreating the document, you can simply move page 1 to position 2. This tutorial will teach you how to perform such page reordering efficiently.

## Understanding Page Movement Mechanics

Before diving into the implementation, it's important to understand how page movement works:

1. When you move a page, the content of that page is relocated to a new position
2. All other pages are shifted accordingly
3. Page numbers refer to the current order (page 1 is the first page, page 2 is the second, etc.)
4. After moving, pages are renumbered sequentially in the new order

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

### Step 2: Define Your Page Move Request

Create a JSON request that specifies:
- The source document path
- The page number to move
- The new page number (destination position)
- The destination path for the new document

### Step 3: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/move" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': { 'FilePath': 'words/four-pages.docx'},
  'PageNumber': 1,
  'NewPageNumber': 2,
  'OutputPath': 'output/move-page.docx'
 }"
```

#### Try it yourself!

Replace the placeholder values with your actual Client ID, Client Secret, and file paths, then run the command. The API will return a JSON response with the path to your newly created document, which will have page 1 moved to position 2.

### Step 4: Verify the Results

Download the resulting document from your storage and open it. You should see that:
- The content that was originally on page 1 is now on page 2
- The content that was originally on page 2 is now on page 1
- All other pages maintain their relative order, just shifted accordingly

## Working with Password-Protected Documents

If your document is password-protected, you need to include the password in your request:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/move" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': { 
      'FilePath': 'words/protected-document.docx',
      'Password': 'your_password'
  },
  'PageNumber': 1,
  'NewPageNumber': 2,
  'OutputPath': 'output/move-page-protected.docx'
 }"
```

## SDK Implementation Examples

### C# Example

```csharp
// Move Pages using C# SDK
string MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new PagesApi(configuration);

var options = new MoveOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "words/four-pages.docx"
    },
    PageNumber = 1,
    NewPageNumber = 2,
    OutputPath = "output/move-page.docx"
};

var response = apiInstance.Move(new MoveRequest(options));
Console.WriteLine("Output file path: " + response.Path);
```

### Java Example

```java
// Move Pages using Java SDK
String MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
PagesApi apiInstance = new PagesApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("words/four-pages.docx");

MoveOptions options = new MoveOptions();
options.setFileInfo(fileInfo);
options.setPageNumber(1);
options.setNewPageNumber(2);
options.setOutputPath("output/move-page.docx");

MoveRequest request = new MoveRequest(options);
DocumentResult response = apiInstance.move(request);
System.out.println("Output file path: " + response.getPath());
```

### Python Example

```python
# Move Pages using Python SDK
import groupdocs_merger_cloud

# Get your app_sid and app_key at https://dashboard.groupdocs.cloud
my_client_id = ""
my_client_secret = ""

# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(my_client_id, my_client_secret)
api_instance = groupdocs_merger_cloud.PagesApi(configuration)

file_info = groupdocs_merger_cloud.FileInfo()
file_info.file_path = "words/four-pages.docx"

options = groupdocs_merger_cloud.MoveOptions()
options.file_info = file_info
options.page_number = 1
options.new_page_number = 2
options.output_path = "output/move-page.docx"

request = groupdocs_merger_cloud.MoveRequest(options)
response = api_instance.move(request)
print("Output file path: " + response.path)
```

## Special Case: Moving to the End of the Document

If you want to move a page to the end of the document, you need to specify the total number of pages plus 1 as the new page number. For example, in a 4-page document, to move page 1 to the end:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/move" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': { 'FilePath': 'words/four-pages.docx'},
  'PageNumber': 1,
  'NewPageNumber': 5,
  'OutputPath': 'output/move-to-end.docx'
 }"
```

## Troubleshooting Tips

- Error 401: If you receive an authentication error, make sure your JWT token is valid and hasn't expired.
- Error 400: Verify that the file path is correct and the file exists in your storage.
- Invalid Page Numbers: Ensure that the page number to move exists in the source document and the new page number is valid (between 1 and total pages + 1).
- Same Page Number: If the page number and new page number are the same, the API will not make any changes to the document.
- Password Issues: If working with protected documents, check that the provided password is correct.

## What You've Learned

In this tutorial, you've learned how to:
- Move a page from one position to another within a document
- Work with both regular and password-protected documents
- Implement page movement functionality using various programming languages
- Handle special cases like moving a page to the end of a document

## Further Practice

To reinforce your learning, try these exercises:
1. Move the last page of a document to the first position
2. Move a page from the middle of the document to the end
3. Create a program that reverses the order of all pages in a document by performing multiple move operations

## Next Tutorial

Ready to learn more? Continue to the next tutorial: [How to Rotate Pages in Documents](/page-processes/rotate-pages/) to learn how to change page rotation angles for specific pages in your documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

If you have any questions about this tutorial, please let us know in the comments below or through our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!
