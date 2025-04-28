---
title: Adding Images to Your Search Context Tutorial
url: /reverse-image-search/add-image/
weight: 30
description: Learn how to add images to your search context for reverse image search in this step-by-step tutorial using Aspose.Imaging Cloud API.
---

# Tutorial: Adding Images to Your Search Context

## Learning Objectives

In this tutorial, you'll learn:
- How to add images to your search context
- The process of image feature extraction during upload
- Best practices for managing images in a search context
- How to verify successful image addition

## Prerequisites

Before starting this tutorial, please ensure you have:
- Completed the [Creating Your First Search Context](/reverse-image-search/create-search-context/) tutorial
- A valid search context ID from the previous tutorial
- Your Aspose Cloud credentials (Client ID and Client Secret)
- A valid access token for the Aspose Cloud API
- An image file ready for upload (JPG, PNG, or other supported format)

## Understanding Image Addition to Search Context

When you add an image to a search context, two important things happen:

1. The image itself is stored and associated with your search context
2. Visual features are automatically extracted and indexed for similarity matching

These features are what enable the system to find similar images later, so the quality and variety of images you add are important factors in the effectiveness of your searches.

## Step 1: Obtain an Access Token

First, authenticate with the Aspose Cloud API:

```bash
curl -v "https://api.aspose.cloud/oauth2/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the access token from the response for use in the next step.

## Step 2: Add an Image to Your Search Context

Use the following API call to add an image to your search context:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/ai/imageSearch/YOUR_SEARCH_CONTEXT_ID/image?imageId=sample_image.jpg" \
-X POST \
-T /path/to/your/sample_image.jpg \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace the following placeholders:
- `YOUR_SEARCH_CONTEXT_ID` with your actual search context ID
- `YOUR_ACCESS_TOKEN` with the token from Step 1
- `/path/to/your/sample_image.jpg` with the local path to your image file
- `sample_image.jpg` in the `imageId` parameter with a unique identifier for this image

The `imageId` parameter is important as it will be used to reference this specific image in future operations.

## Step 3: Verify the Operation

If successful, you'll receive a response like this:

```json
{
  "Code": 200,
  "Status": "OK"
}
```

This confirms that your image was successfully added to the search context.

## Step 4: Check Search Context Status

After adding an image, the search context will process the image to extract features. This may take a moment to complete. You can check the status:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/ai/imageSearch/YOUR_SEARCH_CONTEXT_ID/status" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Wait until the status returns to "Idle" before adding more images or performing searches.

## Try It Yourself

Now, let's practice adding images to your search context:

1. Prepare a few images on your computer (JPG, PNG, or other supported formats)
2. Obtain an access token using the first curl command
3. Add each image to your search context, giving each a unique `imageId`
4. Check the search context status after each addition to ensure it returns to "Idle"
5. Try adding images of different types and sizes to understand any limitations

## Using SDK Libraries

### Java SDK Example

```java
import com.aspose.imaging.cloud.sdk.api.ImagingApi;

import java.io.File;
import java.nio.file.Files;

public class AddImageToSearchContextExample {
    
    public static void main(String[] args) {
        // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Create instance of the API
        ImagingApi api = new ImagingApi(clientId, clientSecret);
        
        // Your search context ID from previous operations
        String searchContextId = "YOUR_SEARCH_CONTEXT_ID";
        
        // Path to your local image file
        String imagePath = "/path/to/your/sample_image.jpg";
        
        // Unique identifier for this image in the search context
        String imageId = "sample_image.jpg";
        
        try {
            // Read the image into a byte array
            File imageFile = new File(imagePath);
            byte[] imageData = Files.readAllBytes(imageFile.toPath());
            
            // Add the image to the search context
            api.addSearchImage(searchContextId, imageData, imageId, null, null);
            
            System.out.println("Image successfully added to search context with ID: " + imageId);
            
            // Optional: Wait for the search context to return to Idle state
            boolean isIdle = false;
            while (!isIdle) {
                String status = api.getImageSearchStatus(searchContextId, null, null).getSearchStatus();
                System.out.println("Current status: " + status);
                isIdle = status.equals("Idle");
                if (!isIdle) {
                    Thread.sleep(1000); // Wait 1 second before checking again
                }
            }
            
            System.out.println("Search context is now ready for more operations");
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### .NET SDK Example

```csharp
using Aspose.Imaging.Cloud.Sdk.Api;
using System;
using System.IO;
using System.Threading;

namespace AsposeImagingCloudTutorials
{
    class AddImageToSearchContextExample
    {
        static void Main(string[] args)
        {
            // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
            string clientId = "YOUR_CLIENT_ID";
            string clientSecret = "YOUR_CLIENT_SECRET";
            
            // Create instance of the API
            ImagingApi api = new ImagingApi(clientId, clientSecret);
            
            // Your search context ID from previous operations
            string searchContextId = "YOUR_SEARCH_CONTEXT_ID";
            
            // Path to your local image file
            string imagePath = @"C:\path\to\your\sample_image.jpg";
            
            // Unique identifier for this image in the search context
            string imageId = "sample_image.jpg";
            
            try
            {
                // Read the image into a byte array
                byte[] imageData = File.ReadAllBytes(imagePath);
                
                // Add the image to the search context
                api.AddSearchImage(searchContextId, imageData, imageId);
                
                Console.WriteLine("Image successfully added to search context with ID: " + imageId);
                
                // Optional: Wait for the search context to return to Idle state
                bool isIdle = false;
                while (!isIdle)
                {
                    string status = api.GetImageSearchStatus(searchContextId).SearchStatus;
                    Console.WriteLine("Current status: " + status);
                    isIdle = status == "Idle";
                    if (!isIdle)
                    {
                        Thread.Sleep(1000); // Wait 1 second before checking again
                    }
                }
                
                Console.WriteLine("Search context is now ready for more operations");
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error: " + ex.Message);
            }
        }
    }
}
```

## Best Practices for Adding Images

1. Unique Image IDs: Always use descriptive and unique image IDs to easily identify images later.
2. Image Quality: Higher quality images provide better feature extraction, but be mindful of file size.
3. Batch Processing: If adding many images, consider implementing a queue system to add them sequentially.
4. Status Monitoring: Always check that the search context returns to "Idle" before adding more images.
5. Diversity: Include diverse images to improve the quality of similarity searches.

## What You've Learned

In this tutorial, you've learned:
- How to add images to your search context
- The importance of unique image IDs
- How to verify successful image addition
- Best practices for managing images in a search context
- How to wait for feature extraction to complete

## Troubleshooting Tips

- Image Format Issues: If an image fails to upload, check that it's in a supported format (JPG, PNG, BMP, etc.)
- Timeout Errors: Large images may cause timeouts. Consider resizing very large images before upload.
- Duplicate Image IDs: If you use an existing image ID, you may overwrite the previous image. Always use unique IDs.
- Status Stuck: If the status seems stuck, wait a few minutes and try again; feature extraction for complex images can take time.

## Next Steps

Now that you know how to add images to your search context, you're ready to move on to the next tutorial in our series:

→ [Tutorial: Extracting Image Features](/reverse-image-search/extract-image-features/)

In the next tutorial, you'll learn about extracting features from images and how these features are used for similarity matching.

## Further Practice

To reinforce what you've learned:
- Add a variety of images to your search context (faces, objects, landscapes, etc.)
- Create a simple script that adds multiple images sequentially
- Try using different image formats to see if there are any differences in processing
- Create a naming convention for your image IDs that helps you organize and find them later

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Please feel free to post your questions on our [support forum](https://forum.aspose.cloud/c/imaging/10/).