---
title: Document Information with InfoResult Tutorial
url: /data-structures/info-result/
weight: 8
description: Learn how to extract and analyze document information in this step-by-step InfoResult tutorial for GroupDocs.Viewer Cloud API
---

# Tutorial: Document Information with InfoResult

## Learning Objectives

In this tutorial, you'll learn:
- How to use the Document Info API to extract document metadata
- Techniques for analyzing document structure and content
- Methods for accessing page dimensions and text content
- How to work with format-specific information like CAD layouts and PDF permissions

## Prerequisites

Before starting this tutorial:
- Create a [GroupDocs Cloud account](https://dashboard.groupdocs.cloud) and get your Client ID and Client Secret
- Have basic knowledge of RESTful API concepts
- Prepare sample documents of various formats (PDF, DOCX, CAD, etc.)

## Introduction to InfoResult

The InfoResult data structure is what you receive when calling the Document Info API. It provides comprehensive information about a document, including its format, pages, text content, and format-specific details. This information is valuable for document analysis, pre-rendering decisions, and building smarter document viewing applications.

Think of InfoResult as a document analyzer that lets you understand a document's content and structure before deciding how to process or display it.

## Understanding the InfoResult Structure

InfoResult contains several key components:

- FormatExtension/Format: Document format information
- Pages: List of document pages with dimensions and content
- Attachments: List of document attachments (if any)
- Format-specific information: Special data for CAD files, PDF documents, etc.

Let's explore these components with practical examples.

## Tutorial Steps

### Step 1: Getting Basic Document Information

Let's start by retrieving basic document information:

```python
# Tutorial Code Example: Getting basic document information
import os
from groupdocs_viewer_cloud import Configuration, ViewerApi, InfoOptions, FileInfo

# Configure the API client
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
viewer_api = ViewerApi.from_config(configuration)

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/sample.pdf"

# Create info options
info_options = InfoOptions()
info_options.file_info = file_info

# Get document information
info_result = viewer_api.get_info(info_options)

# Display basic document information
print(f"Document format: {info_result.format}")
print(f"File extension: {info_result.format_extension}")
print(f"Total pages: {len(info_result.pages)}")

# Display page dimensions for the first page
if info_result.pages:
    first_page = info_result.pages[0]
    print(f"\nFirst page information:")
    print(f"  - Page number: {first_page.number}")
    print(f"  - Width: {first_page.width} pixels")
    print(f"  - Height: {first_page.height} pixels")
    print(f"  - Visible: {first_page.visible}")
```

### Step 2: Extracting Text Content from Pages

InfoResult can include text content from document pages when requested:

```python
# Tutorial Code Example: Extracting text content
from groupdocs_viewer_cloud import InfoOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/text-document.docx"

# Create info options with text extraction
info_options = InfoOptions()
info_options.file_info = file_info
info_options.extract_text = True  # Request text extraction

# Get document information with text content
info_result = viewer_api.get_info(info_options)

# Display text content statistics
print(f"Document has {len(info_result.pages)} pages with text content")

# Analyze text content from the first page
if info_result.pages and hasattr(info_result.pages[0], 'lines') and info_result.pages[0].lines:
    first_page = info_result.pages[0]
    lines_count = len(first_page.lines)
    print(f"\nText content from first page:")
    print(f"  - Total lines: {lines_count}")
    
    # Show first few lines as example
    for i, line in enumerate(first_page.lines[:3]):
        print(f"  - Line {i+1}: {line.value}")
        if hasattr(line, 'words') and line.words:
            word_count = len(line.words)
            print(f"    Contains {word_count} words")
    
    # Count total words on the page
    total_words = sum(len(line.words) if hasattr(line, 'words') else 0 for line in first_page.lines)
    print(f"\nTotal words on first page: {total_words}")
else:
    print("\nNo text content extracted or available")
```

### Step 3: Working with Document Attachments

Some documents may contain attachments that you can identify with InfoResult:

```python
# Tutorial Code Example: Analyzing document attachments
from groupdocs_viewer_cloud import InfoOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/with-attachments.msg"  # Email with attachments

# Create info options
info_options = InfoOptions()
info_options.file_info = file_info

# Get document information
info_result = viewer_api.get_info(info_options)

# Check for attachments
if info_result.attachments and len(info_result.attachments) > 0:
    print(f"Document contains {len(info_result.attachments)} attachments:")
    for i, attachment in enumerate(info_result.attachments):
        print(f"  {i+1}. {attachment.name}")
    
    # Generate example code for handling attachments
    print("\nExample code for handling attachments:")
    print("python")
    print("# Process each attachment")
    print("for attachment in info_result.attachments:")
    print("    # Get the attachment name")
    print("    attachment_name = attachment.name")
    print("    print(f'Processing attachment: {attachment_name}')")
    print("    ")
    print("    # Create file info for the attachment")
    print("    attachment_file_info = FileInfo()")
    print("    attachment_file_info.file_path = f'documents/attachments/{attachment_name}'")
    print("    ")
    print("    # Now you can process the attachment separately")
    print("    # For example, render it to HTML")
    print("    attachment_view_options = ViewOptions()")
    print("    attachment_view_options.file_info = attachment_file_info")
    print("    attachment_view_options.view_format = 'HTML'")
    print("    attachment_result = viewer_api.view(attachment_view_options)")
    print("")
else:
    print("Document does not contain attachments")
```

### Step 4: Analyzing PDF-Specific Information

For PDF documents, InfoResult provides special information about security permissions:

```python
# Tutorial Code Example: Analyzing PDF-specific information
from groupdocs_viewer_cloud import InfoOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/secured.pdf"

# Create info options
info_options = InfoOptions()
info_options.file_info = file_info

# Get document information
info_result = viewer_api.get_info(info_options)

# Check for PDF-specific information
if hasattr(info_result, 'pdf_view_info') and info_result.pdf_view_info:
    print(f"PDF document information:")
    print(f"  - Printing allowed: {info_result.pdf_view_info.printing_allowed}")
    
    # Make a decision based on printing permission
    if info_result.pdf_view_info.printing_allowed:
        print("  This document can be printed")
    else:
        print("  This document has printing restrictions")
    
    # Print Security Considerations
    print("\nSecurity considerations for this PDF:")
    if not info_result.pdf_view_info.printing_allowed:
        print("  - Disable print button in your viewer application")
        print("  - Add a watermark indicating printing is not allowed")
    
else:
    print("No PDF-specific information available or not a PDF document")
```

### Step 5: Working with CAD Drawings Information

For CAD drawings, InfoResult provides details about layouts and layers:

```python
# Tutorial Code Example: Analyzing CAD drawing information
from groupdocs_viewer_cloud import InfoOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/drawing.dwg"

# Create info options
info_options = InfoOptions()
info_options.file_info = file_info

# Get document information
info_result = viewer_api.get_info(info_options)

# Check for CAD-specific information
if hasattr(info_result, 'cad_view_info') and info_result.cad_view_info:
    print(f"CAD drawing information:")
    
    # Display layouts
    if hasattr(info_result.cad_view_info, 'layouts') and info_result.cad_view_info.layouts:
        print(f"  - Available layouts ({len(info_result.cad_view_info.layouts)}):")
        for layout in info_result.cad_view_info.layouts:
            print(f"    * {layout.name} - {layout.width}x{layout.height}")
    
    # Display layers
    if hasattr(info_result.cad_view_info, 'layers') and info_result.cad_view_info.layers:
        print(f"\n  - Available layers ({len(info_result.cad_view_info.layers)}):")
        for layer in info_result.cad_view_info.layers:
            visibility = "Visible" if layer.visible else "Hidden"
            print(f"    * {layer.name} - {visibility}")
    
    # Generate example code for rendering specific layout and layers
    print("\nExample code for rendering specific layout and layers:")
    print("python")
    print("from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, CadOptions, FileInfo")
    print("")
    print("# Set up file info")
    print("file_info = FileInfo()")
    print("file_info.file_path = 'documents/drawing.dwg'")
    print("")
    print("# Create CAD options")
    print("cad_options = CadOptions()")
    if info_result.cad_view_info.layouts and len(info_result.cad_view_info.layouts) > 0:
        print(f"cad_options.layout_name = '{info_result.cad_view_info.layouts[0].name}'  # Render specific layout")
    if info_result.cad_view_info.layers and len(info_result.cad_view_info.layers) > 0:
        visible_layers = [layer.name for layer in info_result.cad_view_info.layers if layer.visible]
        if visible_layers:
            layers_str = "', '".join(visible_layers[:3])  # Take up to 3 layers for example
            print(f"cad_options.layers = ['{layers_str}']  # Render specific layers")
    print("")
    print("# Set up rendering options")
    print("html_options = HtmlOptions()")
    print("html_options.cad_options = cad_options")
    print("")
    print("# Set up view options")
    print("view_options = ViewOptions()")
    print("view_options.file_info = file_info")
    print("view_options.view_format = 'HTML'")
    print("view_options.render_options = html_options")
    print("")
    print("# Render the CAD drawing")
    print("result = viewer_api.view(view_options)")
    print("")
else:
    print("No CAD-specific information available or not a CAD drawing")
```

### Step 6: Working with Archive Information

For archive files, InfoResult provides details about the folder structure:

```python
# Tutorial Code Example: Analyzing archive information
from groupdocs_viewer_cloud import InfoOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/archive.zip"

# Create info options
info_options = InfoOptions()
info_options.file_info = file_info

# Get document information
info_result = viewer_api.get_info(info_options)

# Check for archive-specific information
if hasattr(info_result, 'archive_view_info') and info_result.archive_view_info:
    print(f"Archive information:")
    
    # Display folders
    if hasattr(info_result.archive_view_info, 'folders') and info_result.archive_view_info.folders:
        print(f"  - Folders in archive ({len(info_result.archive_view_info.folders)}):")
        for folder in info_result.archive_view_info.folders:
            print(f"    * {folder}")
    
    # Generate example code for rendering a specific folder
    print("\nExample code for rendering a specific folder from the archive:")
    print("python")
    print("from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, ArchiveOptions, FileInfo")
    print("")
    print("# Set up file info")
    print("file_info = FileInfo()")
    print("file_info.file_path = 'documents/archive.zip'")
    print("")
    print("# Create archive options")
    print("html_options = HtmlOptions()")
    print("html_options.archive_options = ArchiveOptions()")
    if info_result.archive_view_info.folders and len(info_result.archive_view_info.folders) > 0:
        print(f"html_options.archive_options.folder = '{info_result.archive_view_info.folders[0]}'  # Render specific folder")
    print("html_options.archive_options.items_per_page = 15  # Items per page when rendering")
    print("")
    print("# Set up view options")
    print("view_options = ViewOptions()")
    print("view_options.file_info = file_info")
    print("view_options.view_format = 'HTML'")
    print("view_options.render_options = html_options")
    print("")
    print("# Render the archive contents")
    print("result = viewer_api.view(view_options)")
    print("")
else:
    print("No archive-specific information available or not an archive file")
```

### Step 7: Building a Pre-Render Analysis Tool

Now let's combine our knowledge to build a comprehensive document analysis tool:

```python
# Tutorial Code Example: Comprehensive document analysis tool
from groupdocs_viewer_cloud import InfoOptions, FileInfo
import json

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/unknown-document.pdf"  # This could be any format

# Create info options with text extraction
info_options = InfoOptions()
info_options.file_info = file_info
info_options.extract_text = True  # Get text content

# Get document information
info_result = viewer_api.get_info(info_options)

# Create comprehensive analysis report
print("Document Analysis Report")
print("======================\n")

# Basic document information
print(f"Document Type: {info_result.format}")
print(f"Extension: {info_result.format_extension}")
print(f"Total Pages: {len(info_result.pages)}")

# Page dimensions analysis
if info_result.pages:
    # Collect page dimensions
    widths = [page.width for page in info_result.pages if hasattr(page, 'width')]
    heights = [page.height for page in info_result.pages if hasattr(page, 'height')]
    
    if widths and heights:
        # Analyze page sizes
        min_width = min(widths)
        max_width = max(widths)
        min_height = min(heights)
        max_height = max(heights)
        avg_width = sum(widths) / len(widths)
        avg_height = sum(heights) / len(heights)
        
        # Check if all pages have same dimensions
        uniform_pages = min_width == max_width and min_height == max_height
        
        print("\nPage Dimensions Analysis:")
        print(f"  - Page size is {'uniform' if uniform_pages else 'variable'}")
        if uniform_pages:
            print(f"  - All pages are {widths[0]}x{heights[0]} pixels")
        else:
            print(f"  - Width range: {min_width}-{max_width} pixels (avg: {avg_width:.1f})")
            print(f"  - Height range: {min_height}-{max_height} pixels (avg: {avg_height:.1f})")
        
        # Determine optimal rendering settings based on page sizes
        print("\nRecommended Rendering Settings:")
        if max_width > 1000 or max_height > 1400:
            print("  - Use high-resolution rendering for detailed content")
        
        # Page orientation analysis
        portrait_pages = sum(1 for w, h in zip(widths, heights) if h > w)
        landscape_pages = sum(1 for w, h in zip(widths, heights) if w > h)
        square_pages = sum(1 for w, h in zip(widths, heights) if w == h)
        
        print("\nPage Orientation Analysis:")
        print(f"  - Portrait pages: {portrait_pages}")
        print(f"  - Landscape pages: {landscape_pages}")
        print(f"  - Square pages: {square_pages}")
        
        if landscape_pages > 0:
            print("  - Note: Document contains landscape pages, consider appropriate viewer layout")

# Text content analysis
has_text = False
text_lines_count = 0
total_words = 0

for page in info_result.pages:
    if hasattr(page, 'lines') and page.lines:
        has_text = True
        text_lines_count += len(page.lines)
        page_words = sum(len(line.words) if hasattr(line, 'words') else 0 for line in page.lines)
        total_words += page_words

if has_text:
    print("\nText Content Analysis:")
    print(f"  - Total text lines: {text_lines_count}")
    print(f"  - Approximate word count: {total_words}")
    print(f"  - Average words per page: {total_words / len(info_result.pages):.1f}")
    
    # Determine if document is text-heavy
    if total_words > 500:
        print("  - Document is text-heavy, consider enabling text search functionality")
else:
    print("\nText Content Analysis:")
    print("  - No text content found or extracted")
    print("  - Document may be image-based or scanned")
    print("  - Consider OCR processing if text search is required")

# Format-specific analysis
print("\nFormat-Specific Information:")

# PDF analysis
if hasattr(info_result, 'pdf_view_info') and info_result.pdf_view_info:
    print("PDF Document Properties:")
    print(f"  - Printing allowed: {info_result.pdf_view_info.printing_allowed}")
    if not info_result.pdf_view_info.printing_allowed:
        print("  - Security Note: Document has printing restrictions")

# CAD analysis
if hasattr(info_result, 'cad_view_info') and info_result.cad_view_info:
    print("CAD Drawing Properties:")
    if hasattr(info_result.cad_view_info, 'layouts') and info_result.cad_view_info.layouts:
        print(f"  - Contains {len(info_result.cad_view_info.layouts)} layouts")
    if hasattr(info_result.cad_view_info, 'layers') and info_result.cad_view_info.layers:
        visible_layers = sum(1 for layer in info_result.cad_view_info.layers if layer.visible)
        hidden_layers = len(info_result.cad_view_info.layers) - visible_layers
        print(f"  - Contains {len(info_result.cad_view_info.layers)} layers " +
              f"({visible_layers} visible, {hidden_layers} hidden)")

# Archive analysis
if hasattr(info_result, 'archive_view_info') and info_result.archive_view_info:
    print("Archive Properties:")
    if hasattr(info_result.archive_view_info, 'folders') and info_result.archive_view_info.folders:
        print(f"  - Contains {len(info_result.archive_view_info.folders)} folders")

# Project management analysis
if hasattr(info_result, 'project_management_view_info') and info_result.project_management_view_info:
    print("Project Management Properties:")
    if hasattr(info_result.project_management_view_info, 'start_date'):
        print(f"  - Project start date: {info_result.project_management_view_info.start_date}")
    if hasattr(info_result.project_management_view_info, 'end_date'):
        print(f"  - Project end date: {info_result.project_management_view_info.end_date}")

# Outlook data analysis
if hasattr(info_result, 'outlook_view_info') and info_result.outlook_view_info:
    print("Outlook Data Properties:")
    if hasattr(info_result.outlook_view_info, 'folders') and info_result.outlook_view_info.folders:
        print(f"  - Contains {len(info_result.outlook_view_info.folders)} folders")

# Final recommendations
print("\nRendering Recommendations:")
print("  - Recommended format: ", end="")
if has_text:
    print("HTML (for text searchability)")
elif hasattr(info_result, 'cad_view_info') and info_result.cad_view_info:
    print("PNG (for crisp lines and details)")
else:
    print("PDF (for compatibility and document fidelity)")

# Generate rendering code based on analysis
print("\nSuggested Rendering Code:")
print("python")
print("from groupdocs_viewer_cloud import ViewOptions, FileInfo")

if has_text:
    print("from groupdocs_viewer_cloud import HtmlOptions")
    print("\n# Create HTML options for text searchability")
    print("html_options = HtmlOptions()")
    print("html_options.is_responsive = True")
    
    if landscape_pages > 0:
        print("# Enable proper layout for landscape pages")
        
    if hasattr(info_result, 'cad_view_info') and info_result.cad_view_info:
        print("# Configure CAD options")
        print("html_options.cad_options = CadOptions()")
        if info_result.cad_view_info.layouts and len(info_result.cad_view_info.layouts) > 0:
            print(f"html_options.cad_options.layout_name = '{info_result.cad_view_info.layouts[0].name}'")
        
    print("\n# Set up view options")
    print("view_options = ViewOptions()")
    print("view_options.file_info = file_info")
    print("view_options.view_format = 'HTML'")
    print("view_options.render_options = html_options")
    
elif hasattr(info_result, 'cad_view_info') and info_result.cad_view_info:
    print("from groupdocs_viewer_cloud import ImageOptions, CadOptions")
    print("\n# Create PNG options for CAD drawing")
    print("image_options = ImageOptions()")
    print("image_options.cad_options = CadOptions()")
    if info_result.cad_view_info.layouts and len(info_result.cad_view_info.layouts) > 0:
        print(f"image_options.cad_options.layout_name = '{info_result.cad_view_info.layouts[0].name}'")
    print("image_options.width = 1200  # High resolution for details")
    
    print("\n# Set up view options")
    print("view_options = ViewOptions()")
    print("view_options.file_info = file_info")
    print("view_options.view_format = 'PNG'")
    print("view_options.render_options = image_options")
    
else:
    print("from groupdocs_viewer_cloud import PdfOptions")
    print("\n# Create PDF options for general compatibility")
    print("pdf_options = PdfOptions()")
    
    print("\n# Set up view options")
    print("view_options = ViewOptions()")
    print("view_options.file_info = file_info")
    print("view_options.view_format = 'PDF'")
    print("view_options.render_options = pdf_options")

print("\n# Render the document")
print("result = viewer_api.view(view_options)")
print("")
```

## Try It Yourself

Now that you've learned how to use InfoResult, try these exercises:

1. Create a document analyzer that compares text density across pages to identify text-heavy sections
2. Build a CAD drawing inspector that lists all layouts and layers with their properties
3. Develop a batch processor that examines multiple documents and recommends optimal viewing settings for each

## Troubleshooting Tips

- Missing text content: Ensure that `extract_text` is set to `true` in your InfoOptions
- Format-specific information not available: Verify that your document is actually of the expected format
- Page dimensions are zero: Some document formats may not report dimensions until rendered
- Text extraction slow for large documents: Consider extracting text only when needed or limiting to specific pages

## What You've Learned

In this tutorial, you've mastered:
- How to retrieve comprehensive document information with InfoResult
- Extracting and analyzing text content from documents
- Accessing format-specific information for PDF, CAD, and other formats
- Building intelligent document analysis systems
- Creating optimal rendering settings based on document analysis

## Next Steps

Ready to learn how to clean up after rendering operations? Continue your learning with our [DeleteViewOptions Tutorial](/data-structures/delete-view-options) to discover how to properly manage rendered document views.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about document information extraction? Post them on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
