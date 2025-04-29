---
title: How to Reorder Pages in GroupDocs.Viewer Cloud Tutorial
url: /advanced-usage/reorder-pages/
description: Learn how to customize page order when rendering documents with GroupDocs.Viewer Cloud API in this step-by-step tutorial for developers.
weight: 110
---

## Tutorial: How to Reorder Document Pages

In this tutorial, you'll learn how to reorder pages when rendering documents with GroupDocs.Viewer Cloud API. This allows you to present document pages in a different sequence than they appear in the original file, which can be useful for custom presentations, comparing pages, or reorganizing content.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Specify a custom order for document pages when rendering
- Render selected pages in any sequence you choose
- Implement custom page ordering across different document types
- Combine page reordering with other rendering options

## Prerequisites

Before you begin this tutorial, you need:

1. A GroupDocs.Viewer Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials from the GroupDocs Cloud Dashboard
3. Basic understanding of REST APIs
4. Development environment for your preferred language (C#, Java, Python, etc.)
5. [GroupDocs.Viewer Cloud SDK](https://github.com/groupdocs-viewer-cloud) installed for your language of choice

## The Practical Scenario

Imagine you're building a document comparison tool where users need to compare specific pages that aren't consecutive in the original document. For instance, a user wants to compare page 5 with page 2 side-by-side. Your application needs to render these pages in the requested order rather than their original sequence.

## Step 1: Understanding Page Reordering

GroupDocs.Viewer Cloud uses the `PagesToRender` property to specify which pages to render and in what order. This property takes a list of page numbers, and the rendering will follow exactly the sequence you provide.

Key points about page reordering:
- Page numbers start from 1 (not 0-based)
- You can render the same page multiple times if needed
- Pages will be rendered in exactly the order specified
- You can combine this with other rendering options

## Step 2: Set Up Your Project

First, set up authentication with your Client ID and Client Secret:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = ""; // Get your Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get your Client ID from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);
```

## Step 3: Reordering Pages - Basic Example

Let's render pages 2 and 1 in reverse order (2 first, then 1):

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.docx" // Document in your cloud storage
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new RenderOptions
    {
        PagesToRender = new List<int?> {2, 1} // Render page 2 first, then page 1
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 4: Rendering Non-Sequential Pages

Now, let's render pages in a completely custom order, for example pages 4, 2, and 8:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new RenderOptions
    {
        PagesToRender = new List<int?> {4, 2, 8} // Render pages 4, 2, and 8 in that order
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 5: Rendering the Same Page Multiple Times

In some scenarios, you might want to render the same page multiple times. This can be useful for comparison or emphasis:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new RenderOptions
    {
        PagesToRender = new List<int?> {3, 3, 3} // Render page 3 three times
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 6: Reordering Pages with PDF Output

The same page reordering works when rendering to PDF. This example renders pages 5, 2, and 1 in that order to a PDF document:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "sample.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    
    RenderOptions = new RenderOptions
    {
        PagesToRender = new List<int?> {5, 2, 1} // Render pages 5, 2, and 1 in that order
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Try It Yourself

Now it's your turn to experiment with page reordering:

1. Try creating a document with pages in reverse order (last to first)
2. Implement a "key pages" feature that extracts and renders only specific important pages
3. Create a before/after comparison by rendering the same page twice
4. Combine page reordering with other options like watermarking

## Common Issues and Troubleshooting

Issue: Specified page number is beyond the document's page count  
Solution: The API will skip invalid page numbers. Make sure to check the document's total page count first.

Issue: Empty result when all page numbers are invalid  
Solution: Ensure at least one valid page number is included in your PagesToRender list.

Issue: Original page order is maintained despite specifying PagesToRender  
Solution: Check that you're passing the page numbers as a collection/array/list, not as separate parameters.

## Complete Code Examples

### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Reorder pages
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
  'RenderOptions': {
    'PagesToRender': [2, 1]
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

    RenderOptions = new RenderOptions
    {
        // Pass page numbers in the order you want to render them
        PagesToRender = new List<int?> {2, 1}
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
view_options.render_options = groupdocs_viewer_cloud.HtmlOptions()
# Pass page numbers in the order you want to render them
view_options.render_options.pages_to_render = [2, 1]

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
RenderOptions renderOptions = new RenderOptions();
// Pass page numbers in the order you want to render them
renderOptions.addPagesToRenderItem(2);
renderOptions.addPagesToRenderItem(1);
viewOptions.setRenderOptions(renderOptions);

ViewResult response = apiInstance.createView(new CreateViewRequest(viewOptions));
```

## What You've Learned

In this tutorial, you've learned:

- How to render document pages in a custom order
- Ways to select and arrange specific pages for rendering
- How to repeat pages in the rendered output
- Techniques for implementing page reordering across different output formats

## Further Practice

To solidify your knowledge, try these exercises:

1. Create a document viewer that allows users to drag and drop pages to rearrange their order
2. Build a document comparison tool that places selected pages side by side
3. Implement a feature that highlights important pages by extracting and placing them at the beginning
4. Create a presentation mode that rearranges pages for optimal viewing flow

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Did you find this tutorial helpful? Do you have questions about implementing page reordering in your application? Let us know in the [GroupDocs.Viewer Cloud forum](https://forum.groupdocs.cloud/c/viewer/9).
