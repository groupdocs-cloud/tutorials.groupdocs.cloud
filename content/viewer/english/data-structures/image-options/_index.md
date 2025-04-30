---
title: Image Rendering with ImageOptions Tutorial
url: /data-structures/image-options/
weight: 4
description: Learn how to convert documents to high-quality images in this step-by-step ImageOptions tutorial for GroupDocs.Viewer Cloud API
---

# Tutorial: Image Rendering with ImageOptions

## Learning Objectives

In this tutorial, you'll learn:
- How to render documents to JPG and PNG formats using ImageOptions
- Techniques for controlling image dimensions and quality
- Methods for extracting text to enable search capabilities
- Best practices for image-based document viewing

## Prerequisites

Before starting this tutorial:
- Complete the [ViewOptions Tutorial](/data-structures/view-options)
- Have your GroupDocs.Viewer Cloud API credentials ready
- Prepare sample documents of various formats (PDF, DOCX, presentations)

## Introduction to ImageOptions

ImageOptions is a specialized data structure that inherits from RenderOptions and provides image-specific rendering configurations. When you need to convert documents to image formats (JPG or PNG), this structure gives you precise control over image dimensions, quality, and text extraction.

Image rendering is ideal for universal compatibility, protecting document content from editing, or when you need pixel-perfect representation across all devices.

## Understanding the ImageOptions Structure

ImageOptions adds these key image-specific properties to the base RenderOptions:

- Width/Height: Control output image dimensions
- JpegQuality: Adjust compression level for JPG output
- ExtractText: Enable text extraction for searchability

Let's explore these options with practical examples.

## Tutorial Steps

### Step 1: Basic Image Rendering

Let's start with a simple image rendering setup:

```python
# Tutorial Code Example: Basic image rendering
import os
from groupdocs_viewer_cloud import Configuration, ViewerApi, ViewOptions, ImageOptions, FileInfo

# Configure the API client
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
viewer_api = ViewerApi.from_config(configuration)

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/contract.pdf"

# Create basic image options
image_options = ImageOptions()
# Use default settings

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "JPG"  # Can also use "PNG"
view_options.render_options = image_options
view_options.output_path = "output/contract_images"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered to JPG images: {len(result.pages)} pages created")
for page in result.pages:
    print(f"Page {page.number} path: {page.path}")
```

### Step 2: Controlling Image Dimensions

You can specify exact dimensions for your output images:

```python
# Tutorial Code Example: Setting image dimensions
from groupdocs_viewer_cloud import ViewOptions, ImageOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/presentation.pptx"

# Create image options with custom dimensions
image_options = ImageOptions()
image_options.width = 1024  # Width in pixels
image_options.height = 768  # Height in pixels

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "PNG"  # PNG for better quality
view_options.render_options = image_options
view_options.output_path = "output/presentation_images"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered to 1024x768 PNG images: {len(result.pages)} pages")
```

### Step 3: Maintaining Aspect Ratio

To maintain the aspect ratio while resizing, you can set only one dimension:

```python
# Tutorial Code Example: Maintaining aspect ratio
from groupdocs_viewer_cloud import ViewOptions, ImageOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/magazine.pdf"

# Create image options preserving aspect ratio
image_options = ImageOptions()
image_options.width = 800  # Set only width
image_options.height = 0   # Height will be calculated automatically

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "JPG"
view_options.render_options = image_options
view_options.output_path = "output/magazine_images"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered with preserved aspect ratio")
print(f"All images have width of 800px with proportional height")
```

### Step 4: Adjusting JPEG Quality

For JPG output, you can control the compression level to balance file size and quality:

```python
# Tutorial Code Example: Adjusting JPEG quality
from groupdocs_viewer_cloud import ViewOptions, ImageOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/large-document.pdf"

# Create image options with quality settings
image_options = ImageOptions()
image_options.jpeg_quality = 75  # 75% quality (range: 1-100)
                                 # Lower values = smaller files but lower quality

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "JPG"
view_options.render_options = image_options
view_options.output_path = "output/optimized_images"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered with 75% JPEG quality for optimal file size")
```

### Step 5: Extracting Text for Searchability

One limitation of image-based rendering is that text isn't normally searchable. You can overcome this:

```python
# Tutorial Code Example: Extracting text for searchability
from groupdocs_viewer_cloud import ViewOptions, ImageOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/report.pdf"

# Create image options with text extraction
image_options = ImageOptions()
image_options.extract_text = True  # Enable text extraction
image_options.width = 1200  # Set width for better readability
image_options.jpeg_quality = 90  # Higher quality for text clarity

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "JPG"
view_options.render_options = image_options
view_options.output_path = "output/searchable_report"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered with extracted text layer for searchability")
print(f"You can now implement text search on the image-based viewer")
```

### Step 6: Creating a High-Resolution Output

For printing or detailed viewing, you might need high-resolution images:

```python
# Tutorial Code Example: High-resolution output
from groupdocs_viewer_cloud import ViewOptions, ImageOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/detailed-diagram.pdf"

# Create high-resolution image options
image_options = ImageOptions()
image_options.width = 2400  # High resolution width
image_options.height = 0    # Maintain aspect ratio
image_options.jpeg_quality = 100  # Maximum quality

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "PNG"  # PNG for lossless quality
view_options.render_options = image_options
view_options.output_path = "output/hires_diagram"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered in high resolution for detailed viewing or printing")
```

### Step 7: Practical Integration Example

Here's how to integrate image-based rendering with a simple viewer interface:

```python
# Tutorial Code Example: Image viewer integration
from groupdocs_viewer_cloud import ViewOptions, ImageOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/whitepaper.pdf"

# Create optimized image options
image_options = ImageOptions()
image_options.width = 1000  # Standard width
image_options.jpeg_quality = 85  # Good balance of quality and file size
image_options.extract_text = True  # Enable text search

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "JPG"
view_options.render_options = image_options
view_options.output_path = "output/whitepaper_images"

# Render the document
result = viewer_api.view(view_options)

# Generate example HTML to demonstrate integration
print("Example HTML for image viewer integration:")
print("html")
print("<!DOCTYPE html>")
print("<html>")
print("<head>")
print("  <title>Document Image Viewer</title>")
print("  <style>")
print("    .viewer-container { width: 100%; max-width: 1000px; margin: 0 auto; }")
print("    .page-nav { text-align: center; margin: 20px 0; }")
print("    .image-container { text-align: center; }")
print("    .page-image { max-width: 100%; border: 1px solid #ddd; }")
print("  </style>")
print("</head>")
print("<body>")
print("  <div class=\"viewer-container\">")
print("    <div class=\"page-nav\">")
print("      <button onclick=\"prevPage()\">Previous</button>")
print("      <span id=\"page-num\">1</span> / <span id=\"page-count\">" + str(len(result.pages)) + "</span>")
print("      <button onclick=\"nextPage()\">Next</button>")
print("    </div>")
print("    <div class=\"image-container\">")
print("      <img id=\"page-image\" class=\"page-image\" src=\"\" alt=\"Document page\">")
print("    </div>")
print("  </div>")
print("")
print("  <script>")
print("    let currentPage = 1;")
print("    const pageCount = " + str(len(result.pages)) + ";")
print("    const imageUrls = [")
for page in result.pages:
    print("      \"" + page.download_url + "\",")
print("    ];")
print("")
print("    function loadPage(pageNum) {")
print("      document.getElementById('page-image').src = imageUrls[pageNum-1];")
print("      document.getElementById('page-num').textContent = pageNum;")
print("    }")
print("")
print("    function nextPage() {")
print("      if (currentPage < pageCount) {")
print("        currentPage++;")
print("        loadPage(currentPage);")
print("      }")
print("    }")
print("")
print("    function prevPage() {")
print("      if (currentPage > 1) {")
print("        currentPage--;")
print("        loadPage(currentPage);")
print("      }")
print("    }")
print("")
print("    // Load first page on start")
print("    loadPage(1);")
print("  </script>")
print("</body>")
print("</html>")
print("")
```

## Try It Yourself

Now that you've learned how to use ImageOptions, try these exercises:

1. Render a large PDF document to optimized JPG images (balancing quality and file size)
2. Create a simple image viewer that loads document pages on demand
3. Experiment with different width/height combinations and observe how aspect ratio is maintained

## Troubleshooting Tips

- Images too large: Reduce width/height or lower JPEG quality to decrease file size
- Text not searchable: Make sure `extract_text` is set to `true`
- Poor quality text: Increase JPEG quality or switch to PNG format for text-heavy documents
- Slow loading: Consider implementing lazy loading in your viewer to only load visible pages

## What You've Learned

In this tutorial, you've mastered:
- How to configure basic image rendering
- Controlling image dimensions while maintaining aspect ratio
- Optimizing JPEG quality for the right balance of size and clarity
- Enabling text extraction for searchable image-based viewing
- Building a simple image viewer integration

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about image rendering? Post them on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
