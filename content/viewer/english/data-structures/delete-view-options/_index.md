---
title: How to Delete Views with DeleteViewOptions Tutorial
url: /data-structures/delete-view-options/
weight: 9
description: Learn how to manage rendered document views in this step-by-step DeleteViewOptions tutorial for GroupDocs.Viewer Cloud API
---

# Tutorial: How to Delete Views with DeleteViewOptions

## Learning Objectives

In this tutorial, you'll learn:
- Why and when to delete rendered document views
- How to use DeleteViewOptions to clean up storage space
- Best practices for document view management
- Implementation strategies for different scenarios

## Prerequisites

Before starting this tutorial:
- Complete the [ViewOptions Tutorial](/data-structures/view-options/)
- Have your GroupDocs.Viewer Cloud API credentials ready
- Understand basic document rendering concepts

## Introduction to DeleteViewOptions

The DeleteViewOptions data structure is used to specify which rendered document views should be deleted from storage. When you render documents with GroupDocs.Viewer Cloud, the output files (HTML pages, images, PDF) are stored in your cloud storage. The DeleteViewOptions structure lets you clean up these files when they're no longer needed.

Proper view management is essential for:
- Keeping your storage space optimized
- Removing outdated rendered content
- Maintaining document security
- Implementing document workflow lifecycles

## Understanding the DeleteViewOptions Structure

DeleteViewOptions has a simple structure with one primary component:

- FileInfo: Contains information about the document whose views should be deleted

Let's explore how to use this structure with practical examples.

## Tutorial Steps

### Step 1: Basic View Deletion

Let's start with a simple example of deleting views for a document:

```python
# Tutorial Code Example: Basic view deletion
import os
from groupdocs_viewer_cloud import Configuration, ViewerApi, DeleteViewOptions, FileInfo

# Configure the API client
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
viewer_api = ViewerApi.from_config(configuration)

# Set up file info for the document whose views we want to delete
file_info = FileInfo()
file_info.file_path = "documents/sample.docx"

# Create delete view options
delete_options = DeleteViewOptions()
delete_options.file_info = file_info

# Delete all rendered views for this document
viewer_api.delete_view(delete_options)

print(f"Successfully deleted all rendered views for: {file_info.file_path}")
```

### Step 2: Understanding What Gets Deleted

When you call the delete_view method, what exactly gets removed? Let's see with an example:

```python
# Tutorial Code Example: Understanding what gets deleted
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, DeleteViewOptions, FileInfo

# First render a document to HTML
file_info = FileInfo()
file_info.file_path = "documents/document.pdf"

# Create HTML options
html_options = HtmlOptions()

# Set up view options
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = "output/document_html"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered to HTML with {len(result.pages)} pages")
print(f"Output path: {view_options.output_path}")
for page in result.pages[:2]:  # Show first two pages
    print(f"  - Page {page.number}: {page.path}")

# Now delete the rendered views
delete_options = DeleteViewOptions()
delete_options.file_info = file_info

# Delete all rendered views
viewer_api.delete_view(delete_options)

print(f"\nDeleted all rendered views at: {view_options.output_path}")
print("This includes all HTML pages and resources that were created during rendering")
```

### Step 3: Implementing a Render-Use-Delete Workflow

For applications with limited storage or security requirements, implementing a complete workflow is ideal:

```python
# Tutorial Code Example: Render-use-delete workflow
from groupdocs_viewer_cloud import ViewOptions, PdfOptions, DeleteViewOptions, FileInfo

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/confidential-report.docx"

# Step 1: Render document to PDF
pdf_options = PdfOptions()

view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "PDF"
view_options.render_options = pdf_options
view_options.output_path = "output/temp_pdf"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered to PDF: {result.file}")

# Step 2: Process or use the rendered document
# In a real application, you might:
#  - Provide the PDF to the user for download
#  - Process the PDF further
#  - Store it in a different location
print("Processing the PDF file...")
print("For example, allowing user to download it...")

# Step 3: Delete the rendered view when finished
delete_options = DeleteViewOptions()
delete_options.file_info = file_info

# Delete all rendered views
viewer_api.delete_view(delete_options)

print(f"\nDeleted temporary PDF file after processing")
print("This keeps your storage clean and ensures temporary files aren't left behind")
```

### Step 4: Integrating Delete Operations in a User Session

Here's how you might integrate view deletion into a user session lifecycle:

```python
# Tutorial Code Example: Session-based document viewing
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, DeleteViewOptions, FileInfo
import time

# Simulate a user session
print("User session started")
print("User requests to view a document")

# Set up file info
file_info = FileInfo()
file_info.file_path = "documents/large-report.xlsx"

# Create HTML options with responsive design
html_options = HtmlOptions()
html_options.is_responsive = True

# Generate a unique session ID (in real app, this would be a session token)
session_id = f"session_{int(time.time())}"

# Set up view options with session-specific output path
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = f"output/sessions/{session_id}"

# Render the document
result = viewer_api.view(view_options)

print(f"Document rendered to HTML with {len(result.pages)} pages")
print(f"Session-specific output path: {view_options.output_path}")

# Simulate user viewing the document
print("User is viewing the document...")
time.sleep(2)  # In a real application, this would be actual user interaction time

# Simulate session end (user logs out or session expires)
print("User session ended")

# Clean up session resources
delete_options = DeleteViewOptions()
delete_options.file_info = file_info

# Delete all rendered views
viewer_api.delete_view(delete_options)

print(f"\nSession cleanup: Deleted all rendered views for session {session_id}")
print("This prevents storage bloat and protects document confidentiality")
```

### Step 5: Scheduled Cleanup Operations

For long-running applications, implementing scheduled cleanup is a good practice:

```python
# Tutorial Code Example: Scheduled cleanup operations
from groupdocs_viewer_cloud import DeleteViewOptions, FileInfo
import datetime

# This would typically run as a scheduled task or cron job
def run_scheduled_cleanup():
    print(f"Starting scheduled cleanup at {datetime.datetime.now()}")
    
    # In a real application, you would have a database or log of rendered documents
    # Here we'll use a simple list for demonstration
    documents_to_clean = [
        {"path": "documents/report1.docx", "render_date": "2023-08-15"},
        {"path": "documents/invoice.pdf", "render_date": "2023-08-16"},
        {"path": "documents/presentation.pptx", "render_date": "2023-08-17"}
    ]
    
    # Get current date for age comparison
    current_date = datetime.datetime.now().date()
    
    # Process each document
    for doc in documents_to_clean:
        file_path = doc["path"]
        render_date = datetime.datetime.strptime(doc["render_date"], "%Y-%m-%d").date()
        
        # Calculate age in days
        age_days = (current_date - render_date).days
        
        # If rendered view is older than 7 days, delete it
        if age_days > 7:
            file_info = FileInfo()
            file_info.file_path = file_path
            
            delete_options = DeleteViewOptions()
            delete_options.file_info = file_info
            
            # Delete all rendered views
            viewer_api.delete_view(delete_options)
            
            print(f"Deleted rendered views for {file_path} (age: {age_days} days)")
        else:
            print(f"Keeping rendered views for {file_path} (age: {age_days} days)")
    
    print(f"Scheduled cleanup completed at {datetime.datetime.now()}")

# Run the cleanup function
run_scheduled_cleanup()
```

### Step 6: Implementing Version-Based Cleanup

When documents are updated, you might want to clean up older rendered versions:

```python
# Tutorial Code Example: Version-based cleanup
from groupdocs_viewer_cloud import ViewOptions, HtmlOptions, DeleteViewOptions, FileInfo

# Scenario: A document has been updated, and we need to re-render and replace old views

# Document information
document_path = "documents/company-policy.docx"
document_version = "v2"  # Current version

# Step 1: Delete previous rendered views
delete_options = DeleteViewOptions()
delete_options.file_info = FileInfo(file_path=document_path)

# Delete all rendered views of previous version
viewer_api.delete_view(delete_options)

print(f"Deleted previously rendered views for {document_path}")

# Step 2: Render new version
file_info = FileInfo()
file_info.file_path = document_path

# Create HTML options
html_options = HtmlOptions()
html_options.is_responsive = True

# Set up view options with version in the output path
view_options = ViewOptions()
view_options.file_info = file_info
view_options.view_format = "HTML"
view_options.render_options = html_options
view_options.output_path = f"output/{document_version}/{document_path}"

# Render the document
result = viewer_api.view(view_options)

print(f"Rendered new version ({document_version}) to HTML with {len(result.pages)} pages")
print(f"New version output path: {view_options.output_path}")

print("\nThis version management approach ensures users always see the latest document version")
```

## Try It Yourself

Now that you've learned how to use DeleteViewOptions, try these exercises:

1. Implement a document viewer that automatically cleans up rendered files after a specified time period
2. Create a batch cleanup operation that removes all rendered views for documents in a specific folder
3. Build a version-based document management system that maintains only the latest rendered views

## Troubleshooting Tips

- Nothing seems to be deleted: Verify that the file path in FileInfo matches exactly what was used when rendering
- Error during deletion: Check if you have proper permissions to delete files in the storage
- Unable to find rendered views: Ensure you're looking in the correct output path

## Best Practices

1. Clean up promptly: Delete rendered views as soon as they're no longer needed
2. Use unique output paths: Organize rendered views by user, session, or version to make cleanup easier
3. Implement scheduled cleanup: Set up regular maintenance to remove old rendered views
4. Log cleanup operations: Keep records of what was deleted and when for troubleshooting
5. Check deletion success: Verify that cleanup operations completed successfully

## What You've Learned

In this tutorial, you've mastered:
- How to use DeleteViewOptions to clean up rendered document views
- Implementing complete document viewing workflows with proper cleanup
- Session-based view management strategies
- Scheduled and version-based cleanup approaches
- Best practices for maintaining optimal storage usage

## Next Steps

Congratulations! You've completed all the tutorials in our Document Data Structures series. To continue your learning journey with GroupDocs.Viewer Cloud API, consider exploring these advanced topics:

- [Viewer API Working with Storage](https://docs.groupdocs.cloud/viewer/working-with-storage/)
- [Viewer API Working with Folders](https://docs.groupdocs.cloud/viewer/working-with-folders/)
- [Viewer API Working with Files](https://docs.groupdocs.cloud/viewer/working-with-files/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about document view management? Post them on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
