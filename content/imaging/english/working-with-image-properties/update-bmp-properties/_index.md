---
title: How to Update BMP Image Properties with Aspose.Imaging Cloud API Tutorial
url: /working-with-image-properties/update-bmp-properties/
weight: 20
description: Learn how to modify BMP image properties like color depth and resolution using Aspose.Imaging Cloud API in this step-by-step tutorial
---

# Tutorial: How to Update BMP Image Properties with Aspose.Imaging Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Modify specific properties of BMP images using Aspose.Imaging Cloud API
- Update color depth (bits per pixel) of bitmap images
- Adjust horizontal and vertical resolution
- Process BMP images stored in the cloud storage
- Work with Device Independent Bitmap (DIB) file format

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account (sign up at [dashboard.aspose.cloud](https://dashboard.aspose.cloud))
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A BMP image uploaded to your Aspose Cloud Storage
- Basic knowledge of REST API concepts
- Familiarity with either cURL, .NET, or Java for the examples

## Understanding BMP Properties

BMP (Bitmap) is a raster graphics image file format used to store bitmap digital images. The key properties you can modify with Aspose.Imaging Cloud include:

- Bits Per Pixel: Determines the color depth of the image (1, 4, 8, 16, 24, or 32 bits)
- Horizontal Resolution: The horizontal pixel density (dots per inch)
- Vertical Resolution: The vertical pixel density (dots per inch)

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

### Step 2: Upload a BMP Image to Cloud Storage (if needed)

If you don't already have a BMP image in your cloud storage, you'll need to upload one using the Storage API:

```bash
curl -v "https://api.aspose.cloud/v3/storage/file/WaterMark.bmp" \
-X PUT \
-H "Content-Type: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-T "path/to/your/local/WaterMark.bmp"
```

### Step 3: Update BMP Image Properties

Now, let's update the properties of the BMP image stored in the cloud:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/WaterMark.bmp/bmp?bitsPerPixel=32&horizontalResolution=300&verticalResolution=300" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o WaterMark_updated.bmp
```

In this request:
- `bitsPerPixel=32` changes the color depth to 32 bits per pixel
- `horizontalResolution=300` and `verticalResolution=300` set the resolution to 300 DPI
- The result is saved to a local file named "WaterMark_updated.bmp"

#### Parameters Explained

| Parameter | Description | Possible Values |
|-----------|-------------|-----------------|
| bitsPerPixel | Color depth in bits per pixel | 1, 4, 8, 16, 24, 32 |
| horizontalResolution | Horizontal pixel density in DPI | Any positive number (common: 72, 96, 150, 300, 600) |
| verticalResolution | Vertical pixel density in DPI | Any positive number (common: 72, 96, 150, 300, 600) |

#### Try it yourself

Experiment with different values for the parameters to see how they affect the image. For instance, try:
- Reducing the color depth to 8 bits per pixel
- Setting different horizontal and vertical resolutions
- Working with different BMP images to see how properties affect various content types

### Step 4: Verify the Updated Properties

After updating the BMP image, you can verify the changes by retrieving the properties:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/WaterMark_updated.bmp/properties" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

Compare the properties in the response with the original values to confirm your changes were applied successfully.

### Step 5: Using the SDK for Easier Integration

For a more programmatic approach, you can use the Aspose.Imaging Cloud SDKs. Here are examples for both .NET and Java:

#### .NET SDK Example

```csharp
// Update parameters of BMP image in cloud storage
public static void UpdateBmpImagePropertiesInCloud()
{
    // Create Imaging API client
    var imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

    try
    {
        // Specify image parameters
        int bitsPerPixel = 32;
        double horizontalResolution = 300;
        double verticalResolution = 300;
        
        // Create request
        var request = new ModifyBmpRequest(
            "WaterMark.bmp",
            bitsPerPixel,
            horizontalResolution,
            verticalResolution
        );
        
        Console.WriteLine("Updating BMP image properties...");
        
        // Get updated image as stream
        using var updatedImageStream = imagingApi.ModifyBmp(request);
        
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
// Update parameters of BMP image in cloud storage
public static void updateBmpImagePropertiesInCloud() {
    try {
        // Create Imaging API client
        ImagingApi imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Specify image parameters
        Integer bitsPerPixel = 32;
        Double horizontalResolution = 300.0;
        Double verticalResolution = 300.0;
        
        // Create request
        ModifyBmpRequest request = new ModifyBmpRequest(
            "WaterMark.bmp",
            bitsPerPixel,
            horizontalResolution,
            verticalResolution
        );
        
        System.out.println("Updating BMP image properties...");
        
        // Get updated image as stream
        byte[] updatedImage = imagingApi.modifyBmp(request);
        
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

### Step 6: Understanding the Effects of Property Changes

#### Color Depth (Bits Per Pixel)

Changing the color depth affects both the visual quality and file size:

- 1 bit: Black and white only (2 colors)
- 4 bits: 16 colors
- 8 bits: 256 colors
- 16 bits: 65,536 colors (High Color)
- 24 bits: 16.7 million colors (True Color)
- 32 bits: 16.7 million colors + alpha channel for transparency

Higher color depth provides better quality but increases file size. For simple graphics with few colors, reducing color depth can significantly reduce file size without noticeable quality loss.

#### Resolution (DPI)

Resolution affects how the image appears when printed:

- Higher resolution (e.g., 300 DPI) is better for printing
- Lower resolution (e.g., 72 DPI) is sufficient for screen display
- Changing resolution doesn't affect the pixel dimensions of the image

## Troubleshooting Tips

- Invalid BitsPerPixel Value: BMP format supports specific bit depths (1, 4, 8, 16, 24, 32). Using an unsupported value will cause an error.
- Authentication Errors: Verify your JWT token is valid and not expired.
- Missing File: Ensure the BMP image exists in your cloud storage with the exact name specified in the request.
- Large File Processing: For large BMP files, the request may take longer to process. Consider adding appropriate timeouts to your code.

## What You've Learned

In this tutorial, you've learned:
- How to update BMP image properties using Aspose.Imaging Cloud API
- The meaning and impact of different BMP properties like color depth and resolution
- Techniques for processing BMP images stored in cloud storage
- Implementation using both direct REST API calls and SDKs
- How property changes affect image quality and file size

## Further Practice

To reinforce your learning:
1. Try converting color images to black and white by setting bits per pixel to 1
2. Create a program that optimizes BMP images by adjusting their properties based on their content
3. Compare file sizes before and after property changes to understand the impact
4. Create a batch processing script that updates multiple BMP images at once

## Next Tutorial

Continue your learning journey with [Tutorial: How to Update BMP Properties Without Storage](/working-with-image-properties/update-bmp-properties-without-storage/) to learn how to modify BMP images without storing them in the cloud first.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/imaging/10/).
