---
title: How to Adjust Image Quality in PDF Documents with GroupDocs.Viewer Cloud Tutorial
url: /advanced-usage/adjust-image-quality/
description: Learn how to control image quality when rendering PDF documents using GroupDocs.Viewer Cloud API in this step-by-step tutorial for developers.
weight: 30
---

## Tutorial: How to Adjust Image Quality in PDF Documents

In this tutorial, you'll learn how to adjust the quality of images when rendering PDF documents using GroupDocs.Viewer Cloud API. PDF files often contain images, and controlling their quality is essential for balancing visual fidelity and file size in the rendered output.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Set different image quality levels when rendering PDF documents
- Understand the trade-offs between image quality, file size, and performance
- Implement quality optimization for different use cases
- Apply image quality settings in your applications

## Prerequisites

Before you begin this tutorial, you need:

1. A GroupDocs.Viewer Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials from the GroupDocs Cloud Dashboard
3. Basic understanding of REST APIs
4. Development environment for your preferred language (C#, Java, Python, etc.)

## The Practical Scenario

Imagine you're building a document management system that needs to render PDF documents for web viewing. Some of your users have limited bandwidth, while others prioritize visual quality. You need to implement a feature that allows adjusting image quality based on user preferences or network conditions.

## Step 1: Understanding Image Quality Settings

GroupDocs.Viewer Cloud provides the `ImageQuality` property in the `PdfDocumentOptions` class to control the quality of images in rendered PDF documents. This property accepts three values:

- Low - Fastest rendering with smallest file size, but lower visual quality
- Medium - Balanced option for most use cases
- High - Best visual quality, but larger file size and slower rendering

The image quality setting affects how images within the PDF are compressed and rendered. This setting is applicable when rendering to HTML format.

## Step 2: Set Up Your Project

First, set up authentication with your Client ID and Client Secret:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = ""; // Get your Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get your Client ID from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);
```

## Step 3: Rendering PDF with Low Image Quality

Let's start by rendering a PDF document with low image quality, which is suitable for users with limited bandwidth:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.pdf" // PDF document in your cloud storage
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new HtmlOptions
    {
        PdfDocumentOptions = new PdfDocumentOptions
        {
            ImageQuality = PdfDocumentOptions.ImageQualityEnum.Low
        }
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 4: Rendering PDF with Medium Image Quality

For a balanced approach suitable for most use cases:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.pdf"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new HtmlOptions
    {
        PdfDocumentOptions = new PdfDocumentOptions
        {
            ImageQuality = PdfDocumentOptions.ImageQualityEnum.Medium // Balanced quality
        }
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 5: Rendering PDF with High Image Quality

For users who prioritize visual quality:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.pdf"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new HtmlOptions
    {
        PdfDocumentOptions = new PdfDocumentOptions
        {
            ImageQuality = PdfDocumentOptions.ImageQualityEnum.High // Best quality
        }
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 6: Implementing Dynamic Quality Selection

In a real-world application, you might want to select the image quality dynamically based on user preferences or device capabilities:

```csharp
public ViewResult RenderPdfWithDynamicQuality(string filePath, string qualitySetting)
{
    var viewOptions = new ViewOptions
    {
        FileInfo = new FileInfo
        {
            FilePath = filePath
        },
        ViewFormat = ViewOptions.ViewFormatEnum.HTML,
        
        RenderOptions = new HtmlOptions
        {
            PdfDocumentOptions = new PdfDocumentOptions()
        }
    };
    
    // Set quality based on user preference
    switch (qualitySetting.ToLower())
    {
        case "low":
            viewOptions.RenderOptions.PdfDocumentOptions.ImageQuality = 
                PdfDocumentOptions.ImageQualityEnum.Low;
            break;
        case "high":
            viewOptions.RenderOptions.PdfDocumentOptions.ImageQuality = 
                PdfDocumentOptions.ImageQualityEnum.High;
            break;
        default:
            viewOptions.RenderOptions.PdfDocumentOptions.ImageQuality = 
                PdfDocumentOptions.ImageQualityEnum.Medium;
            break;
    }
    
    return apiInstance.CreateView(new CreateViewRequest(viewOptions));
}
```

## Try It Yourself

Now it's your turn to experiment with PDF image quality settings:

1. Render the same PDF document with different quality settings and compare the results
2. Measure the file size difference between low, medium, and high quality settings
3. Test with PDFs containing different types of images (photographs, diagrams, charts)
4. Implement a user interface that allows users to select their preferred quality

## Common Issues and Troubleshooting

Issue: No noticeable difference in quality despite changing settings  
Solution: Some PDFs may not contain images or may have already compressed images. Try with a PDF that contains high-quality photographs.

Issue: HTML output is very large even with low quality setting  
Solution: The PDF might contain many high-resolution images. Consider using additional optimization techniques such as limiting the pages rendered.

Issue: Image quality setting has no effect  
Solution: Ensure you're setting the property on the correct options object and that you're rendering to HTML format.

## Complete Code Examples

### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Render PDF with medium image quality
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer <jwt token>" \
-d "{
  'FileInfo': {
    'FilePath': 'sample.pdf'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'PdfDocumentOptions': {
      'ImageQuality': 'Medium'
    }
  }
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
        FilePath = "sample.pdf"
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

#### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-python-samples
import groupdocs_viewer_cloud

client_id = "XXXX-XXXX-XXXX-XXXX" # Get Client Id and Client Secret from https://dashboard.groupdocs.cloud
client_secret = "XXXXXXXXXXXXXXXX" # Get Client Id and Client Secret from https://dashboard.groupdocs.cloud

apiInstance = groupdocs_viewer_cloud.ViewApi.from_keys(client_id, client_secret)

view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "sample.pdf"
view_options.view_format = "HTML"
view_options.render_options = groupdocs_viewer_cloud.HtmlOptions()
view_options.render_options.pdf_document_options = groupdocs_viewer_cloud.PdfDocumentOptions()
view_options.render_options.pdf_document_options.image_quality = "Medium"

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
fileInfo.setFilePath("sample.pdf");
ViewOptions viewOptions = new ViewOptions();
viewOptions.setFileInfo(fileInfo);
viewOptions.setViewFormat(ViewFormatEnum.HTML);
HtmlOptions renderOptions = new HtmlOptions();
PdfDocumentOptions pdfDocumentOptions = new PdfDocumentOptions();
pdfDocumentOptions.setImageQuality(ImageQualityEnum.MEDIUM);
renderOptions.setPdfDocumentOptions(pdfDocumentOptions);
viewOptions.setRenderOptions(renderOptions);

ViewResult response = apiInstance.createView(new CreateViewRequest(viewOptions));
```

## Performance Comparison

Here's a comparison of the different image quality settings to help you choose the right option for your use case:

| Quality Setting | File Size | Rendering Speed | Visual Quality | Best For |
|-----------------|-----------|-----------------|----------------|----------|
| Low | Smallest | Fastest | Basic | Mobile devices, slow connections |
| Medium | Moderate | Balanced | Good | Most web applications |
| High | Largest | Slowest | Excellent | Printing, archiving, visual detail |

## What You've Learned

In this tutorial, you've learned:

- How to adjust image quality when rendering PDF documents to HTML
- The different quality levels available and their trade-offs
- How to implement dynamic quality selection based on user preferences
- Ways to optimize PDF rendering for different use cases

## Further Practice

To solidify your knowledge, try these exercises:

1. Build a PDF viewer with a quality selector dropdown (Low, Medium, High)
2. Create an adaptive system that automatically selects quality based on network speed
3. Combine image quality settings with other PDF rendering options
4. Compare file sizes and loading times for different quality settings

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Did you find this tutorial helpful? Do you have questions about implementing image quality controls in your application? Let us know in the [GroupDocs.Viewer Cloud forum](https://forum.groupdocs.cloud/c/viewer/9)!
