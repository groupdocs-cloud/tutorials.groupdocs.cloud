---
url: /advanced-features/image-annotation/
title: How to Add Image Annotations Tutorial
weight: 5
description: Learn how to embed image annotations within your documents using this step-by-step tutorial for GroupDocs.Annotation Cloud API
---

# Tutorial: How to Add Image Annotations

## Learning Objectives

In this tutorial, you'll learn how to add image annotations to documents using the GroupDocs.Annotation Cloud API. Image annotations allow you to embed pictures, logos, or other visual elements directly into your documents as annotations, enhancing the visual communication of your content.

## Prerequisites

Before beginning this tutorial, you should have:

1. Basic knowledge of REST APIs and your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
2. A [GroupDocs.Cloud](https://dashboard.groupdocs.cloud) account and an app created with access to the Annotation API
3. Your Client ID and Client Secret from the dashboard
4. A sample document uploaded to your storage (we'll use a DOCX file in this tutorial)
5. An image file uploaded to your storage (we'll use a PNG file in this tutorial)

## What is an Image Annotation?

An image annotation allows you to add images within a document page. This type of annotation is particularly useful for:

- Adding logos or company branding to documents
- Including visual examples or references
- Embedding screenshots or diagrams
- Enhancing document content with relevant imagery
- Adding signatures or stamps to documents

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

### Step 2: Upload Your Image File (If Not Already Uploaded)

Before you can add an image annotation, you need to ensure that the image file is available in your cloud storage. If you haven't already uploaded your image, you can do so using the following cURL command:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/annotation/storage/file/JohnSmith.png" \
-X PUT \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-F file=@"path/to/your/local/JohnSmith.png"
```

Make sure to replace `path/to/your/local/JohnSmith.png` with the actual path to your local image file.

### Step 3: Prepare Your Image Annotation Request

Now, let's create a request to add an image annotation to your document. The following parameters are important for image annotations:

- Type: Must be set to "Image"
- ImagePath: Path to the image file in your cloud storage
- Box: Defines the position and dimensions where the image will be placed
- PageNumber: Indicates which page to annotate
- Opacity: Controls the transparency of the image

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
    'Type': 'Image',
    'Text': 'This is image annotation',
    'ImagePath': 'JohnSmith.png',
    'CreatorName': 'Anonym A.',
    'Box': {
      'X': 100,
      'Y': 100,
      'Width': 200,
      'Height': 100
    },
    'PageNumber': 0,
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
    'Opacity': 0.7
  }
]
}
"
```

### Step 4: Execute the Request

Execute the prepared request to add the image annotation to your document. The API will return a response with a link to your annotated document.

### Step 5: Analyze the Response

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

### Step 6: Download and Verify the Annotated Document

You can download the annotated document using the link provided in the response. Open the document to verify that your image annotation appears correctly.

## Important Parameters for Image Annotations

When working with image annotations, it's important to understand the key parameters:

1. ImagePath: The path to the image file in your cloud storage. This must be accurately specified, or the annotation will fail.
2. Box: Defines the position (X, Y) and size (Width, Height) of the image annotation. Make sure to set appropriate values for your document.
3. Opacity: Controls the transparency of the image. Values range from 0.0 (completely transparent) to 1.0 (completely opaque).
4. PageNumber: Specifies the page on which the image annotation will appear. Page numbering starts from 0.

## SDK Examples

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = ""; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
var configuration = new Configuration(MyAppSid, MyAppKey);
  
var apiInstance = new AnnotateApi(configuration);
 
var fileInfo = new FileInfo { FilePath = "one-page.docx" };
 
AnnotationInfo[] annotations =
{
    new AnnotationInfo
    {
        Box = new Rectangle { X = 100, Y = 100, Width = 200, Height = 100 },
        PageNumber = 0,
        Type = AnnotationInfo.TypeEnum.Image,
        ImagePath = "JohnSmith.png",
        Text = "This is image annotation",
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
Console.WriteLine("AddImageAnnotation: Image Annotation added: " + link.Title);
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
 
Rectangle r = new Rectangle();
r.setX(100.0);
r.setY(100.0);
r.setWidth(200.0);
r.setHeight(100.0);
 
annotations[0].setBox(r);
annotations[0].setImagePath("Annotationdocs\\JohnSmith.png");
annotations[0].setPageNumber(0);
annotations[0].setType(TypeEnum.IMAGE);
annotations[0].setText("This is image annotation");
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
 
System.out.println("AddImageAnnotation: image Annotation added: " + result.getTitle());
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
 
app_sid = "XXXX-XXXX-XXXX-XXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "XXXXXXXXXXXXXXXX" # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
  
api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)
 
a1 = groupdocs_annotation_cloud.AnnotationInfo()
a1.box = groupdocs_annotation_cloud.Rectangle()
a1.box.x = 100
a1.box.y = 100
a1.box.width = 200
a1.box.height = 100
a1.page_number = 0
a1.opacity = 0.7
a1.type = "Image"
a1.text = "This is Image annotation"
a1.creator_name = "Anonym A."
a1.image_path = "annotationdocs\\JohnSmith.png"
 
file_info = FileInfo()
file_info.file_path = "annotationdocs\\one-page.docx"
options = AnnotateOptions()
options.file_info = file_info
options.annotations = [a1]
options.output_path = "Output\\output.docx"
 
request = AnnotateRequest(options)
result = api.annotate(request)         
 
print("AddImageAnnotation: Image Annotation added: " + result['href'])
```

## Image Formatting and Size Considerations

When working with image annotations, keep these important considerations in mind:

1. Image Formats: The GroupDocs.Annotation Cloud API supports common image formats like PNG, JPG, JPEG, BMP, and GIF.
2. Image Size: Large images may be scaled to fit within the specified box dimensions. Choose appropriate box dimensions to maintain image clarity.
3. Resolution: Higher resolution images will appear clearer in the document, but may increase the document size.
4. Aspect Ratio: The image will be scaled to fit within the box dimensions, which may distort the image if the box's aspect ratio differs from the image's aspect ratio.

## Troubleshooting Tips

1. Missing Image: If your image annotation doesn't appear, check that the ImagePath is correct and the image file exists in your cloud storage.
2. Image Size Issues: If the image appears too small or too large, adjust the Width and Height values in the Box parameter.
3. Image Quality: If the image appears distorted or blurry, try uploading a higher-resolution image or adjusting the box dimensions to match the image's aspect ratio.
4. File Format Problems: If the image doesn't display correctly, ensure you're using a supported image format.

## What You've Learned

In this tutorial, you've learned:
- How to upload image files to your cloud storage
- How to create and add image annotations to documents
- The essential parameters for configuring image annotations
- How to customize the appearance and position of image annotations
- How to implement image annotations in different programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Add multiple image annotations to a single document
2. Experiment with different image formats to compare quality and file size
3. Try overlapping image annotations with different opacity levels
4. Create image annotations of different sizes and positions

## Next Tutorial

Ready to learn about more annotation types? Proceed to our next tutorial on [How to Add Image Annotations](/advanced-features/image-annotation/).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/annotation/)
- [Documentation](https://docs.groupdocs.cloud/annotation/)
- [Live Demo](https://products.groupdocs.app/annotation/family)
- [API Reference](https://reference.groupdocs.cloud/annotation/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.annotation-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/10/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
