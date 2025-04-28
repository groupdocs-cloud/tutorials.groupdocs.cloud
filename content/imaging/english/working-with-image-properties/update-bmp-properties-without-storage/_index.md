---
title: How to Update BMP Image Properties Without Storage Tutorial
url: /working-with-image-properties/update-bmp-properties-without-storage/
weight: 30
description: Learn how to modify BMP image properties without using cloud storage in this step-by-step tutorial using Aspose.Imaging Cloud API
---

# Tutorial: How to Update BMP Image Properties Without Storage

## Learning Objectives

In this tutorial, you'll learn how to:
- Update BMP image properties without first uploading to cloud storage
- Modify color depth (bits per pixel) and resolution directly
- Process BMP images by sending them in the request body
- Save processed images either locally or to cloud storage
- Work with Device Independent Bitmap (DIB) file format

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account (sign up at [dashboard.aspose.cloud](https://dashboard.aspose.cloud))
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A local BMP image file for testing
- Basic knowledge of REST API concepts
- Familiarity with either cURL, .NET, or Java for the examples

## Understanding Direct Image Processing

Sometimes you may want to process images without first uploading them to cloud storage. This approach is beneficial when:
- You need to process images one time only
- You're working with images that don't need to be stored
- You want to simplify your workflow by eliminating the upload step

Aspose.Imaging Cloud API supports direct image processing by allowing you to send the image in the request body.

## Tutorial Steps

### Step 1: Prepare Your Authentication

Every API request to Aspose.Imaging Cloud requires authentication. We'll use the OAuth 2.0 client credentials flow to obtain a JWT token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the JWT token from the response for use in subsequent requests.

### Step 2: Update BMP Image Properties Without Storage

Instead of uploading the image to cloud storage first, we'll send it directly in the request:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/bmp?bitsPerPixel=32&horizontalResolution=300&verticalResolution=300" \
-X POST \
-T "path/to/your/local/WaterMark.bmp" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o WaterMark_updated.bmp
```

In this request:
- The HTTP method is POST (not GET as in the storage-based approach)
- The BMP image file is sent in the request body using the -T option
- The same query parameters are used to specify the desired property changes
- The updated image is saved to a local file named "WaterMark_updated.bmp"

#### Parameters Explained

| Parameter | Description | Possible Values |
|-----------|-------------|-----------------|
| bitsPerPixel | Color depth in bits per pixel | 1, 4, 8, 16, 24, 32 |
| horizontalResolution | Horizontal pixel density in DPI | Any positive number (common: 72, 96, 150, 300, 600) |
| verticalResolution | Vertical pixel density in DPI | Any positive number (common: 72, 96, 150, 300, 600) |
| outPath | Optional parameter to save the result to cloud storage | Path in cloud storage (e.g., "updated/WaterMark.bmp") |

#### Try it yourself

1. Modify the cURL command to use your own BMP image
2. Try different combinations of the parameters
3. Add the outPath parameter to save the result directly to cloud storage

### Step 3: Saving the Result to Cloud Storage

If you want to save the processed image to cloud storage instead of downloading it, you can add the outPath parameter:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/bmp?bitsPerPixel=32&horizontalResolution=300&verticalResolution=300&outPath=updated/WaterMark.bmp" \
-X POST \
-T "path/to/your/local/WaterMark.bmp" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

With this approach:
- The `outPath` parameter specifies where to save the result in cloud storage
- No response body is returned since the image is saved to the cloud
- You can verify the result by checking the specified path in your cloud storage

### Step 4: Using the SDK for Easier Integration

For a more programmatic approach, you can use the Aspose.Imaging Cloud SDKs. Here are examples for both .NET and Java:

#### .NET SDK Example

```csharp
// Update parameters of BMP image without storage
public static void UpdateBmpImagePropertiesWithoutStorage()
{
    // Create Imaging API client
    var imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

    try
    {
        // Prepare parameters
        int bitsPerPixel = 32;
        double horizontalResolution = 300;
        double verticalResolution = 300;
        string outPath = null; // Set to null to get the result in the response, or specify a path to save in the cloud
        
        // Read image from file
        using var imageStream = new FileStream("WaterMark.bmp", FileMode.Open);
        
        Console.WriteLine("Updating BMP image properties without storage...");
        
        // Create request with image from local file
        var request = new CreateModifiedBmpRequest(
            imageStream,
            bitsPerPixel,
            horizontalResolution,
            verticalResolution,
            outPath
        );
        
        // Get updated image as stream
        using var updatedImageStream = imagingApi.CreateModifiedBmp(request);
        
        // Save updated image to local file
        using var fileStream = new FileStream("WaterMark_updated.bmp", FileMode.Create);
        updatedImageStream.CopyTo(fileStream);
        
        Console.WriteLine("Updated BMP image saved to WaterMark_updated.bmp");
    }
    catch (Exception e)
    {
        Console.WriteLine("Error: " + e.Message);
    }
}
```

#### Java SDK Example

```java
// Update parameters of BMP image without storage
public static void updateBmpImagePropertiesWithoutStorage() {
    try {
        // Create Imaging API client
        ImagingApi imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Prepare parameters
        Integer bitsPerPixel = 32;
        Double horizontalResolution = 300.0;
        Double verticalResolution = 300.0;
        String outPath = null; // Set to null to get the result in the response, or specify a path to save in the cloud
        
        // Read image from file
        byte[] imageData = Files.readAllBytes(Paths.get("WaterMark.bmp"));
        
        System.out.println("Updating BMP image properties without storage...");
        
        // Create request with image from local file
        CreateModifiedBmpRequest request = new CreateModifiedBmpRequest(
            imageData,
            bitsPerPixel,
            horizontalResolution,
            verticalResolution,
            outPath
        );
        
        // Get updated image
        byte[] updatedImage = imagingApi.createModifiedBmp(request);
        
        // Save updated image to local file
        try (OutputStream outputStream = new FileOutputStream("WaterMark_updated.bmp")) {
            outputStream.write(updatedImage);
        }
        
        System.out.println("Updated BMP image saved to WaterMark_updated.bmp");
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
    }
}
```

### Step 5: Saving Directly to Cloud Storage Using SDK

If you want to save the result directly to cloud storage when using the SDK, you can specify the `outPath` parameter:

#### .NET SDK Example

```csharp
// Save updated BMP image directly to cloud storage
public static void SaveUpdatedBmpToCloudStorage()
{
    // Create Imaging API client
    var imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

    try
    {
        // Prepare parameters
        int bitsPerPixel = 32;
        double horizontalResolution = 300;
        double verticalResolution = 300;
        string outPath = "updated/WaterMark.bmp"; // Specify path in cloud storage
        
        // Read image from file
        using var imageStream = new FileStream("WaterMark.bmp", FileMode.Open);
        
        Console.WriteLine("Updating BMP image and saving directly to cloud storage...");
        
        // Create request with image from local file and outPath parameter
        var request = new CreateModifiedBmpRequest(
            imageStream,
            bitsPerPixel,
            horizontalResolution,
            verticalResolution,
            outPath
        );
        
        // Process the image and save to cloud storage
        imagingApi.CreateModifiedBmp(request);
        
        Console.WriteLine("Updated BMP image saved to cloud storage at: " + outPath);
    }
    catch (Exception e)
    {
        Console.WriteLine("Error: " + e.Message);
    }
}
```

## Practical Applications

Here are some practical scenarios where updating BMP properties without storage is useful:

1. Image Optimization: Quickly adjust color depth to reduce file size before using the image in a web application
2. Print Preparation: Update the resolution of images to match print requirements without storing intermediates
3. Batch Processing: Process multiple BMP files in sequence, applying the same property changes to each
4. On-the-fly Conversion: Update properties as part of a conversion pipeline where intermediate results don't need to be stored

## Troubleshooting Tips

- File Format Errors: Ensure you're providing a valid BMP file. The API will return an error if the input isn't a proper BMP image.
- Authentication Issues: Check that your JWT token is valid and not expired.
- Parameter Validation: The `bitsPerPixel` parameter must be one of the supported values (1, 4, 8, 16, 24, 32).
- Storage Path: If using `outPath`, ensure the target folder exists in your cloud storage or use a path that doesn't require pre-existing folders.

## What You've Learned

In this tutorial, you've learned:
- How to update BMP image properties without first uploading to cloud storage
- How to send images directly in the request body for processing
- Options for saving the processed image (locally or to cloud storage)
- Implementation using both direct REST API calls and SDKs
- Practical applications for this approach in real-world scenarios

## Further Practice

To reinforce your learning:
1. Create a script that processes multiple BMP images with different property settings
2. Build a simple web application that allows users to adjust BMP properties on-the-fly
3. Compare the performance between the storage-based approach and the direct approach
4. Experiment with changing color depth and observe the visual differences and file size impacts

## Next Tutorial

Continue your learning journey with [Tutorial: How to Update GIF Image Properties](/working-with-image-properties/update-gif-properties/) to learn techniques for working with animated image formats.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/imaging/10/).