---

title: How to Add Area Annotations Tutorial
weight: 1
url: /advanced-features/area-annotation/
description: Learn how to add area annotations to documents with GroupDocs.Annotation Cloud API in this step-by-step tutorial for developers
---

# Tutorial: How to Add Area Annotations

## Learning Objectives

In this tutorial, you'll learn how to add area annotations to documents using the GroupDocs.Annotation Cloud API. Area annotations allow you to mark rectangular regions within a document, which is useful for highlighting specific sections that require attention or additional information.

## Prerequisites

Before beginning this tutorial, you should have:

1. Basic knowledge of REST APIs and your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
2. A [GroupDocs.Cloud](https://dashboard.groupdocs.cloud) account and an app created with access to the Annotation API
3. Your Client ID and Client Secret from the dashboard
4. A sample document uploaded to your storage (we'll use a DOCX file in this tutorial)

## What is an Area Annotation?

Area annotation allows you to mark up a specific rectangular region within a document page. This type of annotation is useful when you need to highlight a particular section of text, an image, or any other content within your document that requires attention or additional information.

## Tutorial Steps

### Step 1: Authenticate with the API

Before adding annotations, you need to authenticate with the GroupDocs.Annotation Cloud API to get an access token.

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

### Step 2: Prepare Your Annotation Request

Now, let's create a request to add an area annotation to your document. The request should include:

1. File information (path to your document)
2. Output path (where to save the annotated document)
3. Annotation details (type, text, position, size, etc.)

Here is a sample cURL request:

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
    'Type': 'Area',
    'Text': 'This is area annotation',
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

Execute the prepared request to add the area annotation to your document. The API will return a response with a link to your annotated document.

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

### Step 5: Download the Annotated Document

You can download the annotated document using the link provided in the response.

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
        Type = AnnotationInfo.TypeEnum.Area,
        Text = "This is area annotation",
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
Console.WriteLine("AddAreaAnnotation: Area Annotation added: " + link.Title);
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
annotations[0].setType(TypeEnum.AREA);
annotations[0].setText("This is area annotation");
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
 
System.out.println("AddAreaAnnotation: Area Annotation added: " + result.getTitle());
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
a1.type = "Area"
a1.text = "This is area annotation"
a1.creator_name = "Anonym A."
 
file_info = FileInfo()
file_info.file_path = "annotationdocs\\one-page.docx"
options = AnnotateOptions()
options.file_info = file_info
options.annotations = [a1]
options.output_path = "Output\\output.docx"
 
request = AnnotateRequest(options)
result = api.annotate(request)         
 
print("AddAreaAnnotation: Area Annotation added: " + result['href'])
```

## Troubleshooting Tips

1. Authentication Errors: If you encounter authentication issues, verify your Client ID and Client Secret and make sure they're correctly input in your request.
2. File Not Found Errors: Ensure the file path is correct and the file exists in your storage.
3. Annotation Position Issues: If the annotation doesn't appear where expected, verify the coordinates and dimensions in your request.

## What You've Learned

In this tutorial, you've learned:
- How to create a request to add an area annotation to a document
- How to implement this functionality in various programming languages
- How to analyze and use the API response
- Common troubleshooting tips for annotation implementation

## Further Practice

To reinforce your learning, try these exercises:
1. Modify the example to change the color and opacity of the area annotation
2. Try adding multiple area annotations in a single request
3. Experiment with different sizes and positions for your area annotations

## Next Tutorial

Ready to learn about more annotation types? Proceed to our next tutorial on [How to Add Arrow Annotations](/advanced-features/arrow-annotation/).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.annotation-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to share your feedback and questions in our support forum!
