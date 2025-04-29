---
title: HTML Rendering with HtmlOptions Tutorial
url: /data-structures/html-options/
weight: 3
description: Learn how to create responsive HTML document views with this step-by-step HtmlOptions tutorial for GroupDocs.Viewer Cloud API
---

# Tutorial: HTML Rendering with HtmlOptions

## Learning Objectives

In this tutorial, you'll learn:
- How to render documents to HTML format using HtmlOptions
- Techniques for creating responsive document views
- Methods for managing external resource handling
- How to customize resource paths for better integration

## Prerequisites

Before starting this tutorial:
- Complete the [ViewOptions Tutorial](/data-structures/view-options/)
- Have your GroupDocs.Viewer Cloud API credentials ready
- Prepare sample documents of various formats (DOCX, PDF, PPTX)

## Introduction to HtmlOptions

HtmlOptions is a specialized data structure that inherits from RenderOptions and provides HTML-specific rendering configurations. When you need to render documents to HTML format, this structure gives you fine control over how the HTML output is generated and how resources are handled.

HTML rendering is ideal for web applications, online document viewers, and situations where interactive document viewing is required.

## Understanding the HtmlOptions Structure

HtmlOptions adds these key HTML-specific properties to the base RenderOptions:

- ExternalResources: Controls whether to embed or externalize resources
- ResourcePath: Defines the path template for externalized resources
- IsResponsive: Enables responsive output for better display on various devices

Let's explore these options with practical examples.

## Tutorial Steps

### Step 1: Basic HTML Rendering

Let's start with a simple HTML rendering setup:

```python
# Tutorial Code Example: Basic HTML rendering
import os
from groupdocs_viewer_cloud import Configuration, ViewerApi, ViewOptions, HtmlOptions, FileInfo

# Configure the API client
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
viewer_api = ViewerApi.from_config(configuration)

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/sample-presentation.pptx"

# Create basic HTML options
html_options = HtmlOptions()
# Use default settings (embedded resources, non-responsive)

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/presentation_html"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered to HTML: {len(result.pages)} pages created")
for page in result.pages:
    print(f"Page {page.number} path: {page.path}")
    # With default settings, resources are embedded in HTML
    print(f"Resources count: {len(page.resources)}")  # Should be 0 with embedded resources
```

### Step 2: Creating Responsive HTML Output

For better viewing on mobile devices and various screen sizes, you can enable responsive rendering:

```python
# Tutorial Code Example: Responsive HTML rendering
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/annual-report.pdf"

# Create responsive HTML options
html_options = HtmlOptions()
html_options.is_responsive = True  # Enable responsive layout

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/responsive_html"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered to responsive HTML: {len(result.pages)} pages created")
print("The HTML output will adapt to different screen sizes automatically")
```

### Step 3: Working with External Resources

For better performance and more flexibility, you can separate resources (images, fonts, stylesheets) from HTML:

```python
# Tutorial Code Example: HTML rendering with external resources
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/whitepaper.docx"

# Create HTML options with external resources
html_options = HtmlOptions()
html_options.external_resources = True  # Extract resources as separate files
html_options.resource_path = "viewer/resources/{resource-name}"  # Resource path template

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/whitepaper_html"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered with external resources: {len(result.pages)} pages")
for page in result.pages:
    print(f"Page {page.number} has {len(page.resources)} external resources:")
    for resource in page.resources[:3]:  # Show first 3 resources as example
        print(f"  - {resource.name} ({resource.file_type})")
```

### Step 4: Customizing Resource Path Templates

You can customize how resource paths are structured using template placeholders:

```python
# Tutorial Code Example: Custom resource path templates
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/financial-charts.pptx"

# Create HTML options with custom resource path
html_options = HtmlOptions()
html_options.external_resources = True
# Define a path template with page number and resource name placeholders
html_options.resource_path = "content/viewer/{page-number}/resources/{resource-name}"

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/financial_slides"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered with custom resource paths: {len(result.pages)} pages")
for page in result.pages:
    if len(page.resources) > 0:
        print(f"Resource example from page {page.number}: {page.resources[0].path}")
        # Path will follow the template pattern: content/viewer/1/resources/image1.png
```

### Step 5: Combining Responsive Design with External Resources

For optimal web viewing experience, combine responsive design with external resources:

```python
# Tutorial Code Example: Responsive design with external resources
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/product-catalog.pdf"

# Create optimized HTML options
html_options = HtmlOptions()
html_options.is_responsive = True  # Responsive layout
html_options.external_resources = True  # External resources for better caching
html_options.resource_path = "assets/{resource-name}"  # Simple resource path

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/product_catalog"

# Render the document
result = viewer_api.view(view_options)

print(f"Created optimized HTML output with responsive design and external resources")
print(f"Total pages: {len(result.pages)}")
print(f"Total resources: {sum(len(page.resources) for page in result.pages)}")
```

### Step 6: Practical Integration Example

Here's how to integrate HTML output with page navigation in a web application:

```python
# Tutorial Code Example: Integration with web application
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/user-manual.pdf"

# Create HTML options for web integration
html_options = HtmlOptions()
html_options.external_resources = True
html_options.is_responsive = True
html_options.resource_path = "/api/document-viewer/resources/{resource-name}"

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/user_manual"

# Render the document
result = viewer_api.view(view_options)

# Generate example HTML to demonstrate integration
print("Example HTML for web integration:")
print("html")
print("<!DOCTYPE html>")
print("<html>")
print("<head>")
print("  <title>Document Viewer</title>")
print("  <style>")
print("    .viewer-container { width: 100%; max-width: 1000px; margin: 0 auto; }")
print("    .page-nav { text-align: center; margin: 20px 0; }")
print("    .page-display { border: 1px solid #ddd; min-height: 800px; }")
print("  </style>")
print("</head>")
print("<body>")
print("  <div class=\"viewer-container\">")
print("    <div class=\"page-nav\">")
print("      <button onclick=\"prevPage()\">Previous</button>")
print("      <span id=\"page-num\">1</span> / <span id=\"page-count\">" + str(len(result.pages)) + "</span>")
print("      <button onclick=\"nextPage()\">Next</button>")
print("    </div>")
print("    <div class=\"page-display\" id=\"viewer\">")
print("      <!-- Page content will be loaded here -->")
print("    </div>")
print("  </div>")
print("")
print("  <script>")
print("    let currentPage = 1;")
print("    const pageCount = " + str(len(result.pages)) + ";")
print("    const pagePaths = [")
for page in result.pages:
    print("      \"" + page.path + "\",")
print("    ];")
print("")
print("    function loadPage(pageNum) {")
print("      fetch(pagePaths[pageNum-1])")
print("        .then(response => response.text())")
print("        .then(html => {")
print("          document.getElementById('viewer').innerHTML = html;")
print("          document.getElementById('page-num').textContent = pageNum;")
print("        });")
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

Now that you've learned how to use HtmlOptions, try these exercises:

1. Render a PPTX file with responsive layout and external resources
2. Create a simple web viewer that displays a PDF document with navigation controls
3. Experiment with different resource path templates and observe the results

## Troubleshooting Tips

- Missing resources: If resources are missing, make sure `external_resources` is set to `true`
- Resource path issues: Check that the `resource_path` template contains the required placeholders (`{resource-name}`)
- Responsive layout not working: Verify that `is_responsive` is set to `true` and test on different devices

## What You've Learned

In this tutorial, you've mastered:
- How to configure basic HTML rendering
- Creating responsive HTML output for better viewing across devices
- Working with external resources for improved performance
- Customizing resource paths for seamless integration with web applications
- Combining various HtmlOptions settings for optimal document viewing

## Next Steps

Ready to explore other rendering formats? Continue your learning with our [Image Rendering Tutorial](/data-structures/image-options/) to discover how to convert documents to high-quality images.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about HTML rendering? Post them on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
