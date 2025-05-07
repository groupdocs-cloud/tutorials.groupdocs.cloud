---
title: How to Rotate Pages in Documents Tutorial
weight: 5
url: /page-processes/rotate-pages/
description: Learn how to change page rotation angles to 90, 180, or 270 degrees for specific pages in your documents using GroupDocs.Merger Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Rotate Pages in Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Rotate specific pages in a document to 90, 180, or 270 degrees
- Rotate pages using either exact page numbers or page ranges
- Filter page ranges to rotate only even or odd pages
- Work with both unprotected and password-protected documents
- Implement page rotation in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret (available in the [API dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your cloud storage
4. The appropriate SDK installed for your language of choice

## Practical Scenario

Consider a scenario where you have a PDF document containing scanned pages, but some pages (like pages 2 and 4) were scanned in the wrong orientation. Rather than rescanning those pages, you can simply rotate them to the correct orientation using the GroupDocs.Merger Cloud API. This tutorial will teach you how to accomplish this task efficiently.

## Understanding Rotation Angles

Before implementation, let's understand the available rotation options:
- Rotate90: Rotates the page 90 degrees clockwise
- Rotate180: Rotates the page 180 degrees (upside down)
- Rotate270: Rotates the page 270 degrees clockwise (or 90 degrees counterclockwise)

## Method 1: Rotate Pages by Exact Page Numbers

This approach allows you to specify exactly which pages should be rotated in the document.

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

### Step 2: Define Your Page Rotation Request

Create a JSON request that specifies:
- The source document path
- The exact page numbers to rotate
- The rotation angle (Mode)
- The destination path for the new document

### Step 3: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/rotate" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 'FilePath': '/Pdf/ten-pages.pdf'},
     'Pages':  [ 2, 4 ],
     'Mode': 'Rotate90',
     'OutputPath': 'output/rotate-pages.pdf'
 }"
```

#### Try it yourself!

Replace the placeholder values with your actual Client ID, Client Secret, and file paths, then run the command. The API will return a JSON response with the path to your newly created document, which will have pages 2 and 4 rotated 90 degrees clockwise.

### Step 4: Verify the Results

Download the resulting document from your storage and open it. You should see that pages 2 and 4 have been rotated 90 degrees clockwise, while all other pages maintain their original orientation.

## Method 2: Rotate Pages by Page Range

This approach allows you to rotate pages based on a range and filter them by even or odd page numbers.

### Step 1: Define Your Range Rotation Request

Create a JSON request that specifies:
- The source document path
- The start and end page numbers of your range
- The range mode (All, EvenPages, or OddPages)
- The rotation angle (Mode)
- The destination path for the new document

### Step 2: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/rotate" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 'FilePath': '/Pdf/ten-pages.pdf'},
     'StartPageNumber': 1,
     'EndPageNumber': 5,
     'RangeMode': 'OddPages',
     'Mode': 'Rotate180',
     'OutputPath': 'output/rotate-odd-pages.pdf'
 }"
```

#### Try it yourself!

Run this command with your own JWT token. The resulting document will have all odd-numbered pages (1, 3, 5) within the range 1-5 rotated 180 degrees, while all other pages maintain their original orientation.

## Working with Password-Protected Documents

If your document is password-protected, you need to include the password in your request:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/rotate" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 
         'FilePath': '/Pdf/protected-document.pdf',
         'Password': 'your_password'
     },
     'Pages':  [ 2, 4 ],
     'Mode': 'Rotate90',
     'OutputPath': 'output/rotate-pages-protected.pdf'
 }"
```

## SDK Implementation Examples

### C# Example

```csharp
// Rotate Pages using C# SDK
string MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new PagesApi(configuration);

var options = new RotateOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "Pdf/ten-pages.pdf"
    },
    Pages = new List<int?> { 2, 4 },
    Mode = RotateMode.Rotate90,
    OutputPath = "output/rotate-pages.pdf"
};

var response = apiInstance.Rotate(new RotateRequest(options));
Console.WriteLine("Output file path: " + response.Path);
```

### Java Example

```java
// Rotate Pages using Java SDK
String MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
PagesApi apiInstance = new PagesApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("Pdf/ten-pages.pdf");

RotateOptions options = new RotateOptions();
options.setFileInfo(fileInfo);
options.setPages(Arrays.asList(2, 4));
options.setMode(RotateMode.ROTATE90);
options.setOutputPath("output/rotate-pages.pdf");

RotateRequest request = new RotateRequest(options);
DocumentResult response = apiInstance.rotate(request);
System.out.println("Output file path: " + response.getPath());
```

### Python Example

```python
# Rotate Pages using Python SDK
import groupdocs_merger_cloud

# Get your app_sid and app_key at https://dashboard.groupdocs.cloud
my_client_id = ""
my_client_secret = ""

# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(my_client_id, my_client_secret)
api_instance = groupdocs_merger_cloud.PagesApi(configuration)

file_info = groupdocs_merger_cloud.FileInfo()
file_info.file_path = "Pdf/ten-pages.pdf"

options = groupdocs_merger_cloud.RotateOptions()
options.file_info = file_info
options.pages = [2, 4]
options.mode = "Rotate90"
options.output_path = "output/rotate-pages.pdf"

request = groupdocs_merger_cloud.RotateRequest(options)
response = api_instance.rotate(request)
print("Output file path: " + response.path)
```

## Troubleshooting Tips

- Error 401: If you receive an authentication error, make sure your JWT token is valid and hasn't expired.
- Error 400: Verify that the file path is correct and the file exists in your storage.
- Invalid Page Numbers: Ensure that you've specified valid page numbers that exist in your source document.
- Invalid Rotation Mode: Make sure you're using one of the supported rotation modes: Rotate90, Rotate180, or Rotate270.
- Password Issues: If working with protected documents, check that the provided password is correct.
- File Format Limitations: Some document formats may have limitations regarding page rotation. PDF files typically work best for rotation operations.

## What You've Learned

In this tutorial, you've learned how to:
- Rotate specific pages in a document to different angles
- Use both exact page numbers and page ranges for rotation
- Filter page ranges to rotate only even or odd pages
- Handle password-protected documents
- Implement page rotation functionality using various programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Rotate all pages in a document by 180 degrees
2. Create a document where alternating pages have different rotations
3. Combine page rotation with other operations like page extraction or moving

## Next Tutorial

Ready to learn more? Continue to the next tutorial: [How to Change Page Orientation](/page-processes/change-page-orientation/) to learn how to switch between portrait and landscape orientations for specific pages in your documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

If you have any questions about this tutorial, please let us know in the comments below or through our [support forum](https://forum.groupdocs.cloud/c/merger/18/)!
