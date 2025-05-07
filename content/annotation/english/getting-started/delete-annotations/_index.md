---
title: How to Delete Annotations from Documents with GroupDocs.Annotation Cloud API Tutorial
url: /getting-started/delete-annotations/
weight: 3
description: Learn how to selectively remove annotations from documents using GroupDocs.Annotation Cloud API in this step-by-step developer tutorial
---

# Tutorial: How to Delete Annotations from Documents with GroupDocs.Annotation Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Remove specific annotations from documents using the GroupDocs.Annotation Cloud API
- Delete all annotations from a document
- Selectively remove annotations by ID
- Implement annotation deletion in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Annotation Cloud account (if you don't have one, [get a free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your API keys (AppSID and AppKey) from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document with annotations uploaded to your storage (ideally, follow the [Add Annotations tutorial](/getting-started/add-annotations/) first to create one)
4. Basic knowledge of REST APIs
5. A development environment set up with your preferred language

## Practical Scenario

In a document management system, you often need to clean up or selectively remove annotations. For example, you might want to remove outdated comments, delete annotations from specific reviewers, or create a clean version of a document without any annotations. This tutorial shows you how to implement these common annotation management scenarios.

## Step-by-Step Guide

### 1. Understanding the API Endpoint

GroupDocs.Annotation Cloud API provides an endpoint to delete annotations from a document:

```
POST https://api.groupdocs.cloud/v2.0/annotation/remove
```

This endpoint requires you to specify the document path and annotation IDs in the request body.

### 2. First Step: Extract Annotation IDs

Before deleting annotations, you typically need to extract them first to get their IDs. Let's revisit the extraction process briefly:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/extract" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FilePath': 'annotationdocs/annotated-sample.docx'
}"
```

From the response, note the IDs of the annotations you want to delete.

### 3. Deleting Specific Annotations

Now, let's delete specific annotations using their IDs:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/remove" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/annotated-sample.docx'
  },
  'AnnotationIds': [1, 3],
  'OutputPath': 'output/annotations-removed.docx'
}"
```

This request will:
- Take the document at `annotationdocs/annotated-sample.docx`
- Remove annotations with IDs 1 and 3
- Save the result to `output/annotations-removed.docx`

### 4. Deleting All Annotations

To delete all annotations from a document, you can first extract all annotation IDs and then use them in the remove request. Alternatively, some implementations of the SDK provide a convenience method to remove all annotations:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/remove" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/annotated-sample.docx'
  },
  'AnnotationIds': [],
  'OutputPath': 'output/all-annotations-removed.docx'
}"
```

By providing an empty array for `AnnotationIds`, the API will remove all annotations.

### 5. Implementing in C#

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyAppSid, MyAppKey);
  
// Create AnnotateApi instance
var apiInstance = new AnnotateApi(configuration);

// Step 1: Extract annotations to get their IDs
var extractRequest = new ExtractRequest(
    new FileInfo { FilePath = "annotationdocs/annotated-sample.docx" }
);

var extractResult = apiInstance.Extract(extractRequest);
Console.WriteLine($"Found {extractResult.Annotations.Count} annotations in the document.");

// Step 2: Select which annotations to delete (in this example, we'll delete the first two)
var annotationIdsToDelete = new List<int>();
if (extractResult.Annotations.Count > 0)
{
    annotationIdsToDelete.Add(extractResult.Annotations[0].Id);
    
    if (extractResult.Annotations.Count > 1)
    {
        annotationIdsToDelete.Add(extractResult.Annotations[1].Id);
    }
    
    Console.WriteLine($"Deleting annotations with IDs: {string.Join(", ", annotationIdsToDelete)}");
    
    // Step 3: Create request to remove specific annotations
    var removeRequest = new RemoveAnnotationsRequest(
        new FileInfo { FilePath = "annotationdocs/annotated-sample.docx" },
        annotationIdsToDelete
    )
    {
        OutputPath = "output/annotations-removed.docx"
    };
    
    // Call API to remove annotations
    var removeResult = apiInstance.RemoveAnnotations(removeRequest);
    
    Console.WriteLine($"Annotations removed successfully. Document saved to {removeResult.Path}");
}
else
{
    Console.WriteLine("No annotations found to delete.");
}

// Example of removing all annotations from a document
Console.WriteLine("\nRemoving all annotations from the document...");

var removeAllRequest = new RemoveAnnotationsRequest(
    new FileInfo { FilePath = "annotationdocs/annotated-sample.docx" },
    new List<int>() // Empty list means remove all annotations
)
{
    OutputPath = "output/all-annotations-removed.docx"
};

var removeAllResult = apiInstance.RemoveAnnotations(removeAllRequest);

Console.WriteLine($"All annotations removed successfully. Document saved to {removeAllResult.Path}");
```

### 6. Implementing in Python

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
# Create API instance
annotate_api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)

# Step 1: Create file info object
file_info = groupdocs_annotation_cloud.FileInfo()
file_info.file_path = "annotationdocs/annotated-sample.docx"

# Step 2: Extract annotations to get their IDs
extract_request = groupdocs_annotation_cloud.ExtractRequest(file_info)
extract_result = annotate_api.extract(extract_request)

print(f"Found {len(extract_result.annotations)} annotations in the document.")

# Step 3: Select which annotations to delete (in this example, we'll delete the first two)
annotation_ids_to_delete = []
if extract_result.annotations and len(extract_result.annotations) > 0:
    annotation_ids_to_delete.append(extract_result.annotations[0].id)
    
    if len(extract_result.annotations) > 1:
        annotation_ids_to_delete.append(extract_result.annotations[1].id)
    
    print(f"Deleting annotations with IDs: {', '.join(map(str, annotation_ids_to_delete))}")
    
    # Step 4: Create request to remove specific annotations
    remove_request = groupdocs_annotation_cloud.RemoveAnnotationsRequest(file_info, annotation_ids_to_delete)
    remove_request.output_path = "output/annotations-removed.docx"
    
    # Call API to remove annotations
    remove_result = annotate_api.remove_annotations(remove_request)
    
    print(f"Annotations removed successfully. Document saved to {remove_result.path}")
else:
    print("No annotations found to delete.")

# Example of removing all annotations from a document
print("\nRemoving all annotations from the document...")

remove_all_request = groupdocs_annotation_cloud.RemoveAnnotationsRequest(file_info, [])  # Empty list means remove all
remove_all_request.output_path = "output/all-annotations-removed.docx"

remove_all_result = annotate_api.remove_annotations(remove_all_request)

print(f"All annotations removed successfully. Document saved to {remove_all_result.path}")
```

### 7. Implementing in Node.js

```javascript
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-node-samples
const { AnnotateApi, FileInfo } = require("groupdocs-annotation-cloud");

// Set up authentication credentials
const appSid = "XXXX-XXXX-XXXX-XXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
const appKey = "XXXXXXXXXXXXXXXX"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud

// Initialize API instance
const annotateApi = new AnnotateApi.fromKeys(appSid, appKey);

// Create file info object
const fileInfo = new FileInfo();
fileInfo.filePath = "annotationdocs/annotated-sample.docx";

// Step 1: Extract annotations to get their IDs
const extractRequest = {
    fileInfo: fileInfo
};

annotateApi.extract(extractRequest)
    .then((extractResult) => {
        console.log(`Found ${extractResult.annotations.length} annotations in the document.`);
        
        // Step 2: Select which annotations to delete (in this example, we'll delete the first two)
        const annotationIdsToDelete = [];
        if (extractResult.annotations && extractResult.annotations.length > 0) {
            annotationIdsToDelete.push(extractResult.annotations[0].id);
            
            if (extractResult.annotations.length > 1) {
                annotationIdsToDelete.push(extractResult.annotations[1].id);
            }
            
            console.log(`Deleting annotations with IDs: ${annotationIdsToDelete.join(', ')}`);
            
            // Step 3: Create request to remove specific annotations
            const removeRequest = {
                fileInfo: fileInfo,
                annotationIds: annotationIdsToDelete,
                outputPath: "output/annotations-removed.docx"
            };
            
            // Call API to remove annotations
            return annotateApi.removeAnnotations(removeRequest);
        } else {
            console.log("No annotations found to delete.");
            return Promise.resolve();
        }
    })
    .then((removeResult) => {
        if (removeResult) {
            console.log(`Annotations removed successfully. Document saved to ${removeResult.path}`);
        }
        
        // Example of removing all annotations from a document
        console.log("\nRemoving all annotations from the document...");
        
        const removeAllRequest = {
            fileInfo: fileInfo,
            annotationIds: [],  // Empty array means remove all annotations
            outputPath: "output/all-annotations-removed.docx"
        };
        
        return annotateApi.removeAnnotations(removeAllRequest);
    })
    .then((removeAllResult) => {
        console.log(`All annotations removed successfully. Document saved to ${removeAllResult.path}`);
    })
    .catch((error) => {
        console.error("Error:", error.message);
    });
```

## Try It Yourself

Now that you've seen the examples, it's time to try implementing annotation deletion in your own application:

1. Get your API credentials from the dashboard
2. Upload an annotated document to your storage or use a sample document
3. Choose your preferred programming language
4. Copy the appropriate code example above
5. Modify it with your credentials and document path
6. Run the code and verify that the annotations are removed

## Troubleshooting Tips

- Invalid Annotation IDs: Make sure you're using valid annotation IDs obtained from the extract operation.
- Authentication Errors: Verify your AppSID and AppKey are correctly set up.
- File Not Found Errors: Ensure the document path is correct and the file exists in your storage.
- Output Path Issues: Make sure the output folder exists in your storage or use a different path.
- Original Document Unchanged: If the original document remains unchanged, check that you're looking at the output document and not the original.

## What You've Learned

In this tutorial, you have learned:
- How to extract annotation IDs from a document
- How to remove specific annotations by ID
- How to delete all annotations from a document
- How to implement annotation deletion in C#, Python, and Node.js

## Further Practice

To solidify your understanding:
1. Create a function that removes annotations based on certain criteria (e.g., by page, type, or content)
2. Build a simple UI that allows users to select which annotations to delete
3. Implement a batch processing feature to remove annotations from multiple documents
4. Create a version control system that maintains both annotated and clean versions of documents

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
