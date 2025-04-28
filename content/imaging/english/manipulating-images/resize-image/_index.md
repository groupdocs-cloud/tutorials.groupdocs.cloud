---
title: Learn to Resize Images with Aspose.Imaging Cloud Tutorial
url: /manipulating-images/resize-image/
weight: 10
description: Learn how to resize images and change formats using Aspose.Imaging Cloud API in this step-by-step tutorial for developers.
---

# Tutorial: How to Resize Images with Aspose.Imaging Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Resize images by specifying new width and height
- Optionally convert the image format during resizing
- Implement image resizing both with and without cloud storage

## Prerequisites

Before you begin this tutorial, make sure you have:

1. An Aspose Cloud account (obtain a [free trial here](https://dashboard.aspose.cloud/#/apps))
2. Your Client ID and Client Secret from the dashboard
3. Basic understanding of REST API concepts
4. Your preferred development environment set up (.NET, Java, or cURL)

## Practical Use Case

Image resizing is essential in numerous applications:
- Creating thumbnails for media galleries
- Optimizing images for web pages to improve loading speed
- Preparing images for specific display dimensions on different devices
- Standardizing image sizes in a collection

## Tutorial Steps

### Step 1: Understanding Image Resize API Options

Aspose.Imaging Cloud provides two API endpoints for resizing images:

1. With Storage (GET method): Upload your image to Cloud Storage first, then resize it using its name.
   - Endpoint: `GET /imaging/{name}/resize`

2. Without Storage (POST method): Directly send your image in the request body for resizing.
   - Endpoint: `POST /imaging/resize`

Both methods allow you to specify new dimensions and optionally change the output format.

### Step 2: Authentication

Before making API calls, you need to obtain a JWT token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the returned JWT token for use in subsequent API calls.

### Step 3: Resize an Image (With Storage)

#### 3.1 First, upload your image to cloud storage

```bash
curl -X PUT "https://api.aspose.cloud/v3/imaging/storage/file/image.jpg" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: application/octet-stream" \
-T /path/to/your/image.jpg
```

#### 3.2 Resize the uploaded image

```bash
curl -v "https://api.aspose.cloud/v3/imaging/image.jpg/resize?format=png&newWidth=400&newHeight=300" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o resized_image.png
```

### Step 4: Resize an Image (Without Storage)

For quick one-off resizing without storing the image first:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/resize?format=png&newWidth=400&newHeight=300" \
-X POST \
-T /path/to/your/image.jpg \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o resized_image.png
```

### Try It Yourself

Now it's your turn! Choose an image from your computer and try:
1. Resizing it to 300x200 pixels
2. Changing its format to PNG if it's not already in PNG format
3. Trying both the storage and non-storage methods

Remember to replace the placeholder values with your actual image path, JWT token, and desired parameters.

## SDK Implementation Examples

### C# (.NET) Example

```csharp
// Resize an image in cloud storage
public static void ResizeImageInCloud()
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
        
        // Set resize parameters
        int newWidth = 400;
        int newHeight = 300;
        string format = "png"; // Convert to PNG format
        
        // Create request to resize the image
        ResizeImageRequest request = new ResizeImageRequest(
            uploadedImageName, newWidth, newHeight, format, cloudFolder);
            
        // Perform the resize operation
        using (var resultStream = imagingApi.ResizeImage(request))
        {
            // Save the resized image to a local file
            using (FileStream fileStream = 
                new FileStream("resized_image.png", FileMode.Create))
            {
                resultStream.CopyTo(fileStream);
            }
        }
        
        Console.WriteLine("Image has been successfully resized!");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error: " + ex.Message);
    }
}
```

### Java Example

```java
// Resize an image in the request body
public static void resizeImageWithoutStorage() throws Exception 
{
    // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
    ImagingApi imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
    
    try 
    {
        // Specify the local image file path
        String localInputImage = "YourImage.jpg";
        
        // Prepare parameters for resize operation
        int newWidth = 400;
        int newHeight = 300;
        String format = "png"; // Convert to PNG format
        
        // Read the image into a byte array
        byte[] imageData = Files.readAllBytes(Paths.get(localInputImage));
        
        // Create request for resize operation
        CreateResizedImageRequest request = 
            new CreateResizedImageRequest(imageData, newWidth, newHeight, format, null);
        
        // Perform the resize operation
        byte[] resultImage = imagingApi.createResizedImage(request);
        
        // Save the resized image to a local file
        Files.write(Paths.get("resized_image.png"), resultImage);
        
        System.out.println("Image has been successfully resized!");
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
- Invalid Dimensions: Width and height must be positive integers.
- Format Support: Not all image formats support all operations. Check [format support documentation](https://docs.aspose.cloud/imaging/supported-file-formats/) for compatibility.

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the Aspose.Imaging Cloud API
- Resize images by specifying new width and height
- Convert image formats during the resize operation
- Implement resizing with both storage and non-storage approaches
- Handle the API responses to save the resized image

## Further Practice

To strengthen your understanding, try these additional exercises:
1. Create a thumbnail generator that resizes multiple images to the same dimensions
2. Implement proportional resizing (maintaining aspect ratio)
3. Integrate error handling for various edge cases

## Next Tutorial

Ready to learn more? Continue to our [Crop Images Tutorial](/manipulating-images/crop-image/) to discover how to extract specific portions of an image.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to [visit our forum](https://forum.aspose.cloud/c/imaging/10/) for assistance.
