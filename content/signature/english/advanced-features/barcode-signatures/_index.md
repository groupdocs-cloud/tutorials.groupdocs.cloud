---
title: "Learn to Sign Documents with Barcode Signatures"
id: "tutorial-sign-document-with-barcode-signatures"
url: /advanced-features/barcode-signatures/
productName: "GroupDocs.Signature Cloud"
description: "Learn how to implement barcode signatures in documents using GroupDocs.Signature Cloud API in this step-by-step tutorial"
keywords: "barcode signature tutorial, GroupDocs.Signature Cloud, add barcode signature, document signing tutorial, barcode API"
---

# Tutorial: Learn to Sign Documents with Barcode Signatures

## Introduction

In this tutorial, you'll learn how to add barcode signatures to documents using GroupDocs.Signature Cloud API. Barcode signatures provide an excellent way to embed machine-readable information into your documents, making them ideal for automated verification and processing systems.

**Learning Objectives:**
- Understand what barcode signatures are and when to use them
- Learn to create and customize barcode signatures
- Implement barcode signatures in different document formats
- Explore various barcode types and their applications

## Prerequisites

Before you begin this tutorial, make sure you have:

1. A [GroupDocs.Signature Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Access to the [API Reference](https://reference.groupdocs.cloud/signature/)
4. Development environment with the necessary SDK installed (examples provided for multiple languages)
5. Basic understanding of RESTful APIs and your programming language of choice

## What Are Barcode Signatures?

A barcode signature is a machine-readable representation of data that appears as a series of parallel lines or patterns. When added to a document, it provides a way to embed information that can be quickly scanned and verified by barcode readers.

Barcodes are commonly used for:
- Product identification
- Inventory management
- Document tracking
- Quick access to specific information

## Practical Scenario

Let's consider a practical application: A logistics company needs to embed tracking information in shipping documents. Using barcode signatures allows warehouse staff to scan documents quickly to verify package information without manual data entry.

## Step-by-Step Implementation

### Step 1: Upload Your Document to Cloud Storage

Before adding a barcode signature, you need to upload your document to GroupDocs Cloud Storage.

```javascript
// First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

// Upload document
curl -v "https://api.groupdocs.cloud/v2.0/signature/storage/file/one-page.docx" \
-X PUT \
-H "Content-Type: application/octet-stream" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
--data-binary @/path/to/your/one-page.docx
```

### Step 2: Configure Barcode Signature Options

Now, let's configure the barcode signature with specific properties:

1. Choose the barcode type (Code128 in our example)
2. Define the text content for the barcode
3. Set the position, size, and appearance properties

### Step 3: Add the Barcode Signature to Your Document

With your document uploaded and options configured, you can now add the barcode signature:

```javascript
curl -v "https://api.groupdocs.cloud/v2.0/signature/create" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'one-page.docx'
  },
  'Options': [
    {
      'AllPages': true,
      'SignatureType': 'Barcode',
      'BarcodeType': 'Code128',
      'Text': '123456789012',
      'Left': 100,
      'Top': 100,
      'Width': 300,
      'Height': 100
    }
  ],
  'SaveOptions': {
    'OutputFilePath': 'signedBarcode_one-page.docx'
  }
}"
```

### Step 4: Download the Signed Document

Once the document has been signed, you can download it:

```javascript
curl -v "https://api.groupdocs.cloud/v2.0/signature/storage/file/signedBarcode_one-page.docx" \
-X GET \
-H "Accept: application/octet-stream" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
--output signedBarcode_one-page.docx
```

## Try It Yourself

Now that you understand the process, try implementing barcode signatures with different settings:

1. Change the barcode type to "QR_Code" or "DataMatrix"
2. Modify the positioning and size of the barcode
3. Try applying the barcode to specific pages using the `Page` parameter instead of `AllPages`
4. Experiment with different text content to see how it affects the barcode pattern

## SDK Examples

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-signature-cloud/groupdocs-signature-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new SignApi(configuration);

// Sign options.
var options = new SignBarcodeOptions
{
    SignatureType = SignatureTypeEnum.Barcode,
    Text = "123456789012",
    BarcodeType = "Code128",
    CodeTextAlignment = SignBarcodeOptions.CodeTextAlignmentEnum.None,
    Left = 100,
    Top = 100,
    Width = 300,
    Height = 100,
    LocationMeasureType = SignTextOptions.LocationMeasureTypeEnum.Pixels,
    SizeMeasureType = SignTextOptions.SizeMeasureTypeEnum.Pixels,
    // Customize barcode appearance
    ForeColor = new Color {Web = "BlueViolet"},
    BackgroundColor = new Color {Web = "DarkOrange"},
    // Specify which page to apply the signature
    AllPages = false,
    Page = 1
};

// Sign settings.
var signSettings = new SignSettings
{
    FileInfo = new FileInfo { FilePath = "one-page.docx" },
    SaveOptions = new SaveOptions { OutputFilePath = "signedBarcode_one-page.docx" },
    Options = new List<SignOptions> { options }
};

// Create request.
var request = new CreateSignaturesRequest(signSettings);

// Call API to apply the signature
var response = apiInstance.CreateSignatures(request);

// The response contains the download URL and signature details
Console.WriteLine($"Signed document URL: {response.downloadUrl}");
Console.WriteLine($"Signature ID: {response.succeeded[0].signatureId}");
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-signature-cloud/groupdocs-signature-cloud-java-samples
String MyClientSecret = "YOUR_CLIENT_SECRET"; 
String MyClientId = "YOUR_CLIENT_ID"; 

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
SignApi apiInstance = new SignApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("one-page.docx");

// Configure barcode signature options
SignBarcodeOptions options = new SignBarcodeOptions();
options.setSignatureType(SignatureTypeEnum.BARCODE);

// Set signature properties
options.setText("123456789012");
options.setBarcodeType("Code128");
options.setCodeTextAlignment(CodeTextAlignmentEnum.NONE);

// Set signature position on a page
options.setLeft(100);
options.setTop(100);
options.setWidth(300);
options.setHeight(100);
options.setLocationMeasureType(LocationMeasureTypeEnum.PIXELS);
options.setSizeMeasureType(SizeMeasureTypeEnum.PIXELS);

// Set signature appearance
Color backgroundColor = new Color();
backgroundColor.setWeb("DarkOrange");
options.setBackgroundColor(backgroundColor);

// Set pages for signing
options.setPage(1);
options.setAllPages(false);

// Configure save options
SaveOptions saveOptions = new SaveOptions();
saveOptions.setOutputFilePath("signedBarcode_one-page.docx");

// Set up the signature request
SignSettings signSettings = new SignSettings();
signSettings.setFileInfo(fileInfo);
signSettings.addOptionsItem(options);
signSettings.setSaveOptions(saveOptions);

// Execute the signing operation
CreateSignaturesRequest request = new CreateSignaturesRequest(signSettings);
SignResult response = apiInstance.createSignatures(request);

// Process the response
System.out.println("Signed document URL: " + response.getDownloadUrl());
System.out.println("Signature ID: " + response.getSucceeded().get(0).getSignatureId());
```

### Python Example

```python
# For complete examples, go to https://github.com/groupdocs-signature-cloud/groupdocs-signature-cloud-python-samples
import groupdocs_signature_cloud

# Get your app_sid and app_key at https://dashboard.groupdocs.cloud
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create instance of the API
api = groupdocs_signature_cloud.SignApi.from_keys(client_id, client_secret)

# Document information
file_info = groupdocs_signature_cloud.FileInfo()
file_info.file_path = "one-page.docx"

# Configure barcode signature options
opts = groupdocs_signature_cloud.SignBarcodeOptions()
opts.signature_type = 'Barcode'
opts.text = '123456789012'
opts.barcode_type = 'Code128'

# Set signature position on page
opts.left = 100
opts.top = 100
opts.width = 300
opts.height = 100
opts.location_measure_type = "Pixels"
opts.size_measure_type = "Pixels"

# Set appearance options
opts.fore_color = groupdocs_signature_cloud.Color()
opts.fore_color.web = "BlueViolet"
opts.background_color = groupdocs_signature_cloud.Color()
opts.background_color.web = "DarkOrange"

# Set page options
opts.page = 1
opts.all_pages = False

# Configure save options
settings = groupdocs_signature_cloud.SignSettings()
settings.options = [opts]
settings.save_options = groupdocs_signature_cloud.SaveOptions()
settings.save_options.output_file_path = "signedBarcode_one-page.docx"
settings.file_info = file_info

# Execute signing
request = groupdocs_signature_cloud.CreateSignaturesRequest(settings)
response = api.create_signatures(request)

# Process response
print(f"Signed document URL: {response.download_url}")
if response.succeeded:
    print(f"Signature ID: {response.succeeded[0].signature_id}")
```

## Advanced Configuration Options

GroupDocs.Signature Cloud provides several options to customize your barcode signatures:

### Barcode Types
- **1D Barcodes**: Code128, Code39, EAN13, UPC, etc.
- **2D Barcodes**: DataMatrix, Aztec, PDF417, etc.

### Appearance Settings
- `ForeColor`: Sets the color of the barcode lines
- `BackgroundColor`: Sets the background color
- `Border`: Configures a border around the barcode
- `RotationAngle`: Rotates the barcode on the page

### Positioning Options
- `HorizontalAlignment`: Aligns the barcode horizontally (Left, Center, Right)
- `VerticalAlignment`: Aligns the barcode vertically (Top, Center, Bottom)
- `Margin`: Sets the spacing around the barcode

## Troubleshooting

### Common Issues

**Issue**: Barcode is not readable by scanners.  
**Solution**: Ensure the barcode dimensions are adequate and the text content follows the required format for the specific barcode type.

**Issue**: API returns an error about invalid barcode type.  
**Solution**: Verify that you're using a supported barcode type. Use the [Get Supported Barcode Types](https://reference.groupdocs.cloud/signature/#/Signature/GetSupportedBarcodes) endpoint to check.

**Issue**: Barcode appears in wrong position or size.  
**Solution**: Ensure you're using consistent measurement units (pixels, percentages, etc.) for positioning and sizing.

## What You've Learned

In this tutorial, you've learned:
- How to add barcode signatures to documents using GroupDocs.Signature Cloud API
- The structure and configuration options for barcode signatures
- Implementation with different programming languages (C#, Java, Python)
- How to customize the appearance and positioning of barcodes
- Troubleshooting common issues

## Further Practice

To reinforce your learning, try these exercises:

1. Create a document with multiple different barcode types
2. Implement a barcode signature with custom border and rotation
3. Generate a document with barcodes positioned at different page locations
4. Create a program that automatically generates shipping documents with barcode signatures

## Resources

- [Product Page](https://products.groupdocs.cloud/signature/)
- [Documentation](https://docs.groupdocs.cloud/signature/)
- [Live Demo](https://products.groupdocs.app/signature/family)
- [API Reference](https://reference.groupdocs.cloud/signature/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.signature-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/signature/13/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
