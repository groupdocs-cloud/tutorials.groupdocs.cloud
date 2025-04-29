---
title: How to Render Documents with Comments in GroupDocs.Viewer Cloud Tutorial
url: /advanced-usage/render-document-with-comments/
description: Learn how to control comment rendering in documents with GroupDocs.Viewer Cloud API in this step-by-step tutorial for developers.
weight: 60
---

## Tutorial: How to Render Documents with Comments

In this tutorial, you'll learn how to control the rendering of comments in documents using GroupDocs.Viewer Cloud API. By default, comments are hidden when rendering documents, but many collaborative workflows require displaying these comments for review purposes.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Enable comment rendering for supported document types
- Control whether comments appear in the rendered output
- Apply comment rendering settings to various document formats
- Understand which document types support comment rendering

## Prerequisites

Before you begin this tutorial, you need:

1. A GroupDocs.Viewer Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials from the GroupDocs Cloud Dashboard
3. Basic understanding of REST APIs
4. Development environment for your preferred language (C#, Java, Python, etc.)
5. [GroupDocs.Viewer Cloud SDK](https://github.com/groupdocs-viewer-cloud) installed for your language of choice

## The Practical Scenario

Imagine you're developing a document review system where team members collaborate on documents by adding comments and feedback. You need to ensure these comments are visible when the documents are rendered for web viewing, enabling effective collaboration without requiring users to open the original document files.

## Step 1: Understanding Comment Rendering

By default, GroupDocs.Viewer Cloud does not render comments when converting documents to HTML, PDF, or images. The API provides a `RenderComments` property that you can set to `true` to include comments in the rendered output.

This feature is supported for several document types:
- Microsoft Word documents (.docx, .doc)
- PDF documents
- Presentations (.pptx, .ppt)
- Spreadsheets (.xlsx, .xls)

## Step 2: Set Up Your Project

First, set up authentication with your Client ID and Client Secret:

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = ""; // Get your Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get your Client ID from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);
```

## Step 3: Enabling Comment Rendering for Word Documents

Let's start with a basic example of rendering a Word document with comments:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_comment.docx" // Document in your cloud storage
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new RenderOptions
    {
        RenderComments = true // Enable comment rendering
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 4: Rendering Comments in PDF Documents

The same approach works for PDF documents with comments or annotations:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "annotated_document.pdf"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new RenderOptions
    {
        RenderComments = true
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 5: Rendering Comments in Presentations

For presentation files that contain comments or feedback:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "presentation_with_comments.pptx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    
    RenderOptions = new RenderOptions
    {
        RenderComments = true
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Step 6: Rendering Comments When Converting to PDF

You can also enable comment rendering when converting documents to PDF format:

```csharp
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "with_comment.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.PDF,
    
    RenderOptions = new RenderOptions
    {
        RenderComments = true
    }
};

var response = apiInstance.CreateView(new CreateViewRequest(viewOptions));
```

## Try It Yourself

Now it's your turn to experiment with comment rendering:

1. Try rendering the same document with and without comments to see the difference
2. Test with different document types to understand how comments appear in each format
3. Combine comment rendering with other options like watermarking or page selection
4. Implement a toggle in your application to let users choose whether to view comments

## Common Issues and Troubleshooting

Issue: Comments aren't appearing despite enabling RenderComments  
Solution: Ensure the document actually contains comments. Some comments may be resolved or hidden in the original document.

Issue: Comment rendering works for one document type but not for another  
Solution: Not all document formats support comments. Check if your document type is in the supported list.

Issue: Comments appear differently than in the original application  
Solution: The visual style of rendered comments may differ from the original application's display. This is normal as GroupDocs implements its own comment rendering.

## Complete Code Examples

### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Render document with comments
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer <jwt token>" \
-d "{
  'FileInfo': {
    'FilePath': 'with_comment.docx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'RenderComments': true
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
        FilePath = "with_comment.docx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,

    RenderOptions = new RenderOptions
    {
        RenderComments = true
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
view_options.file_info.file_path = "with_comment.docx"
view_options.view_format = "HTML"
view_options.render_options = groupdocs_viewer_cloud.HtmlOptions()
view_options.render_options.render_comments = True

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
fileInfo.setFilePath("with_comment.docx");
ViewOptions viewOptions = new ViewOptions();
viewOptions.setFileInfo(fileInfo);
viewOptions.setViewFormat(ViewFormatEnum.HTML);
RenderOptions renderOptions = new RenderOptions();
renderOptions.setRenderComments(true);
viewOptions.setRenderOptions(renderOptions);

ViewResult response = apiInstance.createView(new CreateViewRequest(viewOptions));
```

## What You've Learned

In this tutorial, you've learned:

- How to enable comment rendering in documents
- Which document types support comment rendering
- How to configure comment display across different output formats
- Practical applications for comment rendering in collaborative scenarios

## Further Practice

To solidify your knowledge, try these exercises:

1. Build a document review system with a toggle to show/hide comments
2. Create a system that extracts all comments from a document and displays them in a separate panel
3. Implement a feature that highlights text associated with each comment
4. Create a workflow where comments can be filtered by author or date

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

Did you find this tutorial helpful? Do you have questions about implementing comment rendering in your application? Let us know in the [GroupDocs.Viewer Cloud forum](https://forum.groupdocs.cloud/c/viewer/9).
