---
title: How to Convert Annotated Documents with GroupDocs.Annotation Cloud API Tutorial
weight: 1
url: /getting-started/convert-documents/
description: Learn how to convert annotated documents to different formats using GroupDocs.Annotation Cloud API in this comprehensive developer tutorial
---

# Tutorial: How to Convert Annotated Documents with GroupDocs.Annotation Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Convert annotated documents to different formats using the GroupDocs.Annotation Cloud API
- Control how annotations appear in the output document
- Specify conversion options for different target formats
- Implement document conversion in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Annotation Cloud account (if you don't have one, [get a free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your API keys (AppSID and AppKey) from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document with annotations uploaded to your storage (ideally, follow the [Add Annotations tutorial](/getting-started/add-annotations/) first to create one)
4. Basic knowledge of REST APIs
5. A development environment set up with your preferred language

## Practical Scenario

When working with annotated documents, you often need to convert them to different formats for various purposes. For example, you might want to:
- Convert a Word document with annotations to PDF for easier sharing
- Convert an annotated PDF to an image format for presentation
- Export documents with or without annotations for different audiences
- Preserve annotations when converting between formats

This tutorial shows you how to implement these document conversion scenarios.

## Understanding Supported Conversion Formats

GroupDocs.Annotation Cloud API supports converting documents with annotations to various formats, including:
- PDF
- Word documents (DOCX, DOC)
- Excel spreadsheets (XLSX, XLS)
- PowerPoint presentations (PPTX, PPT)
- Images (PNG, JPG)
- HTML
- And more

## Step-by-Step Guide

### 1. Understanding the API Endpoint

GroupDocs.Annotation Cloud API provides an endpoint to convert documents:

```
POST https://api.groupdocs.cloud/v2.0/annotation/convert
```

This endpoint requires you to specify the document path and conversion options in the request body.

### 2. Basic Document Conversion

Let's start with a basic example - converting a Word document with annotations to PDF:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/convert" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/annotated-sample.docx'
  },
  'Format': 'pdf',
  'OutputPath': 'output/converted-document.pdf'
}"
```

This request will:
- Take the document at `annotationdocs/annotated-sample.docx`
- Convert it to PDF format while preserving the annotations
- Save the result to `output/converted-document.pdf`

### 3. Converting with Specific Options

You can customize the conversion process using various options. For example, when converting to PDF, you can set password protection:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/convert" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/annotated-sample.docx'
  },
  'Format': 'pdf',
  'OutputPath': 'output/password-protected.pdf',
  'ConvertOptions': {
    'PdfOptions': {
      'Password': 'p@s$w0rd',
      'PdfFormat': 'PDF'
    }
  }
}"
```

### 4. Controlling Annotation Appearance

When converting documents, you can control how annotations appear in the output document. For example, you can:
- Choose to include or exclude annotations
- Control annotation visibility
- Set annotation properties like color, opacity, etc.

These settings are typically specified in the API implementation rather than through the REST API directly.

### 5. Implementing in C#

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyAppSid, MyAppKey);
  
// Create AnnotateApi instance
var apiInstance = new AnnotateApi(configuration);

// Create file info object
var fileInfo = new FileInfo
{
    FilePath = "annotationdocs/annotated-sample.docx"
};

// Basic conversion to PDF
Console.WriteLine("Converting document to PDF...");
var convertRequest = new ConvertRequest(fileInfo, "pdf")
{
    OutputPath = "output/converted-document.pdf"
};

var convertResult = apiInstance.Convert(convertRequest);
Console.WriteLine($"Document converted successfully to PDF. Saved to {convertResult.Path}");

// Conversion to PDF with password protection
Console.WriteLine("\nConverting document to password-protected PDF...");
var pdfOptions = new PdfConvertOptions
{
    Password = "p@s$w0rd",
    PdfFormat = PdfConvertOptions.PdfFormatEnum.PDF
};

var convertWithOptionsRequest = new ConvertRequest(fileInfo, "pdf")
{
    OutputPath = "output/password-protected.pdf",
    ConvertOptions = new ConvertOptions { PdfOptions = pdfOptions }
};

var convertWithOptionsResult = apiInstance.Convert(convertWithOptionsRequest);
Console.WriteLine($"Document converted successfully to password-protected PDF. Saved to {convertWithOptionsResult.Path}");

// Conversion to image format (PNG)
Console.WriteLine("\nConverting document to PNG...");
var convertToImageRequest = new ConvertRequest(fileInfo, "png")
{
    OutputPath = "output/converted-document.png"
};

var convertToImageResult = apiInstance.Convert(convertToImageRequest);
Console.WriteLine($"Document converted successfully to PNG. Saved to {convertToImageResult.Path}");
```

### 6. Implementing in Python

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
# Create API instance
annotate_api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)

# Create file info object
file_info = groupdocs_annotation_cloud.FileInfo()
file_info.file_path = "annotationdocs/annotated-sample.docx"

# Basic conversion to PDF
print("Converting document to PDF...")
convert_request = groupdocs_annotation_cloud.ConvertRequest(file_info, "pdf")
convert_request.output_path = "output/converted-document.pdf"

convert_result = annotate_api.convert(convert_request)
print(f"Document converted successfully to PDF. Saved to {convert_result.path}")

# Conversion to PDF with password protection
print("\nConverting document to password-protected PDF...")
pdf_options = groupdocs_annotation_cloud.PdfConvertOptions()
pdf_options.password = "p@s$w0rd"
pdf_options.pdf_format = "PDF"

convert_options = groupdocs_annotation_cloud.ConvertOptions()
convert_options.pdf_options = pdf_options

convert_with_options_request = groupdocs_annotation_cloud.ConvertRequest(file_info, "pdf")
convert_with_options_request.output_path = "output/password-protected.pdf"
convert_with_options_request.convert_options = convert_options

convert_with_options_result = annotate_api.convert(convert_with_options_request)
print(f"Document converted successfully to password-protected PDF. Saved to {convert_with_options_result.path}")

# Conversion to image format (PNG)
print("\nConverting document to PNG...")
convert_to_image_request = groupdocs_annotation_cloud.ConvertRequest(file_info, "png")
convert_to_image_request.output_path = "output/converted-document.png"

convert_to_image_result = annotate_api.convert(convert_to_image_request)
print(f"Document converted successfully to PNG. Saved to {convert_to_image_result.path}")
```

### 7. Implementing in Node.js

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-node-samples
const { AnnotateApi, FileInfo, ConvertOptions, PdfConvertOptions } = require("groupdocs-annotation-cloud");

// Set up authentication credentials
const appSid = "XXXX-XXXX-XXXX-XXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
const appKey = "XXXXXXXXXXXXXXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud

// Initialize API instance
const annotateApi = new AnnotateApi.fromKeys(appSid, appKey);

// Create file info object
const fileInfo = new FileInfo();
fileInfo.filePath = "annotationdocs/annotated-sample.docx";

// Basic conversion to PDF
console.log("Converting document to PDF...");
const convertRequest = {
    fileInfo: fileInfo,
    format: "pdf",
    outputPath: "output/converted-document.pdf"
};

annotateApi.convert(convertRequest)
    .then((convertResult) => {
        console.log(`Document converted successfully to PDF. Saved to ${convertResult.path}`);
        
        // Conversion to PDF with password protection
        console.log("\nConverting document to password-protected PDF...");
        
        const pdfOptions = new PdfConvertOptions();
        pdfOptions.password = "p@s$w0rd";
        pdfOptions.pdfFormat = "PDF";
        
        const convertOptions = new ConvertOptions();
        convertOptions.pdfOptions = pdfOptions;
        
        const convertWithOptionsRequest = {
            fileInfo: fileInfo,
            format: "pdf",
            outputPath: "output/password-protected.pdf",
            convertOptions: convertOptions
        };
        
        return annotateApi.convert(convertWithOptionsRequest);
    })
    .then((convertWithOptionsResult) => {
        console.log(`Document converted successfully to password-protected PDF. Saved to ${convertWithOptionsResult.path}`);
        
        // Conversion to image format (PNG)
        console.log("\nConverting document to PNG...");
        
        const convertToImageRequest = {
            fileInfo: fileInfo,
            format: "png",
            outputPath: "output/converted-document.png"
        };
        
        return annotateApi.convert(convertToImageRequest);
    })
    .then((convertToImageResult) => {
        console.log(`Document converted successfully to PNG. Saved to ${convertToImageResult.path}`);
    })
    .catch((error) => {
        console.error("Error:", error.message);
    });
```

## Try It Yourself

Now that you've seen the examples, it's time to try implementing document conversion in your own application:

1. Get your API credentials from the dashboard
2. Upload an annotated document to your storage or use a sample document
3. Choose your preferred programming language
4. Decide which format you want to convert to
5. Copy the appropriate code example above
6. Modify it with your credentials, document path, and conversion options
7. Run the code and verify the converted document

## Format-Specific Conversion Options

Depending on the target format, you can specify different conversion options:

### PDF Options
- Password protection
- PDF/A compliance
- Compression settings
- Page layout options

### Image Options
- Resolution (DPI)
- Image quality
- Color mode (RGB, grayscale, etc.)
- Image format (PNG, JPG, etc.)

### Word Options
- Document protection
- Page setup
- Compatibility mode

### HTML Options
- Embedded resources
- CSS styling
- JavaScript options

## Troubleshooting Tips

- Unsupported Format: Make sure both the source and target formats are supported by the API.
- Authentication Errors: Verify your AppSID and AppKey are correctly set up.
- File Not Found Errors: Ensure the document path is correct and the file exists in your storage.
- Output Path Issues: Make sure the output folder exists in your storage or use a different path.
- Annotation Rendering Issues: If annotations don't appear correctly in the output document, check the annotation properties and compatibility with the target format.
- File Size Limitations: Be aware of file size limitations, especially when converting large documents.

## What You've Learned

In this tutorial, you have learned:
- How to convert annotated documents to different formats using the GroupDocs.Annotation Cloud API
- How to specify conversion options for different target formats
- How to implement document conversion in C#, Python, and Node.js
- How to handle format-specific options like password protection

## Further Practice

To solidify your understanding:
1. Try converting documents to different formats not covered in this tutorial
2. Experiment with different conversion options for each format
3. Create a batch conversion function to process multiple documents
4. Build a simple UI that lets users select conversion options

## Next Steps

Ready to learn more? Check out our other tutorials on document operations with GroupDocs.Annotation Cloud API.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
