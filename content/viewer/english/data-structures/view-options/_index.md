---
title: How to Use ViewOptions with GroupDocs.Viewer Cloud API Tutorial
url: /data-structures/view-options/
weight: 1
description: Learn how to configure document viewing options in this step-by-step tutorial for the ViewOptions data structure in GroupDocs.Viewer Cloud API
---

# Tutorial: How to Use ViewOptions with GroupDocs.Viewer Cloud API

## Learning Objectives

In this tutorial, you'll learn:
- What the ViewOptions data structure is and why it's important
- How to configure basic viewing settings for different document formats
- Step-by-step implementation for HTML, Image, and PDF rendering
- How to apply watermarks and customize output paths

## Prerequisites

Before starting this tutorial:
- Create a [GroupDocs Cloud account](https://dashboard.groupdocs.cloud) and get your Client ID and Client Secret
- Have basic knowledge of RESTful API concepts
- Prepare a sample document for testing (DOCX, PDF, XLSX, etc.)

## Introduction to ViewOptions

The ViewOptions data structure is the foundation of document rendering in GroupDocs.Viewer Cloud. It provides the configuration needed to specify how your documents should be rendered, including format, security options, watermarks, and output settings.

Think of ViewOptions as your document rendering instruction set - it tells the API exactly how you want your document processed and displayed.

## Understanding the ViewOptions Structure

The ViewOptions data structure has several key components:

- ViewFormat: Determines the output format (HTML, JPG, PNG, or PDF)
- FileInfo: Contains source document information (path, password, etc.)
- OutputPath: Defines where rendered results will be stored
- Watermark: Settings for applying text watermarks
- RenderOptions: Format-specific rendering settings

Let's explore how to use these components in practical scenarios.

## Tutorial Steps

### Step 1: Set Up Your Environment

First, let's prepare our environment by configuring the API client with your credentials:

```python
# Tutorial Code Example: Setting up GroupDocs.Viewer Cloud API client
import os
from groupdocs_viewer_cloud import Configuration, ViewerApi, FileInfo

# Set your App SID and App Key
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
viewer_api = ViewerApi.from_config(configuration)

print("API client configured successfully")
```

### Step 2: Creating Basic ViewOptions for HTML Rendering

Let's start with the most common use case - rendering a document to HTML:

```python
# Tutorial Code Example: Basic HTML rendering with ViewOptions
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions

# Create FileInfo object
file_info = FileInfo()
file_info.file_path = "documents/sample.docx"  # Document in your cloud storage
file_info.password = ""  # Set if document is password-protected

# Create view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"  # Set output format to HTML
view_options.render_options = HtmlOptions()  # Use HTML-specific options

# Run the rendering operation and get the result
result = viewer_api.view(view_options)

print(f"Document rendered to HTML successfully. Pages: {len(result.pages)}")
for page in result.pages:
    print(f"Page {page.number} available at: {page.path}")
```

### Step 3: Customizing Output with External Resources

When rendering to HTML, you might want to separate resources (images, fonts) from the HTML content:

```python
# Tutorial Code Example: HTML rendering with external resources
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions

# Setup file info
file_info = FileInfo()
file_info.file_path = "documents/presentation.pptx"

# Configure HTML-specific options
html_options = HtmlOptions()
html_options.external_resources = True  # Extract resources as separate files
html_options.resource_path = "viewer/resources/{resource-name}"  # Template for resource paths
html_options.is_responsive = True  # Make output responsive for different screens

# Set up view options with our HTML configuration
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/presentation_html"  # Custom output folder

# Render the document
result = viewer_api.view(view_options)

print(f"Responsive HTML with external resources created successfully")
# Now you can see the resource paths in the result
for page in result.pages:
    print(f"Page {page.number} resources count: {len(page.resources)}")
```

### Step 4: Rendering to Images (JPG/PNG)

For some use cases, you might need to convert documents to images instead of HTML:

```python
# Tutorial Code Example: Image rendering with ViewOptions
from groupdocs_viewer_cloud import ViewOptions, ImageOptions

# Setup file info
file_info = FileInfo()
file_info.file_path = "documents/contract.pdf"

# Configure image-specific options
image_options = ImageOptions()
image_options.width = 800  # Set output width in pixels
image_options.height = 0   # Height will be calculated automatically to maintain aspect ratio
image_options.jpeg_quality = 90  # Set quality for JPG output (1-100)
image_options.extract_text = True  # Extract text to enable search in images

# Set up view options with our image configuration
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "JPG"  # Can also use "PNG"
view_options.render_options = image_options
view_options.output_path = "output/contract_images"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered to JPG images: {len(result.pages)} pages created")
```

### Step 5: Creating PDF Output with Watermarks

PDF output is ideal for creating distributable documents with watermarks:

```python
# Tutorial Code Example: PDF rendering with watermark
from groupdocs_viewer_cloud import ViewOptions, PdfOptions, Watermark

# Setup file info
file_info = FileInfo()
file_info.file_path = "documents/spreadsheet.xlsx"

# Configure watermark
watermark = Watermark()
watermark.text = "CONFIDENTIAL"
watermark.color = "(255,0,0,50)"  # Red with 50% opacity
watermark.position = "Diagonal"
watermark.size = 75  # Size in percentage

# Configure PDF options
pdf_options = PdfOptions()
# No special PDF settings for now - we'll use defaults

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "PDF"
view_options.render_options = pdf_options
view_options.watermark = watermark
view_options.output_path = "output/spreadsheet.pdf"

# Render the document
result = viewer_api.view(view_options)

print(f"PDF created with watermark: {result.file}")
```

### Step 6: Working with Password-Protected Documents

For secured documents, you'll need to provide the password in the FileInfo section:

```python
# Tutorial Code Example: Working with password-protected documents
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions

# Setup file info with password
file_info = FileInfo()
file_info.file_path = "documents/protected_document.docx"
file_info.password = "your_document_password"  # Password to open the document

# Configure view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = HtmlOptions()

# Try to render the document
try:
    result = viewer_api.view(view_options)
    print(f"Password-protected document rendered successfully: {len(result.pages)} pages")
except Exception as e:
    print(f"Error: {str(e)}")
    # If wrong password, you'll get an error here
```

## Try It Yourself

Now that you've learned the basics of ViewOptions, try these exercises to reinforce your knowledge:

1. Render a multi-page document and limit the output to only pages 2-4
2. Apply a custom watermark with your company name positioned at the bottom right
3. Render a document to responsive HTML with external resources

## Troubleshooting Tips

- Error: Document not found: Ensure the file path is correct and the document exists in your cloud storage
- Error: Invalid password: Double-check the document password is correct
- Missing resources in HTML output: Verify that `external_resources` is set to `true` and check the `resource_path` format

## What You've Learned

In this tutorial, you've learned:
- How to configure the ViewOptions data structure for different output formats
- Techniques for customizing HTML, image, and PDF output
- How to apply watermarks to rendered documents
- Methods for handling password-protected files

## Next Steps

Ready to dive deeper? Continue your learning journey with our [RenderOptions Tutorial](/data-structures/render-options) to explore advanced rendering configuration options.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
