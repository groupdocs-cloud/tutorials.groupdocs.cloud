---
title: How to Update JPG Image Properties Without Storage Tutorial
url: /working-with-image-properties/update-jpg-properties-without-storage/
weight: 90
description: Learn how to modify JPEG image properties without using cloud storage in this step-by-step tutorial using Aspose.Imaging Cloud API
---

# Tutorial: How to Update JPG Image Properties Without Storage

## Learning Objectives

In this tutorial, you'll learn how to:
- Update JPEG image properties without first uploading to cloud storage
- Modify quality settings and compression type directly
- Process JPEG images by sending them in the request body
- Save processed images either locally or to cloud storage
- Apply these techniques to optimize images for different purposes

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account (sign up at [dashboard.aspose.cloud](https://dashboard.aspose.cloud))
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A local JPEG image file for testing
- Basic knowledge of REST API concepts
- Familiarity with either cURL, .NET, or Java for the examples
- Understanding of the JPEG format and its properties

## Understanding Direct JPEG Processing

Processing JPEG images directly without storage is particularly useful when:
- You need quick, one-time adjustments to image quality
- You're building web applications that process user-uploaded images
- You want to optimize images on-the-fly for different devices or connection speeds
- You need to streamline your workflow by eliminating separate upload and processing steps

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

### Step 2: Update JPEG Image Properties Without Storage

Instead of uploading the image to cloud storage first, we'll send it directly in the request:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/jpg?quality=65&compressionType=progressive&outPath=updated/aspose-logo.jpg" \
-X POST \
-T "path/to/your/local/aspose-logo.jpg" \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

With this approach:
- The `outPath` parameter specifies where to save the result in cloud storage
- No response body is returned since the image is saved to the cloud
- You can verify the result by checking the specified path in your cloud storage


### Step 3: Saving the Result to Cloud Storage

If you want to save the processed image to cloud storage instead of downloading it, you can add the outPath parameter:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/jpg?quality=65&compressionType=progressive" \
-X POST \
-T "path/to/your/local/aspose-logo.jpg" \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o aspose_logo_updated.jpg
```

In this request:
- The HTTP method is POST (not GET as in the storage-based approach)
- The JPEG image file is sent in the request body using the -T option
- The same query parameters are used to specify the desired property changes
- The updated image is saved to a local file named "aspose_logo_updated.jpg"

#### Parameters Explained

| Parameter | Description | Possible Values |
|-----------|-------------|-----------------|
| quality | Compression quality | 0-100 (0 is lowest quality, 100 is highest) |
| compressionType | Encoding method | baseline, progressive |
| outPath | Optional parameter to save the result to cloud storage | Path in cloud storage (e.g., "updated/image.jpg") |

#### Try it yourself

1. Modify the cURL command to use your own JPEG image
2. Try different quality levels and observe the changes in file size and visual quality
3. Switch between baseline and progressive compression to see the difference in loading behavior


### Step 4: Using the SDK for Easier Integration

For a more programmatic approach, you can use the Aspose.Imaging Cloud SDKs. Here are examples for both .NET and Java:

#### .NET SDK Example

```csharp
// Update parameters of JPEG image without storage
public static void UpdateJpegImagePropertiesWithoutStorage()
{
    // Create Imaging API client
    var imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

    try
    {
        // Prepare parameters
        int quality = 65;
        string compressionType = "progressive";
        string outPath = null; // Set to null to get the result in the response, or specify a path to save in the cloud
        
        // Read image from file
        using var imageStream = new FileStream("aspose-logo.jpg", FileMode.Open);
        
        Console.WriteLine("Updating JPEG image properties without storage...");
        
        // Create request with image from local file
        var request = new CreateModifiedJpegRequest(
            imageStream,
            quality,
            compressionType,
            outPath
        );
        
        // Get updated image as stream
        using var updatedImageStream = imagingApi.CreateModifiedJpeg(request);
        
        // Save updated image to local file
        using var fileStream = new FileStream("aspose_logo_updated.jpg", FileMode.Create);
        updatedImageStream.CopyTo(fileStream);
        
        Console.WriteLine("Updated JPEG image saved to aspose_logo_updated.jpg");
    }
    catch (Exception e)
    {
        Console.WriteLine("Error: " + e.Message);
    }
}
```

#### Java SDK Example

```java
// Update parameters of JPEG image without storage
public static void updateJpegImagePropertiesWithoutStorage() {
    try {
        // Create Imaging API client
        ImagingApi imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Prepare parameters
        Integer quality = 65;
        String compressionType = "progressive";
        String outPath = null; // Set to null to get the result in the response, or specify a path to save in the cloud
        
        // Read image from file
        byte[] imageData = Files.readAllBytes(Paths.get("aspose-logo.jpg"));
        
        System.out.println("Updating JPEG image properties without storage...");
        
        // Create request with image from local file
        CreateModifiedJpegRequest request = new CreateModifiedJpegRequest(
            imageData,
            quality,
            compressionType,
            outPath
        );
        
        // Get updated image
        byte[] updatedImage = imagingApi.createModifiedJpeg(request);
        
        // Save updated image to local file
        try (OutputStream outputStream = new FileOutputStream("aspose_logo_updated.jpg")) {
            outputStream.write(updatedImage);
        }
        
        System.out.println("Updated JPEG image saved to aspose_logo_updated.jpg");
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
    }
}
```

### Step 5: Saving Directly to Cloud Storage Using SDK

If you want to save the result directly to cloud storage when using the SDK, you can specify the `outPath` parameter:

#### .NET SDK Example

```csharp
// Save updated JPEG image directly to cloud storage
public static void SaveUpdatedJpegToCloudStorage()
{
    // Create Imaging API client
    var imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

    try
    {
        // Prepare parameters
        int quality = 65;
        string compressionType = "progressive";
        string outPath = "updated/aspose-logo.jpg"; // Specify path in cloud storage
        
        // Read image from file
        using var imageStream = new FileStream("aspose-logo.jpg", FileMode.Open);
        
        Console.WriteLine("Updating JPEG image and saving directly to cloud storage...");
        
        // Create request with image from local file and outPath parameter
        var request = new CreateModifiedJpegRequest(
            imageStream,
            quality,
            compressionType,
            outPath
        );
        
        // Process the image and save to cloud storage
        imagingApi.CreateModifiedJpeg(request);
        
        Console.WriteLine("Updated JPEG image saved to cloud storage at: " + outPath);
    }
    catch (Exception e)
    {
        Console.WriteLine("Error: " + e.Message);
    }
}
```

## Real-World Application Examples

### Web Image Optimization

Optimizing images for web use often requires balancing quality and file size. Here's a strategy for on-the-fly optimization:

```csharp
// Optimize JPEG for web based on target size
public static void OptimizeJpegForWebSize(string inputFilePath, string outputFilePath, int targetKilobytes)
{
    var imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

    try
    {
        // Start with a reasonable quality
        int quality = 80;
        string compressionType = "progressive"; // Better for web
        
        // Read image from file
        using var imageStream = new FileStream(inputFilePath, FileMode.Open);
        
        // Try up to 10 iterations to find optimal quality
        for (int attempt = 0; attempt < 10; attempt++)
        {
            // Create request
            var request = new CreateModifiedJpegRequest(
                imageStream,
                quality,
                compressionType,
                null
            );
            
            // Get updated image
            using var updatedImageStream = imagingApi.CreateModifiedJpeg(request);
            
            // Check file size
            byte[] imageBytes = ReadFully(updatedImageStream);
            int currentSize = imageBytes.Length / 1024; // Size in KB
            
            Console.WriteLine($"Quality {quality}: {currentSize} KB");
            
            if (currentSize <= targetKilobytes)
            {
                // Target size achieved, save and exit
                using var fileStream = new FileStream(outputFilePath, FileMode.Create);
                fileStream.Write(imageBytes, 0, imageBytes.Length);
                
                Console.WriteLine($"Optimized image saved to {outputFilePath} with quality {quality}");
                return;
            }
            
            // Reduce quality for next attempt
            quality -= 10;
            if (quality < 5) quality = 5; // Minimum quality threshold
            
            // Reset stream position for next iteration
            imageStream.Position = 0;
        }
        
        // If we couldn't reach target size, save the lowest quality version
        using var request = new CreateModifiedJpegRequest(
            imageStream,
            5,  // Lowest quality
            compressionType,
            null
        );
        
        using var finalImageStream = imagingApi.CreateModifiedJpeg(request);
        using var fileStream = new FileStream(outputFilePath, FileMode.Create);
        finalImageStream.CopyTo(fileStream);
        
        Console.WriteLine($"Could not reach target size. Saved lowest quality version to {outputFilePath}");
    }
    catch (Exception e)
    {
        Console.WriteLine("Error: " + e.Message);
    }
}

// Helper to read stream fully into byte array
private static byte[] ReadFully(Stream stream)
{
    using var memoryStream = new MemoryStream();
    stream.CopyTo(memoryStream);
    return memoryStream.ToArray();
}
```

### Batch Processing with Different Quality Levels

When processing multiple images that need different quality settings based on content:

```java
// Batch process JPEGs with content-aware quality settings
public static void batchProcessJpegs(List<String> filePaths, String outputFolder) {
    try {
        ImagingApi imagingApi = new ImagingApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        for (String filePath : filePaths) {
            try {
                File file = new File(filePath);
                String fileName = file.getName();
                String outputPath = outputFolder + "/" + fileName;
                
                // Analyze image to determine content type
                ImageType contentType = analyzeImageContent(filePath);
                
                // Set quality based on content type
                Integer quality;
                switch (contentType) {
                    case PHOTO:
                        quality = 75; // Photos can tolerate more compression
                        break;
                    case GRAPHIC:
                        quality = 85; // Graphics need higher quality
                        break;
                    case TEXT:
                        quality = 90; // Text needs highest quality
                        break;
                    default:
                        quality = 80; // Default quality
                }
                
                byte[] imageData = Files.readAllBytes(Paths.get(filePath));
                
                CreateModifiedJpegRequest request = new CreateModifiedJpegRequest(
                    imageData,
                    quality,
                    "progressive", // Progressive for all web images
                    null  // Return in response
                );
                
                byte[] updatedImage = imagingApi.createModifiedJpeg(request);
                
                try (OutputStream outputStream = new FileOutputStream(outputPath)) {
                    outputStream.write(updatedImage);
                }
                
                System.out.println("Processed " + fileName + " with quality " + quality);
            } catch (Exception e) {
                System.out.println("Error processing " + filePath + ": " + e.getMessage());
                // Continue with next file
            }
        }
        
        System.out.println("Batch processing completed.");
    } catch (Exception e) {
        System.out.println("Error initializing API: " + e.getMessage());
    }
}

// Simple enum for content types
enum ImageType { PHOTO, GRAPHIC, TEXT }

// Simplified function to analyze image content - in a real application,
// you would use more sophisticated image analysis
private static ImageType analyzeImageContent(String filePath) {
    // Example logic - in practice, use ML or analysis algorithms
    if (filePath.contains("photo") || filePath.contains("img")) {
        return ImageType.PHOTO;
    } else if (filePath.contains("graph") || filePath.contains("chart")) {
        return ImageType.GRAPHIC;
    } else if (filePath.contains("text") || filePath.contains("doc")) {
        return ImageType.TEXT;
    }
    return ImageType.PHOTO; // Default
}
```

## Quality vs. File Size Guidelines

Here's a quick reference for JPEG quality settings and their typical impacts:

| Quality Level | File Size Reduction | Visual Impact | Recommended Use |
|---------------|---------------------|---------------|-----------------|
| 90-100 | Minimal | Almost lossless | Professional photography, printing |
| 80-89 | 5x-2x smaller | Very slight | General photography, high-quality web images |
| 70-79 | 5x-8x smaller | Minor | Web photography, general online use |
| 60-69 | 10x-15x smaller | Noticeable but acceptable | Web thumbnails, lower-bandwidth sites |
| 50-59 | 15x-20x smaller | Clearly degraded | Very small previews, extreme bandwidth constraints |
| Below 50 | >20x smaller | Severely degraded | Only when file size is critical above all else |

## Troubleshooting Tips

- Quality Range Errors: Ensure the quality parameter is between 0 and 100.
- Compression Type Validation: Only "baseline" and "progressive" are valid values for compressionType.
- File Size Not Decreasing: If your file size isn't decreasing significantly with lower quality, the image might already be heavily compressed, or contain content that doesn't compress well.
- Visual Artifacts: Blocky areas, color banding, or blurry text indicate quality is set too low for the content type.

## What You've Learned

In this tutorial, you've learned:
- How to update JPEG image properties without first uploading to cloud storage
- How to send JPEG images directly in the request body for processing
- Options for saving the processed image (locally or to cloud storage)
- Implementation using both direct REST API calls and SDKs
- Advanced techniques for web optimization and batch processing
- Guidelines for choosing quality levels based on content and purpose

## Further Practice

To reinforce your learning:
1. Create a web application that allows users to upload images and adjust quality settings with a preview
2. Build a command-line tool that automatically optimizes JPEGs to meet specific file size requirements
3. Experiment with different quality settings for various content types (photos, text, graphics) and document the visual differences
4. Create a benchmark comparing compression efficiency between baseline and progressive JPEG with different content types

## Next Tutorial

Continue your learning journey with [Tutorial: How to Update PSD Image Properties](/working-with-image-properties/update-psd-properties/) to learn techniques for working with professional design files.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/imaging/10/).
