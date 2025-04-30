---
title: Controlling Document Rendering with RenderOptions Tutorial
weight: 2
url: /data-structures/render-options/
description: Learn how to master document rendering controls with this RenderOptions tutorial for GroupDocs.Viewer Cloud API
---

# Tutorial: Controlling Document Rendering with RenderOptions

## Learning Objectives

In this tutorial, you'll learn:
- How to use RenderOptions to control document rendering behavior
- Techniques for page selection, rotation, and formatting
- Format-specific rendering options for spreadsheets, CAD drawings, and more
- How to optimize rendered outputs for different document types

## Prerequisites

Before starting this tutorial:
- Complete the [ViewOptions Tutorial](/data-structures/view-options/)
- Have your GroupDocs.Viewer Cloud API credentials ready
- Prepare various test documents (spreadsheets, word documents, PDF files)

## Introduction to RenderOptions

RenderOptions is a powerful data structure that gives you fine-grained control over how documents are rendered in GroupDocs.Viewer Cloud. While ViewOptions handles the overall configuration, RenderOptions focuses specifically on the rendering process and appearance.

This data structure allows you to specify which pages to render, control page rotation, set default fonts, and configure format-specific rendering behaviors.

## Understanding the RenderOptions Structure

RenderOptions contains several key components:

- Page Selection Options: Control which pages are rendered
- Appearance Options: Configure fonts, encoding, and comments
- Format-Specific Options: Special settings for spreadsheets, CAD files, emails, etc.

Let's explore how to use these components effectively in real-world scenarios.

## Tutorial Steps

### Step 1: Basic Page Selection

One of the most common requirements is to render specific pages rather than the entire document:

```python
# Tutorial Code Example: Page selection with RenderOptions
import os
from groupdocs_viewer_cloud import Configuration, ViewerApi, ViewOptions, HtmlOptions, FileInfo

# Configure the API client
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
viewer_api = ViewerApi.from_config(configuration)

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/large-document.pdf"

# Create HTML options with page selection
html_options = HtmlOptions()
# Method 1: Start page and count
html_options.start_page_number = 5  # Start from page 5
html_options.count_pages_to_render = 3  # Render 3 pages total (5, 6, and 7)

# Method 2: Specific pages list
# Uncomment to use this method instead
# html_options.pages_to_render = [2, 5, 9]  # Only render pages 2, 5, and 9

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options

# Render the document
result = viewer_api.view(view_options)

print(f"Rendered {len(result.pages)} pages from the document")
for page in result.pages:
    print(f"Page {page.number} path: {page.path}")
```

### Step 2: Page Rotation

Sometimes you need to rotate pages for better viewing, especially for landscape documents:

```python
# Tutorial Code Example: Page rotation with RenderOptions
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, PageRotation

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/landscape-document.pdf"

# Create rotation settings for specific pages
page_rotations = []

# Create rotation for page 1 - rotate 90 degrees
rotation1 = PageRotation()
rotation1.page_number = 1  # First page
rotation1.rotation_angle = "On90Degree"  # 90-degree rotation
page_rotations.append(rotation1)

# Create rotation for page 2 - rotate 180 degrees
rotation2 = PageRotation()
rotation2.page_number = 2  # Second page
rotation2.rotation_angle = "On180Degree"  # 180-degree rotation
page_rotations.append(rotation2)

# Configure HTML options with rotations
html_options = HtmlOptions()
html_options.page_rotations = page_rotations

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered with page rotations applied")
```

### Step 3: Working with Spreadsheets

Spreadsheets have unique rendering requirements. Here's how to optimize their rendering:

```python
# Tutorial Code Example: Spreadsheet rendering options
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, SpreadsheetOptions

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/financial-report.xlsx"

# Configure spreadsheet-specific options
html_options = HtmlOptions()
html_options.spreadsheet_options = SpreadsheetOptions()
html_options.spreadsheet_options.paginate_sheets = True  # Split sheets into pages
html_options.spreadsheet_options.count_rows_per_page = 40  # Rows per page
html_options.spreadsheet_options.render_grid_lines = True  # Show gridlines
html_options.spreadsheet_options.render_headings = True  # Show row/column headings
html_options.spreadsheet_options.render_empty_rows = False  # Skip empty rows
html_options.spreadsheet_options.render_hidden_columns = False  # Skip hidden columns

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options

# Render the spreadsheet
result = viewer_api.view(view_options)

print(f"Spreadsheet rendered with pagination: {len(result.pages)} pages created")
```

### Step 4: Rendering CAD Drawings

CAD drawings require special handling for proper rendering:

```python
# Tutorial Code Example: CAD rendering options
from groupdocs_viewer_cloud import ViewOptions, PngOptions, CadOptions

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/engineering-drawing.dwg"

# Configure CAD-specific options
png_options = PngOptions()
png_options.cad_options = CadOptions()
png_options.cad_options.scale_factor = 1.5  # Increase scale by 50%
png_options.cad_options.width = 1200  # Output width in pixels
png_options.cad_options.height = 800  # Output height in pixels

# Specify layers to render (leave empty for all layers)
png_options.cad_options.layers = ["Layer1", "Dimensions"]

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "PNG"
view_options.render_options = png_options

# Render the CAD drawing
result = viewer_api.view(view_options)

print(f"CAD drawing rendered to PNG images: {len(result.pages)} pages")
```

### Step 5: Email Document Rendering

Email documents have their own specific formatting needs:

```python
# Tutorial Code Example: Email rendering options
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, EmailOptions, Field, FieldLabel

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/business-email.eml"

# Create custom field labels
field_labels = []

# Customize "From" field label
from_field = FieldLabel()
from_field.field = "From"
from_field.label = "Sender"
field_labels.append(from_field)

# Customize "To" field label
to_field = FieldLabel()
to_field.field = "To"
to_field.label = "Recipients"
field_labels.append(to_field)

# Configure email-specific options
html_options = HtmlOptions()
html_options.email_options = EmailOptions()
html_options.email_options.field_labels = field_labels
html_options.email_options.date_time_format = "MMM d, yyyy 'at' h:mm tt"
html_options.email_options.time_zone_offset = "+01:00"  # GMT+1

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options

# Render the email
result = viewer_api.view(view_options)

print(f"Email document rendered with custom field labels")
```

### Step 6: PDF Document Rendering Options

PDF documents can be rendered with specific options for better quality and performance:

```python
# Tutorial Code Example: PDF rendering options
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, PdfDocumentOptions

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/technical-specification.pdf"

# Configure PDF-specific options
html_options = HtmlOptions()
html_options.pdf_document_options = PdfDocumentOptions()
html_options.pdf_document_options.disable_chars_grouping = True  # Better text positioning
html_options.pdf_document_options.enable_layered_rendering = True  # Respect z-order
html_options.pdf_document_options.image_quality = "High"  # High quality images
html_options.pdf_document_options.render_text_as_image = False  # Keep text selectable

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options

# Render the PDF
result = viewer_api.view(view_options)

print(f"PDF document rendered with enhanced options: {len(result.pages)} pages")
```

### Step 7: Word Processing Document Options

Word documents can be rendered with these specialized options:

```python
# Tutorial Code Example: Word document rendering options
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, WordProcessingOptions

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/contract-with-changes.docx"

# Configure Word-specific options
html_options = HtmlOptions()
html_options.word_processing_options = WordProcessingOptions()
html_options.word_processing_options.render_tracked_changes = True  # Show track changes
html_options.word_processing_options.left_margin = 50  # Left margin in points
html_options.word_processing_options.right_margin = 50  # Right margin in points
html_options.word_processing_options.top_margin = 40  # Top margin in points
html_options.word_processing_options.bottom_margin = 40  # Bottom margin in points

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options

# Render the Word document
result = viewer_api.view(view_options)

print(f"Word document rendered with tracked changes visible")
```

## Try It Yourself

Now that you've learned how to use RenderOptions for different document types, try these exercises:

1. Render a spreadsheet showing only the print area
2. Create a rendering of a PDF document with text rendered as images for copy protection
3. Implement page rotation for a multi-page document where odd pages are rotated 90 degrees

## Troubleshooting Tips

- Format-specific options not applied: Make sure you're using the right format-specific option for your document type
- Page selection issues: Verify that page numbers start from 1, not 0
- Missing content in spreadsheets: Check if hidden rows/columns are being skipped with the rendering options

## What You've Learned

In this tutorial, you've mastered:
- How to select specific pages for rendering
- Techniques for rotating pages to improve document viewing
- Format-specific rendering for spreadsheets, CAD drawings, emails, and more
- Advanced options for optimizing rendered output quality

## Next Steps

Ready to learn more about format-specific rendering? Continue your journey with our [HTML Rendering Tutorial](/data-structures/htmloptions) or [Image Rendering Tutorial](/data-structures/image-options/).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about configuring RenderOptions? Post them on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
