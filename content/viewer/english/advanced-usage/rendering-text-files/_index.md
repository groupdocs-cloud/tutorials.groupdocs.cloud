---
title: How to Render Text Files with GroupDocs.Viewer Cloud API Tutorial
url: /advanced-usage/rendering-text-files/
description: Learn how to render plain text files with proper formatting, line breaks, and character limits in this comprehensive step-by-step tutorial
weight: 130
---

# Tutorial: How to Render Text Files with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to render plain text files (TXT, CSV, and other text formats) with advanced formatting options using GroupDocs.Viewer Cloud API. By the end of this tutorial, you'll be able to:

- Render text files with proper formatting
- Control the maximum number of characters per row
- Limit the number of rows per page
- Apply custom styles to rendered text
- Handle different text encodings

## Prerequisites

Before starting this tutorial, you should have:

1. A GroupDocs.Viewer Cloud account (or [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials
3. Sample text files to test with
4. Basic understanding of REST API concepts
5. Familiarity with your preferred programming language (C#, Java, Python, etc.)

## Use Case Scenario

Imagine you're developing a code review application that needs to display source code and log files with proper formatting. Users need to view these files with consistent line breaks, character limits, and pagination to ensure readability across different screen sizes. Your application needs to render text files efficiently while maintaining their structure and formatting.

## Tutorial Steps

### 1. Basic Text File Rendering

Let's start by rendering a simple text file with default settings.

#### Try it yourself

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now render a basic text file
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.txt'
  },
  'ViewFormat': 'HTML'
}"
```

#### What you should see

After executing the API call, you'll receive a JSON response with links to the rendered HTML pages. When you open these pages, you'll see the text file content rendered with default formatting.

### 2. Controlling Characters Per Row

Long lines of text can be difficult to read, especially on smaller screens. GroupDocs.Viewer Cloud allows you to specify the maximum number of characters per row, automatically wrapping text when it exceeds this limit.

#### Try it yourself

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.txt'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'TextOptions': {
      'MaxCharsPerRow': 100
    }
  }
}"
```

This will limit each line to a maximum of 100 characters, wrapping any longer lines.

#### Troubleshooting Tip

If you notice that text is being wrapped at odd places, disrupting the reading flow, try adjusting the `MaxCharsPerRow` value. A common practice is to use values between 80 and 120 characters, which is standard for most code style guides.

### 3. Limiting Rows Per Page

For very long text files, it's helpful to split the content into multiple pages. You can control how many rows appear on each page using the `MaxRowsPerPage` option.

#### Try it yourself

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.txt'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'TextOptions': {
      'MaxRowsPerPage': 100
    }
  }
}"
```

This will create multiple pages if your text file has more than 100 rows, with each page containing up to 100 rows.

### 4. Combining Character and Row Limits

For optimal text rendering, you can combine both character and row limits:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.txt'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'TextOptions': {
      'MaxCharsPerRow': 100,
      'MaxRowsPerPage': 100
    }
  }
}"
```

#### Learning checkpoint

What would happen if you set a very low value for `MaxCharsPerRow` (e.g., 20) and a low value for `MaxRowsPerPage` (e.g., 10)? Try it and observe how it affects the pagination and overall readability of the text.

### 5. Choosing the Right Output Format

For text files, HTML is usually the preferred output format as it preserves formatting better than image formats. However, you can also render text files to images if needed:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.txt'
  },
  'ViewFormat': 'PNG',
  'RenderOptions': {
    'TextOptions': {
      'MaxCharsPerRow': 100,
      'MaxRowsPerPage': 50
    }
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

// Example: Render text file with character and row limits
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/sample.txt"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    RenderOptions = new HtmlOptions
    {
        TextOptions = new TextOptions
        {
            MaxCharsPerRow = 100,
            MaxRowsPerPage = 100
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

# Example: Render text file with character and row limits
view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "SampleFiles/sample.txt"
view_options.view_format = "HTML"
view_options.render_options = groupdocs_viewer_cloud.HtmlOptions()
view_options.render_options.text_options = groupdocs_viewer_cloud.TextOptions()    
view_options.render_options.text_options.max_chars_per_row = 100
view_options.render_options.text_options.max_rows_per_page = 100

request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
response = apiInstance.create_view(request)
```

## What You've Learned

In this tutorial, you've learned how to:
- Render text files with proper formatting
- Control the maximum number of characters per row
- Limit the number of rows per page
- Choose the right output format for text rendering

## Further Practice

Try experimenting with different values for `MaxCharsPerRow` and `MaxRowsPerPage` to find the optimal settings for different types of text content. For example, code files might benefit from different settings than log files or plain text documents.

## Next Steps

You've now completed our tutorial series on advanced usage of GroupDocs.Viewer Cloud API! To continue your learning journey, explore our comprehensive [API documentation](https://docs.groupdocs.cloud/viewer/) or check out our other product tutorials.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

If you have questions about this tutorial or any aspect of using GroupDocs.Viewer Cloud API, please visit our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
