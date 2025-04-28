---
title: Learn to Perform Multiple Operations on Images Tutorial
url: /manipulating-images/perform-operations/
weight: 10
description: This tutorial teaches developers how to scale, crop, flip, and export images in a single API call using Aspose.Imaging Cloud.
---

# Tutorial: Learn to Perform Multiple Operations on Images

## Learning Objectives

In this tutorial, you'll learn how to:
- Perform multiple image operations in a single API call
- Scale images to specific dimensions
- Crop images with precise coordinates
- Flip and rotate images
- Convert images between different formats
- Work with both storage-based and direct upload approaches

## Prerequisites

Before starting this tutorial:
- Create an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps)
- Get your App SID and App Key credentials
- Basic understanding of REST API concepts
- Familiarity with your preferred programming language (we provide examples in multiple languages)

## The Power of Combined Operations

One of the most powerful features of Aspose.Imaging Cloud API is the ability to perform multiple operations on images in a single API call. This not only makes your code more efficient but also reduces the number of network requests needed.

## Understanding the API Endpoints

Aspose.Imaging Cloud provides two main endpoints for performing combined operations:

1. GET /imaging/{name}/updateImage - For images already stored in Cloud Storage
2. POST /imaging/updateImage - For direct image upload in the request body

Let's explore each approach in detail.

## Method 1: Using Cloud Storage Approach

This approach requires you to first upload an image to your Aspose Cloud Storage, then reference it by name in the API call.

### Step 1: Authentication

First, get your JSON Web Token using your App SID and App Key:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_APP_SID&client_secret=YOUR_APP_KEY' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the token for use in subsequent requests.

### Step 2: Upload Image to Cloud Storage

```bash
curl -v "https://api.aspose.cloud/v3/imaging/storage/file/input_image.jpg" \
-X PUT \
-T "path/to/your/local/image.jpg" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### Step 3: Perform Multiple Operations

Now let's perform scaling, cropping, and rotation in a single call:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/input_image.jpg/updateImage?format=png&newWidth=300&newHeight=300&x=10&y=10&rectWidth=200&rectHeight=200&rotateFlipMethod=Rotate90FlipX" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o result_image.png
```

### Understanding the Parameters

- `format` - Output format for the processed image (png, jpg, etc.)
- `newWidth` & `newHeight` - Scale the image to these dimensions
- `x` & `y` - Starting coordinates for cropping
- `rectWidth` & `rectHeight` - Width and height of the cropping rectangle
- `rotateFlipMethod` - Rotation and flip method (e.g., Rotate90FlipX)

## Method 2: Direct Upload Approach

This approach allows you to directly upload and process an image without first storing it.

### Step 1: Authentication

Same as in Method 1, get your JWT token.

### Step 2: Upload and Process Image

```bash
curl -v "https://api.aspose.cloud/v3/imaging/updateImage?format=png&newWidth=300&newHeight=300&x=10&y=10&rectWidth=200&rectHeight=200&rotateFlipMethod=Rotate90FlipX" \
-X POST \
-T "path/to/your/local/image.jpg" \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o result_image.png
```

## Try It Yourself!

Now that you understand the two approaches, try creating a workflow that:
1. Takes an image
2. Resizes it to 400x400 pixels
3. Crops 50 pixels from each edge
4. Rotates it 180 degrees
5. Saves it as a PNG file

## Using SDKs for Easier Implementation

While direct API calls work well, using SDKs can make development even faster.

### .NET SDK Example

```csharp
// Example .NET code for performing multiple operations on an image
// Full code available in the GitHub Gist linked below
public void UpdateImageInCloud()
{
    // The image name to be updated
    string imageName = "sample.psd";
    
    // First upload the image to cloud
    using (FileStream imageStream = new FileStream(Path.Combine(ExampleImagesFolder, imageName), FileMode.Open))
    {
        UploadFileRequest uploadRequest = new UploadFileRequest(imageName, imageStream);
        FileApi.UploadFile(uploadRequest);
    }
    
    // Update image parameters
    string format = "png";     // Convert image to PNG format
    int newWidth = 300;        // Set new image width
    int newHeight = 300;       // Set new image height
    int x = 10;                // Set X position of cropping rectangle
    int y = 10;                // Set Y position of cropping rectangle
    int rectWidth = 200;       // Set width of cropping rectangle
    int rectHeight = 200;      // Set height of cropping rectangle
    string rotateFlipMethod = "Rotate90FlipX";  // Set rotate/flip method
    
    // Process the image with specified parameters
    UpdateImageRequest request = new UpdateImageRequest(
        imageName, format, newWidth, newHeight, x, y, rectWidth, rectHeight, rotateFlipMethod);
        
    using (Stream updatedImage = ImagingApi.UpdateImage(request))
    {
        // Save updated image to local storage
        using (FileStream outputStream = new FileStream(
            Path.Combine(OutputFolder, $"{Path.GetFileNameWithoutExtension(imageName)}_updated.png"), 
            FileMode.Create))
        {
            updatedImage.CopyTo(outputStream);
        }
    }
}
```

### Java SDK Example

```java
// Example Java code for performing multiple operations on an image
// Full code available in the GitHub Gist linked below
public void updateImageInCloud() throws Exception {
    // The image name to be updated
    String imageName = "sample.psd";
    
    // First upload the image to cloud
    byte[] imageData = Files.readAllBytes(Paths.get(ExampleImagesFolder, imageName));
    UploadFileRequest uploadRequest = new UploadFileRequest(imageName, imageData);
    fileApi.uploadFile(uploadRequest);
    
    // Update image parameters
    String format = "png";     // Convert image to PNG format
    Integer newWidth = 300;    // Set new image width
    Integer newHeight = 300;   // Set new image height
    Integer x = 10;            // Set X position of cropping rectangle
    Integer y = 10;            // Set Y position of cropping rectangle
    Integer rectWidth = 200;   // Set width of cropping rectangle
    Integer rectHeight = 200;  // Set height of cropping rectangle
    String rotateFlipMethod = "Rotate90FlipX";  // Set rotate/flip method
    
    // Process the image with specified parameters
    UpdateImageRequest request = new UpdateImageRequest(
        imageName, format, newWidth, newHeight, x, y, rectWidth, rectHeight, rotateFlipMethod);
        
    byte[] updatedImage = imagingApi.updateImage(request);
    
    // Save updated image to local storage
    Files.write(Paths.get(OutputFolder, imageName + "_updated.png"), updatedImage);
}
```

## Troubleshooting Tips

- Authentication Issues: Ensure your JWT token is valid and not expired
- 404 Not Found: Verify that the image path in Cloud Storage is correct
- Parameter Validation: Check parameter boundaries (e.g., x+rectWidth should not exceed image width)
- Format Support: Confirm that the source and target formats are supported

## What You've Learned

In this tutorial, you've learned:
- How to perform multiple image operations in a single API call
- Two different approaches: Cloud Storage and Direct Upload
- Parameter configuration for scaling, cropping, and rotating
- Implementation with cURL commands and SDK examples
- Troubleshooting common issues

## Further Practice

To reinforce your learning:
1. Try combining different operations with various parameters
2. Process batches of images with different transformation settings
3. Create an image processing pipeline that applies operations conditionally

## Next Steps

Continue your learning journey with our [Tutorial: How to Deskew Images](/manipulating-images/deskew-images/) to learn automatic correction of skewed images.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/imaging/10/).
