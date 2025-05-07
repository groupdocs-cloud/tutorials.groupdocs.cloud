---
title: How to Join Images Tutorial
url: /document-management/join-images/
weight: 35
description: Learn how to combine multiple images horizontally or vertically using GroupDocs.Merger Cloud API in this comprehensive tutorial for developers.
---

# Tutorial: How to Join Images

## Introduction

In this tutorial, you'll learn how to join multiple images into a single composite image using GroupDocs.Merger Cloud API. Image joining is a powerful feature that allows you to combine separate image files either horizontally (side by side) or vertically (stacked). This capability is particularly useful for creating panoramas, comparison images, or consolidated visual reports.

GroupDocs.Merger Cloud provides a simple yet flexible way to join images with control over the orientation of the combined result.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Join multiple images into a single composite image
- Control the joining orientation (horizontal or vertical)
- Implement image joining in different programming languages
- Handle various image formats during joining operations

## Prerequisites

Before starting this tutorial, make sure you have:
- A GroupDocs.Merger Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic knowledge of RESTful APIs
- Sample image files to join (JPG, PNG, etc.)

## Use Case Scenario

Let's consider a practical scenario: You're developing a product comparison application that needs to generate side-by-side image comparisons of different products. Users upload individual product images, and your application needs to combine them horizontally into a single comparison image that can be shared or included in reports.

## Understanding Image Joining Modes

GroupDocs.Merger Cloud provides two modes for joining images:

1. Horizontal Mode: Images are placed side by side, creating a wider composite image
2. Vertical Mode: Images are stacked on top of each other, creating a taller composite image

The mode is specified using the `ImageJoinMode` parameter in the join request.

## Implementation Steps

### Step 1: Authenticate with the API

First, authenticate with the GroupDocs.Merger Cloud API by obtaining a JWT token using your Client ID and Client Secret.

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

### Step 2: Prepare Your Image Join Request

To join images, create a JSON request specifying:
- The image files to be joined
- The joining mode (horizontal or vertical)
- Output path for the resultant image

Here's a basic request structure for joining images:

```json
{
    "JoinItems": [
        {
            "FileInfo": {
                "FilePath": "Img/task1.jpg"
            }
        },
        {
            "FileInfo": {
                "FilePath": "Img/task2.jpg"
            },
            "ImageJoinMode": "Vertical"
        }
    ],
    "OutputPath": "Output/joined.jpg"
}
```

Note that the `ImageJoinMode` parameter determines how the images will be combined. In this example, the images will be stacked vertically.

### Step 3: Join Images Using the API

Now let's execute the image join operation using the API:

#### cURL Example

```bash
curl -v "https://api.groupdocs.cloud/v1.0/merger/join" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'JoinItems':
    [
        {
            'FileInfo':
            {
                'FilePath': 'Img/task1.jpg'
            }
        },
        {
            'FileInfo':
            {
                'FilePath': 'Img/task2.jpg'
            },
            'ImageJoinMode': 'Vertical'
        }
    ],
    'OutputPath': 'Output/joined.jpg'
}"
```

The API will respond with:

```json
{
  "path": "Output/joined.jpg"
}
```

### Step 4: Retrieve the Joined Image

After successful joining, you can download or use the merged image from the output path provided in the response.

## Try It Yourself

Let's implement image joining in different programming languages. First, make sure your image files are uploaded to your GroupDocs Cloud storage, then use the appropriate SDK for your language.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new DocumentApi(configuration);

try
{
    // Create first image join item
    var item1 = new JoinItem
    {
        FileInfo = new FileInfo
        {
            FilePath = "Img/task1.jpg"
        }
    };

    // Create second image join item with vertical mode
    var item2 = new JoinItem
    {
        FileInfo = new FileInfo
        {
            FilePath = "Img/task2.jpg"
        },
        ImageJoinMode = JoinItem.ImageJoinModeEnum.Vertical
    };

    // Configure join options
    var options = new JoinOptions
    {
        JoinItems = new List<JoinItem> { item1, item2 },
        OutputPath = "Output/joined.jpg"
    };

    // Execute join operation
    var request = new JoinRequest(options);
    var response = apiInstance.Join(request);

    Console.WriteLine("Output file path: " + response.Path);
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling API: " + e.Message);
}
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-java-samples
String MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
String MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

Configuration configuration = new Configuration(MyClientId, MyClientSecret);
DocumentApi apiInstance = new DocumentApi(configuration);

try {
    // Configure first image
    FileInfo fileInfo1 = new FileInfo();
    fileInfo1.setFilePath("Img/task1.jpg");
    JoinItem item1 = new JoinItem();
    item1.setFileInfo(fileInfo1);

    // Configure second image with vertical mode
    FileInfo fileInfo2 = new FileInfo();
    fileInfo2.setFilePath("Img/task2.jpg");
    JoinItem item2 = new JoinItem();
    item2.setFileInfo(fileInfo2);
    item2.setImageJoinMode(ImageJoinModeEnum.VERTICAL);

    // Configure join options
    JoinOptions options = new JoinOptions();
    options.setJoinItems(Arrays.asList(item1, item2));
    options.setOutputPath("Output/joined.jpg");

    // Execute join operation
    JoinRequest request = new JoinRequest(options);
    DocumentResult response = apiInstance.join(request);

    System.out.println("Output file path: " + response.getPath());
} catch (ApiException e) {
    System.err.println("Exception while calling API:");
    e.printStackTrace();
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-merger_cloud-cloud/groupdocs-merger_cloud-cloud-python-samples
from groupdocs_merger_cloud import *
import groupdocs_merger_cloud

client_id = "YOUR_CLIENT_ID" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

# Create instance of the API
document_api = groupdocs_merger_cloud.DocumentApi.from_keys(client_id, client_secret)

# Configure first image
item1 = groupdocs_merger_cloud.JoinItem()
item1.file_info = groupdocs_merger_cloud.FileInfo("Img/task1.jpg")

# Configure second image with vertical mode
item2 = groupdocs_merger_cloud.JoinItem()
item2.file_info = groupdocs_merger_cloud.FileInfo("Img/task2.jpg")
item2.image_join_mode = "Vertical"  # Use "Vertical" or "Horizontal"

# Configure join options
options = groupdocs_merger_cloud.JoinOptions()
options.join_items = [item1, item2]
options.output_path = "Output/joined.jpg"

# Execute join operation
result = document_api.join(groupdocs_merger_cloud.JoinRequest(options))        

print("Output file path = " + result.path)
```

## Exploring Image Joining Modes

Let's explore the two image joining modes in more detail:

### Horizontal Mode

When using the horizontal mode, images are placed side by side from left to right. This is useful for:
- Creating panoramic images
- Side-by-side comparisons
- Before/after presentations

To use horizontal mode, set the `ImageJoinMode` parameter to "Horizontal" or use the enum value in your code:

```json
"ImageJoinMode": "Horizontal"
```

### Vertical Mode

When using the vertical mode, images are stacked on top of each other from top to bottom. This is useful for:
- Creating photo strips
- Timeline presentations
- Sequential visual narratives

To use vertical mode, set the `ImageJoinMode` parameter to "Vertical" or use the enum value in your code:

```json
"ImageJoinMode": "Vertical"
```

## Joining Multiple Images

You can join more than two images in a single operation by adding additional items to the `JoinItems` array:

```json
{
    "JoinItems": [
        {
            "FileInfo": {
                "FilePath": "Img/task1.jpg"
            }
        },
        {
            "FileInfo": {
                "FilePath": "Img/task2.jpg"
            },
            "ImageJoinMode": "Vertical"
        },
        {
            "FileInfo": {
                "FilePath": "Img/task3.jpg"
            },
            "ImageJoinMode": "Vertical"
        }
    ],
    "OutputPath": "Output/joined.jpg"
}
```

## Supported Image Formats

GroupDocs.Merger Cloud supports joining various image formats, including:
- JPEG/JPG
- PNG
- BMP
- GIF (static)
- TIFF

You can mix different formats in the same join operation, and the output will be in the format specified by the output file extension.

## Common Issues and Troubleshooting

1. Authentication Error: If you receive a 401 Unauthorized error, make sure your Client ID and Client Secret are correct and that your JWT token hasn't expired.

2. File Not Found: Verify that the file paths are correct and that the images exist in your cloud storage.

3. Image Size Differences: When joining images of different dimensions:
   - In horizontal mode, the height of the resultant image will match the tallest input image, and shorter images will be padded.
   - In vertical mode, the width of the resultant image will match the widest input image, and narrower images will be padded.

4. Large Images: Be mindful of memory constraints when joining large or high-resolution images. Consider resizing images before joining if necessary.

5. Unsupported Formats: Ensure you're using supported image formats. Some formats like SVG are not supported for joining operations.

## What You've Learned

In this tutorial, you've learned how to:
- Join multiple images into a single composite image
- Control the orientation of joined images (horizontal or vertical)
- Implement image joining in different programming languages
- Handle various image formats and considerations

## Further Practice

To reinforce your learning, try these exercises:
1. Join three or more images in both horizontal and vertical modes
2. Create a grid-like composite by joining horizontal strips vertically
3. Join images of different formats and dimensions to observe how they are handled
4. Build a simple application that allows users to select images and joining mode

## Next Steps

Ready to explore more document operations? Check out these related tutorials:
- [Tutorial: How to Join Multiple Documents](/document-management/join-multiple-documents/)
- [Tutorial: How to Generate Document Page Previews](/document-management/generate-document-pages-preview/)
- [Tutorial: How to Split a Document](/document-management/split-document/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/merger/)
- [Documentation](https://docs.groupdocs.cloud/merger/)
- [Live Demo](https://products.groupdocs.app/merger/family)
- [API Reference](https://reference.groupdocs.cloud/merger/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.merger-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/merger/18/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to share your feedback or ask questions on our [support forum](https://forum.groupdocs.cloud/c/merger/18/).
