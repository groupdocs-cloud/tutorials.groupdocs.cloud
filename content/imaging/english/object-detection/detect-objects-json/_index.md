---
title: Learn to Detect Objects and Get Results as JSON Tutorial
url: /object-detection/detect-objects-json/
weight: 10
description: Learn how to detect objects in images and get structured JSON responses. A hands-on tutorial for developers using Aspose.Imaging Cloud API.
---

# Tutorial: Learn to Detect Objects and Get Results as JSON

## Learning Objectives

In this tutorial, you'll learn how to:
- Set up object detection with Aspose.Imaging Cloud API
- Detect objects in images and receive structured JSON results
- Process and interpret detection results in your applications
- Implement object detection with and without cloud storage

## Prerequisites

Before starting this tutorial, ensure you have:
- An Aspose Cloud account (get one [here](https://dashboard.aspose.cloud/#/apps))
- Your Client ID and Client Secret from the Aspose Dashboard
- Basic familiarity with REST API concepts
- A development environment for your preferred language (examples in cURL, .NET, and Java are provided)

## Introduction

Object detection is a powerful computer vision technique that identifies and locates objects within images. In this tutorial, we'll focus on implementing object detection that returns results as structured JSON data, which is ideal for further processing in your applications.

The Aspose.Imaging Cloud API supports the Single Shot Detector (SSD) method for object recognition and can work with BMP, JPEG, and JPEG2000 image formats.

## 1. Understanding the API Parameters

The `/bounds` method accepts several parameters that control the detection process:

| Parameter | Type | Description |
|-----------|------|-------------|
| name | string (required) | Image name to process |
| method | string (optional) | Detection method, currently only "ssd" is supported |
| threshold | number (optional) | Minimum probability threshold (0-100) for including objects |
| includeLabel | boolean (optional) | Whether to include object labels in the response |
| includeScore | boolean (optional) | Whether to include object probabilities in the response |
| folder | string (optional) | Storage folder containing the image |
| storage | string (optional) | Storage name |

## 2. Getting Started with Authentication

Before making API calls, you need to obtain a JWT token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Store the JWT token for use in subsequent API calls.

## 3. Detecting Objects Using Cloud Storage

In this approach, you first upload an image to Cloud Storage, then process it:

### Try it yourself: Detect Objects in a Stored Image

```bash
curl -v "https://api.aspose.cloud/v3/imaging/ai/your_image.jpg/bounds?method=ssd&threshold=50&includeLabel=true&includeScore=true" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

The response will be a JSON object containing detected objects:

```json
{
   "detectedObjects": [
       {
           "score": 0.599537253,
           "label": "dog",
           "bounds": {
               "x": 33.0,
               "y": -1.0,
               "width": 510.0,
               "height": 352.0
           }
       },
       {
           "score": 0.66614604,
           "label": "cat",
           "bounds": {
               "x": 37.0,
               "y": 3.0,
               "width": 493.0,
               "height": 344.0
           }
       }
   ]
}
```

## 4. Detecting Objects Directly from Request Body

For more flexibility, you can send the image directly in the request body:

### Try it yourself: Detect Objects from Request Body

```bash
curl -v "https://api.aspose.cloud/v3/imaging/ai/bounds?method=ssd&threshold=50&includeLabel=true&includeScore=true" \
-X POST \
-T your_image.jpg \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

The response format will be the same as the previous example.

## 5. Implementing Detection with SDKs

### .NET Implementation

The following C# code demonstrates object detection:

```csharp
// Tutorial Code Example - Object Detection with Aspose.Imaging Cloud .NET SDK
using Aspose.Imaging.Cloud.Sdk.Api;
using Aspose.Imaging.Cloud.Sdk.Model.Requests;
using System;
using System.IO;

namespace AsposeTutorials
{
    class ObjectDetectionTutorial
    {
        // Your Client ID and Client Secret
        private const string ClientId = "YOUR_CLIENT_ID";
        private const string ClientSecret = "YOUR_CLIENT_SECRET";

        static void Main(string[] args)
        {
            // Create instance of API
            var imagingApi = new ImagingApi(ClientId, ClientSecret);

            // 1. Example with storage: Process an image from cloud storage
            string imageName = "object_detection_image.jpg";
            string folder = ""; // File is in the root
            string storage = ""; // Use default storage
            
            Console.WriteLine("Detecting objects in stored image...");
            try
            {
                // Object detection parameters
                var request = new GetObjectBoundsRequest(
                    name: imageName,
                    method: "ssd",
                    threshold: 50,
                    includeLabel: true,
                    includeScore: true,
                    folder: folder,
                    storage: storage
                );

                var detectionResult = imagingApi.GetObjectBounds(request);
                
                // Process results
                Console.WriteLine($"Found {detectionResult.DetectedObjects.Count} objects:");
                foreach (var obj in detectionResult.DetectedObjects)
                {
                    Console.WriteLine($"- {obj.Label} (Score: {obj.Score})");
                    Console.WriteLine($"  Location: X={obj.Bounds.X}, Y={obj.Bounds.Y}, Width={obj.Bounds.Width}, Height={obj.Bounds.Height}");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error: {ex.Message}");
            }

            // 2. Example without storage: Process an image directly
            string imageFilePath = @"C:\path\to\your\local_image.jpg";
            Console.WriteLine("\nDetecting objects in local image...");
            
            try
            {
                using (FileStream fileStream = new FileStream(imageFilePath, FileMode.Open))
                {
                    var request = new CreateObjectBoundsRequest(
                        imageData: fileStream,
                        method: "ssd",
                        threshold: 50,
                        includeLabel: true,
                        includeScore: true
                    );

                    var detectionResult = imagingApi.CreateObjectBounds(request);
                    
                    // Process results
                    Console.WriteLine($"Found {detectionResult.DetectedObjects.Count} objects:");
                    foreach (var obj in detectionResult.DetectedObjects)
                    {
                        Console.WriteLine($"- {obj.Label} (Score: {obj.Score})");
                        Console.WriteLine($"  Location: X={obj.Bounds.X}, Y={obj.Bounds.Y}, Width={obj.Bounds.Width}, Height={obj.Bounds.Height}");
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error: {ex.Message}");
            }
        }
    }
}
```

### Java Implementation

```java
// Tutorial Code Example - Object Detection with Aspose.Imaging Cloud Java SDK
import com.aspose.imaging.cloud.sdk.api.ImagingApi;
import com.aspose.imaging.cloud.sdk.model.DetectedObjectList;
import com.aspose.imaging.cloud.sdk.model.requests.GetObjectBoundsRequest;
import com.aspose.imaging.cloud.sdk.model.requests.CreateObjectBoundsRequest;

import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;

public class ObjectDetectionTutorial {
    // Your Client ID and Client Secret
    private static final String CLIENT_ID = "YOUR_CLIENT_ID";
    private static final String CLIENT_SECRET = "YOUR_CLIENT_SECRET";

    public static void main(String[] args) {
        try {
            // Create instance of the API
            ImagingApi imagingApi = new ImagingApi(CLIENT_ID, CLIENT_SECRET);

            // 1. Example with storage: Process an image from cloud storage
            String imageName = "object_detection_image.jpg";
            String folder = ""; // File is in the root
            String storage = ""; // Use default storage
            
            System.out.println("Detecting objects in stored image...");
            
            // Object detection parameters
            GetObjectBoundsRequest request = new GetObjectBoundsRequest(
                imageName,     // name
                "ssd",         // method
                50,            // threshold
                true,          // includeLabel
                true,          // includeScore
                folder,        // folder
                storage        // storage
            );

            DetectedObjectList detectionResult = imagingApi.getObjectBounds(request);
            
            // Process results
            System.out.println("Found " + detectionResult.getDetectedObjects().size() + " objects:");
            detectionResult.getDetectedObjects().forEach(obj -> {
                System.out.println("- " + obj.getLabel() + " (Score: " + obj.getScore() + ")");
                System.out.println("  Location: X=" + obj.getBounds().getX() + 
                                 ", Y=" + obj.getBounds().getY() + 
                                 ", Width=" + obj.getBounds().getWidth() + 
                                 ", Height=" + obj.getBounds().getHeight());
            });

            // 2. Example without storage: Process an image directly
            String imageFilePath = "C:/path/to/your/local_image.jpg";
            System.out.println("\nDetecting objects in local image...");
            
            File inputFile = new File(imageFilePath);
            InputStream imageStream = new FileInputStream(inputFile);
            
            CreateObjectBoundsRequest directRequest = new CreateObjectBoundsRequest(
                imageStream,    // imageData
                "ssd",          // method
                50,             // threshold
                true,           // includeLabel
                true,           // includeScore
                null            // outPath (null means the result will be returned in the response)
            );

            DetectedObjectList directResult = imagingApi.createObjectBounds(directRequest);
            
            // Process results
            System.out.println("Found " + directResult.getDetectedObjects().size() + " objects:");
            directResult.getDetectedObjects().forEach(obj -> {
                System.out.println("- " + obj.getLabel() + " (Score: " + obj.getScore() + ")");
                System.out.println("  Location: X=" + obj.getBounds().getX() + 
                                 ", Y=" + obj.getBounds().getY() + 
                                 ", Width=" + obj.getBounds().getWidth() + 
                                 ", Height=" + obj.getBounds().getHeight());
            });
            
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## 6. Understanding the JSON Response

The detection results include:

- `detectedObjects`: An array of all detected objects
  - `score`: The confidence level (0-1) of the detection
  - `label`: The identified object category
  - `bounds`: The object's location and dimensions
    - `x`, `y`: Top-left corner coordinates
    - `width`, `height`: Object dimensions

## 7. Common Issues and Troubleshooting

- Authentication errors: Ensure your Client ID and Secret are correct
- 404 Not Found: Check that your image path is correct
- Low-quality detections: Try adjusting the threshold parameter
- Unsupported format: Confirm you're using BMP, JPEG, or JPEG2000

## What You've Learned

In this tutorial, you've learned:
- How to authenticate and make API calls to Aspose.Imaging Cloud
- How to detect objects in images stored in the cloud
- How to detect objects in images sent directly in the request
- How to implement object detection using .NET and Java SDKs
- How to interpret and process JSON detection results

## Further Practice

To reinforce your learning:
1. Try detecting objects in different types of images
2. Experiment with different threshold values
3. Build a simple application that processes multiple images
4. Try filtering detections by specific object categories

## Next Steps

Continue your learning journey with our next tutorial: [Tutorial: How to Detect and Visualize Objects in Images](/object-detection/detect-visualize-objects/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? We're here to help! Reach out on our [support forum](https://forum.aspose.cloud/c/imaging/10/) with any questions about implementing object detection in your applications.
