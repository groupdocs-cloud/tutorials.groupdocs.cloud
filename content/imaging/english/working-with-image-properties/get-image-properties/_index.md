---
title: Learn to Get Image Properties with Aspose.Imaging Cloud API Tutorial
url: /working-with-image-properties/get-image-properties/
weight: 10
description: Learn how to retrieve and analyze image properties using Aspose.Imaging Cloud API in this beginner-friendly step-by-step tutorial
---

# Tutorial: Learn to Get Image Properties with Aspose.Imaging Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve properties of images using Aspose.Imaging Cloud API
- Work with both storage-based and direct image upload approaches
- Access common and format-specific image properties
- Implement this functionality using REST API calls and SDKs

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account (sign up at [dashboard.aspose.cloud](https://dashboard.aspose.cloud))
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- Basic knowledge of REST API concepts
- Familiarity with either cURL, .NET, or Java for the examples

## Understanding Image Properties

Images contain various properties that provide information about their format, dimensions, color depth, and other characteristics. These properties can be essential for proper image processing and manipulation. Aspose.Imaging Cloud API provides a simple way to access these properties.

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

### Step 2: Getting Image Properties Using Cloud Storage

#### 2.1: Upload an Image to Cloud Storage

First, upload your image to Aspose Cloud Storage using the Storage API or Aspose Cloud Dashboard.

#### 2.2: Call the Get Properties API

Now, retrieve the properties of the uploaded image:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/WaterMark.bmp/properties" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

This request retrieves properties of the BMP image named "WaterMark.bmp" from your cloud storage.

#### Try it yourself

Replace "WaterMark.bmp" with the name of your own image file, and execute the request. Examine the response to see all available properties.

### Step 3: Getting Image Properties Without Storage

If you don't want to upload the image to cloud storage first, you can directly send the image in the request body:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/properties" \
-X POST \
-F file=@"path/to/your/local/image.jpg" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

This approach is useful for one-time operations or when you don't need to store the image in the cloud.

#### Try it yourself

Try this request with different image formats (JPEG, PNG, GIF, etc.) and compare the property information returned for each.

### Step 4: Understanding the Response

The API returns a JSON response with image properties. Here's an example response for a BMP image:

```json
{
  "Height": 500,
  "Width": 500,
  "BitsPerPixel": 24,
  "BmpProperties": {
    "Compression": "Rgb"
  },
  "GifProperties": null,
  "JpegProperties": null,
  "PngProperties": null,
  "TiffProperties": null,
  "PsdProperties": null,
  "DjvuProperties": null,
  "WebPProperties": null,
  "Jpeg2000Properties": null,
  "DicomProperties": null,
  "DngProperties": null,
  "OdgProperties": null,
  "HorizontalResolution": 96.012,
  "VerticalResolution": 96.012,
  "IsCached": false,
  "Code": 200,
  "Status": "OK"
}
```

Notice how:
- Common properties like Height, Width, and BitsPerPixel are at the top level
- Format-specific properties are grouped under their respective property objects (e.g., BmpProperties)
- Properties for other formats are null when not applicable

### Step 5: Using the SDK for Easier Integration

While direct REST API calls work, using one of the Aspose.Imaging Cloud SDKs can simplify your code. Let's look at examples:

#### .NET SDK Example

```csharp
// Get properties of an image from cloud storage
public static void GetImagePropertiesFromStorage()
{
    // Create Imaging API client
    var imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

    try
    {
        // Get image properties
        Console.WriteLine("Get image properties from storage");
        
        var request = new GetImagePropertiesRequest("WaterMark.bmp");
        var imagingResponse = imagingApi.GetImageProperties(request);
        
        // Print properties
        Console.WriteLine("Width: {0}", imagingResponse.Width);
        Console.WriteLine("Height: {0}", imagingResponse.Height);
        Console.WriteLine("Bits per pixel: {0}", imagingResponse.BitsPerPixel);
        Console.WriteLine("Horizontal resolution: {0}", imagingResponse.HorizontalResolution);
        Console.WriteLine("Vertical resolution: {0}", imagingResponse.VerticalResolution);
    }
    catch (Exception e)
    {
        Console.WriteLine("Error: " + e.Message);
    }
}
```

#### Java SDK Example

```java
// Get properties of an image from cloud storage
public static void getImagePropertiesFromStorage() {
    try {
        // Create Imaging API client
        ImagingApi imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Get image properties
        System.out.println("Get image properties from storage");
        
        GetImagePropertiesRequest request = new GetImagePropertiesRequest("WaterMark.bmp");
        ImagingResponse imagingResponse = imagingApi.getImageProperties(request);
        
        // Print properties
        System.out.println("Width: " + imagingResponse.getWidth());
        System.out.println("Height: " + imagingResponse.getHeight());
        System.out.println("Bits per pixel: " + imagingResponse.getBitsPerPixel());
        System.out.println("Horizontal resolution: " + imagingResponse.getHorizontalResolution());
        System.out.println("Vertical resolution: " + imagingResponse.getVerticalResolution());
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
    }
}
```

### Step 6: Using Properties for Image Analysis

Now that you can retrieve image properties, you can use this information to make decisions about how to process images. For example:

- Determine if an image needs resizing based on its dimensions
- Check the color depth to decide if color conversion is necessary
- Verify the format-specific properties to ensure compatibility

## Troubleshooting Tips

- Authentication Errors: Double-check your Client ID and Client Secret, and ensure your JWT token is valid and not expired.
- 404 Not Found: Make sure your image file exists in the cloud storage with the exact path and name you specified.
- Unsupported Format: The API supports most common image formats, but if you get an error, check if your format is in the supported list.

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with Aspose.Imaging Cloud API
- Two methods to retrieve image properties (with and without cloud storage)
- How to interpret the response containing image properties
- Implementation using both direct REST API calls and SDKs
- Practical use cases for image property information

## Further Practice

To reinforce your learning:
1. Try retrieving properties for different image formats
2. Parse the format-specific properties for different formats
3. Create a simple application that analyses multiple images and summarizes their properties
4. Explore how to use the property information to determine appropriate image processing operations

## Next Tutorial

Continue your learning with [Tutorial: How to Update BMP Image Properties](/working-with-image-properties/update-bmp-properties/) to learn how to modify bitmap image properties.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/imaging/10/).
