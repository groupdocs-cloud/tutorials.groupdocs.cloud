---
title: How to Add Text Redaction Annotations in GroupDocs.Annotation Cloud Tutorial
description: Learn how to securely redact sensitive text in documents using the GroupDocs.Annotation Cloud API in this step-by-step tutorial
weight: 110
url: /advanced-features/text-redaction/
---

# Tutorial: How to Add Text Redaction Annotations

## Learning Objectives

In this tutorial, you'll learn how to:
- Add text redaction annotations to securely hide sensitive text in documents
- Configure redaction properties and appearance
- Implement text redaction in different programming languages
- Create a secure workflow for handling sensitive information

## What is a Text Redaction Annotation?

Text redaction annotations allow you to permanently hide sensitive text in documents by replacing it with black rectangles. Unlike other annotation types that can be removed, redaction permanently removes the underlying text, making it ideal for securing sensitive information in documents before sharing them.

## Prerequisites

Before you begin:
1. A GroupDocs.Annotation Cloud account (or [get a free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. A development environment for your preferred language
4. A sample document uploaded to your GroupDocs.Annotation Cloud storage

## Implementation Steps

Let's walk through the process of adding text redaction annotations to a document:

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

### 2. Define Text Redaction Properties

Text redaction annotations require the following properties:
- Coordinates of the text area to redact (Points array)
- Page number
- Creator information
- Optional comments (replies)

### 3. Add the Text Redaction Annotation

Use the `POST /annotation/add` endpoint to add the text redaction:

```javascript
curl -v "https://api.groupdocs.cloud/v2.0/annotation/add" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'FileInfo': {
    'FilePath': 'your-document.docx'
  },
  'OutputPath': 'output/redacted-document.docx',
  'Annotations': [
    {
      'Type': 'TextRedaction',
      'Text': 'This text will be redacted',
      'CreatorName': 'Security Officer',
      'Points': [
        {
          'X': 80,
          'Y': 730
        },
        {
          'X': 240,
          'Y': 730
        },
        {
          'X': 80,
          'Y': 650
        },
        {
          'X': 240,
          'Y': 650
        }
      ],
      'PageNumber': 0,
      'Replies': [
        {
          'Comment': 'Sensitive information redacted',
          'RepliedOn': '2023-04-25T12:00:00.000Z'
        }
      ]
    }
  ]
}"
```

### 4. Download the Redacted Document

After successful redaction, download your redacted document using the File API.

## Try It Yourself

Now, try implementing text redaction annotations in your preferred programming language.

### C# Example

```csharp
// For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-dotnet-samples
string MyAppKey = "YOUR_APP_KEY"; // Get AppKey and AppSID from https://dashboard.groupdocs.cloud
string MyAppSid = "YOUR_APP_SID";

var configuration = new Configuration(MyAppSid, MyAppKey);
var apiInstance = new AnnotateApi(configuration);

var fileInfo = new FileInfo { FilePath = "input/document-with-sensitive-info.docx" };

AnnotationInfo[] annotations =
{
    new AnnotationInfo
    {
        Points = new List<Point>
        {
            new Point {X = 80, Y = 730}, 
            new Point {X = 240, Y = 730}, 
            new Point {X = 80, Y = 650}, 
            new Point {X = 240, Y = 650}
        },
        PageNumber = 0,
        Type = AnnotationInfo.TypeEnum.TextRedaction,
        Text = "This text will be redacted",
        CreatorName = "Security Officer",
        CreatedOn = DateTime.Now,
        Replies = new List<AnnotationReplyInfo>
        {
            new AnnotationReplyInfo
            {
                Comment = "Sensitive information redacted",
                RepliedOn = DateTime.Now
            }
        }
    },
};

var options = new AnnotateOptions
{
    FileInfo = fileInfo,
    Annotations = annotations.ToList(),
    OutputPath = "output/redacted-document.docx"
};

var link = apiInstance.Annotate(new AnnotateRequest(options));
Console.WriteLine("Text Redaction Annotation added: " + link.Title);
```

### Java Example

```java
// For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-java-samples
String clientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String clientSecret = "YOUR_CLIENT_SECRET";

Configuration configuration = new Configuration(clientId, clientSecret);
AnnotateApi apiInstance = new AnnotateApi(configuration);

// Create annotation
AnnotationInfo annotation = new AnnotationInfo();

Point[] points = new Point[4];
points[0] = new Point();
points[0].setX(80.0);
points[0].setY(730.0);
points[1] = new Point();
points[1].setX(240.0);
points[1].setY(730.0);
points[2] = new Point();
points[2].setX(80.0);
points[2].setY(650.0);
points[3] = new Point();
points[3].setX(240.0);
points[3].setY(650.0);

annotation.setPoints(Arrays.asList(points));
annotation.setPageNumber(0);
annotation.setType(TypeEnum.TEXTREDACTION);
annotation.setText("This text will be redacted");
annotation.setCreatorName("Security Officer");

// Create reply/comment
AnnotationReplyInfo reply = new AnnotationReplyInfo();
reply.setComment("Sensitive information redacted");
reply.setRepliedOn(OffsetDateTime.now());
annotation.setReplies(Collections.singletonList(reply));

// Create request object
FileInfo fileInfo = new FileInfo();
fileInfo.setFilePath("input/document-with-sensitive-info.docx");

AnnotateOptions options = new AnnotateOptions();
options.setFileInfo(fileInfo);
options.setAnnotations(Collections.singletonList(annotation));
options.setOutputPath("output/redacted-document.docx");

AnnotateRequest request = new AnnotateRequest(options);

// Execute API method
AnnotationApiLink result = apiInstance.annotate(request);

System.out.println("Text Redaction Annotation added: " + result.getTitle());
```

### Python Example

```python
# For complete examples, visit: https://github.com/groupdocs-annotation-cloud/groupdocs-annotation-cloud-python-samples
import groupdocs_annotation_cloud
from datetime import datetime

app_sid = "YOUR_APP_SID"  # Get AppKey and AppSID from https://dashboard.groupdocs.cloud
app_key = "YOUR_APP_KEY"

api = groupdocs_annotation_cloud.AnnotateApi.from_keys(app_sid, app_key)

# Create annotation
annotation = groupdocs_annotation_cloud.AnnotationInfo()

# Define points for text redaction
p1 = groupdocs_annotation_cloud.Point()
p1.x = 80
p1.y = 730
p2 = groupdocs_annotation_cloud.Point()
p2.x = 240
p2.y = 730
p3 = groupdocs_annotation_cloud.Point()
p3.x = 80
p3.y = 650
p4 = groupdocs_annotation_cloud.Point()
p4.x = 240
p4.y = 650
annotation.points = [p1, p2, p3, p4]

annotation.page_number = 0
annotation.type = "TextRedaction"
annotation.text = "This text will be redacted"
annotation.creator_name = "Security Officer"

# Create reply/comment
reply = groupdocs_annotation_cloud.AnnotationReplyInfo()
reply.comment = "Sensitive information redacted"
reply.replied_on = datetime.now().isoformat()
annotation.replies = [reply]

# Set up options
file_info = groupdocs_annotation_cloud.FileInfo()
file_info.file_path = "input/document-with-sensitive-info.docx"

options = groupdocs_annotation_cloud.AnnotateOptions()
options.file_info = file_info
options.annotations = [annotation]
options.output_path = "output/redacted-document.docx"

request = groupdocs_annotation_cloud.AnnotateRequest(options)
result = api.annotate(request)

print("Text Redaction Annotation added: " + result.title)
```

## Security Considerations for Text Redaction

When implementing text redaction, keep these security best practices in mind:

1. Permanent Redaction: Text redaction in GroupDocs.Annotation Cloud is permanent - the original text cannot be recovered after redaction.

2. Review Before Sharing: Always review redacted documents before sharing them to ensure all sensitive content has been properly redacted.

3. Metadata Check: Remember that document metadata might contain sensitive information. Consider using additional tools to remove metadata.

4. Access Control: Implement proper access controls to ensure only authorized personnel can perform redactions.

5. Audit Trail: Maintain an audit trail of all redaction activities for compliance purposes.

## Troubleshooting Tips

1. Text Not Fully Redacted: Ensure the Points array completely covers the text to be redacted
2. Redaction Not Visible: Check that the page number is correct (zero-based)
3. Redaction Box Size: If the redaction box is too small, adjust the Points coordinates

## What You've Learned

In this tutorial, you've learned:
- How to implement text redaction annotations to permanently hide sensitive information
- Best practices for secure document redaction
- How to implement the redaction process in different programming languages
- Security considerations for document redaction workflows

## Further Practice

To enhance your understanding of text redaction, try these exercises:
1. Redact multiple sections of text in a single document
2. Create a workflow that identifies personally identifiable information (PII) and applies redactions automatically
3. Combine text redaction with resource redaction to create fully sanitized documents

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
