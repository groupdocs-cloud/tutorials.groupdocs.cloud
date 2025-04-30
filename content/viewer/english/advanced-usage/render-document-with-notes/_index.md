---
title: How to Render Documents with Notes in GroupDocs.Viewer Cloud Tutorial
url: /advanced-usage/render-document-with-notes/
description: Learn how to render presentation notes and other document annotations using GroupDocs.Viewer Cloud API in this step-by-step tutorial for developers.
weight: 70
---

## Tutorial: How to Render Documents with Notes

In this tutorial, you'll learn how to render documents with notes using GroupDocs.Viewer Cloud API. Notes are particularly common in presentation files, where presenters add speaking points and additional information that isn't shown during the presentation but is valuable for understanding the content.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Enable rendering of notes in presentation files
- Control the visibility of notes in your rendered documents
- Apply note rendering across different output formats
- Understand which document types support note rendering

## Prerequisites

Before you begin this tutorial, you need:

1. A GroupDocs.Viewer Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials from the GroupDocs Cloud Dashboard
3. Basic understanding of REST APIs
4. Development environment for your preferred language (C#, Java, Python, etc.)
5. [GroupDocs.Viewer Cloud SDK](https://github.com/groupdocs-viewer-cloud) installed for your language of choice

## The Practical Scenario

Imagine you're developing a training platform where instructors upload presentations for students. The instructors include detailed notes with each slide to provide context, additional information, and learning prompts. You need to ensure these valuable notes are visible to students when they view the presentations online.

## Step 1: Understanding Note Rendering

By default, GroupDocs.Viewer Cloud does not include notes when rendering documents. However, you can enable note rendering by setting the `RenderNotes` property to `true` in your rendering options.

This feature is primarily designed for:
- PowerPoint presentations (.pptx, .ppt)
- Other presentation formats that support notes

When enabled, notes will be displayed below each slide in the rendered output.

## Step 2: Set Up Your Project

First, set up authentication with your Client ID and Client Secret:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = ""; // Get your Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get your Client ID from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);
```

## Step 3: Rendering a Presentation with Notes to HTML

Let's start with a basic example of rendering a PowerPoint presentation with notes:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_notes.pptx" // Document in your cloud storage
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new RenderOptions
    {
        RenderNotes = true // Enable notes rendering
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 4: Rendering Notes in PDF Output

You can also include notes when converting presentations to PDF:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_notes.pptx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    
    RenderOptions = new RenderOptions
    {
        RenderNotes = true
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 5: Rendering Notes in Image Output

When rendering to image formats like JPG or PNG, you can still include the notes:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_notes.pptx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PNG,
    
    RenderOptions = new RenderOptions
    {
        RenderNotes = true
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 6: Combining Note Rendering with Other Options

You can combine note rendering with other rendering options, such as watermarking:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_notes.pptx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new RenderOptions
    {
        RenderNotes = true
    },
    
    Watermark = new Watermark
    {
        Text = "DRAFT",
        Color = "#FF0000",
        Position = Watermark.PositionEnum.Diagonal
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Try It Yourself

Now it's your turn to experiment with note rendering:

1. Try rendering the same presentation with and without notes to see the difference
2. Test with presentations that have extensive notes to see how they appear in the output
3. Compare how notes look in different output formats (HTML, PDF, images)
4. Implement a toggle in your application to let users show or hide notes

## Common Issues and Troubleshooting

Issue: Notes aren't appearing despite enabling RenderNotes  
Solution: Ensure the presentation actually contains notes. Not all slides may have notes attached.

Issue: Notes are appearing but the formatting looks different  
Solution: Note formatting may be simplified in the rendered output compared to the original PowerPoint view.

Issue: Notes are cut off or truncated in the rendered output  
Solution: Long notes may be truncated in some formats. Consider adjusting page settings or using a format that better accommodates lengthy notes.

## Complete Code Examples

### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Render presentation with notes
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer <jwt token>" \
-d "{
  'FileInfo': {
    'FilePath': 'with_notes.pptx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'RenderNotes': true
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
        FilePath = "with_notes.pptx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,

    RenderOptions = new RenderOptions
    {
        RenderNotes = true
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
view_options.file_info.file_path = "with_notes.pptx"
view_options.view_format = "HTML"
view_options.render_options = groupdocs_viewer_cloud.HtmlOptions()
view_options.render_options.render_notes = True

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
fileInfo.setFilePath("with_notes.pptx");
ViewOptions viewOptions = new ViewOptions();
viewOptions.setFileInfo(fileInfo);
viewOptions.setViewFormat(ViewFormatEnum.HTML);
RenderOptions renderOptions = new RenderOptions();
renderOptions.setRenderNotes(true);
viewOptions.setRenderOptions(renderOptions);

ViewResult response = apiInstance.createView(new CreateViewRequest(viewOptions));
```

## What You've Learned

In this tutorial, you've learned:

- How to enable note rendering in presentations
- How to apply note rendering across different output formats
- Techniques for combining note rendering with other rendering options
- How to handle issues with note rendering in different scenarios

## Further Practice

To solidify your knowledge, try these exercises:

1. Create a presentation viewer that toggles between showing and hiding notes
2. Build a system that extracts only the notes from presentations for review
3. Implement a feature that allows notes to be printed separately from slides
4. Develop a tool that compares presenter notes across multiple presentations


## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Did you find this tutorial helpful? Do you have questions about implementing note rendering in your application? Let us know in the [GroupDocs.Viewer Cloud forum](https://forum.groupdocs.cloud/c/viewer/9).
