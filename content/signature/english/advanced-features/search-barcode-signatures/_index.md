---
title: "Search for Barcode Signatures"
id: "tutorial-search-for-barcode-signatures"
url: /advanced-features/search-barcode-signatures/
productName: "GroupDocs.Signature Cloud"
description: "Learn how to search for and extract information from barcode signatures in documents using GroupDocs.Signature Cloud API"
keywords: "barcode search tutorial, GroupDocs.Signature Cloud, find barcode, extract barcode data, document signatures tutorial"
toc: true
---

# Tutorial: How to Search for Barcode Signatures

## Introduction

In this tutorial, you'll learn how to search for barcode signatures in documents using GroupDocs.Signature Cloud API. Finding and extracting information from barcodes is crucial for document verification, data extraction, and automated processing workflows.

**Learning Objectives:**
- Understand the barcode searching capabilities of GroupDocs.Signature Cloud
- Learn to configure search options for different barcode types
- Implement barcode search functionality in your applications
- Extract and process data from discovered barcodes

## Prerequisites

Before starting this tutorial, make sure you have:

1. A [GroupDocs.Signature Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Access to the [API Reference](https://reference.groupdocs.cloud/signature/)
4. Development environment with the necessary SDK installed
5. A document with barcode signatures that you want to search

## Why Search for Barcode Signatures?

Searching for barcodes in documents enables several valuable use cases:

- **Automated Document Verification**: Quickly validate if specific barcodes exist in documents
- **Data Extraction**: Pull encoded information from barcodes for further processing
- **Document Classification**: Sort documents based on contained barcode types or values
- **Content Auditing**: Identify all barcodes present in a document repository

## Practical Scenario

Let's consider a practical application: An invoice processing system needs to extract tracking numbers from shipping documents. Each document contains barcode signatures encoding shipment IDs, and the system must identify and extract this information automatically.

## Step-by-Step Implementation

### Step 1: Upload a Document with Barcode Signatures to Cloud Storage

First, let's upload a document containing barcode signatures:

```javascript
// First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

// Upload document
curl -v "https://api.groupdocs.cloud/v2.0/signature/storage/file/signedBarcode_one-page.docx" \
-X PUT \
-H "Content-Type: application/octet-stream" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
--data-binary @/path/to/your/signedBarcode_one-page.docx
```

### Step 2: Configure Barcode Search Options

Now, let's configure options for searching barcode signatures:

1. Specify the signature type as "Barcode"
2. Optionally define the barcode type to search for
3. Set specific text to match in the barcode content
4. Configure page settings (search all pages or specific pages)

### Step 3: Execute the Barcode Search

With your document uploaded and search options configured, you can now search for barcodes:

```javascript
curl -v "https://api.groupdocs.cloud/v2.0/signature/search" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'signedBarcode_one-page.docx'
  },
  'Options': [
    {
      'AllPages': true,
      'SignatureType': 'Barcode',
      'BarcodeType': 'Code128',
      'Text': '123456789012',
      'MatchType': 'Contains'
    }
  ]
}"
```

### Step 4: Process the Search Results

The API will return information about all matching barcode signatures:

```javascript
{
  "fileInfo": {
    "filePath": "signedBarcode_one-page.docx",
    "storageName": null,
    "versionId": null,
    "password": null
  },
  "size": 1360015,
  "signatures": [
    {
      "barcodeType": "Code128",
      "text": "123456789012",
      "format": "Portable Network Graphic",
      "signatureType": "Barcode",
      "pageNumber": 1,
      "signatureId": "114bc076-f734-4edf-9ce4-2277725a6ea5",
      "isSignature": true,
      "createdOn": "2020-07-22T07:45:01.6812929+00:00",
      "modifiedOn": "2020-07-22T07:45:01.6812929+00:00",
      "top": 100,
      "left": 100,
      "width": 300,
      "height": 100
    }
  ]
}
```

## Try It Yourself

Now that you understand the process, try implementing barcode searches with different criteria:

1. Search for barcodes with different match types (Contains, StartsWith, EndsWith, Exact)
2. Look for various barcode types in the same document
3. Search for barcodes on specific pages using the Page parameter
4. Experiment with different text patterns to find partial matches

## SDK Examples

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-signature-cloud/groupdocs-signature-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new SignApi(configuration);

// Search options.
var options = new SearchBarcodeOptions
{
    SignatureType = SignatureTypeEnum.Barcode,
    // Specify search criteria
    MatchType = SearchBarcodeOptions.MatchTypeEnum.Contains,
    Text = "123456789012",
    BarcodeType = "Code128",
    // Specify which pages to search
    AllPages = true,
    // Alternatively, search specific pages
    // Page = 1,
    // PagesSetup = new PagesSetup
    // {
    //     EvenPages = false,
    //     FirstPage = true,
    //     LastPage = false,
    //     OddPages = false,
    //     PageNumbers = new List<int?> {1}
    // }
};

// Search settings.
var searchSettings = new SearchSettings
{
    FileInfo = new FileInfo
    {
        FilePath = "signedBarcode_one-page.docx"
    },
    Options = new List<SearchOptions> { options }
};

// Create request.
var request = new SearchSignaturesRequest(searchSettings);

// Call api method with request.
var response = apiInstance.SearchSignatures(request);

// Process results
Console.WriteLine($"Document: {response.fileInfo.filePath}");
Console.WriteLine($"Found signatures: {response.signatures.Count}");

foreach (var signature in response.signatures)
{
    Console.WriteLine($"Barcode type: {signature.barcodeType}");
    Console.WriteLine($"Text: {signature.text}");
    Console.WriteLine($"Page number: {signature.pageNumber}");
    Console.WriteLine($"Signature ID: {signature.signatureId}");
    Console.WriteLine($"Position: Top={signature.top}, Left={signature.left}");
    Console.WriteLine($"Size: Width={signature.width}, Height={signature.height}");
    Console.WriteLine("-----------------------------------");
}
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-signature-cloud/groupdocs-signature-cloud-java-samples
String MyClientSecret = "YOUR_CLIENT_SECRET"; 
String MyClientId = "YOUR_CLIENT_ID"; 

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
SignApi apiInstance = new SignApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("signedBarcode_one-page.docx");

// Configure barcode search options
SearchBarcodeOptions options = new SearchBarcodeOptions();
options.setSignatureType(SignatureTypeEnum.BARCODE);

// Set search criteria
options.setBarcodeType("Code128");
options.setText("123456789012");
options.setMatchType(MatchTypeEnum.CONTAINS);

// Set page options
options.setPage(1);
options.setAllPages(true);

// Configure search settings
SearchSettings searchSettings = new SearchSettings();
searchSettings.setFileInfo(fileInfo);
searchSettings.addOptionsItem(options);

// Create and execute the search request
SearchSignaturesRequest request = new SearchSignaturesRequest(searchSettings);
SearchResult response = apiInstance.searchSignatures(request);

// Process results
System.out.println("Document: " + response.getFileInfo().getFilePath());
System.out.println("Found signatures: " + response.getSignatures().size());

for (SignatureInfo signature : response.getSignatures()) {
    BarcodeSignature barcodeSignature = (BarcodeSignature) signature;
    System.out.println("Barcode type: " + barcodeSignature.getBarcodeType());
    System.out.println("Text: " + barcodeSignature.getText());
    System.out.println("Page number: " + barcodeSignature.getPageNumber());
    System.out.println("Signature ID: " + barcodeSignature.getSignatureId());
    System.out.println("Position: Top=" + barcodeSignature.getTop() + 
                      ", Left=" + barcodeSignature.getLeft());
    System.out.println("Size: Width=" + barcodeSignature.getWidth() + 
                      ", Height=" + barcodeSignature.getHeight());
    System.out.println("-----------------------------------");
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-signature-cloud/groupdocs-signature-cloud-python-samples
import groupdocs_signature_cloud

# Get your app_sid and app_key at https://dashboard.groupdocs.cloud
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create instance of the API
api = groupdocs_signature_cloud.SignApi.from_keys(client_id, client_secret)

# Document information
file_info = groupdocs_signature_cloud.FileInfo()
file_info.file_path = "signedBarcode_one-page.docx"

# Configure barcode search options
opts = groupdocs_signature_cloud.SearchBarcodeOptions()
opts.signature_type = 'Barcode'
opts.barcode_type = 'Code128'
opts.text = '123456789012'
opts.match_type = 'Contains'
opts.page = 1
opts.all_pages = True

# Configure search settings
settings = groupdocs_signature_cloud.SearchSettings()
settings.options = [opts]
settings.file_info = file_info

# Execute search request
request = groupdocs_signature_cloud.SearchSignaturesRequest(settings)
response = api.search_signatures(request)

# Process results
print(f"Document: {response.file_info.file_path}")
print(f"Found signatures: {len(response.signatures)}")

for signature in response.signatures:
    print(f"Barcode type: {signature.barcode_type}")
    print(f"Text: {signature.text}")
    print(f"Page number: {signature.page_number}")
    print(f"Signature ID: {signature.signature_id}")
    print(f"Position: Top={signature.top}, Left={signature.left}")
    print(f"Size: Width={signature.width}, Height={signature.height}")
    print("-----------------------------------")
```

## Advanced Search Options

### Match Types
GroupDocs.Signature Cloud provides several matching options for text search:

- **Contains**: Finds barcodes where the text contains the search string
- **StartsWith**: Matches barcodes where the text starts with the search string
- **EndsWith**: Matches barcodes where the text ends with the search string
- **Exact**: Requires an exact match of the entire barcode text

### Barcode Types
You can search for specific barcode types by setting the `BarcodeType` property:

- For 1D barcodes: Code128, Code39, EAN13, etc.
- For 2D barcodes (if they're stored as barcode signatures): DataMatrix, PDF417, etc.

### Page Selection Options
Control which pages are searched:

- `AllPages`: Search the entire document
- `Page`: Search a specific page number
- `PagesSetup`: Configure advanced page filtering:
  - `FirstPage`: Only search the first page
  - `LastPage`: Only search the last page
  - `OddPages`: Only search odd-numbered pages
  - `EvenPages`: Only search even-numbered pages
  - `PageNumbers`: Search specific page numbers in a list

## Troubleshooting

### Common Issues

**Issue**: No barcodes found despite being present in the document.  
**Solution**: Ensure the barcode type and text criteria match what's in the document. Try using broader search criteria first (e.g., `AllPages = true` and no text criteria) and then narrow down.

**Issue**: Barcode text not matching expected format.  
**Solution**: Barcode data may include hidden characters or different formatting. Try using the "Contains" match type instead of "Exact".

**Issue**: Search taking too long for large documents.  
**Solution**: Limit the search to specific pages if you know where the barcodes are likely to be located.

## What You've Learned

In this tutorial, you've learned:
- How to search for barcode signatures in documents using GroupDocs.Signature Cloud API
- Configuring search options for barcode types, text content, and page selection
- Implementing barcode search functionality using different programming languages
- How to process and extract information from the search results
- Troubleshooting common issues with barcode searching

## Further Practice

To reinforce your learning, try these exercises:

1. Create a document with multiple different barcode types and practice searching for each type
2. Implement a search that finds all barcodes and extracts their text into a report
3. Write a program that searches for barcodes in a batch of documents
4. Create a workflow that verifies if expected barcodes are present in a document

## Resources

- [Product Page](https://products.groupdocs.cloud/signature/)
- [Documentation](https://docs.groupdocs.cloud/signature/)
- [Live Demo](https://products.groupdocs.app/signature/family)
- [API Reference](https://reference.groupdocs.cloud/signature/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.signature-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/signature/13/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
