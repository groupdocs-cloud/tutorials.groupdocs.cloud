---
title: How to Fetch Detected Regions from the API Tutorial
weight: 20
url: /detect-regions/fetch-regions/
description: Learn to retrieve, decode, and interpret region detection results from the Aspose.OCR Cloud API in this hands-on developer tutorial.
---

# Tutorial: How to Fetch Detected Regions from the API

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve region detection results from the Aspose.OCR Cloud API
- Interpret task status codes and handle different states
- Decode Base64-encoded region coordinates
- Work with region coordinates to identify text blocks
- Handle potential errors in the API response

## Prerequisites

Before starting this tutorial, make sure you have:
- Completed the [previous tutorial on sending images for region detection](/detect-regions/send-for-detection/)
- A task ID received from the region detection request
- An active access token for the Aspose.OCR Cloud API
- Basic understanding of JSON data structures

## Introduction

After sending an image for region detection, the next step is to retrieve the results from the Aspose.OCR Cloud API. In this tutorial, we'll guide you through the process of fetching detected regions, interpreting the API response, and working with the coordinate data to identify text blocks within your image.

## Step 1: Check Your Task ID

You should have received a task ID (GUID) after sending your image for region detection in the previous tutorial. It looks something like this:

```
a371d027-4b0d-4d86-8825-c8d818dd4ed9
```

This ID is unique to your region detection task and will be used to fetch the results.

## Step 2: Send a Request to Fetch Regions

With your task ID and access token, you can now fetch the detected regions using a GET request to the Aspose.OCR Cloud API:

### Using cURL

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/DetectRegions?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

### Using Python

```python
import requests

# Your access token and task ID
access_token = "YOUR_ACCESS_TOKEN"
task_id = "YOUR_TASK_ID"

# Set headers
headers = {
    "Accept": "text/plain",
    "Content-Type": "application/json",
    "Authorization": f"Bearer {access_token}"
}

# Send request
response = requests.get(
    f"https://api.aspose.cloud/v5.0/ocr/DetectRegions?id={task_id}",
    headers=headers
)

# Print response
print(response.text)
```

### Using C#

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;

public class FetchRegionsExample
{
    public static async Task FetchRegions()
    {
        // Your access token and task ID
        string accessToken = "YOUR_ACCESS_TOKEN";
        string taskId = "YOUR_TASK_ID";
        
        // Create HTTP client
        using (HttpClient client = new HttpClient())
        {
            // Set headers
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("text/plain"));
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
            
            // Send request
            HttpResponseMessage response = await client.GetAsync(
                $"https://api.aspose.cloud/v5.0/ocr/DetectRegions?id={taskId}"
            );
            
            // Get response
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

## Step 3: Interpret the API Response

The API will return a JSON response with information about your region detection task. Here's an example response:

```json
{
    "id": "a371d027-4b0d-4d86-8825-c8d818dd4ed9",
    "responseStatusCode": "Ok",
    "taskStatus": "Completed",
    "results": [
        {
            "type": "Other",
            "data": "W1swLCA4LCA2OTcsIDQ0MV1d"
        }
    ],
    "error": null
}
```

Let's break down the key components of this response:

| Property | Description |
|----------|-------------|
| `id` | The unique identifier of your region detection task |
| `responseStatusCode` | The status of the API response |
| `taskStatus` | The current state of the region detection task |
| `results` | An array of detected regions, encoded in Base64 |
| `error` | Error messages, if any |

## Step 4: Check the Task Status

Before proceeding, check the `taskStatus` property to ensure your task has been completed. Here are the possible statuses:

| Status | Description | Action |
|--------|-------------|--------|
| `Pending` | The image is queued for processing | Wait and retry in a few seconds |
| `Processing` | Regions are currently being detected | Wait and retry in a few seconds |
| `Completed` | Region detection is finished | Proceed to decode the results |
| `Error` | An error occurred | Check the `error` property for details |
| `NotExist` | The task ID doesn't exist | Verify your task ID or send the image again |

If the status is `Pending` or `Processing`, you'll need to retry the request after a short delay. Here's how to implement a simple polling mechanism:

### Using Python

```python
import requests
import time
import json

def fetch_regions(access_token, task_id, max_retries=10, delay=2):
    headers = {
        "Accept": "text/plain",
        "Content-Type": "application/json",
        "Authorization": f"Bearer {access_token}"
    }
    
    for attempt in range(max_retries):
        response = requests.get(
            f"https://api.aspose.cloud/v5.0/ocr/DetectRegions?id={task_id}",
            headers=headers
        )
        
        result = response.json()
        
        if result["taskStatus"] in ["Completed", "Error", "NotExist"]:
            return result
        
        print(f"Task status: {result['taskStatus']}. Retrying in {delay} seconds...")
        time.sleep(delay)
    
    return {"error": "Maximum retries reached"}

# Usage
result = fetch_regions("YOUR_ACCESS_TOKEN", "YOUR_TASK_ID")
print(json.dumps(result, indent=4))
```

## Step 5: Decode the Region Data

If the task status is `Completed`, the detected regions will be available in the `results` array. Each region is returned with:
- A `type` (always "Other" for region detection)
- `data` in Base64-encoded format

To use these regions, you need to decode the Base64 string:

### Using Python

```python
import base64
import json

# The Base64-encoded data from the response
encoded_data = "W1swLCA4LCA2OTcsIDQ0MV1d"

# Decode the Base64 string
decoded_bytes = base64.b64decode(encoded_data)
decoded_str = decoded_bytes.decode('utf-8')

# Parse the JSON string
regions = json.loads(decoded_str)

print("Decoded regions:", regions)
```

### Using JavaScript

```javascript
// The Base64-encoded data from the response
const encodedData = "W1swLCA4LCA2OTcsIDQ0MV1d";

// Decode the Base64 string
const decodedStr = atob(encodedData);

// Parse the JSON string
const regions = JSON.parse(decodedStr);

console.log("Decoded regions:", regions);
```

### Using C#

```csharp
using System;
using System.Text;
using Newtonsoft.Json;

// The Base64-encoded data from the response
string encodedData = "W1swLCA4LCA2OTcsIDQ0MV1d";

// Decode the Base64 string
byte[] decodedBytes = Convert.FromBase64String(encodedData);
string decodedStr = Encoding.UTF8.GetString(decodedBytes);

// Parse the JSON string
var regions = JsonConvert.DeserializeObject<int[][]>(decodedStr);

Console.WriteLine("Decoded regions:");
foreach (var region in regions)
{
    Console.WriteLine($"[{string.Join(", ", region)}]");
}
```

## Step 6: Work with Region Coordinates

The decoded data contains arrays of coordinates for each detected region. Each region is represented by four coordinates:

```
[x1, y1, x2, y2]
```

Where:
- `(x1, y1)` is the top-left corner of the region
- `(x2, y2)` is the bottom-right corner of the region

You can use these coordinates to:
- Draw rectangles around text regions in your image
- Crop specific regions for further processing
- Calculate region sizes and positions

Here's an example of how to use these coordinates to draw rectangles on your image:

### Using Python with Pillow

```python
from PIL import Image, ImageDraw
import base64
import json

def draw_regions(image_path, encoded_regions, output_path):
    # Load the image
    img = Image.open(image_path)
    draw = ImageDraw.Draw(img)
    
    # Decode regions
    decoded_bytes = base64.b64decode(encoded_regions)
    decoded_str = decoded_bytes.decode('utf-8')
    regions = json.loads(decoded_str)
    
    # Draw rectangles for each region
    for region in regions:
        x1, y1, x2, y2 = region
        draw.rectangle([x1, y1, x2, y2], outline="red", width=2)
    
    # Save the result
    img.save(output_path)
    print(f"Image with regions saved to {output_path}")

# Usage
draw_regions("your_image.png", "W1swLCA4LCA2OTcsIDQ0MV1d", "image_with_regions.png")
```

## Common Issues and Troubleshooting

### Issue: Task Not Found

If you receive a `NotExist` status:

Solution: Verify that you're using the correct task ID and that it hasn't expired. Detected regions are stored in the Aspose cloud for 24 hours after the image was sent for OCR.

### Issue: Error in Region Detection

If you receive an `Error` status:

Solution: Check the `error` property in the response for specific error messages. Common issues include invalid image formats or problems with the image content.

### Issue: Decoded Coordinates Don't Match Expected Regions

If the regions don't accurately identify the text blocks in your image:

Solution: Try adjusting the detection settings in your original request. For example, enabling `makeContrastCorrection` or `makeSkewCorrect` might improve detection accuracy.

## Try It Yourself!

Now it's your turn to practice fetching and working with detected regions:

1. Use the task ID from the previous tutorial to fetch the detected regions
2. Decode the Base64-encoded region data
3. If possible, visualize the regions on your original image
4. Experiment with different polling intervals for tasks still in progress

## What You've Learned

In this tutorial, you've learned how to:
- Fetch region detection results from the Aspose.OCR Cloud API
- Interpret task statuses and handle pending tasks
- Decode Base64-encoded region data
- Work with region coordinates to identify text blocks
- Visualize detected regions on the original image

## Next Steps

In the next tutorial, you'll learn how to [implement region detection using the Aspose.OCR Cloud SDK](/detect-regions/region-detection-sdk/), which simplifies the process by wrapping API calls into easy-to-use methods.

## Further Practice

To reinforce your understanding:
- Implement a complete polling mechanism that automatically retries until the task is complete
- Create a utility function that visualizes detected regions on any input image
- Experiment with extracting specific regions from an image based on the coordinates

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Please visit our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance.
