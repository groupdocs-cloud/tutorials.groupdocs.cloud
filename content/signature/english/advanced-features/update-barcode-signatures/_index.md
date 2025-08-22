---
title: "Updating Barcode Signatures"
id: "tutorial-update-barcode-signatures"
url: /advanced-features/update-barcode-signatures/
productName: "GroupDocs.Signature Cloud"
description: "Learn how to update barcode signatures in documents using GroupDocs.Signature Cloud API in this comprehensive step-by-step tutorial"
keywords: "update barcode tutorial, GroupDocs.Signature Cloud, modify barcode, document signatures tutorial, barcode API"
toc: true
---

# Tutorial: Updating Barcode Signatures

## Introduction

In this tutorial, you'll learn how to update existing barcode signatures in documents using GroupDocs.Signature Cloud API. The ability to modify barcode signatures after they've been placed in a document offers flexibility for adjusting position, size, and other properties without having to recreate the entire signature.

**Learning Objectives:**
- Understand when and why to update barcode signatures
- Learn to search for existing barcode signatures to update
- Implement barcode signature updates in different programming languages
- Master various modification options for barcode signatures

## Prerequisites

Before you begin this tutorial, make sure you have:

1. A [GroupDocs.Signature Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Access to the [API Reference](https://reference.groupdocs.cloud/signature/)
4. Development environment with the necessary SDK installed
5. A document with existing barcode signatures that you want to update

## Why Update Barcode Signatures?

There are several scenarios where updating barcode signatures is valuable:

- **Layout Adjustments**: Reposition barcodes for better document formatting
- **Visibility Improvements**: Modify the size or appearance of barcodes for better readability
- **Content Status Changes**: Mark or unmark barcodes as official signatures
- **Metadata Updates**: Change properties like signature ID or timestamp

## Practical Scenario

Let's consider a practical application: A shipping company has a document template with barcode signatures in suboptimal positions. They need to update these barcodes to be more prominent and repositioned to a standardized location to ensure consistent scanning across all documents.

## Step-by-Step Implementation

### Step 1: Upload a Document with Existing Barcode Signatures to Cloud Storage

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

### Step 2: Search for Existing Barcode Signatures to Get Their IDs

Before you can update a barcode signature, you need to know its unique ID. Let's search for barcode signatures in the document:

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
      'SignatureType': 'Barcode'
    }
  ]
}"
```

From the search results, note the `signatureId` of the barcode signature you want to update.

### Step 3: Configure Barcode Update Options

Now, let's configure the update options, including the new position, size, and other properties:

```javascript
curl -v "https://api.groupdocs.cloud/v2.0/signature/update" \
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
      'SignatureType': 'Barcode',
      'SignatureId': '4cb67aa8-835d-4877-8a5d-5a9ad015a098',
      'Left': 200,
      'Top': 200,
      'Width': 300,
      'Height': 100,
      'IsSignature': true
    }
  ]
}"
```

### Step 4: Process the Update Result

The API will return information about the updated barcode signature:

```javascript
{
  "fileInfo": {
    "filePath": "signedBarcode_one-page.docx",
    "storageName": null,
    "versionId": null,
    "password": null
  },
  "size": 1360021,
  "succeeded": [
    {
      "barcodeType": "Code128",
      "text": "123456789012",
      "format": null,
      "signatureType": "Barcode",
      "pageNumber": 1,
      "signatureId": "4cb67aa8-835d-4877-8a5d-5a9ad015a098",
      "isSignature": true,
      "createdOn": "2020-07-23T07:26:42.7549544+00:00",
      "modifiedOn": "2020-07-23T07:36:03.9037414+00:00",
      "top": 200,
      "left": 200,
      "width": 300,
      "height": 100
    }
  ],
  "failed": []
}
```

### Step 5: Download the Updated Document

Once the document has been updated, you can download it:

```javascript
curl -v "https://api.groupdocs.cloud/v2.0/signature/storage/file/signedBarcode_one-page.docx" \
-X GET \
-H "Accept: application/octet-stream" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
--output updated_signedBarcode_one-page.docx
```

## Try It Yourself

Now that you understand the process, try implementing barcode updates with different settings:

1. Change the position of the barcode to different coordinates
2. Adjust the size of the barcode to make it larger or smaller
3. Toggle the `IsSignature` property to change its status
4. Update multiple barcode signatures in a single request

## SDK Examples

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-signature-cloud/groupdocs-signature-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new SignApi(configuration);

// Search for barcode signatures to get their IDs
var searchBarcodeOptions = new SearchBarcodeOptions
{
    SignatureType = SignatureTypeEnum.Barcode,
    MatchType = SearchBarcodeOptions.MatchTypeEnum.Contains,
    Text = "123456789012",
    BarcodeType = "Code128",
    AllPages = true
};

// Search settings.
var searchSettings = new SearchSettings
{
    FileInfo = new FileInfo
    {
        FilePath = "signedBarcode_one-page.docx",
    },
    Options = new List<SearchOptions> { searchBarcodeOptions }
};

// Call search API to get signature IDs
var searchResult = apiInstance.SearchSignatures(new SearchSignaturesRequest(searchSettings));

// Check if any signatures were found
if (searchResult.Signatures != null && searchResult.Signatures.Count > 0)
{
    // Get the ID of the first signature found
    string signatureId = searchResult.Signatures[0].SignatureId;
    
    // Configure update options
    var updateOptions = new UpdateOptions
    {
        SignatureType = UpdateOptions.SignatureTypeEnum.Barcode,
        SignatureId = signatureId,
        // Set new position
        Left = 200,
        Top = 200,
        // Set new size
        Width = 300,
        Height = 100,
        // Mark as official signature
        IsSignature = true
    };
    
    // Update settings
    var updateSettings = new UpdateSettings
    {
        FileInfo = new FileInfo
        {
            FilePath = "signedBarcode_one-page.docx"
        },
        Options = new List<UpdateOptions> { updateOptions }
    };
    
    // Create update request
    var request = new UpdateSignaturesRequest(updateSettings);
    
    // Call API to update the signature
    var response = apiInstance.UpdateSignatures(request);
    
    // Process update result
    if (response.Succeeded != null && response.Succeeded.Count > 0)
    {
        Console.WriteLine("Barcode signature was successfully updated!");
        Console.WriteLine($"New position: Left={response.Succeeded[0].Left}, Top={response.Succeeded[0].Top}");
        Console.WriteLine($"New size: Width={response.Succeeded[0].Width}, Height={response.Succeeded[0].Height}");
        Console.WriteLine($"Is signature: {response.Succeeded[0].IsSignature}");
        Console.WriteLine($"Modified on: {response.Succeeded[0].ModifiedOn}");
    }
    else
    {
        Console.WriteLine("Failed to update barcode signature.");
    }
}
else
{
    Console.WriteLine("No barcode signatures found in the document.");
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

// Search for barcode signatures to get their IDs
SearchBarcodeOptions searchOptions = new SearchBarcodeOptions();
searchOptions.setSignatureType(SignatureTypeEnum.BARCODE);
searchOptions.setAllPages(true);

SearchSettings searchSettings = new SearchSettings();
searchSettings.setFileInfo(fileInfo);
searchSettings.addOptionsItem(searchOptions);

SearchSignaturesRequest searchRequest = new SearchSignaturesRequest(searchSettings);
SearchResult searchResult = apiInstance.searchSignatures(searchRequest);

// Check if any signatures were found
if (searchResult.getSignatures() != null && !searchResult.getSignatures().isEmpty()) {
    // Get the ID of the first signature found
    String signatureId = searchResult.getSignatures().get(0).getSignatureId();
    
    // Configure update options
    UpdateOptions updateOptions = new UpdateOptions();
    updateOptions.setSignatureType(UpdateOptions.SignatureTypeEnum.BARCODE);
    updateOptions.setSignatureId(signatureId);
    
    // Set new position
    updateOptions.setLeft(200);
    updateOptions.setTop(200);
    
    // Set new size
    updateOptions.setWidth(300);
    updateOptions.setHeight(100);
    
    // Mark as official signature
    updateOptions.setIsSignature(true);
    
    // Update settings
    UpdateSettings updateSettings = new UpdateSettings();
    updateSettings.setFileInfo(fileInfo);
    updateSettings.addOptionsItem(updateOptions);
    
    // Create update request
    UpdateSignaturesRequest updateRequest = new UpdateSignaturesRequest(updateSettings);
    
    // Call API to update the signature
    UpdateResult updateResult = apiInstance.updateSignatures(updateRequest);
    
    // Process update result
    if (updateResult.getSucceeded() != null && !updateResult.getSucceeded().isEmpty()) {
        System.out.println("Barcode signature was successfully updated!");
        System.out.println("New position: Left=" + updateResult.getSucceeded().get(0).getLeft() +
                          ", Top=" + updateResult.getSucceeded().get(0).getTop());
        System.out.println("New size: Width=" + updateResult.getSucceeded().get(0).getWidth() +
                          ", Height=" + updateResult.getSucceeded().get(0).getHeight());
        System.out.println("Is signature: " + updateResult.getSucceeded().get(0).getIsSignature());
        System.out.println("Modified on: " + updateResult.getSucceeded().get(0).getModifiedOn());
    } else {
        System.out.println("Failed to update barcode signature.");
    }
} else {
    System.out.println("No barcode signatures found in the document.");
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

# Search for barcode signatures to get their IDs
search_opts = groupdocs_signature_cloud.SearchBarcodeOptions()
search_opts.signature_type = 'Barcode'
search_opts.all_pages = True

search_settings = groupdocs_signature_cloud.SearchSettings()
search_settings.options = [search_opts]
search_settings.file_info = file_info

search_request = groupdocs_signature_cloud.SearchSignaturesRequest(search_settings)
search_result = api.search_signatures(search_request)

# Check if any signatures were found
if search_result.signatures and len(search_result.signatures) > 0:
    # Get the ID of the first signature found
    signature_id = search_result.signatures[0].signature_id
    
    # Configure update options
    update_opts = groupdocs_signature_cloud.UpdateOptions()
    update_opts.signature_type = 'Barcode'
    update_opts.signature_id = signature_id
    
    # Set new position
    update_opts.left = 200
    update_opts.top = 200
    
    # Set new size
    update_opts.width = 300
    update_opts.height = 100
    
    # Mark as official signature
    update_opts.is_signature = True
    
    # Update settings
    update_settings = groupdocs_signature_cloud.UpdateSettings()
    update_settings.options = [update_opts]
    update_settings.file_info = file_info
    
    # Create update request
    update_request = groupdocs_signature_cloud.UpdateSignaturesRequest(update_settings)
    
    # Call API to update the signature
    update_result = api.update_signatures(update_request)
    
    # Process update result
    if update_result.succeeded and len(update_result.succeeded) > 0:
        print("Barcode signature was successfully updated!")
        print(f"New position: Left={update_result.succeeded[0].left}, Top={update_result.succeeded[0].top}")
        print(f"New size: Width={update_result.succeeded[0].width}, Height={update_result.succeeded[0].height}")
        print(f"Is signature: {update_result.succeeded[0].is_signature}")
        print(f"Modified on: {update_result.succeeded[0].modified_on}")
    else:
        print("Failed to update barcode signature.")
else:
    print("No barcode signatures found in the document.")
```

## Advanced Update Options

### Properties You Can Update

GroupDocs.Signature Cloud allows you to update several properties of barcode signatures:

1. **Position Properties**:
   - `Left`: Horizontal position from the left edge of the page
   - `Top`: Vertical position from the top edge of the page

2. **Size Properties**:
   - `Width`: Width of the barcode
   - `Height`: Height of the barcode

3. **Status Properties**:
   - `IsSignature`: Boolean value indicating if the barcode should be considered an official signature

4. **Appearance Properties** (some document formats only):
   - `Border`: Border settings for the barcode
   - `BackgroundColor`: Background color of the barcode
   - `TransparencyLevel`: Transparency level of the barcode

### Updating Multiple Signatures

You can update multiple barcode signatures in a single request by adding multiple update options:

```csharp
// First barcode update options
var updateOptions1 = new UpdateOptions
{
    SignatureType = UpdateOptions.SignatureTypeEnum.Barcode,
    SignatureId = "signature-id-1",
    Left = 200,
    Top = 200
};

// Second barcode update options
var updateOptions2 = new UpdateOptions
{
    SignatureType = UpdateOptions.SignatureTypeEnum.Barcode,
    SignatureId = "signature-id-2",
    Left = 300,
    Top = 300
};

// Add both to update settings
var updateSettings = new UpdateSettings
{
    FileInfo = new FileInfo { FilePath = "document.docx" },
    Options = new List<UpdateOptions> { updateOptions1, updateOptions2 }
};
```

## Limitations and Considerations

When updating barcode signatures, be aware of these limitations:

1. **Signature ID Required**: You must know the barcode's signature ID, which typically requires a search operation first
2. **Read-Only Properties**: Some properties like barcode type, text content, and decoding performance cannot be updated
3. **Document Format Support**: Not all document formats support all update operations equally
4. **Visibility Concerns**: Moving or resizing barcodes may impact their readability by barcode scanners

## Troubleshooting

### Common Issues

**Issue**: "Signature not found" error despite using a valid signature ID.  
**Solution**: Ensure the signature ID is correct and from the same document version. Signature IDs change when documents are modified.

**Issue**: Updates don't apply as expected in certain document formats.  
**Solution**: Some document formats have limitations on signature updates. Test with your specific document format.

**Issue**: Barcode becomes unreadable after position/size updates.  
**Solution**: Ensure the barcode maintains proper proportions after resizing. Avoid extreme distortion that might make the barcode unreadable.

**Issue**: "Failed to update" error with no additional information.  
**Solution**: Check if the document is password-protected or if the signature is in a protected area of the document.

## What You've Learned

In this tutorial, you've learned:
- How to search for and identify barcode signatures in documents
- Updating the position, size, and status of barcode signatures
- Implementing barcode updates in different programming languages (C#, Java, Python)
- Working with multiple barcode signatures in a single operation
- Understanding limitations and troubleshooting common issues

## Further Practice

To reinforce your learning, try these exercises:

1. Create a document with multiple barcode signatures, then write code to standardize their positions
2. Implement a program that searches for barcodes and updates them based on their content
3. Create a workflow that checks if barcodes are positioned correctly and adjusts them if needed
4. Build a system that updates barcode signature statuses based on document processing stages

## Resources

- [Product Page](https://products.groupdocs.cloud/signature/)
- [Documentation](https://docs.groupdocs.cloud/signature/)
- [Live Demo](https://products.groupdocs.app/signature/family)
- [API Reference](https://reference.groupdocs.cloud/signature/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.signature-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/signature/13/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
