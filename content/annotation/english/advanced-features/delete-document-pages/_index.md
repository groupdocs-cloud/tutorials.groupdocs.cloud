---
title: How to Delete Document Pages in GroupDocs.Annotation Cloud Tutorial
description: Learn how to clean up document page preview resources using the GroupDocs.Annotation Cloud API in this step-by-step tutorial
weight: 230
url: /advanced-features/delete-document-pages/
---

# Tutorial: How to Delete Document Pages

## Learning Objectives

In this tutorial, you'll learn how to:
- Delete document page preview resources from cloud storage
- Implement efficient resource management for previews
- Handle cleanup operations in different programming languages
- Create a complete preview workflow with cleanup

## What is Page Preview Cleanup?

When you generate document page previews using the Get Pages method, the API creates image files in your cloud storage. The Delete Pages method allows you to remove these preview images when they're no longer needed, helping you manage storage space efficiently.

## Prerequisites

Before starting this tutorial, ensure you have:
1. A GroupDocs.Annotation Cloud account (or [get a free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. A development environment for your preferred language
4. Document preview images already generated in your cloud storage

## Implementation Steps

Let's walk through the process of deleting document page preview resources:

### 1. Authentication

First, authenticate with the GroupDocs.Annotation Cloud API:

```javascript
// Get JWT token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the received JWT token for subsequent API calls.

### 2. Delete Page Preview Resources

Use the `POST /annotation/preview/remove` endpoint to delete the preview images:

```javascript
// cURL example to delete document preview images
curl -v "https://api.groupdocs.cloud/v2.0/annotation/preview/remove" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{ \"FilePath\": \"sample-document.docx\"}"
```

### 3. Process the Response

If the operation is successful, the API returns a `200 OK` status code. There's no specific response body for this operation.

## Try It Yourself

Let's implement document page preview deletion in different programming languages.

### C# Example

```csharp
// For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = "YOUR_APP_KEY"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = "YOUR_APP_SID";

var configuration = new Configuration(MyAppSid, MyAppKey);
var apiInstance = new AnnotateApi(configuration);

// Create file info object
var fileInfo = new FileInfo { FilePath = "sample-document.docx" };

// Delete page preview resources
var request = new DeletePagesRequest(fileInfo);
apiInstance.DeletePages(request);

Console.WriteLine("Page preview resources have been successfully deleted.");
```

### Java Example

```java
// For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-java-samples
String clientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String clientSecret = "YOUR_CLIENT_SECRET";

Configuration configuration = new Configuration(clientId, clientSecret);
AnnotateApi apiInstance = new AnnotateApi(configuration);

// Create file info object
FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("sample-document.docx");

// Delete page preview resources
DeletePagesRequest request = new DeletePagesRequest(fileInfo);
apiInstance.deletePages(request);

System.out.println("Page preview resources have been successfully deleted.");
```

### Python Example

```python
# For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud

app_sid = "YOUR_APP_SID"  # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "YOUR_APP_KEY"

api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)

# Set up file info for the document
file_info = groupdocs_annotation_cloud.FileInfo()
file_info.file_path = "sample-document.docx"

# Delete page preview resources
request = groupdocs_annotation_cloud.DeletePagesRequest(file_info)
api.delete_pages(request)

print("Page preview resources have been successfully deleted.")
```

## Complete Preview Workflow with Cleanup

Here's a complete workflow that generates previews, processes them, and cleans up afterward:

```python
# Pseudocode for complete preview workflow
def document_preview_workflow(document_path):
    try:
        # Step 1: Generate page previews
        preview_options = create_preview_options(document_path)
        page_images = generate_previews(preview_options)
        
        # Step 2: Process the previews (e.g., display, analyze, etc.)
        for page in page_images:
            process_page(page)
        
        # Step 3: Clean up resources when done
        delete_preview_resources(document_path)
        
        return "Document preview workflow completed successfully"
    except Exception as e:
        return f"Error in document preview workflow: {str(e)}"
```

## Best Practices for Resource Management

1. Always Clean Up: Always delete preview resources when they're no longer needed to avoid accumulating unused files in your storage.

2. Implement Try-Finally Blocks: Use try-finally blocks to ensure cleanup happens even if errors occur:

```csharp
try
{
    // Generate and process previews
    var pages = apiInstance.GetPages(new GetPagesRequest(options));
    ProcessPages(pages);
}
finally
{
    // Clean up resources regardless of success or failure
    apiInstance.DeletePages(new DeletePagesRequest(fileInfo));
}
```

3. Batch Processing: For batch processing of multiple documents, consider using a queue system:

```python
# Pseudocode for batch processing
documents_to_process = ["doc1.pdf", "doc2.docx", "doc3.pptx"]

for document in documents_to_process:
    try:
        # Generate previews
        previews = generate_previews(document)
        
        # Process previews
        process_previews(previews)
    finally:
        # Clean up
        delete_preview_resources(document)
```

4. Temporary Preview Retention: If you need previews for a limited time, consider implementing a retention policy:

```java
// Pseudocode for preview retention
class PreviewManager {
    private Map<String, Long> previewExpirations = new HashMap<>();
    private final long RETENTION_PERIOD = 30 * 60 * 1000; // 30 minutes
    
    public void generatePreview(String documentPath) {
        // Generate previews
        generatePreviews(documentPath);
        
        // Set expiration time
        previewExpirations.put(documentPath, System.currentTimeMillis() + RETENTION_PERIOD);
    }
    
    public void cleanupExpiredPreviews() {
        long currentTime = System.currentTimeMillis();
        
        for (Iterator<Map.Entry<String, Long>> it = previewExpirations.entrySet().iterator(); it.hasNext();) {
            Map.Entry<String, Long> entry = it.next();
            
            if (entry.getValue() < currentTime) {
                // Preview has expired, clean it up
                deletePreviewResources(entry.getKey());
                it.remove();
            }
        }
    }
}
```

## Troubleshooting Tips

1. File Not Found: If you get a file not found error, ensure the document path is correct
2. Authentication Issues: Verify your JWT token is valid and not expired
3. No Effect: If deletion appears to have no effect, verify that the preview images were actually generated first
4. Permission Issues: Ensure your API credentials have proper permissions for file deletion

## What You've Learned

In this tutorial, you've learned how to:
- Delete document page preview resources from your cloud storage
- Implement resource cleanup in different programming languages
- Create a complete preview workflow with proper resource management
- Apply best practices for efficient storage management

## Further Practice

To enhance your understanding of preview resource management, try these exercises:
1. Create a scheduled task that automatically cleans up preview files older than a certain time
2. Build a preview cache system that efficiently manages resources
3. Implement a user interface for previewing and managing document resources
4. Create an audit system that tracks preview generation and cleanup operations

## Next Steps

Continue your learning journey with these related tutorials:
- [Tutorial: How to Add Multiple Annotations](/advanced-features/multiple-annotations/)
- [Tutorial: How to Extract Annotations](/advanced-features/extract-annotations/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
