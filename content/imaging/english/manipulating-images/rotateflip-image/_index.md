---
title: Guide to Rotating and Flipping Images with Aspose.Imaging Cloud Tutorial
url: /manipulating-images/rotateflip-image/
weight: 30
description: Learn how to rotate and flip images using Aspose.Imaging Cloud API in this hands-on tutorial for developers.
---

# Tutorial: How to Rotate and Flip Images with Aspose.Imaging Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Rotate images by 90, 180, or 270 degrees
- Flip images horizontally, vertically, or both
- Combine rotation and flipping operations
- Optionally change the image format during the operation
- Implement these operations both with and without cloud storage

## Prerequisites

Before you begin this tutorial, make sure you have:

1. An Aspose Cloud account (obtain a [free trial here](https://dashboard.aspose.cloud/#/apps))
2. Your Client ID and Client Secret from the dashboard
3. Basic understanding of REST API concepts
4. Your preferred development environment set up (.NET, Java, or cURL)
5. Completed the [Resize Images Tutorial](/manipulating-images/resize-image/) and [Crop Images Tutorial](/manipulating-images/crop-image/) (recommended)

## Practical Use Case

Rotating and flipping images are common operations needed for:
- Correcting image orientation from digital cameras
- Creating image variations for design purposes
- Standardizing the orientation of a collection of images
- Creating mirrored effects for artistic presentations
- Preparing images for specific layout requirements

## Tutorial Steps

### Step 1: Understanding RotateFlip API Options

Aspose.Imaging Cloud provides two API endpoints for rotating and flipping images:

1. With Storage (GET method): Upload your image to Cloud Storage first, then apply the operation using its name.
   - Endpoint: `GET /imaging/{name}/rotateflip`

2. Without Storage (POST method): Directly send your image in the request body.
   - Endpoint: `POST /imaging/rotateflip`

Both methods allow you to specify a rotation/flip method and optionally change the output format.

### Step 2: RotateFlip Method Options

The API provides 16 different combinations of rotation and flipping. Here are the available method values:

| Method Value | Description |
|--------------|-------------|
| `RotateNoneFlipNone` | No change (default) |
| `RotateNoneFlipX` | Flip horizontally |
| `RotateNoneFlipY` | Flip vertically |
| `RotateNoneFlipXY` | Flip both horizontally and vertically |
| `Rotate90FlipNone` | Rotate 90° clockwise |
| `Rotate90FlipX` | Rotate 90° clockwise and flip horizontally |
| `Rotate90FlipY` | Rotate 90° clockwise and flip vertically |
| `Rotate90FlipXY` | Rotate 90° clockwise and flip both horizontally and vertically |
| `Rotate180FlipNone` | Rotate 180° |
| `Rotate180FlipX` | Rotate 180° and flip horizontally |
| `Rotate180FlipY` | Rotate 180° and flip vertically |
| `Rotate180FlipXY` | Rotate 180° and flip both horizontally and vertically |
| `Rotate270FlipNone` | Rotate 270° clockwise (90° counterclockwise) |
| `Rotate270FlipX` | Rotate 270° clockwise and flip horizontally |
| `Rotate270FlipY` | Rotate 270° clockwise and flip vertically |
| `Rotate270FlipXY` | Rotate 270° clockwise and flip both horizontally and vertically |

### Step 3: Authentication

Before making API calls, you need to obtain a JWT token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the returned JWT token for use in subsequent API calls.

### Step 4: Rotate and Flip an Image (With Storage)

#### 4.1 First, upload your image to cloud storage

```bash
curl -X PUT "https://api.aspose.cloud/v3/imaging/storage/file/sample.jpg" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: application/octet-stream" \
-T /path/to/your/sample.jpg
```

#### 4.2 Apply the rotate and flip operation

```bash
curl -v "https://api.aspose.cloud/v3/imaging/sample.jpg/rotateflip?format=png&method=Rotate90FlipX" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o rotated_flipped_image.png
```

In this example:
- `method=Rotate90FlipX` - Rotate the image 90° clockwise and flip it horizontally
- `format=png` - The desired output format

### Step 5: Rotate and Flip an Image (Without Storage)

For quick one-off operations without storing the image first:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/rotateflip?format=png&method=Rotate90FlipX" \
-X POST \
-T /path/to/your/sample.jpg \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o rotated_flipped_image.png
```

### Try It Yourself

Now it's your turn! Choose an image from your computer and try:
1. Rotating the image 180 degrees
2. Flipping the image horizontally
3. Rotating 90 degrees and flipping vertically
4. Trying both the storage and non-storage methods

Remember to replace the placeholder values with your actual image path, JWT token, and desired parameters.

## SDK Implementation Examples

### C# (.NET) Example

```csharp
// Rotate and flip an image in cloud storage
public static void RotateFlipImageInCloud()
{
    // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
    var imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
    
    try
    {
        // Upload an image to cloud storage (required for this example)
        string localInputImage = "YourImage.jpg";
        string uploadedImageName = "sample_image.jpg";
        string cloudFolder = "imaging_samples";
        
        using (FileStream imageStream = new FileStream(localInputImage, FileMode.Open))
        {
            UploadFileRequest uploadRequest = 
                new UploadFileRequest(cloudFolder + "/" + uploadedImageName, imageStream);
            imagingApi.UploadFile(uploadRequest);
        }
        
        // Set rotate/flip parameters
        string rotateFlipMethod = "Rotate90FlipX"; // Rotate 90° clockwise and flip horizontally
        string format = "png"; // Convert to PNG format
        
        // Create request for the rotate/flip operation
        RotateFlipImageRequest request = new RotateFlipImageRequest(
            uploadedImageName, rotateFlipMethod, format, cloudFolder);
            
        // Perform the rotate/flip operation
        using (var resultStream = imagingApi.RotateFlipImage(request))
        {
            // Save the processed image to a local file
            using (FileStream fileStream = 
                new FileStream("rotated_flipped_image.png", FileMode.Create))
            {
                resultStream.CopyTo(fileStream);
            }
        }
        
        Console.WriteLine("Image has been successfully rotated and flipped!");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error: " + ex.Message);
    }
}
```

### Java Example

```java
// Rotate and flip an image in the request body
public static void rotateFlipImageWithoutStorage() throws Exception 
{
    // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
    ImagingApi imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
    
    try 
    {
        // Specify the local image file path
        String localInputImage = "YourImage.jpg";
        
        // Prepare parameters for rotate/flip operation
        String rotateFlipMethod = "Rotate90FlipX"; // Rotate 90° clockwise and flip horizontally
        String format = "png"; // Convert to PNG format
        
        // Read the image into a byte array
        byte[] imageData = Files.readAllBytes(Paths.get(localInputImage));
        
        // Create request for rotate/flip operation
        CreateRotateFlippedImageRequest request = 
            new CreateRotateFlippedImageRequest(imageData, rotateFlipMethod, format, null);
        
        // Perform the rotate/flip operation
        byte[] resultImage = imagingApi.createRotateFlippedImage(request);
        
        // Save the processed image to a local file
        Files.write(Paths.get("rotated_flipped_image.png"), resultImage);
        
        System.out.println("Image has been successfully rotated and flipped!");
    } 
    catch (Exception e) 
    {
        System.out.println("Error: " + e.getMessage());
        e.printStackTrace();
    }
}
```

## Troubleshooting Tips

- Authentication Errors: Ensure your Client ID and Client Secret are correct and your subscription is active.
- 404 Not Found: Check if the image path in cloud storage is correct.
- Invalid Method: Ensure the rotateFlipMethod parameter contains one of the valid values (case-sensitive).
- Format Support: Not all image formats support all operations. Check the [format support documentation](https://docs.aspose.cloud/imaging/supported-file-formats/) for compatibility.

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the Aspose.Imaging Cloud API
- Understand the various rotation and flip combinations available
- Apply rotation and flipping operations to images
- Convert image formats during the operation
- Implement these operations with both storage and non-storage approaches
- Handle the API responses to save the processed image

## Further Practice

To strengthen your understanding, try these additional exercises:
1. Create a tool that shows all 16 possible rotate/flip variations of a single image
2. Build a function that automatically corrects the orientation of images based on EXIF data
3. Implement a batch processor that standardizes the orientation of multiple images

## Next Tutorial

Ready to learn more? Continue to our [Converting Image Formats Tutorial](/manipulating-images/convert-image/) to discover how to convert images between different file formats.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to [visit our forum](https://forum.aspose.cloud/c/imaging/10/) for assistance.