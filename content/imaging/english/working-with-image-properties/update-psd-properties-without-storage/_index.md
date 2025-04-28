---
title: How to Update PSD Image Properties Without Storage Tutorial
url: /working-with-image-properties/update-psd-properties-without-storage/
weight: 30
description: Learn to update PSD image properties without cloud storage by sending the image directly in the request body using Aspose.Imaging Cloud API
---

# Tutorial: How to Update PSD Image Properties Without Storage

## Learning Objectives

In this tutorial, you'll learn how to update properties of Adobe Photoshop PSD files without requiring them to be present in cloud storage. By the end, you'll be able to send PSD files directly in your API request, modify their properties, and receive the updated file in the response.

## Prerequisites

Before starting this tutorial, make sure you have:

- An [Aspose Cloud account](https://dashboard.aspose.cloud/) (free trial available)
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
- A sample PSD file on your local machine
- Basic understanding of RESTful APIs
- Familiarity with your preferred programming language (C#, Java, Python, etc.)

## Practical Scenario

Imagine you're building a web application that allows users to upload PSD files, have them optimized on-the-fly (by changing properties like compression), and then download the optimized versions. Since these are one-time operations, you don't want to store the files in the cloud - you just need to process them and return the results directly.

## Step-by-Step Tutorial

### Step 1: Authentication

First, obtain a JWT token for authentication:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the token you receive for use in subsequent API calls.

### Step 2: Update PSD Properties Without Storage

Unlike the previous tutorial where we updated a file already in cloud storage, here we'll send the file directly in the request body:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/psd?channelsCount=3&compressionMethod=raw" \
-X POST \
-T sample.psd \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-o updated_sample.psd
```

This request:
- Sends your local `sample.psd` file in the request body
- Sets the color channels count to 3 (RGB)
- Changes the compression method to raw (uncompressed)
- Saves the result to your local machine as `updated_sample.psd`

#### Learning Checkpoint:
- How does this approach differ from updating a file in cloud storage? (Answer: The file is sent directly in the request rather than referenced from storage)
- When would you choose this approach over the storage-based approach? (Answer: For one-time operations or when you don't need to store files in the cloud)

### Step 3: Save Updated File to Cloud Storage (Optional)

If you want to save the updated file to cloud storage after processing, you can add the `outPath` parameter:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/psd?channelsCount=3&compressionMethod=raw&outPath=updated_sample.psd" \
-X POST \
-T sample.psd \
-H "Content-Type: application/json" \
-H "Accept: multipart/form-data" \
-H "Authorization: Bearer YOUR_JWT_TOKEN"
```

This will process your local file and save the result as `updated_sample.psd` in your cloud storage, without returning the file content in the response.

### Step 4: Implement with SDK (C#)

For a more programmatic approach, you can use one of Aspose.Imaging Cloud SDKs. Here's how to do it with C#:

```csharp
// Tutorial Code Example: Update PSD Image Properties Without Storage
using System;
using System.IO;
using Aspose.Imaging.Cloud.Sdk.Api;
using Aspose.Imaging.Cloud.Sdk.Model.Requests;

namespace Aspose.Imaging.Cloud.Tutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
            string clientId = "YOUR_CLIENT_ID";
            string clientSecret = "YOUR_CLIENT_SECRET";

            // Create API instance (the SDK takes care of token obtaining)
            ImagingApi imagingApi = new ImagingApi(clientId, clientSecret);

            // Set parameters
            string localFilePath = "sample.psd";       // Local file path
            string outputFilePath = "updated_sample.psd"; // Local output file path
            string outPath = null;                     // Cloud storage path (null for no storage)
            int? channelsCount = 3;                    // RGB (3 channels)
            string compressionMethod = "raw";          // Compression method
            
            try
            {
                Console.WriteLine("Calling Aspose.Imaging Cloud API to update PSD properties...");
                
                // Read the file
                var imageStream = File.OpenRead(localFilePath);
                
                // Create request object
                var request = new CreateModifiedPsdRequest(
                    imageStream,         // The image data
                    channelsCount,       // channelsCount 
                    compressionMethod,   // compressionMethod
                    outPath              // outPath - null means don't save to storage
                );

                // Call the API
                var updatedImageData = imagingApi.CreateModifiedPsd(request);

                // Save the returned data to a local file
                File.WriteAllBytes(outputFilePath, updatedImageData);
                
                Console.WriteLine($"PSD file updated successfully and saved as {outputFilePath}");
                
                // Clean up
                imageStream.Close();
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 5: Implement with SDK (Java)

Here's how to implement the same operation using the Java SDK:

```java
// Tutorial Code Example: Update PSD Image Properties Without Storage
import com.aspose.imaging.cloud.sdk.api.ImagingApi;
import com.aspose.imaging.cloud.sdk.model.requests.CreateModifiedPsdRequest;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;

public class UpdatePsdPropertiesWithoutStorageExample {
    public static void main(String[] args) {
        try {
            // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
            String clientId = "YOUR_CLIENT_ID";
            String clientSecret = "YOUR_CLIENT_SECRET";

            // Create API instance (the SDK takes care of token obtaining)
            ImagingApi imagingApi = new ImagingApi(clientId, clientSecret);

            // Set parameters
            String localFilePath = "sample.psd";       // Local file path
            String outputFilePath = "updated_sample.psd"; // Local output file path
            String outPath = null;                     // Cloud storage path (null for no storage)
            Integer channelsCount = 3;                 // RGB (3 channels)
            String compressionMethod = "raw";          // Compression method
            
            System.out.println("Calling Aspose.Imaging Cloud API to update PSD properties...");
            
            // Read the file
            File file = new File(localFilePath);
            FileInputStream imageStream = new FileInputStream(file);
            byte[] imageData = new byte[(int)file.length()];
            imageStream.read(imageData);
            imageStream.close();
            
            // Create request object
            CreateModifiedPsdRequest request = new CreateModifiedPsdRequest(
                imageData,           // imageData
                channelsCount,       // channelsCount
                compressionMethod,   // compressionMethod
                outPath              // outPath - null means don't save to storage
            );
            
            // Call the API
            byte[] updatedImageData = imagingApi.createModifiedPsd(request);
            
            // Save the returned data to a local file
            FileOutputStream fos = new FileOutputStream(outputFilePath);
            fos.write(updatedImageData);
            fos.close();
            
            System.out.println("PSD file updated successfully and saved as " + outputFilePath);
            
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Troubleshooting Tips

Here are solutions to common issues you might encounter:

1. Authentication Error:
   - Make sure your Client ID and Client Secret are correct
   - Check that your JWT token hasn't expired (they typically last 1 hour)

2. File Upload Issues:
   - Verify the file path is correct and the file exists
   - Ensure the file is a valid PSD format
   - Check if the file size is within API limits

3. Parameter Errors:
   - Valid `channelsCount` values are typically 1, 3, or 4
   - Valid `compressionMethod` values include "raw", "rle", and "zip"

4. Network Issues:
   - For large files, you might encounter timeout issues. Consider implementing retry logic.

## What You've Learned

In this tutorial, you've learned how to:

- Update PSD properties without requiring the file to be in cloud storage
- Send a PSD file directly in the API request body
- Modify color channel count and compression method
- Save the updated file locally or optionally to cloud storage
- Implement the solution using cURL and SDKs

## Further Practice

To reinforce your learning:

1. Create a small web application that accepts PSD uploads, processes them with different compression methods, and lets users download the results
2. Compare the performance between this approach and the storage-based approach for different file sizes
3. Add a feature to extract basic statistics (like file size reduction) after changing compression methods

## Comparison: Storage vs. No Storage Approach

| Aspect | Storage Approach | Without Storage Approach |
|--------|------------------|--------------------------|
| File location | Must be in cloud storage | Sent directly in request |
| API endpoint | GET /v3/imaging/{name}/psd | POST /v3/imaging/psd |
| Good for | Files already in cloud storage, batch processing | One-time operations, direct user uploads |
| Implementation | Requires file upload first | Single request operation |
| Response | File content or storage path | File content or storage path |
| Workflow complexity | Two steps (upload + modify) | Single step (modify directly) |



## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [Live Demo](https://products.aspose.app/imaging/family)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Blog](https://blog.aspose.cloud/category/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them in the comments section below or reach out through our [support forums](https://forum.aspose.cloud/c/imaging/10/).