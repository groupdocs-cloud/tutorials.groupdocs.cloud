---
title: How to Add Watermarks to Documents in GroupDocs.Viewer Cloud Tutorial
url: /advanced-usage/add-watermark/
description: Learn how to add text watermarks to documents when rendering with GroupDocs.Viewer Cloud API in this step-by-step tutorial for developers.
weight: 20
---

## Tutorial: How to Add Watermarks to Documents

In this tutorial, you'll learn how to add text watermarks to documents when rendering them with GroupDocs.Viewer Cloud API. Watermarks are useful for indicating document status, protecting copyrighted content, or adding branding to your documents.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Add customized text watermarks to various document formats
- Control watermark position, color, and opacity
- Implement watermarking across different output formats (HTML, PDF, images)
- Apply watermarks to different document types

## Prerequisites

Before you begin this tutorial, you need:

1. A GroupDocs.Viewer Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials from the GroupDocs Cloud Dashboard
3. Basic understanding of REST APIs
4. Development environment for your preferred language (C#, Java, Python, etc.)
5. [GroupDocs.Viewer Cloud SDK](https://github.com/groupdocs-viewer-cloud) installed for your language of choice

## The Practical Scenario

Let's imagine you're developing a document management system where users can view documents but shouldn't be able to claim them as their own. You want to overlay a "CONFIDENTIAL" watermark on all rendered documents to protect sensitive information.

## Step 1: Understanding Watermark Properties

GroupDocs.Viewer Cloud allows you to customize various aspects of watermarks:

- Text - The content of the watermark
- Color - The color of the watermark text (in HTML color format)
- Width - The width of the watermark
- Height - The height of the watermark
- FontName - The name of the font to use
- FontSize - The size of the font
- Position - The position on the page (Diagonal, TopLeft, TopCenter, TopRight, etc.)
- Opacity - The transparency level of the watermark (0-1)

## Step 2: Set Up Your Project

First, set up authentication with your Client ID and Client Secret:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = ""; // Get your Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get your Client ID from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);
```

## Step 3: Adding a Simple Watermark

Let's add a basic "CONFIDENTIAL" watermark to a document:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.docx" // Document in your cloud storage
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    // Add a watermark
    Watermark = new Watermark
    {
        Text = "CONFIDENTIAL"
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 4: Customizing the Watermark

Now, let's make our watermark more professional by customizing its appearance:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    // Add a customized watermark
    Watermark = new Watermark
    {
        Text = "CONFIDENTIAL",
        Color = "#FF0000", // Red color
        FontSize = 48,
        Position = Watermark.PositionEnum.Diagonal,
        Opacity = 0.5 // 50% opacity
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 5: Applying Watermarks to Different Output Formats

Watermarks work across different output formats. Here's how to add a watermark when rendering to PDF:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    
    Watermark = new Watermark
    {
        Text = "CONFIDENTIAL",
        Color = "#FF0000",
        FontSize = 48,
        Position = Watermark.PositionEnum.Diagonal,
        Opacity = 0.5
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Try It Yourself

Now it's your turn to experiment with watermarks:

1. Try different watermark positions (Diagonal, TopCenter, BottomRight, etc.)
2. Adjust the opacity to see how it affects readability
3. Change the color and font size to match your branding
4. Apply watermarks to different document types (PDFs, spreadsheets, presentations)

## Common Issues and Troubleshooting

Issue: Watermark text is too small or difficult to see  
Solution: Increase the font size and adjust opacity for better visibility

Issue: Watermark position doesn't look right on certain document types  
Solution: Different document formats may respond differently to positioning. Test various positions for optimal results.

Issue: Watermark text appears cut off  
Solution: Ensure your watermark text isn't too long. Consider using shorter text or increasing width/height.

## Complete Code Examples

### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Add watermark to document
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer <jwt token>" \
-d "{
  'FileInfo': {
    'FilePath': 'sample.docx'
  },
  'ViewFormat': 'HTML',
  'Watermark': {
    'Text': 'CONFIDENTIAL',
    'Color': '#FF0000',
    'FontSize': 48,
    'Position': 'Diagonal',
    'Opacity': 0.5
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
        FilePath = "sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,

    Watermark = new Watermark
    {
        Text = "CONFIDENTIAL",
        Color = "#FF0000",
        FontSize = 48,
        Position = Watermark.PositionEnum.Diagonal,
        Opacity = 0.5
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
view_options.file_info.file_path = "sample.docx"
view_options.view_format = "HTML"
view_options.watermark = groupdocs_viewer_cloud.Watermark()
view_options.watermark.text = "CONFIDENTIAL"
view_options.watermark.color = "#FF0000"
view_options.watermark.font_size = 48
view_options.watermark.position = "Diagonal"
view_options.watermark.opacity = 0.5

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
fileInfo.setFilePath("sample.docx");
ViewOptions viewOptions = new ViewOptions();
viewOptions.setFileInfo(fileInfo);
viewOptions.setViewFormat(ViewFormatEnum.HTML);
Watermark watermark = new Watermark();
watermark.setText("CONFIDENTIAL");
watermark.setColor("#FF0000");
watermark.setFontSize(48);
watermark.setPosition(PositionEnum.DIAGONAL);
watermark.setOpacity(0.5);
viewOptions.setWatermark(watermark);

ViewResult response = apiInstance.createView(new CreateViewRequest(viewOptions));
```

## What You've Learned

In this tutorial, you've learned:

- How to add text watermarks to documents when rendering
- Ways to customize watermark appearance including color, position, and opacity
- How to apply watermarks across different output formats
- Techniques for optimizing watermark visibility and appearance

## Further Practice

To solidify your knowledge, try these exercises:

1. Create a document viewer that allows users to toggle watermarks on and off
2. Implement different watermarks based on document sensitivity levels
3. Create a dynamic watermark that includes the current viewer's username
4. Combine watermarking with other rendering options like page selection

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Did you find this tutorial helpful? Do you have questions about implementing watermarks in your application? Let us know in the [GroupDocs.Viewer Cloud forum](https://forum.groupdocs.cloud/c/viewer/9).
