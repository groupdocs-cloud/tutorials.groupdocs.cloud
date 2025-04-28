---
title: How to Deskew Images Tutorial
url: /manipulating-images/deskew-images/
weight: 20
description: Learn how to automatically correct skewed images using Aspose.Imaging Cloud API in this step-by-step developer tutorial.
---

# Tutorial: How to Deskew Images

## Learning Objectives

In this tutorial, you'll learn how to:
- Understand what image deskewing is and why it's important
- Use Aspose.Imaging Cloud API to automatically correct skewed images
- Implement deskewing with both cloud storage and direct upload approaches
- Process scanned documents and images to improve their readability

## Prerequisites

Before starting this tutorial:
- Create an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps)
- Get your App SID and App Key credentials
- Basic understanding of REST API concepts
- Familiarity with your preferred programming language

## Understanding Image Deskewing

Image deskewing is the process of automatically detecting and correcting the orientation of a skewed (tilted) image. This is particularly useful for:

- Scanned documents that weren't perfectly aligned
- Photos of documents taken from mobile devices
- OCR preprocessing to improve text recognition
- Batch processing of document images

## API Endpoints for Deskewing

Aspose.Imaging Cloud provides two main endpoints for deskewing images:

1. GET /imaging/{name}/deskew - For images already stored in Cloud Storage
2. POST /imaging/deskew - For direct image upload in the request body

Let's learn how to use each approach.

## Method 1: Using Cloud Storage Approach

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
curl -v "https://api.aspose.cloud/v3/imaging/storage/file/skewed_document.jpg" \
-X PUT \
-T "path/to/your/local/skewed_document.jpg" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### Step 3: Deskew the Image

```bash
curl -v "https://api.aspose.cloud/v3/imaging/skewed_document.jpg/deskew" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o deskewed_document.jpg
```

## Method 2: Direct Upload Approach

This approach allows you to directly upload and deskew an image without first storing it.

### Step 1: Authentication

Same as in Method 1, get your JWT token.

### Step 2: Upload and Deskew Image

```bash
curl -v "https://api.aspose.cloud/v3/imaging/deskew" \
-X POST \
-T "path/to/your/local/skewed_document.jpg" \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o deskewed_document.jpg
```

## Understanding the Deskew Process

When you call the deskew API, Aspose.Imaging Cloud:

1. Analyzes the image to detect the skew angle
2. Rotates the image to compensate for the detected angle
3. Returns the corrected image in the response
4. Maintains the original format unless specified otherwise

## Try It Yourself!

For this hands-on exercise:
1. Find a skewed document image (or intentionally scan a document at an angle)
2. Use either method above to deskew the image
3. Compare the original and deskewed images to see the improvement

## Using SDKs for Easier Implementation

### .NET SDK Example

```csharp
// Example .NET code for deskewing an image
// Full code available in the GitHub Gist linked below
public void DeskewImageInCloud()
{
    // The image name to be deskewed
    string imageName = "skewed_document.bmp";
    
    // First upload the image to cloud
    using (FileStream imageStream = new FileStream(Path.Combine(ExampleImagesFolder, imageName), FileMode.Open))
    {
        UploadFileRequest uploadRequest = new UploadFileRequest(imageName, imageStream);
        FileApi.UploadFile(uploadRequest);
    }
    
    // Optional parameters
    string format = null;      // Leave as null to keep original format, or specify output format
    string folder = null;      // Cloud Storage folder where the image is stored
    string storage = null;     // Cloud Storage name
    
    // Process the image
    DeskewImageRequest request = new DeskewImageRequest(imageName, format, folder, storage);
        
    using (Stream deskewedImage = ImagingApi.DeskewImage(request))
    {
        // Save deskewed image to local storage
        using (FileStream outputStream = new FileStream(
            Path.Combine(OutputFolder, $"{Path.GetFileNameWithoutExtension(imageName)}_deskewed.bmp"), 
            FileMode.Create))
        {
            deskewedImage.CopyTo(outputStream);
        }
    }
}
```

### Java SDK Example

```java
// Example Java code for deskewing an image
// Full code available in the GitHub Gist linked below
public void deskewImageInCloud() throws Exception {
    // The image name to be deskewed
    String imageName = "skewed_document.bmp";
    
    // First upload the image to cloud
    byte[] imageData = Files.readAllBytes(Paths.get(ExampleImagesFolder, imageName));
    UploadFileRequest uploadRequest = new UploadFileRequest(imageName, imageData);
    fileApi.uploadFile(uploadRequest);
    
    // Optional parameters
    String format = null;      // Leave as null to keep original format, or specify output format
    String folder = null;      // Cloud Storage folder where the image is stored
    String storage = null;     // Cloud Storage name
    
    // Process the image
    DeskewImageRequest request = new DeskewImageRequest(imageName, format, folder, storage);
        
    byte[] deskewedImage = imagingApi.deskewImage(request);
    
    // Save deskewed image to local storage
    Files.write(Paths.get(OutputFolder, imageName + "_deskewed.bmp"), deskewedImage);
}
```

## Advanced Deskewing Tips

- Preprocessing: For better results, consider converting images to grayscale or black and white before deskewing
- Resolution: Higher resolution images often yield better deskewing results
- Batch Processing: For large volumes, use the direct upload approach in an automated pipeline
- Multi-step Process: For highly skewed documents, sometimes multiple passes of deskewing provide better results

## Troubleshooting Tips

- No Visible Change: If the image doesn't appear different, it might have been only slightly skewed or the algorithm couldn't detect the skew
- Quality Loss: If quality degrades, try specifying a lossless format like PNG or TIFF
- Content Cropping: In some cases, deskewing might crop content at edges; add padding to your original image if needed

## What You've Learned

In this tutorial, you've learned:
- What image deskewing is and why it's important
- How to use Aspose.Imaging Cloud API to automatically correct skewed images
- Two different implementation approaches: Cloud Storage and Direct Upload
- SDK implementations for .NET and Java
- Advanced tips for better deskewing results

## Further Practice

To reinforce your learning:
1. Process a batch of differently skewed images
2. Create a comparison tool that shows before/after results
3. Integrate deskewing into a document scanning workflow

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/imaging/10/).
