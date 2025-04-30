---
title: How to Render Spreadsheets with GroupDocs.Viewer Cloud API Tutorial
url: /advanced-usage/rendering-spreadsheets/
description: Learn how to render Excel and other spreadsheet formats with advanced options like grid lines, hidden rows, and text overflow in this step-by-step tutorial
weight: 120
---

# Tutorial: How to Render Spreadsheets with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn how to render Excel and other spreadsheet formats with advanced options using GroupDocs.Viewer Cloud API. By the end of this tutorial, you'll be able to:

- Render grid lines in spreadsheets
- Control the visibility of hidden rows and columns
- Manage text overflow in cells
- Skip rendering empty rows and columns
- Split worksheets into pages
- Render print areas only

## Prerequisites

Before starting this tutorial, you should have:

1. A GroupDocs.Viewer Cloud account (or [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials
3. Sample spreadsheet files (Excel, CSV, etc.) to test with
4. Basic understanding of REST API concepts
5. Familiarity with your preferred programming language (C#, Java, Python, etc.)

## Use Case Scenario

Imagine you're building a financial reporting application where users need to view spreadsheets with various formatting options. Some users want to see grid lines for better readability, while others need to view hidden data for auditing purposes. Additionally, you need to handle large spreadsheets efficiently by paginating them and skipping empty content.

## Tutorial Steps

### 1. Rendering Grid Lines

By default, GroupDocs.Viewer Cloud doesn't render grid lines in spreadsheets. Enable them to improve readability and visual structure.

#### Try it yourself

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now render the spreadsheet with grid lines
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.xlsx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'SpreadsheetOptions': {
      'RenderGridLines': true
    }
  }
}"
```

#### What you should see

After executing the API call, you'll receive a JSON response with links to the rendered HTML pages. When you open these pages, you'll see that the spreadsheet is rendered with visible grid lines, making it easier to distinguish between cells.

### 2. Showing Hidden Rows and Columns

Spreadsheets often contain hidden rows and columns that may contain important data. GroupDocs.Viewer Cloud lets you control whether these hidden elements are rendered.

#### Try it yourself

```bash
# Render spreadsheet with hidden rows and columns visible
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/with_hidden_row_and_column.xlsx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'SpreadsheetOptions': {
      'RenderHiddenColumns': true,
      'RenderHiddenRows': true
    }
  }
}"
```

#### Troubleshooting Tip

If you're not seeing all the data you expect in the rendered output, it's possible that some content is in hidden rows or columns. Try enabling both `RenderHiddenColumns` and `RenderHiddenRows` to ensure all data is visible.

### 3. Managing Text Overflow in Cells

When cells contain more text than can fit within their boundaries, you can control how the overflow is handled. GroupDocs.Viewer Cloud provides several options:

- HideText: Hides any text that doesn't fit in the cell
- OverlayIfNextIsEmpty: Allows text to overlay subsequent cells if they're empty (default)
- Overlay: Allows text to overlay subsequent cells even if they contain data
- AutoFitColumn: Expands the column width to fit the text

#### Try it yourself

```bash
# Render spreadsheet with overflow text hidden
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.xlsx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'SpreadsheetOptions': {
      'TextOverflowMode': 'HideText'
    }
  }
}"
```

#### Learning checkpoint

What happens if you change the `TextOverflowMode` from `HideText` to `AutoFitColumn`? Try it and observe how the column widths adjust to accommodate the full text content in each cell.

### 4. Skipping Empty Rows and Columns

When dealing with large spreadsheets that contain many empty rows or columns, you can improve rendering performance by skipping them.

#### Try it yourself

```bash
# Skip rendering empty rows
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/with_empty_row.xlsx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'SpreadsheetOptions': {
      'RenderEmptyRows': false
    }
  }
}"
```

Similarly, you can skip rendering empty columns:

```bash
# Skip rendering empty columns
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/with_empty_column.xlsx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'SpreadsheetOptions': {
      'RenderEmptyColumns': false
    }
  }
}"
```

### 5. Splitting Worksheets into Pages

For large worksheets, you can paginate the content by specifying the number of rows to render on each page.

#### Try it yourself

```bash
# Split worksheet into pages with 45 rows per page
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/sample.xlsx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'SpreadsheetOptions': {
      'PaginateSheets': true,
      'CountRowsPerPage': 45
    }
  }
}"
```

#### What you should see

The response will include multiple page links, with each page containing up to 45 rows from the worksheet. This makes it easier to navigate through large spreadsheets and improves loading performance.

### 6. Rendering Print Areas Only

If your spreadsheet has defined print areas, you can choose to render only those sections.

#### Try it yourself

```bash
# Render only print areas
curl -v "https://api.groupdocs.cloud/v2.0/viewer/view" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'SampleFiles/with_four_print_areas.xlsx'
  },
  'ViewFormat': 'HTML',
  'RenderOptions': {
    'SpreadsheetOptions': {
      'RenderPrintAreaOnly': true
    }
  }
}"
```

If the spreadsheet has multiple print areas, each print area will be rendered as a separate page.

## SDK Examples

### C# Example

```csharp
// For complete examples, see: https://github.com/groupdocs-viewer-cloud/groupdocs-viewer-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; 
string MyClientId = "YOUR_CLIENT_ID"; 

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new ViewApi(configuration);

// Example: Render spreadsheet with grid lines and show hidden rows/columns
var viewOptions = new ViewOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "SampleFiles/with_hidden_row_and_column.xlsx"
    },
    ViewFormat = ViewOptions.ViewFormatEnum.HTML,
    RenderOptions = new HtmlOptions
    {
        SpreadsheetOptions = new SpreadsheetOptions
        {
            RenderGridLines = true,
            RenderHiddenColumns = true,
            RenderHiddenRows = true
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

# Example: Split worksheets into pages
view_options = groupdocs_viewer_cloud.ViewOptions()
view_options.file_info = groupdocs_viewer_cloud.FileInfo()
view_options.file_info.file_path = "SampleFiles/sample.xlsx"
view_options.view_format = "HTML"
view_options.render_options = groupdocs_viewer_cloud.HtmlOptions()
view_options.render_options.spreadsheet_options = groupdocs_viewer_cloud.SpreadsheetOptions()
view_options.render_options.spreadsheet_options.paginate_sheets = True
view_options.render_options.spreadsheet_options.count_rows_per_page = 45

request = groupdocs_viewer_cloud.CreateViewRequest(view_options)
response = apiInstance.create_view(request)
```

## What You've Learned

In this tutorial, you've learned how to:
- Render grid lines in spreadsheets for improved readability
- Show hidden rows and columns for complete data visibility
- Control text overflow behavior in cells
- Optimize rendering by skipping empty rows and columns
- Split large worksheets into manageable pages
- Render only the defined print areas in a spreadsheet

## Further Practice

Try combining multiple spreadsheet rendering options in a single request. For example, enable grid lines, show hidden rows, and paginate the worksheet simultaneously to see how these options work together to create a customized viewing experience.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback and Questions

If you have questions about this tutorial or any aspect of using GroupDocs.Viewer Cloud API, please visit our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
