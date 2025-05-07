

## Try It Yourself

Now that you've seen the examples, it's time to try implementing annotation extraction in your own application:

1. Get your API credentials from the dashboard
2. Upload an annotated document to your storage or use a sample document
3. Choose your preferred programming language
4. Copy the appropriate code example above
5. Modify it with your credentials and document path
6. Run the code and analyze the extracted annotations

## Troubleshooting Tips

- No Annotations Found: If the API returns an empty list, verify that the document actually contains annotations.
- Wrong Document Format: Make sure you're using a supported document format.
- Authentication Errors: Verify your AppSID and AppKey are correctly set up.
- File Not Found Errors: Ensure the document path is correct and the file exists in your storage.
- Date Parsing Issues: If you encounter problems with date parsing, make sure you're handling timezone information correctly.

## What You've Learned

In this tutorial, you have learned:
- How to extract annotations from documents using the GroupDocs.Annotation Cloud API
- How to process different annotation types and their properties
- How to filter and group annotations for analysis
- How to implement annotation extraction in C#, Python, and Node.js

## Further Practice

To solidify your understanding:
1. Create a function to export annotations to a CSV or JSON file
2. Build a simple UI that displays extracted annotations in a table
3. Implement annotation filtering by date range, user, or content
4. Create a report generator that summarizes annotations by type and page

## Next Steps

Ready to learn more? Check out our next tutorial on converting annotated documents to different formats.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to ask on our [support forum](https://forum.groupdocs.cloud/c/annotation/10/)!---
id: "extract-annotations-tutorial"
url: /getting-started/extract-annotations"
title: "Tutorial: How to Extract Annotations from Documents with GroupDocs.Annotation Cloud API"

weight: 2
description: "Learn how to extract and process existing annotations from documents using GroupDocs.Annotation Cloud API in this step-by-step tutorial"
keywords: "extract annotations, get annotations, document annotation tutorial, cloud api, annotation retrieval, process annotations, groupdocs cloud"
toc: True
---

# Tutorial: How to Extract Annotations from Documents with GroupDocs.Annotation Cloud API

Difficulty Level: Intermediate  
Estimated Time: 25 minutes

## Learning Objectives

In this tutorial, you'll learn how to:
- Extract existing annotations from documents using the GroupDocs.Annotation Cloud API
- Parse and process annotation data including position, type, and content
- Filter annotations by different criteria
- Implement annotation extraction in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Annotation Cloud account (if you don't have one, [get a free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your API keys (AppSID and AppKey) from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document with annotations uploaded to your storage (ideally, follow the [Add Annotations tutorial](/getting-started/add-annotations) first to create one)
4. Basic knowledge of REST APIs
5. A development environment set up with your preferred language

## Practical Scenario

In a collaborative document review system, you not only need to add annotations but also retrieve and process them. For example, you might want to display all comments made by a specific reviewer, compile a summary of feedback, or generate a report of all annotations in a document. This tutorial shows you how to extract annotations from documents to enable these scenarios.

## Step-by-Step Guide

### 1. Understanding the API Endpoint

GroupDocs.Annotation Cloud API provides an endpoint to extract annotations from a document:

```
POST https://api.groupdocs.cloud/v2.0/annotation/extract
```

This endpoint requires you to specify the document path in the request body.

### 2. Making the API Request with cURL

Let's try calling the API using cURL to understand the request and response structure:

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

Replace `YOUR_ACCESS_TOKEN` with your actual JWT token obtained from your AppSID and AppKey.

### 3. Understanding the Response

The API returns a JSON response containing an array of annotations with their properties. Here's an example of what the response might look like:

```json
{
  "annotations": [
    {
      "type": "Area",
      "pageNumber": 1,
      "box": {
        "x": 100,
        "y": 100,
        "width": 200,
        "height": 100
      },
      "backgroundColor": 16761035,
      "opacity": 0.7,
      "createdOn": "2023-07-25T14:20:30.123Z",
      "id": 1
    },
    {
      "type": "Text",
      "pageNumber": 1,
      "box": {
        "x": 300,
        "y": 200,
        "width": 150,
        "height": 30
      },
      "text": "This is a text annotation",
      "fontColor": 255,
      "fontSize": 12,
      "opacity": 1.0,
      "createdOn": "2023-07-25T14:22:45.678Z",
      "id": 2
    }
  ]
}
```

### 4. Processing Annotation Data

Once you've extracted annotations, you can process them to:
- Count total annotations by type
- Extract text content from text-based annotations
- Determine annotation positions and sizes
- Filter annotations by page, type, or other criteria
- Generate reports or summaries

### 5. Implementing in C#

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyAppSid, MyAppKey);
  
// Create AnnotateApi instance
var apiInstance = new AnnotateApi(configuration);

// Create file info object
var fileInfo = new FileInfo
{
    FilePath = "annotationdocs/annotated-sample.docx"
};

// Create request object
var request = new ExtractRequest(fileInfo);

// Call API method
var result = apiInstance.Extract(request);

// Process the extracted annotations
if (result.Annotations != null && result.Annotations.Count > 0)
{
    Console.WriteLine($"Found {result.Annotations.Count} annotations:");
    
    // Group annotations by type for easy analysis
    var annotationsByType = result.Annotations.GroupBy(a => a.Type);
    
    foreach (var group in annotationsByType)
    {
        Console.WriteLine($"\n{group.Key} annotations ({group.Count()}):");
        
        foreach (var annotation in group)
        {
            Console.WriteLine($"  - ID: {annotation.Id}");
            Console.WriteLine($"    Page: {annotation.PageNumber}");
            Console.WriteLine($"    Position: X={annotation.Box.X}, Y={annotation.Box.Y}");
            
            // Handle specific annotation type properties
            if (annotation.Type == AnnotationInfo.TypeEnum.Text ||
                annotation.Type == AnnotationInfo.TypeEnum.TextReplacement)
            {
                Console.WriteLine($"    Text: {annotation.Text}");
            }
            
            if (annotation.CreatedOn.HasValue)
            {
                Console.WriteLine($"    Created: {annotation.CreatedOn.Value:yyyy-MM-dd HH:mm:ss}");
            }
            
            Console.WriteLine();
        }
    }
}
else
{
    Console.WriteLine("No annotations found in the document.");
}
```

### 6. Implementing in Python

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
from collections import defaultdict
from datetime import datetime
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
# Create API instance
annotate_api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)

# Create file info object
file_info = groupdocs_annotation_cloud.FileInfo()
file_info.file_path = "annotationdocs/annotated-sample.docx"

# Create request object
request = groupdocs_annotation_cloud.ExtractRequest(file_info)

# Call API method
result = annotate_api.extract(request)

# Process the extracted annotations
if result.annotations and len(result.annotations) > 0:
    print(f"Found {len(result.annotations)} annotations:")
    
    # Group annotations by type
    annotations_by_type = defaultdict(list)
    for annotation in result.annotations:
        annotations_by_type[annotation.type].append(annotation)
    
    # Process each annotation type
    for annotation_type, annotations in annotations_by_type.items():
        print(f"\n{annotation_type} annotations ({len(annotations)}):")
        
        for annotation in annotations:
            print(f"  - ID: {annotation.id}")
            print(f"    Page: {annotation.page_number}")
            print(f"    Position: X={annotation.box.x}, Y={annotation.box.y}")
            
            # Handle specific annotation type properties
            if annotation_type in ["Text", "TextReplacement"]:
                print(f"    Text: {annotation.text}")
            
            if annotation.created_on:
                created_time = datetime.fromisoformat(annotation.created_on.replace('Z', '+00:00'))
                print(f"    Created: {created_time.strftime('%Y-%m-%d %H:%M:%S')}")
            
            print()
            
    # Example of filtering annotations by page
    page_1_annotations = [a for a in result.annotations if a.page_number == 1]
    print(f"\nAnnotations on page 1: {len(page_1_annotations)}")
    
    # Example of extracting all text content from text annotations
    text_contents = [a.text for a in result.annotations if hasattr(a, 'text') and a.text]
    if text_contents:
        print("\nText annotation contents:")
        for i, text in enumerate(text_contents, 1):
            print(f"  {i}. {text}")
else:
    print("No annotations found in the document.")
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

// Create request object
const request = {
    fileInfo: fileInfo
};

// Call API method to extract annotations
annotateApi.extract(request)
    .then((result) => {
        if (result.annotations && result.annotations.length > 0) {
            console.log(`Found ${result.annotations.length} annotations:`);
            
            // Group annotations by type
            const annotationsByType = {};
            result.annotations.forEach(annotation => {
                if (!annotationsByType[annotation.type]) {
                    annotationsByType[annotation.type] = [];
                }
                annotationsByType[annotation.type].push(annotation);
            });
            
            // Process each annotation type
            Object.keys(annotationsByType).forEach(type => {
                const annotations = annotationsByType[type];
                console.log(`\n${type} annotations (${annotations.length}):`);
                
                annotations.forEach(annotation => {
                    console.log(`  - ID: ${annotation.id}`);
                    console.log(`    Page: ${annotation.pageNumber}`);
                    console.log(`    Position: X=${annotation.box.x}, Y=${annotation.box.y}`);
                    
                    // Handle specific annotation type properties
                    if (type === "Text" || type === "TextReplacement") {
                        console.log(`    Text: ${annotation.text}`);
                    }
                    
                    if (annotation.createdOn) {
                        const createdDate = new Date(annotation.createdOn);
                        console.log(`    Created: ${createdDate.toISOString().replace('T', ' ').substr(0, 19)}`);
                    }
                    
                    console.log();
                });
            });
            
            // Example of filtering annotations by page
            const page1Annotations = result.annotations.filter(a => a.pageNumber === 1);
            console.log(`\nAnnotations on page 1: ${page1Annotations.length}`);
            
            // Example of extracting all text content from text annotations
            const textContents = result.annotations
                .filter(a => a.text && a.text.trim().length > 0)
                .map(a => a.text);
                
            if (textContents.length > 0) {
                console.log("\nText annotation contents:");
                textContents.forEach((text, index) => {
                    console.log(`  ${index + 1}. ${text}`);
                });
            }
        } else {
            console.log("No annotations found in the document.");
        }
    })
    .catch((error) => {
        console.error("Error extracting annotations:", error.message);
    });

// Expected output:
// Found 4 annotations:
//
// Area annotations (1):
//   - ID: 1
//     Page: 1
//     Position: X=100, Y=100
//     Created: 2023-07-25 14:20:30
//
// Text annotations (1):
//   - ID: 2
//     Page: 1
//     Position: X=300, Y=200
//     Text: This is a text annotation
//     Created: 2023-07-25 14:22:45
//
// Arrow annotations (1):
//   - ID: 3
//     Page: 1
//     Position: X=400, Y=400
//
// TextHighlight annotations (1):
//   - ID: 4
//     Page: 1
//     Position: X=50, Y=350
//
// Annotations on page 1: 4
//
// Text annotation contents:
//   1. This is a text annotation