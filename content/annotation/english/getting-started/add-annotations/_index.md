---
title: How to Add Annotations to Documents with GroupDocs.Annotation Cloud API Tutorial
weight: 1
url: /getting-started/add-annotations/
description: Learn how to add different types of annotations to documents using GroupDocs.Annotation Cloud API in this step-by-step developer tutorial
---

# Tutorial: How to Add Annotations to Documents with GroupDocs.Annotation Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Add various types of annotations to documents using the GroupDocs.Annotation Cloud API
- Configure annotation properties such as position, color, and opacity
- Understand the different annotation types and their specific properties
- Implement annotation functionality in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
1. A GroupDocs.Annotation Cloud account (if you don't have one, [get a free trial here](https://dashboard.groupdocs.cloud/#/apps)
2. Your API keys (AppSID and AppKey) from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. A document uploaded to your storage (or you can use the sample documents provided by the API)
4. Completed the [Get Document Information tutorial](/getting-started/get-document-information/) (recommended)
5. Basic knowledge of REST APIs
6. A development environment set up with your preferred language

## Practical Scenario

Imagine you're developing a document review system where multiple team members need to provide feedback on documents. Your application needs to support different annotation types like highlighting text, adding comments, marking areas, and drawing arrows to point out specific elements. This tutorial will show you how to implement these annotation capabilities.

## Understanding Annotation Types

GroupDocs.Annotation Cloud API supports several annotation types:

1. Area Annotation: Highlights a rectangular area on the page
2. Point Annotation: Places a point marker at a specific location
3. Text Annotation: Adds text notes with optional comments
4. Text Highlight Annotation: Highlights text within the document
5. Text Strikeout Annotation: Strikes through text
6. Text Underline Annotation: Underlines text
7. Polyline Annotation: Creates lines and shapes with multiple points
8. Arrow Annotation: Draws an arrow pointing to specific content
9. Watermark Annotation: Adds text watermarks to pages
10. Distance Annotation: Measures distance between two points

## Step-by-Step Guide

### 1. Understanding the API Endpoint

GroupDocs.Annotation Cloud API provides an endpoint to add annotations to a document:

```
POST https://api.groupdocs.cloud/v2.0/annotation/add
```

This endpoint requires you to specify the document path and annotation objects in the request body.

### 2. Preparing the Annotation Request

To add annotations, you'll need to:
1. Specify the input document path
2. Provide annotation objects with their properties
3. Optionally specify output options (like the output file path)

### 3. Creating an Area Annotation

Let's start with a simple example - adding an area annotation to highlight a rectangular region on a page:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/add" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/one-page.docx'
  },
  'OutputPath': 'output/annotated-with-area.docx',
  'Annotations': [
    {
      'Type': 'Area',
      'PageNumber': 1,
      'Box': {
        'X': 100,
        'Y': 100,
        'Width': 200,
        'Height': 100
      },
      'BackgroundColor': 16761035,
      'Opacity': 0.7
    }
  ]
}"
```

This request will:
- Take the document at `annotationdocs/one-page.docx`
- Add a pink semi-transparent area annotation on page 1
- Save the result to `output/annotated-with-area.docx`

### 4. Understanding Annotation Properties

Each annotation type has specific properties. Here are some common ones:

- Type: Specifies the annotation type (Area, Point, Text, etc.)
- PageNumber: The page where the annotation should appear (1-based)
- Box: Position and size of the annotation (X, Y, Width, Height)
- BackgroundColor: Color of the annotation (RGB integer value)
- Opacity: Transparency level (0.0 to 1.0)
- Text: Text content for text-based annotations
- FontColor: Color of the text
- FontSize: Size of the text
- Points: Array of points for polyline annotations

### 5. Adding Multiple Annotation Types

Let's see how to add multiple annotation types in a single request:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/add" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'annotationdocs/one-page.docx'
  },
  'OutputPath': 'output/multi-annotated.docx',
  'Annotations': [
    {
      'Type': 'Area',
      'PageNumber': 1,
      'Box': {
        'X': 100,
        'Y': 100,
        'Width': 200,
        'Height': 100
      },
      'BackgroundColor': 16761035,
      'Opacity': 0.7
    },
    {
      'Type': 'Point',
      'PageNumber': 1,
      'Box': {
        'X': 50,
        'Y': 50,
        'Width': 0,
        'Height': 0
      }
    },
    {
      'Type': 'Text',
      'PageNumber': 1,
      'Box': {
        'X': 300,
        'Y': 200,
        'Width': 200,
        'Height': 30
      },
      'Text': 'This is a text annotation',
      'FontColor': 255,
      'FontSize': 12
    }
  ]
}"
```

This request adds an area annotation, a point annotation, and a text annotation to the document.

### 6. Implementing in C#

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
    FilePath = "annotationdocs/one-page.docx"
};

// Create a collection of annotations
var annotations = new List<AnnotationInfo>
{
    // Area annotation
    new AnnotationInfo
    {
        Type = AnnotationInfo.TypeEnum.Area,
        PageNumber = 1,
        Box = new Rectangle
        {
            X = 100,
            Y = 100,
            Width = 200,
            Height = 100
        },
        BackgroundColor = 16761035, // RGB color (Pink)
        Opacity = 0.7
    },
    
    // Text annotation
    new AnnotationInfo
    {
        Type = AnnotationInfo.TypeEnum.Text,
        PageNumber = 1,
        Box = new Rectangle
        {
            X = 300,
            Y = 200,
            Width = 200,
            Height = 30
        },
        Text = "This is a text annotation",
        FontColor = 255, // RGB color (Blue)
        FontSize = 12
    }
};

// Create request object
var request = new AnnotateRequest(fileInfo, annotations)
{
    OutputPath = "output/annotated-document.docx"
};

// Call API method
var result = apiInstance.Annotate(request);

// Process the result
Console.WriteLine($"Document saved to {result.Path}");
```

### 7. Implementing in Python

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
# Create API instance
annotate_api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)

# Create file info object
file_info = groupdocs_annotation_cloud.FileInfo()
file_info.file_path = "annotationdocs/one-page.docx"

# Create annotations
annotations = []

# Area annotation
area_annotation = groupdocs_annotation_cloud.AnnotationInfo()
area_annotation.type = "Area"
area_annotation.page_number = 1
area_annotation.box = groupdocs_annotation_cloud.Rectangle()
area_annotation.box.x = 100
area_annotation.box.y = 100
area_annotation.box.width = 200
area_annotation.box.height = 100
area_annotation.background_color = 16761035  # RGB color (Pink)
area_annotation.opacity = 0.7
annotations.append(area_annotation)

# Arrow annotation
arrow_annotation = groupdocs_annotation_cloud.AnnotationInfo()
arrow_annotation.type = "Arrow"
arrow_annotation.page_number = 1
arrow_annotation.box = groupdocs_annotation_cloud.Rectangle()
arrow_annotation.box.x = 400
arrow_annotation.box.y = 400
arrow_annotation.box.width = 100
arrow_annotation.box.height = 100
annotations.append(arrow_annotation)

# Create request
request = groupdocs_annotation_cloud.AnnotateRequest(file_info, annotations)
request.output_path = "output/annotated-document.docx"

# Call API method
result = annotate_api.annotate(request)

# Process the result
print(f"Document saved to {result.path}")
```

## Try It Yourself

Now that you've seen the examples, it's time to try implementing annotations in your own application:

1. Get your API credentials from the dashboard
2. Upload a document to your storage or use a sample document
3. Choose your preferred programming language
4. Decide which annotation types you want to add
5. Configure the annotation properties (position, colors, etc.)
6. Run the code and observe the results

## Troubleshooting Tips

- Annotation Position Issues: If annotations don't appear where expected, make sure you've retrieved the document information first to understand page dimensions.
- Color Formatting: Colors are represented as RGB integer values. Use online color pickers to find the right values for your annotations.
- Annotation Type Errors: Double-check that you're using the correct properties for each annotation type. Some properties are specific to certain annotation types.
- Authentication Errors: Verify your AppSID and AppKey are correctly set up.
- File Not Found Errors: Make sure the document path is correct and the file exists in your storage.

## What You've Learned

In this tutorial, you have learned:
- How to add different types of annotations to documents using the GroupDocs.Annotation Cloud API
- How to configure annotation properties like position, color, and opacity
- How to use different annotation types for specific scenarios
- How to implement annotation functionality in C# and Python

## Further Practice

To solidify your understanding:
1. Try adding other annotation types not covered in this tutorial
2. Create a function that adds annotations based on user input (e.g., from a form)
3. Implement a feature to annotate multiple pages in a single request
4. Add annotations with different colors, opacity levels, and text formatting

## Next Steps

Ready to learn more? Check out our next tutorial on extracting annotations from documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

