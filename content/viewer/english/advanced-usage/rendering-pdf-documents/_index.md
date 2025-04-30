---
title: How to Render PDF Documents with GroupDocs.Viewer Cloud API Tutorial
url: /advanced-usage/rendering-pdf-documents/
description: Learn step-by-step how to render PDF documents with advanced options using GroupDocs.Viewer Cloud API in this comprehensive tutorial
weight: 50
---

# Tutorial: How to Render PDF Documents with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to render PDF documents with advanced options using GroupDocs.Viewer Cloud API. By the end of this tutorial, you'll be able to:

- Adjust image quality in PDF documents for optimal rendering
- Enable font hinting to improve text display
- Implement layered rendering for multi-layered PDFs
- Disable character grouping for improved content positioning
- Render pages at their original size

## Prerequisites

Before starting this tutorial, you should have:

1. A GroupDocs.Viewer Cloud account (or [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials
3. A sample PDF document to test with
4. Basic understanding of REST API concepts
5. Familiarity with your preferred programming language (C#, Java, Python, etc.)

## Use Case Scenario

Let's imagine you're developing a document management system where users need to view PDF documents with various quality levels depending on their internet connection speed. You'll need to implement options to adjust rendering quality, enable font hinting for better text display, and handle multi-layered PDFs correctly.

## Tutorial Steps

### 1. Adjusting Image Quality

When rendering PDF documents to HTML, you can adjust the quality of embedded images to balance between performance and visual quality. GroupDocs.Viewer Cloud API provides three quality levels:

- Low: Fast performance with smaller file size but lower image quality
- Medium: Balanced performance and quality
- High: Best quality but slower performance and larger file size

#### Try it yourself

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now render the PDF with Medium image quality
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.pdf'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'PdfDocumentOptions': {
      'ImageQuality': 'Medium'
    }
  }
}"
```

#### What you should see

After executing the API call, you'll receive a JSON response with links to the rendered HTML pages. The embedded images in these pages will be rendered with medium quality, providing a good balance between visual quality and file size.

#### Learning checkpoint
What happens if you change the `ImageQuality` from `Medium` to `High`? Try it and observe the difference in the rendered output and file size.

### 2. Enabling Font Hinting

Font hinting adjusts the display of outline fonts to improve readability at different sizes and resolutions. This is especially useful when rendering PDFs to image formats like PNG or JPG.

#### Try it yourself

```bash
# Render PDF with font hinting enabled
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.pdf'
  },
  'ViewFormat': 'PNG',
  'RenderOptions': {
    'PdfDocumentOptions': {
      'EnableFontHinting': true
    }
  }
}"
```

#### What you should see

The rendered PNG images will have improved text display, particularly for smaller font sizes, as the font hinting feature adjusts character outlines for better readability.

### 3. Enabling Layered Rendering

Multi-layered PDF documents can have complex layouts with text and graphics arranged in different layers. By enabling layered rendering, GroupDocs.Viewer Cloud renders text and graphics according to their z-order in the original PDF document.

#### Try it yourself

```bash
# Render PDF with layered rendering enabled
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.pdf'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'PdfDocumentOptions': {
      'EnableLayeredRendering': true
    }
  }
}"
```

#### Troubleshooting Tip

If you notice overlapping text or misaligned graphics in your rendered PDF, enabling layered rendering often resolves these issues. This option is only supported when rendering to HTML format.

### 4. Disabling Characters Grouping

To improve content positioning when rendering PDFs, especially those with complex layouts, you can disable character grouping:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.pdf'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'PdfDocumentOptions': {
      'DisableCharsGrouping': true
    }
  }
}"
```

### 5. Rendering at Original Page Size

When rendering to image formats, you can maintain the exact dimensions of the original PDF pages:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.pdf'
  },
  'ViewFormat': 'PNG',
  'RenderOptions': {
    'PdfDocumentOptions': {
      'RenderOriginalPageSize': true
    }
  }
}"
```

### 6. Getting PDF Document Information

Before rendering, you may want to get information about the PDF document:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.pdf'
  }
}"
```

## SDK Examples

### C# Example

```csharp
// For complete examples, see: https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; 
string MyClientId = "YOUR_CLIENT_ID"; 

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);

// Example: Adjust image quality when rendering PDF to HTML
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/sample.pdf"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    RenderOptions = new HtmlOptions
    {
        PdfDocumentOptions = new PdfDocumentOptions
        {
            ImageQuality = PdfDocumentOptions.ImageQualityEnum.Medium
        }
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

### Python Example

```python
# For complete examples, see: https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-python-samples
import groupdocs_viewer_cloud

client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

apiInstance = groupdocs_viewer_cloud.ViewApi.from_keys(client_id, client_secret)

# Example: Enable font hinting when rendering to PNG
view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "SampleFiles/sample.pdf"
view_options.view_format = "PNG"
view_options.render_options = groupdocs_viewer_cloud.ImageOptions()
view_options.render_options.pdf_document_options = groupdocs_viewer_cloud.PdfDocumentOptions()
view_options.render_options.pdf_document_options.enable_font_hinting = True

request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
response = apiInstance.create_view(request)
```

## What You've Learned

In this tutorial, you've learned how to:
- Adjust image quality in PDF documents for optimal rendering
- Enable font hinting to improve text display in rendered images
- Implement layered rendering for complex multi-layered PDFs
- Disable character grouping for better content positioning
- Render pages at their original size
- Retrieve PDF document information before rendering

## Further Practice

Try combining multiple PDF rendering options in a single request to see how they interact. For example, enable both layered rendering and disable character grouping to see how they work together for complex PDF layouts.

## Next Tutorial

Continue your learning journey with our [Tutorial on Rendering Spreadsheets](/advanced-usage/rendering-spreadsheets/), where you'll discover techniques for handling Excel and other spreadsheet formats with GroupDocs.Viewer Cloud API.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

If you have questions about this tutorial or any aspect of using GroupDocs.Viewer Cloud API, please visit our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
