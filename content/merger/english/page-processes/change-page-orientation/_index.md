---
title: How to Change Page Orientation in Documents Tutorial
weight: 6
url: /page-processes/change-page-orientation/
description: Learn how to switch between portrait and landscape orientations for specific pages in your documents using GroupDocs.Merger Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Change Page Orientation in Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Change page orientation between portrait and landscape for specific pages
- Apply orientation changes using either exact page numbers or page ranges
- Filter page ranges to change orientation for only even or odd pages
- Work with both unprotected and password-protected documents
- Implement page orientation changes in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret (available in the [API dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your cloud storage
4. The appropriate SDK installed for your language of choice

## Practical Scenario

Imagine you're preparing a report that contains both text and wide data tables. Most pages should be in portrait orientation for the text sections, but specific pages containing tables (like pages 2 and 4) would be better presented in landscape orientation to accommodate the wide data. This tutorial will teach you how to create such a mixed-orientation document efficiently.

## Understanding Page Orientation Options

Before implementation, let's understand the available orientation options:
- Portrait: The page is taller than it is wide (standard orientation)
- Landscape: The page is wider than it is tall (horizontal orientation)

## Method 1: Change Orientation for Specific Pages

This approach allows you to specify exactly which pages should have their orientation changed.

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

### Step 2: Define Your Page Orientation Request

Create a JSON request that specifies:
- The source document path
- The exact page numbers to change orientation
- The orientation mode (Portrait or Landscape)
- The destination path for the new document

### Step 3: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/orientation" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 'FilePath': '/WordProcessing/four-pages.docx'},
     'Pages':  [ 2, 4 ],
     'Mode': 'Landscape',
     'OutputPath': 'output/change-page-orientation.docx'
 }"
```

#### Try it yourself!

Replace the placeholder values with your actual Client ID, Client Secret, and file paths, then run the command. The API will return a JSON response with the path to your newly created document, which will have pages 2 and 4 changed to landscape orientation.

### Step 4: Verify the Results

Download the resulting document from your storage and open it. You should see that pages 2 and 4 are now in landscape orientation, while pages 1 and 3 maintain their original portrait orientation.

## Method 2: Change Orientation by Page Range

This approach allows you to change orientation for pages based on a range and filter them by even or odd page numbers.

### Step 1: Define Your Range Orientation Request

Create a JSON request that specifies:
- The source document path
- The start and end page numbers of your range
- The range mode (All, EvenPages, or OddPages)
- The orientation mode (Portrait or Landscape)
- The destination path for the new document

### Step 2: Execute the API Request

#### Using cURL

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/orientation" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 'FilePath': '/WordProcessing/ten-pages.docx'},
     'StartPageNumber': 1,
     'EndPageNumber': 10,
     'RangeMode': 'EvenPages',
     'Mode': 'Landscape',
     'OutputPath': 'output/change-even-pages-orientation.docx'
 }"
```

#### Try it yourself!

Run this command with your own JWT token. The resulting document will have all even-numbered pages (2, 4, 6, 8, 10) changed to landscape orientation, while all odd-numbered pages maintain their original portrait orientation.

## Working with Password-Protected Documents

If your document is password-protected, you need to include the password in your request:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/pages/orientation" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
     'FileInfo': { 
         'FilePath': '/WordProcessing/protected-document.docx',
         'Password': 'your_password'
     },
     'Pages':  [ 2, 4 ],
     'Mode': 'Landscape',
     'OutputPath': 'output/change-orientation-protected.docx'
 }"
```

## SDK Implementation Examples

### C# Example

```csharp
// Change Page Orientation using C# SDK
string MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyClientId, MyClientSecret);
  
var apiInstance = new PagesApi(configuration);

var options = new OrientationOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "WordProcessing/four-pages.docx"
    },
    Pages = new List<int?> { 2, 4 },
    Mode = OrientationMode.Landscape,
    OutputPath = "output/change-page-orientation.docx"
};

var response = apiInstance.Orientation(new OrientationRequest(options));
Console.WriteLine("Output file path: " + response.Path);
```

### Java Example

```java
// Change Page Orientation using Java SDK
String MyClientSecret = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = ""; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
PagesApi apiInstance = new PagesApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("WordProcessing/four-pages.docx");

OrientationOptions options = new OrientationOptions();
options.setFileInfo(fileInfo);
options.setPages(Arrays.asList(2, 4));
options.setMode(OrientationMode.LANDSCAPE);
options.setOutputPath("output/change-page-orientation.docx");

OrientationRequest request = new OrientationRequest(options);
DocumentResult response = apiInstance.orientation(request);
System.out.println("Output file path: " + response.getPath());
```

### Python Example

```python
# Change Page Orientation using Python SDK
import groupdocs_merger_cloud

# Get your app_sid and app_key at https://dashboard.groupdocs.cloud
my_client_id = ""
my_client_secret = ""

# Create instance of the API
configuration = groupdocs_merger_cloud.Configuration(my_client_id, my_client_secret)
api_instance = groupdocs_merger_cloud.PagesApi(configuration)

file_info = groupdocs_merger_cloud.FileInfo()
file_info.file_path = "WordProcessing/four-pages.docx"

options = groupdocs_merger_cloud.OrientationOptions()
options.file_info = file_info
options.pages = [2, 4]
options.mode = "Landscape"
options.output_path = "output/change-page-orientation.docx"

request = groupdocs_merger_cloud.OrientationRequest(options)
response = api_instance.orientation(request)
print("Output file path: " + response.path)
```

## Troubleshooting Tips

- Error 401: If you receive an authentication error, make sure your JWT token is valid and hasn't expired.
- Error 400: Verify that the file path is correct and the file exists in your storage.
- Invalid Page Numbers: Ensure that you've specified valid page numbers that exist in your source document.
- Invalid Orientation Mode: Make sure you're using one of the supported orientation modes: Portrait or Landscape.
- Password Issues: If working with protected documents, check that the provided password is correct.
- File Format Limitations: Some document formats may have limitations regarding page orientation changes. Word documents (DOCX, DOC) and PDF files typically work best for orientation operations.

## What You've Learned

In this tutorial, you've learned how to:
- Change page orientation between portrait and landscape for specific pages
- Apply orientation changes using both exact page numbers and page ranges
- Filter page ranges to change orientation for only even or odd pages
- Handle password-protected documents
- Implement page orientation changes using various programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Create a document where all odd pages are in portrait and all even pages are in landscape
2. Change the orientation of only the first and last pages of a document
3. Combine page orientation changes with other operations like rotation or page extraction

## Next Steps

Congratulations! You've completed the Document Page Processes Tutorial series. To continue expanding your knowledge of the GroupDocs.Merger Cloud API, explore other tutorial categories or try combining multiple operations to create more complex document processing workflows.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

If you have any questions about this tutorial, please let us know in the comments below or through our [support forum](https://forum.groupdocs.cloud/c/merger/18/)! 
