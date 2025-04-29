---
title: Working with ViewResult Tutorial
url: /data-structures/view-result/
weight: 7
description: Learn how to process and utilize the output from document rendering in this step-by-step ViewResult tutorial for GroupDocs.Viewer Cloud API
---

# Tutorial: Working with ViewResult

## Learning Objectives

In this tutorial, you'll learn:
- What the ViewResult data structure contains after document rendering
- How to access and process rendered pages and resources
- Techniques for building UI components with ViewResult data
- Methods for downloading and utilizing rendered content

## Prerequisites

Before starting this tutorial:
- Complete the [ViewOptions Tutorial](/data-structures/view-options/)
- Have your GroupDocs.Viewer Cloud API credentials ready
- Understand basic document rendering concepts

## Introduction to ViewResult

The ViewResult data structure is what you receive after calling the Document View API. It contains all the information about the rendered output, including references to rendered pages, resources, and attachments. Understanding this structure is essential for building document viewing applications that effectively utilize the GroupDocs.Viewer Cloud API.

Think of ViewResult as your roadmap to the rendered content - it tells you where to find each piece of the rendered document and provides the information needed to display them correctly.

## Understanding the ViewResult Structure

ViewResult contains several key components:

- pages: List of rendered document pages with their metadata
- attachments: List of document attachments (if any)
- file: Path to the output PDF file (when rendering to PDF format)

Let's explore these components with practical examples.

## Tutorial Steps

### Step 1: Rendering a Document and Examining ViewResult

Let's start by rendering a document and examining the ViewResult structure:

```python
# Tutorial Code Example: Basic ViewResult examination
import os
from groupdocs_viewer_cloud import Configuration, ViewerApi, ViewOptions, HtmlOptions, FileInfo

# Configure the API client
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
viewer_api = ViewerApi.from_config(configuration)

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/sample.docx"

# Create HTML options
html_options = HtmlOptions()

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/sample_html"

# Render the document and get ViewResult
result = viewer_api.view(view_options)

# Examine the ViewResult structure
print(f"ViewResult contains {len(result.pages)} pages")
if result.pages:
    print("\nFirst page information:")
    page = result.pages[0]
    print(f"  - Page number: {page.number}")
    print(f"  - Path: {page.path}")
    print(f"  - Download URL: {page.download_url}")
    print(f"  - Resources count: {len(page.resources)}")

# Check if there are any attachments
if result.attachments:
    print(f"\nDocument has {len(result.attachments)} attachments")
    for attachment in result.attachments:
        print(f"  - {attachment.name}")
```

### Step 2: Working with HTML Page Output

When rendering to HTML format, you'll receive pages that can be displayed in a web browser:

```python
# Tutorial Code Example: Processing HTML ViewResult
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/multi-page.pdf"

# Create HTML options with external resources
html_options = HtmlOptions()
html_options.external_resources = True

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/multipage_html"

# Render the document
result = viewer_api.view(view_options)

# Process the HTML pages
print(f"Document rendered to {len(result.pages)} HTML pages")
for page in result.pages:
    print(f"\nPage {page.number}:")
    print(f"  - Path: {page.path}")
    print(f"  - Download URL: {page.download_url}")
    
    # Process external resources
    if page.resources:
        print(f"  - Resources ({len(page.resources)}):")
        for resource in page.resources[:3]:  # Show first 3 resources
            print(f"    * {resource.name} - {resource.path}")
        if len(page.resources) > 3:
            print(f"    * ... and {len(page.resources) - 3} more resources")

# Generate code for a simple viewer using the results
print("\nExample HTML code for a basic viewer:")
print("html")
print("<div id='viewer-container'>")
print("  <div class='navigation'>")
print("    <button onclick='prevPage()'>Previous</button>")
print("    <span id='current-page'>1</span> / <span id='total-pages'>" + str(len(result.pages)) + "</span>")
print("    <button onclick='nextPage()'>Next</button>")
print("  </div>")
print("  <div id='viewer-content'></div>")
print("</div>")
print("")
print("<script>")
print("  const pages = [")
for page in result.pages:
    print(f"    '{page.download_url}',")
print("  ];")
print("  let currentPage = 1;")
print("")
print("  function loadPage(pageNum) {")
print("    fetch(pages[pageNum-1])")
print("      .then(response => response.text())")
print("      .then(html => {")
print("        document.getElementById('viewer-content').innerHTML = html;")
print("        document.getElementById('current-page').textContent = pageNum;")
print("      });")
print("  }")
print("")
print("  function prevPage() {")
print("    if (currentPage > 1) {")
print("      currentPage--;")
print("      loadPage(currentPage);")
print("    }")
print("  }")
print("")
print("  function nextPage() {")
print("    if (currentPage < " + str(len(result.pages)) + ") {")
print("      currentPage++;")
print("      loadPage(currentPage);")
print("    }")
print("  }")
print("")
print("  // Load the first page on start")
print("  loadPage(1);")
print("</script>")
print("")
```

### Step 3: Working with Image Output

When rendering to image formats (JPG/PNG), the ViewResult structure is similar:

```python
# Tutorial Code Example: Processing Image ViewResult
from groupdocs_viewer_cloud import ViewOptions, ImageOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/presentation.pptx"

# Create image options
image_options = ImageOptions()
image_options.width = 800  # Set width for better viewing

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "PNG"  # PNG for better quality
view_options.render_options = image_options
view_options.output_path = "output/presentation_images"

# Render the document
result = viewer_api.view(view_options)

# Process the image pages
print(f"Presentation rendered to {len(result.pages)} PNG images")
for page in result.pages:
    print(f"\nSlide {page.number}:")
    print(f"  - Path: {page.path}")
    print(f"  - Download URL: {page.download_url}")

# Generate code for an image gallery viewer
print("\nExample JavaScript code for an image gallery viewer:")
print("javascript")
print("const slides = [")
for page in result.pages:
    print(f"  {{")
    print(f"    number: {page.number},")
    print(f"    url: '{page.download_url}'")
    print(f"  }},")
print("];")
print("")
print("let currentSlide = 0;")
print("")
print("function renderGallery() {")
print("  const gallery = document.getElementById('presentation-gallery');")
print("  gallery.innerHTML = '';")
print("")
print("  // Create thumbnails")
print("  slides.forEach((slide, index) => {")
print("    const thumbnail = document.createElement('div');")
print("    thumbnail.className = 'thumbnail';")
print("    if (index === currentSlide) {")
print("      thumbnail.className += ' active';")
print("    }")
print("")
print("    const img = document.createElement('img');")
print("    img.src = slide.url;")
print("    img.alt = `Slide ${slide.number}`;")
print("    img.onclick = () => {")
print("      currentSlide = index;")
print("      showCurrentSlide();")
print("      renderGallery();")
print("    };")
print("")
print("    thumbnail.appendChild(img);")
print("    gallery.appendChild(thumbnail);")
print("  });")
print("}")
print("")
print("function showCurrentSlide() {")
print("  const mainViewer = document.getElementById('main-viewer');")
print("  mainViewer.src = slides[currentSlide].url;")
print("  document.getElementById('slide-number').textContent = slides[currentSlide].number;")
print("}")
print("")
print("function nextSlide() {")
print("  if (currentSlide < slides.length - 1) {")
print("    currentSlide++;")
print("    showCurrentSlide();")
print("    renderGallery();")
print("  }")
print("}")
print("")
print("function prevSlide() {")
print("  if (currentSlide > 0) {
    currentSlide--;
    showCurrentSlide();
    renderGallery();
  }
}

// Initialize the viewer
document.addEventListener('DOMContentLoaded', () => {
  showCurrentSlide();
  renderGallery();
});
```

### Step 4: Working with PDF Output

When rendering to PDF format, the ViewResult structure is different and contains a file path instead of pages:

```python
# Tutorial Code Example: Processing PDF ViewResult
from groupdocs_viewer_cloud import ViewOptions, PdfOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/contract.docx"

# Create PDF options
pdf_options = PdfOptions()

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "PDF"
view_options.render_options = pdf_options
view_options.output_path = "output/contract.pdf"

# Render the document
result = viewer_api.view(view_options)

# Process the PDF output
print(f"Document rendered to PDF format")
print(f"PDF file path: {result.file}")
print(f"Download URL: {result.file}")

# Generate example code for PDF viewer embedding
print("\nExample HTML code for embedding the PDF:")
print("html")
print("<div class='pdf-container'>")
print("  <h3>Contract Document</h3>")
print("  <object")
print(f"    data='{result.file}'")
print("    type='application/pdf'")
print("    width='100%'")
print("    height='600px'")
print("  >")
print("    <p>It appears your browser doesn't support embedded PDFs.</p>")
print(f"    <p><a href='{result.file}'>Download the PDF</a> instead.</p>")
print("  </object>")
print("</div>")
print("")
```

### Step 5: Working with Attachments

Some documents may contain attachments that you can access through ViewResult:

```python
# Tutorial Code Example: Handling document attachments
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, FileInfo

# Set up file info for a document with attachments (e.g., an email with attachments)
file_info = FileInfo()
file_info.file_path = "documents/email-with-attachments.eml"

# Create HTML options
html_options = HtmlOptions()

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/email_content"

# Render the document
result = viewer_api.view(view_options)

# Check for attachments
if result.attachments and len(result.attachments) > 0:
    print(f"Document contains {len(result.attachments)} attachments:")
    for attachment in result.attachments:
        print(f"  - {attachment.name}")
    
    # Generate example code for displaying attachments list
    print("\nExample HTML code for displaying attachments:")
    print("html")
    print("<div class='attachments-container'>")
    print("  <h3>Attachments</h3>")
    print("  <ul class='attachments-list'>")
    for attachment in result.attachments:
        print(f"    <li><a href='#' onclick='viewAttachment(\"{attachment.name}\")'>{attachment.name}</a></li>")
    print("  </ul>")
    print("</div>")
    print("")
    print("<script>")
    print("  function viewAttachment(attachmentName) {")
    print("    // Code to request and display the attachment")
    print("    console.log(`Viewing attachment: ${attachmentName}`);")
    print("    alert(`Attachment viewing functionality would be implemented here for: ${attachmentName}`);")
    print("  }")
    print("</script>")
    print("")
else:
    print("Document does not contain attachments")
```

### Step 6: Building a Complete Document Viewer with ViewResult

Now let's combine our knowledge to build a complete viewer interface:

```python
# Tutorial Code Example: Complete document viewer application
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, FileInfo
import json

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/technical-document.pdf"

# Create HTML options with external resources
html_options = HtmlOptions()
html_options.external_resources = True
html_options.is_responsive = True

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/technical_doc_html"

# Render the document
result = viewer_api.view(view_options)

# Generate complete viewer application with ViewResult data
print("Complete HTML document viewer application:")
print("html")
print("<!DOCTYPE html>")
print("<html lang='en'>")
print("<head>")
print("  <meta charset='UTF-8'>")
print("  <meta name='viewport' content='width=device-width, initial-scale=1.0'>")
print("  <title>Document Viewer - Technical Document</title>")
print("  <style>")
print("    body { font-family: Arial, sans-serif; margin: 0; padding: 0; }")
print("    .app-container { display: flex; flex-direction: column; height: 100vh; }")
print("    .toolbar { background: #f0f0f0; padding: 10px; display: flex; justify-content: space-between; align-items: center; }")
print("    .nav-buttons { display: flex; gap: 10px; }")
print("    .page-display { padding: 0 15px; }")
print("    .viewer-container { flex: 1; overflow: auto; padding: 20px; }")
print("    .page-content { background: white; box-shadow: 0 2px 8px rgba(0,0,0,0.1); padding: 20px; max-width: 1000px; margin: 0 auto; }")
print("    .zoom-controls { display: flex; align-items: center; gap: 10px; }")
print("    .loading { text-align: center; padding: 20px; }")
print("  </style>")
print("</head>")
print("<body>")
print("  <div class='app-container'>")
print("    <div class='toolbar'>")
print("      <div class='nav-buttons'>")
print("        <button id='prev-btn' onclick='prevPage()'>Previous</button>")
print("        <div class='page-display'>")
print("          <span id='current-page'>1</span> / <span id='total-pages'>" + str(len(result.pages)) + "</span>")
print("        </div>")
print("        <button id='next-btn' onclick='nextPage()'>Next</button>")
print("      </div>")
print("      <div class='zoom-controls'>")
print("        <button onclick='changeZoom(-0.1)'>-</button>")
print("        <span id='zoom-level'>100%</span>")
print("        <button onclick='changeZoom(0.1)'>+</button>")
print("        <button onclick='resetZoom()'>Reset</button>")
print("      </div>")
print("    </div>")
print("    <div class='viewer-container'>")
print("      <div id='page-content' class='page-content'>")
print("        <div id='loading' class='loading'>Loading document...</div>")
print("      </div>")
print("    </div>")
print("  </div>")
print("")
print("  <script>")
print("    // Document pages data")
print("    const pages = " + json.dumps([{"number": page.number, "url": page.download_url} for page in result.pages]) + ";")
print("    let currentPage = 1;")
print("    let zoomLevel = 1;")
print("")
print("    // Load the specified page")
print("    function loadPage(pageNum) {")
print("      const pageIndex = pageNum - 1;")
print("      if (pageIndex < 0 || pageIndex >= pages.length) return;")
print("      ")
print("      document.getElementById('loading').style.display = 'block';")
print("      document.getElementById('current-page').textContent = pageNum;")
print("      ")
print("      fetch(pages[pageIndex].url)")
print("        .then(response => response.text())")
print("        .then(html => {")
print("          document.getElementById('page-content').innerHTML = html;")
print("          document.getElementById('loading').style.display = 'none';")
print("          applyZoom();")
print("        })")
print("        .catch(error => {")
print("          console.error('Error loading page:', error);")
print("          document.getElementById('loading').textContent = 'Error loading page';")
print("        });")
print("      ")
print("      // Update button states")
print("      document.getElementById('prev-btn').disabled = pageNum <= 1;")
print("      document.getElementById('next-btn').disabled = pageNum >= pages.length;")
print("    }")
print("")
print("    function prevPage() {")
print("      if (currentPage > 1) {")
print("        currentPage--;")
print("        loadPage(currentPage);")
print("      }")
print("    }")
print("")
print("    function nextPage() {")
print("      if (currentPage < pages.length) {")
print("        currentPage++;")
print("        loadPage(currentPage);")
print("      }")
print("    }")
print("")
print("    function changeZoom(delta) {")
print("      zoomLevel = Math.max(0.5, Math.min(2, zoomLevel + delta));")
print("      applyZoom();")
print("    }")
print("")
print("    function resetZoom() {")
print("      zoomLevel = 1;")
print("      applyZoom();")
print("    }")
print("")
print("    function applyZoom() {")
print("      const content = document.getElementById('page-content');")
print("      content.style.transform = `scale(${zoomLevel})`;")
print("      content.style.transformOrigin = 'top center';")
print("      document.getElementById('zoom-level').textContent = `${Math.round(zoomLevel * 100)}%`;")
print("    }")
print("")
print("    // Initialize the viewer")
print("    document.addEventListener('DOMContentLoaded', () => {")
print("      loadPage(1);")
print("    });")
print("")
print("    // Add keyboard navigation")
print("    document.addEventListener('keydown', (e) => {")
print("      if (e.key === 'ArrowRight' || e.key === ' ') {")
print("        nextPage();")
print("      } else if (e.key === 'ArrowLeft') {")
print("        prevPage();")
print("      }")
print("    });")
print("  </script>")
print("</body>")
print("</html>")
print("")
```

## Try It Yourself

Now that you've learned how to work with ViewResult, try these exercises:

1. Create a viewer interface that displays multiple pages side by side
2. Implement a thumbnail navigator using the page URLs from ViewResult
3. Build a simple document management system that lists document attachments and allows you to view them

## Troubleshooting Tips

- Pages not displaying: Verify that the download URLs are accessible and that you're fetching them correctly
- Resource loading issues: When using external resources, ensure that resource paths are correctly referenced
- Authentication problems: Check if the download URLs require authentication and implement proper headers
- Cross-origin issues: Be aware that loading resources from different domains may require CORS configuration

## What You've Learned

In this tutorial, you've mastered:
- How to interpret the ViewResult structure after document rendering
- Accessing page content and resources for different output formats
- Building HTML, image, and PDF viewers using ViewResult data
- Working with document attachments
- Creating complete viewer applications with navigation and zoom controls

## Next Steps

Ready to learn about extracting document information? Continue your learning with our [InfoResult Tutorial](/data-structures/info-result) to discover how to extract metadata and page information from documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about working with ViewResult? Post them on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).