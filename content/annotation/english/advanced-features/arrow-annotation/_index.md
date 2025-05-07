---
title: How to Add Arrow Annotations Tutorial
weight: 2
url: /advanced-features/arrow-annotation/
description: Master the process of adding arrow annotations to documents with this step-by-step tutorial for GroupDocs.Annotation Cloud API
---

# Tutorial: How to Add Arrow Annotations

## Learning Objectives

In this tutorial, you'll learn how to add arrow annotations to documents using the GroupDocs.Annotation Cloud API. Arrow annotations are useful for pointing to specific elements in a document and can be customized with different colors, styles, and properties.

## Prerequisites

Before beginning this tutorial, you should have:

1. Basic knowledge of REST APIs and your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
2. A [GroupDocs.Cloud](https://dashboard.groupdocs.cloud) account and an app created with access to the Annotation API
3. Your Client ID and Client Secret from the dashboard
4. A sample document uploaded to your storage (we'll use a DOCX file in this tutorial)

## What is an Arrow Annotation?

An arrow annotation draws a directional indicator on a document page, allowing you to point to specific content that requires attention. It's commonly used for:

- Highlighting important sections
- Indicating relationships between document elements
- Providing visual guidance for document reviews
- Drawing attention to specific details within the document

## Tutorial Steps

### Step 1: Authenticate with the API

Before adding any annotations, you need to authenticate with the GroupDocs.Annotation Cloud API to obtain an access token.

Try it yourself:

Use the following cURL command to authenticate and obtain your access token:

```bash
// First get JSON Web Token
// Please get your Client Id and Client Secret from https://dashboard.groupdocs.cloud/applications
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the access token from the response for use in the next steps.

### Step 2: Prepare Your Arrow Annotation Request

Now, let's create a request to add an arrow annotation to your document. The following parameters are important for arrow annotations:

- Type: Must be set to "Arrow"
- Box: Defines the position and dimensions of the arrow
- PageNumber: Specifies which page to annotate
- PenColor: Sets the color of the arrow
- PenWidth: Controls the thickness of the arrow
- PenStyle: Defines the line style (solid, dashed, etc.)
- Text: Optional comment associated with the annotation

Here's a sample cURL request:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/add" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "
{
  'FileInfo': {
    'FilePath': 'annotationdocs/one-page.docx'
  },
  'OutputPath': 'Output/output.docx',
  'Annotations': [
  {
    'Type': 'Arrow',
    'Text': 'This is arrow annotation',
    'CreatorName': 'Anonym A.',
    'Box': {
      'X': 100,
      'Y': 100,
      'Width': 100,
      'Height': 100
    },
    'PageNumber': 0,
    'AnnotationPosition': {
      'X': 1,
      'Y': 1
    },
    'Replies': [
      {
        'Comment': 'First comment',
        'RepliedOn': '2023-01-15T06:52:01.376Z'
      },
      {
        'Comment': 'Second comment',
        'RepliedOn': '2023-01-15T06:52:01.376Z'
      }
    ],
    'CreatedOn': '2023-01-15T06:52:01.376Z',
    'PenStyle': 'Solid',
    'PenColor': 65535,
    'PenWidth': 3,
    'BackgroundColor': 65535,
    'Opacity': 0.7
  }
]
}
"
```

### Step 3: Execute the Request

Execute the prepared request to add the arrow annotation to your document. The API will return a response with a link to your annotated document.

### Step 4: Analyze the Response

After executing the request, you'll receive a response similar to the following:

```json
{
  "href": "https://api.groupdocs.cloud/v2.0/annotation/storage/file/Output/output.docx",
  "rel": "self",
  "type": "file",
  "title": "output.docx"
}
```

This response provides a link to your annotated document stored in your cloud storage.

### Step 5: Download and Verify the Annotated Document

You can download the annotated document using the link provided in the response. Open the document to verify that your arrow annotation appears correctly.

## Arrow Annotation Customization Options

You can customize your arrow annotations in various ways:

- Color: Change the `PenColor` value to use different colors
- Transparency: Adjust the `Opacity` value between 0.0 (transparent) and 1.0 (opaque)
- Thickness: Modify the `PenWidth` value to make thinner or thicker arrows
- Style: Set `PenStyle` to different values like "Solid", "Dash", or "Dot"

## SDK Examples

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyAppSid, MyAppKey);
  
var apiInstance = new AnnotateApi(configuration);
 
var fileInfo = new FileInfo {FilePath = "one-page.docx"};
 
AnnotationInfo[] annotations =
{
    new AnnotationInfo
    {
        AnnotationPosition = new Point { X = 1, Y = 1 },
        Box = new Rectangle { X = 100, Y = 100, Width = 100, Height = 100 },
        PageNumber = 0,
        BackgroundColor = 65535,
        PenColor = 65535,
        PenStyle = AnnotationInfo.PenStyleEnum.Solid,
        PenWidth = 3,
        Opacity = 0.7,
        Type = AnnotationInfo.TypeEnum.Arrow,
        Text = "This is arrow annotation",
        CreatorName = "Anonym A.",
        CreatedOn = DateTime.Now,
        Replies = new List<AnnotationReplyInfo>
        {
            new AnnotationReplyInfo
            {
                Comment = "First comment",
                RepliedOn = DateTime.Now
            },
            new AnnotationReplyInfo
            {
                Comment = "Second comment",
                RepliedOn = DateTime.Now
            }
        }
    },
};
 
var options = new AnnotateOptions
{
    FileInfo = fileInfo,
    Annotations = annotations.ToList(),
    OutputPath = "Output/output.docx"
};
 
var link = apiInstance.Annotate(new AnnotateRequest(options));
Console.WriteLine("AddArrowAnnotation: Arrow Annotation added: " + link.Title);
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-java-samples
String MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
String MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
Configuration configuration = new Configuration(MyAppSid, MyAppKey);
  
AnnotateApi apiInstance = new AnnotateApi(configuration); 
 
// Create annotation/s.
AnnotationInfo[] annotations = new AnnotationInfo[1];
annotations[0] = new AnnotationInfo();
 
Point pt = new Point();
pt.setX(1.0);
pt.setY(1.0);
annotations[0].setAnnotationPosition(pt);
 
Rectangle r = new Rectangle();
r.setX(100.0);
r.setY(100.0);
r.setWidth(200.0);
r.setHeight(100.0);
 
annotations[0].setBox(r);
annotations[0].setPageNumber(0);
annotations[0].setPenColor(1201033);
annotations[0].setPenStyle(PenStyleEnum.SOLID);
annotations[0].setPenWidth(1);
annotations[0].setBackgroundColor(65535);
annotations[0].setOpacity(0.7);
annotations[0].setType(TypeEnum.ARROW);
annotations[0].setText("This is arrow annotation");
annotations[0].setCreatorName("Anonym A.");
 
// Create request object.
FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("Annotationdocs\\one-page.docx");
 
AnnotateOptions options = new AnnotateOptions();
options.setFileInfo(fileInfo);
options.setAnnotations(Arrays.asList(annotations));
options.setOutputPath("Output/one-page-annotated.docx");
 
AnnotateRequest request = new AnnotateRequest(options);
 
// Executing api method.
AnnotationApiLink result = apiInstance.annotate(request);
 
System.out.println("AddArrowAnnotation: Arrow Annotation added: " + result.getTitle());
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)
 
a1 = groupdocs_annotation_cloud.AnnotationInfo()
a1.annotation_position = groupdocs_annotation_cloud.Point()
a1.annotation_position.x = 1
a1.annotation_position.y = 1
a1.box = groupdocs_annotation_cloud.Rectangle()
a1.box.x = 100
a1.box.y = 100
a1.box.width = 200
a1.box.height = 100
a1.page_number = 0
a1.pen_color = 1201033
a1.pen_style = "Solid"
a1.pen_width = 1
a1.opacity = 0.7
a1.type = "Arrow"
a1.text = "This is arrow annotation"
a1.creator_name = "Anonym A."
 
file_info = FileInfo()
file_info.file_path = "annotationdocs\\one-page.docx"
options = AnnotateOptions()
options.file_info = file_info
options.annotations = [a1]
options.output_path = "Output\\output.docx"
 
request = AnnotateRequest(options)
result = api.annotate(request)         
 
print("AddArrowAnnotation: Arrow Annotation added: " + result['href'])
```

## Troubleshooting Tips

1. Invisible Arrows: If your arrow isn't visible, check that the pen color offers enough contrast with the document background.
2. Size Issues: If the arrow is too small, try increasing the Width and Height values in the Box parameter.
3. Direction Problems: Remember that the arrow direction is determined by the Box coordinates. The arrow points from the top-left to the bottom-right corner of the Box.
4. Color Display Issues: If colors don't display correctly, ensure you're using valid RGB color values.

## What You've Learned

In this tutorial, you've learned:
- How to create and add arrow annotations to documents
- The essential parameters for configuring arrow annotations
- How to customize the appearance of arrow annotations
- How to implement arrow annotations in different programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Create an arrow annotation with a different color
2. Add multiple arrow annotations to a document in a single request
3. Experiment with different pen styles (Solid, Dash, Dot)
4. Create arrows of different sizes and thicknesses

## Next Tutorial

Ready to learn about more annotation types? Proceed to our next tutorial on [How to Add Distance Annotations](/advanced-features/distance-annotation/).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.annotation-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
