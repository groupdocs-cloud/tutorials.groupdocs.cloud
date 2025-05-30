---
title: Learn to Edit WordProcessing Documents with GroupDocs.Editor Cloud Tutorial
url: /document-edit-operations/working-with-wordprocessing-documents/
weight: 1
description: This tutorial teaches you how to load, edit and save Word documents using GroupDocs.Editor Cloud API in a step-by-step approach.
---

# Tutorial: Working with WordProcessing Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Load WordProcessing documents (DOC, DOCX, DOCM, etc.) from cloud storage
- Convert them to an editable HTML representation
- Download and edit the HTML content
- Upload the edited content back to cloud storage
- Save the edited document back to its original format

## Prerequisites

Before starting this tutorial, you should have:
- A [GroupDocs.Editor Cloud dashboard](https://dashboard.groupdocs.cloud/) account
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with your preferred programming language (C#, Java, PHP, etc.)

## The WordProcessing Document Editing Process

WordProcessing document formats include DOC, DOT, DOCX, DOCM, DOTX, ODT, RTF, and more. GroupDocs.Editor Cloud supports all these formats as part of its document editing functionality.

### Step 1: Load the Document for Editing

The first step in editing is to load the document into an editable HTML representation:

```
HTTP POST ~/load
```

This operation converts your document to HTML and stores it in the cloud storage. You can customize several options:

| Option | Description |
|--------|-------------|
| FileInfo.FilePath | The file path in storage (required) |
| FileInfo.StorageName | Storage name (optional for default storage) |
| FileInfo.Password | Password for protected documents |
| OutputPath | Directory where editable files will be stored |
| EnablePagination | Enable/disable pagination in HTML (default: false) |
| EnableLanguageInformation | Export language information to HTML (default: false) |
| FontExtraction | Control how fonts are extracted from the document |

#### Try It Yourself: Loading a Document

Let's see how to load a DOCX file using both cURL and SDK examples:

#### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now load the document
curl -v "https://api.groupdocs.cloud/v1.0/editor/load" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'WordProcessing/sample.docx' },
    'OutputPath': 'output',
    'EnablePagination': true,
    'FontExtraction': 'ExtractAllEmbedded'
}"
```

Upon successful execution, you'll receive a response like:

```json
{
  "resourcesPath": "output/sample.files",
  "htmlPath": "output/sample.html"
}
```

#### SDK Example (C#)

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-editor-cloud/groupdocs-editor-cloud-dotnet-samples
string MyClientSecret = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new EditApi(configuration);

// Prepare request
var fileInfo = new FileInfo
{
    FilePath = "WordProcessing/sample.docx"
};

var loadOptions = new LoadOptions
{
    FileInfo = fileInfo,
    OutputPath = "output",
    EnablePagination = true,
    FontExtraction = LoadOptions.FontExtractionEnum.ExtractAllEmbedded
};

// Execute request
var response = apiInstance.Load(new LoadRequest(loadOptions));

// The HTML representation is now available at the path in the response
Console.WriteLine("HTML file path: " + response.HtmlPath);
Console.WriteLine("Resources path: " + response.ResourcesPath);
```

### Step 2: Download and Edit the HTML Content

Now that your document is converted to HTML, you can download it and make your edits. The HTML file and its associated resources need to be downloaded from the paths provided in the response:

1. Download the HTML file from the path specified in `htmlPath`
2. Download the resources (if needed) from the path specified in `resourcesPath`
3. Edit the HTML using your preferred HTML editor or programmatically

### Step 3: Upload the Edited HTML Back to Storage

After editing, you need to upload the modified HTML content back to the cloud storage.

### Step 4: Save the Edited Document

Finally, you can save the edited HTML back to its original format using the save operation:

```
HTTP POST ~/save
```

This operation allows you to specify the following options:

| Option | Description |
|--------|-------------|
| FileInfo.FilePath | The output file path in storage |
| FileInfo.StorageName | Storage name (optional for default storage) |
| FileInfo.Password | Password for protected documents |
| HtmlPath | The path to the edited HTML file |
| ResourcesPath | The path to the resources folder |
| Options | Format-specific save options |

#### Try It Yourself: Saving an Edited Document

#### cURL Example

```bash
# Save the edited document
curl -v "https://api.groupdocs.cloud/v1.0/editor/save" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'WordProcessing/edited-sample.docx' },
    'HtmlPath': 'output/sample.html',
    'ResourcesPath': 'output/sample.files'
}"
```

#### SDK Example (C#)

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-editor-cloud/groupdocs-editor-cloud-dotnet-samples
string MyClientSecret = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new EditApi(configuration);

// Prepare save options
var saveOptions = new SaveOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "WordProcessing/edited-sample.docx"
    },
    HtmlPath = "output/sample.html",
    ResourcesPath = "output/sample.files"
};

// Execute save request
var response = apiInstance.Save(new SaveRequest(saveOptions));

// The edited document is now saved to the specified path
Console.WriteLine("Document saved successfully to: " + saveOptions.FileInfo.FilePath);
```

## Troubleshooting Tips

- Authentication Error: Ensure your Client ID and Client Secret are correct and that your token hasn't expired
- File Not Found: Double-check that the file path is correct and the file exists in the specified storage
- HTML Conversion Issues: For complex documents, check the FontExtraction option to ensure fonts are properly handled
- Resource Loading Errors: Make sure all resource paths are correctly specified when saving

## What You've Learned

In this tutorial, you've learned how to:
- Load a WordProcessing document into an editable HTML representation
- Configure various loading options to control the conversion process
- Access and modify the HTML content
- Save the edited document back to its original format
- Use both REST API calls and SDK methods to perform these operations

## Further Practice

To reinforce your learning, try these exercises:
1. Load a document with pagination enabled and examine the resulting HTML structure
2. Experiment with different font extraction options and observe the differences
3. Create a simple web application that allows users to upload, edit, and download WordProcessing documents

## Next Steps

Ready to learn more? Continue your journey with these related tutorials:
- [Tutorial: Working with Spreadsheet Documents](/document-edit-operations/working-with-spreadsheets/)
- [Tutorial: Working with Text Documents](/document-edit-operations/working-with-text-documents/)

## Resources

- [Product Page](https://products.groupdocs.cloud/editor/)
- [Documentation](https://docs.groupdocs.cloud/editor/)
- [Live Demo](https://products.groupdocs.app/editor/family)
- [API Reference](https://reference.groupdocs.cloud/editor/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.editor-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/editor/20/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
