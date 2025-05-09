---
title: How to Get Change Coordinates Tutorial
description: Learn to detect the exact spatial location of document changes using GroupDocs.Comparison Cloud API in this comprehensive step-by-step tutorial
weight: 90
url: /advanced-features/get-changes-coordinates/
---

# Tutorial: How to Get Change Coordinates

## Learning Objectives

In this tutorial, you'll learn how to:
- Detect the precise spatial location of changes within document pages
- Retrieve coordinate information for all changes between source and target documents
- Use coordinate data for advanced visualization and integration scenarios

## Prerequisites

Before starting this tutorial, you should have:
1. A GroupDocs.Comparison Cloud subscription or [free trial](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, PHP, Node.js, Python, or Ruby)
5. Source and target documents ready for comparison in your cloud storage

## The Practical Scenario

Consider developing an advanced document review application that not only identifies changes between document versions but also displays them in a custom interface. You want to:

1. Create a document viewer with visual indicators precisely placed where changes occur
2. Implement an interactive navigation system that jumps to specific changes
3. Generate thumbnails with change highlights at exact positions
4. Support zooming to change locations with proper scaling

To accomplish these features, you need to know exactly where each change appears on the document page. GroupDocs.Comparison Cloud API provides coordinate information for each detected change, making these advanced visualizations possible.

## Understanding Change Coordinates

When you enable coordinate calculation, each change detected by the API includes:

- PageInfo: Contains information about the page:
  - Width and height of the page
  - Page number where the change appears

- Box: Contains the spatial coordinates of the change:
  - X and Y: The position (in points) of the change from the top-left corner of the page
  - Width and height: The dimensions of the area occupied by the change

These coordinates use a coordinate system where (0,0) is at the top-left corner of the page, with X increasing to the right and Y increasing downward.

## Step 1: Upload Your Documents to Cloud Storage

Before comparing documents, you need to upload them to cloud storage. For this tutorial, we'll work with:
- source.docx (original document)
- target.docx (modified document)

Refer to the [File API documentation](https://docs.groupdocs.cloud/comparison/developer-guide/working-with-file-api/) for detailed instructions on uploading files.

## Step 2: Obtain Authorization Token

To authorize API requests, you need to obtain a JWT (JSON Web Token) first:

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

The API will return a token in the response. Save this token for use in subsequent API calls.

## Step 3: Get Changes with Coordinates

To retrieve changes with their coordinates, we need to set the `CalculateComponentCoordinates` option to `true` in our API request:

```bash
curl -v "https://api.groupdocs.cloud/v2.0/comparison/changes" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'SourceFile': {
    'FilePath': 'source_files/word/source.docx'
  },
  'TargetFiles': [
    {
      'FilePath': 'target_files/word/target.docx'
    }
  ],
  'Settings': {
    'CalculateComponentCoordinates': true
  }
}"
```

The API will return a JSON array containing all detected changes, including their coordinates. Here's a sample response showing the coordinate information:

```json
[
  {
    "id": 0,
    "comparisonAction": "None",
    "type": "Inserted",
    "text": "lol",
    "targetText": "Latin (i/ˈlætɪn/, /ˈlætɪn/; Latin: lingua latīna, IPA: [ˈlɪŋɡʷa laˈtiːna]) is a classical language, originally spoken inLatium, Italy, which belongs to the Italic branch of the Indo-European languages.[3] The Latin alphabet is derived from the Etruscan and Greek alphabetslol.",
    "authors": [
      "GroupDocs"
    ],
    "styleChangeInfo": [],
    "pageInfo": {
      "width": 612,
      "height": 792,
      "pageNumber": 1
    },
    "box": {
      "height": 16.0934353,
      "width": 14.0021219,
      "x": 388.503021,
      "y": 115.730377
    }
  },
  // More changes...
]
```

## Step 4: Utilizing Coordinate Information

Once you have the coordinate data, you can use it in various ways:

1. Visual Overlays: Create visual indicators (like highlight boxes) positioned at the exact coordinates of each change in your document viewer.

2. Interactive Navigation: Implement "jump to change" functionality that scrolls to the exact location of a selected change.

3. Spatial Filtering: Filter changes based on their location within the document (e.g., changes in headers, footers, or specific sections).

4. Scaling: Apply proper scaling to the coordinates when zooming in or out of the document view.

## Try It Yourself

Now it's your turn to implement change coordinate detection in your preferred programming language. Below are examples in various languages to help you get started.

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-dotnet-samples
string MyClientSecret = "YOUR_CLIENT_SECRET"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
string MyClientId = "YOUR_CLIENT_ID"; // Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);

var apiInstance = new CompareApi(configuration);
var options = new ComparisonOptions
{
    SourceFile = new FileInfo
    {
        FilePath = "source_files/word/source.docx"
    },
    TargetFiles = new List<FileInfo> {
        new FileInfo {
            FilePath = "target_files/word/target.docx"
        }
    },
    Settings = new Settings
    {
        CalculateComponentCoordinates = true
    }
};

var request = new PostChangesRequest(options);
var changes = apiInstance.PostChanges(request);

// Process the changes with coordinates
Console.WriteLine($"Total changes found: {changes.Count}");
foreach (var change in changes.Take(3)) // Display first 3 changes
{
    Console.WriteLine($"Change ID: {change.Id}");
    Console.WriteLine($"Type: {change.Type}");
    Console.WriteLine($"Text: {change.Text}");
    Console.WriteLine($"Page Number: {change.PageInfo.PageNumber}");
    Console.WriteLine($"Position: X={change.Box.X}, Y={change.Box.Y}");
    Console.WriteLine($"Size: Width={change.Box.Width}, Height={change.Box.Height}");
    Console.WriteLine("---------------------");
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/groupdocs-comparison-cloud/groupdocs-comparison-cloud-python-samples
import groupdocs_comparison_cloud

client_id = "YOUR_CLIENT_ID" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud
client_secret = "YOUR_CLIENT_SECRET" # Get ClientId and ClientSecret from https://dashboard.groupdocs.cloud

api_instance = groupdocs_comparison_cloud.CompareApi.from_keys(client_id, client_secret)

source = groupdocs_comparison_cloud.FileInfo()
source.file_path = "source_files/word/source.docx"
target = groupdocs_comparison_cloud.FileInfo()
target.file_path = "target_files/word/target.docx"
options = groupdocs_comparison_cloud.ComparisonOptions()
options.source_file = source
options.target_files = [target]
settings = groupdocs_comparison_cloud.Settings()
settings.calculate_component_coordinates = True
options.settings = settings

changes = api_instance.post_changes(groupdocs_comparison_cloud.PostChangesRequest(options))

# Process the changes with coordinates
print(f"Total changes found: {len(changes)}")
for i, change in enumerate(changes[:3]):  # Display first 3 changes
    print(f"Change ID: {change.id}")
    print(f"Type: {change.type}")
    print(f"Text: {change.text}")
    print(f"Page Number: {change.page_info.page_number}")
    print(f"Position: X={change.box.x}, Y={change.box.y}")
    print(f"Size: Width={change.box.width}, Height={change.box.height}")
    print("---------------------")
```

## Common Issues and Troubleshooting

1. Missing Coordinate Information: If the coordinate information is missing or zeros, check that you've set `CalculateComponentCoordinates` to `true` in your request.

2. Coordinate Scaling: The coordinates are provided in points (1/72 of an inch). If you're displaying the document at a different scale, remember to convert the coordinates accordingly.

3. Different Document Formats: Different document formats might have varying precision in coordinate information. Test with your specific document types to understand any variations.

4. Performance Considerations: Calculating coordinates requires additional processing. For very large documents, this might slightly increase the comparison time.

## Learning Checkpoint

Let's verify your understanding with a quick check:
1. What setting do you need to enable to get coordinate information for changes?
2. What coordinate system is used for the change coordinates?
3. What information does the "PageInfo" object provide?
4. How can you use coordinate information in a document review application?

## What You've Learned

In this tutorial, you've learned how to:
- Detect the precise spatial location of changes within document pages
- Retrieve coordinate information for all changes between source and target documents
- Understand the structure of coordinate data
- Use coordinate data for advanced visualization and integration scenarios

## Further Practice

To reinforce your learning, try these exercises:
1. Create a simple HTML viewer that displays a document and overlays rectangles at the positions of detected changes
2. Implement a page thumbnail generator that highlights areas with changes
3. Build a function that groups changes by their proximity on the page
4. Develop a utility that converts change coordinates to different scale systems

## Next Steps

Now that you've learned to get change coordinates, check out these related tutorials:
- [Tutorial: How to Accept or Reject Document Changes](/advanced-features/accept-reject-document-changes/)
- [Tutorial: How to Customize Change Styles](/advanced-features/customize-changes-styles/)
- [Tutorial: How to Get List of Changes](/advanced-features/get-list-of-changes/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

