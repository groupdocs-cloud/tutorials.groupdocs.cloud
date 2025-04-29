---
title: How to Render Documents with Custom Fonts in GroupDocs.Viewer Cloud Tutorial
description: Learn how to render documents with custom fonts using GroupDocs.Viewer Cloud API in this step-by-step tutorial for developers.
url: /advanced-usage/render-with-custom-fonts/
weight: 100
---

## Tutorial: How to Render Documents with Custom Fonts

In this tutorial, you'll learn how to render documents with custom fonts using GroupDocs.Viewer Cloud API. Font consistency is crucial for maintaining the visual integrity of documents, especially when working with documents that use specialized or brand-specific fonts.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Configure custom font sources for document rendering
- Ensure consistent visual appearance across different platforms
- Handle missing font scenarios effectively
- Apply custom font rendering across various document formats

## Prerequisites

Before you begin this tutorial, you need:

1. A GroupDocs.Viewer Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials from the GroupDocs Cloud Dashboard
3. Basic understanding of REST APIs
4. Development environment for your preferred language (C#, Java, Python, etc.)
5. [GroupDocs.Viewer Cloud SDK](https://github.com/groupdocs-viewer-cloud) installed for your language of choice
6. Custom font files that you want to use (if any)

## The Practical Scenario

Imagine you're developing a corporate document management system for a company with a strict brand identity. The company uses custom fonts in all their documents, and it's essential that these fonts render correctly when viewed through your application. Without proper font handling, documents would appear with substituted fonts, potentially breaking layouts and diminishing the professional appearance.

## Step 1: Understanding Custom Font Handling

When rendering documents, GroupDocs.Viewer Cloud needs access to all fonts used in those documents. If a document uses fonts that aren't available, the API will substitute them with standard fonts, which can affect the document's appearance.

To solve this problem, GroupDocs.Viewer Cloud provides the `FontsPath` property, which allows you to specify a folder in your cloud storage containing custom fonts. When rendering documents, the API will check this folder for any required fonts.

## Step 2: Set Up Your Project

First, set up authentication with your Client ID and Client Secret:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = ""; // Get your Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get your Client ID from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);
```

## Step 3: Upload Custom Fonts to Cloud Storage

Before rendering documents with custom fonts, you need to upload those fonts to your cloud storage. You can do this using the Storage API:

```csharp
// Example to upload fonts to cloud storage
string fontsFolderPath = "Fonts"; // Path in cloud storage where fonts will be stored
string localFontPath = "BrandFont.ttf"; // Path to local font file

// Create fonts folder if it doesn't exist
var folderApi = new FolderApi(configuration);
if (!folderApi.Exists(new ObjectExistsRequest(fontsFolderPath)).Exists)
{
    folderApi.CreateFolder(new CreateFolderRequest(fontsFolderPath));
}

// Upload font file
var fileApi = new FileApi(configuration);
using (var stream = File.Open(localFontPath, FileMode.Open))
{
    var uploadRequest = new UploadFileRequest(fontsFolderPath + "/" + Path.GetFileName(localFontPath), stream);
    fileApi.UploadFile(uploadRequest);
}
```

## Step 4: Rendering a Document with Custom Fonts

Now that your fonts are in cloud storage, you can render documents using them:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_missing_font.odg" // Document in your cloud storage
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    // Specify the folder containing custom fonts
    FontsPath = "Fonts"
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 5: Handling Missing Fonts with Default Font

In some cases, you might want to specify a default font to use when a specific font is missing. You can combine the `FontsPath` property with the `DefaultFontName` property:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_missing_font.odg"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    // Specify the folder containing custom fonts
    FontsPath = "Fonts",
    
    RenderOptions = new RenderOptions
    {
        DefaultFontName = "Arial" // Fallback font if specific font is still missing
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 6: Using Custom Fonts in PDF Output

The same approach works when converting to PDF format:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_missing_font.odg"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    
    // Specify the folder containing custom fonts
    FontsPath = "Fonts"
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Try It Yourself

Now it's your turn to experiment with custom font rendering:

1. Upload some custom fonts to your cloud storage
2. Render documents with and without the FontsPath setting to see the difference
3. Try different combinations of font substitution strategies
4. Test with documents that use a variety of fonts

## Common Issues and Troubleshooting

Issue: Fonts still appear substituted despite specifying FontsPath  
Solution: Ensure the font file names exactly match what's referenced in the document. Font matching is case-sensitive.

Issue: Only some characters appear with the correct font  
Solution: The font might not contain all the required glyphs. Ensure your font files are complete.

Issue: Performance degradation with many font files  
Solution: Only include the fonts you need in your fonts folder. Too many fonts can slow down rendering.

Issue: Font files not found when rendering  
Solution: Double-check that the fonts were successfully uploaded to the specified path in cloud storage.

## Complete Code Examples

### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Render document with custom fonts
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer <jwt token>" \
-d "{
  'FileInfo': {
    'FilePath': 'with_missing_font.odg'
  },
  'ViewFormat': 'HTML',
  'FontsPath': 'Fonts'
}"
```

### SDK Examples

#### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);

var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_missing_font.odg"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,

    // NOTE: Upload fonts to the folder using storage API before rendering
    FontsPath = "Fonts"
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

#### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-python-samples
import groupdocs_viewer_cloud

client_id = "XXXX-XXXX-XXXX-XXXX" # Get Client Id and Client Secret from https://dashboard.groupdocs.cloud
client_secret = "XXXXXXXXXXXXXXXX" # Get Client Id and Client Secret from https://dashboard.groupdocs.cloud

apiInstance = groupdocs_viewer_cloud.ViewApi.from_keys(client_id, client_secret)

view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "with_missing_font.odg"
view_options.view_format = "HTML"
# NOTE: Upload fonts to the folder using storage API before rendering
view_options.fonts_path = "Fonts"

request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
response = apiInstance.create_view(request)
```

#### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-java-samples
String MyClientSecret = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud
String MyClientId = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
ViewApi apiInstance = new ViewApi(configuration);

FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("with_missing_font.odg");
ViewOptions viewOptions = new ViewOptions();
viewOptions.setFileInfo(fileInfo);
viewOptions.setViewFormat(ViewFormatEnum.HTML);

// NOTE: Upload fonts to the folder using storage API before rendering
viewOptions.setFontsPath("Fonts");

ViewResult response = apiInstance.createView(new CreateViewRequest(viewOptions));
```

## What You've Learned

In this tutorial, you've learned:

- How to configure custom font sources for document rendering
- How to upload custom fonts to cloud storage
- How to specify default fonts for handling missing font scenarios
- How to apply custom font rendering across different output formats

## Further Practice

To solidify your knowledge, try these exercises:

1. Build a font management system that allows uploading and managing custom fonts
2. Create a preview feature that shows how a document will look with different fonts
3. Implement a font detection system that identifies missing fonts in documents
4. Develop a solution that automatically downloads required fonts from a repository

## Next Tutorial

Ready to explore more advanced rendering options? Check out our document-specific rendering tutorials, starting with [PDF Document Rendering](/advanced-usage/rendering-pdf-documents/) to learn specialized techniques for different document formats.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Did you find this tutorial helpful? Do you have questions about implementing custom font rendering in your application? Let us know in the [GroupDocs.Viewer Cloud forum](https://forum.groupdocs.cloud/c/viewer/9).