---
title: Learn to Deskew Images for Better OCR Results
weight: 30
url: /preprocess-image/deskew-image/
description: Tutorial on how to correct skewed and tilted images for improved OCR accuracy using Aspose.OCR Cloud API's automatic and manual deskewing features.
---

# Tutorial: Learn to Deskew Images for Better OCR Results

## Learning Objectives

In this tutorial, you'll learn:
- What image skew is and how it impacts OCR accuracy
- How to use Aspose.OCR Cloud's automatic skew correction
- When and how to apply manual rotation for severely skewed images
- How to integrate deskewing into your OCR preprocessing workflow
- Best practices for handling different types of skewed images

## Prerequisites

- Basic understanding of REST APIs
- An [Aspose Cloud account](https://dashboard.aspose.cloud/) with an active subscription
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
- Familiarity with your preferred programming language (cURL, Python, C#, or Java)
- Sample skewed images with text for testing

## Understanding Skew in Document Images

Skew refers to the slight rotation or tilt of text lines in an image relative to the horizontal or vertical axis. Skew commonly occurs when:
- Documents are placed incorrectly on scanners
- Photos are taken at an angle with cameras or smartphones
- Original documents are not perfectly aligned

Even minor skew can significantly impact OCR accuracy because OCR engines typically expect text lines to be horizontal. When text lines are tilted, the engine may:
- Incorrectly segment characters and words
- Miss entire lines of text
- Confuse character recognition
- Produce disorganized output

## Tutorial Steps

### 1. Types of Skew and Their Impact

There are two main categories of skew that we need to handle differently:

1. Slight skew (up to 15 degrees)
   - Can be corrected automatically
   - Common in scanned documents
   
2. Severe skew (more than 15 degrees) or completely inverted images
   - Requires manual rotation
   - Common in photos taken with handheld devices

Let's see examples of both:

![Skew Examples](skew-examples.png)

Left: Slightly skewed image (~5°). Right: Severely skewed image (~30°).

### 2. Automatic Skew Correction

Aspose.OCR Cloud offers automatic skew correction through the `makeSkewCorrect` parameter. This works well for images with slight skew up to 15 degrees.

#### Try it yourself - Automatic Skew Correction

Here's a cURL example to enable automatic skew correction during recognition:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeImage' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "resultType": "Text"
  }
}'
```

And using the .NET SDK:

```csharp
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;
using System;
using System.IO;

namespace DeskewExample
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                // Initialize API with your credentials
                RecognizeImageApi api = new RecognizeImageApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
                
                // Read the image file
                byte[] imageData = File.ReadAllBytes("skewed-document.jpg");
                
                // Set up recognition settings with automatic skew correction enabled
                OCRSettingsRecognizeImage settings = new OCRSettingsRecognizeImage
                {
                    Language = "English",
                    MakeSkewCorrect = true,
                    ResultType = "Text"
                };
                
                // Create recognition request
                OCRRecognizeImageBody requestBody = new OCRRecognizeImageBody(imageData, settings);
                
                // Send recognition request
                string taskId = api.PostRecognizeImage(requestBody);
                
                Console.WriteLine($"Task ID: {taskId}");
                Console.WriteLine("Waiting for recognition to complete...");
                
                // Add a small delay to ensure processing has started
                System.Threading.Thread.Sleep(2000);
                
                // Get recognition result
                OCRResponse result = api.GetRecognizeImage(taskId);
                
                // Check if the task is complete
                while (result.TaskStatus != "Completed" && result.TaskStatus != "Error")
                {
                    Console.WriteLine($"Current status: {result.TaskStatus}. Waiting...");
                    System.Threading.Thread.Sleep(1000);
                    result = api.GetRecognizeImage(taskId);
                }
                
                if (result.TaskStatus == "Error")
                {
                    Console.WriteLine("Error occurred during recognition:");
                    foreach (var message in result.Error.Messages)
                    {
                        Console.WriteLine($" - {message}");
                    }
                    return;
                }
                
                // Display the recognized text
                Console.WriteLine("\nRecognized Text from Deskewed Image:");
                Console.WriteLine(result.Results[0].Data);
            }
            catch (Exception ex)
            {
                Console.WriteLine($"An error occurred: {ex.Message}");
            }
        }
    }
}
```

Learning Checkpoint: The automatic skew correction works best for:
- Documents with slight skew angles (up to 15 degrees)
- Documents with clear text lines
- Images with good contrast between text and background

### 3. Manual Rotation for Severe Skew

For images with severe skew (more than 15 degrees) or completely inverted images, you'll need to specify the rotation angle manually. While Aspose.OCR Cloud doesn't currently offer a dedicated endpoint for manual deskewing, you can handle this prior to uploading or by setting rotation parameters in your recognition request.

#### Try it yourself - Handling Severely Skewed Images

When dealing with severely skewed images, the best approach is to incorporate rotation parameters in your workflow:

1. Determine the approximate angle of rotation needed
2. Apply the rotation before sending the image to Aspose.OCR Cloud
3. Proceed with recognition

Here's a simple C# code snippet showing how you might handle rotation before sending the image (using System.Drawing):

```csharp
using System;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;

// Load the image
using (Bitmap originalImage = new Bitmap("severely-skewed.jpg"))
{
    // Create a new bitmap to hold the rotated image
    Bitmap rotatedImage = new Bitmap(originalImage.Width, originalImage.Height);
    
    // Set up rotation transform (example: rotate 90 degrees)
    using (Graphics g = Graphics.FromImage(rotatedImage))
    {
        // Set the rotation point to the center of the image
        g.TranslateTransform(originalImage.Width / 2, originalImage.Height / 2);
        g.RotateTransform(90); // Rotate 90 degrees
        g.TranslateTransform(-originalImage.Width / 2, -originalImage.Height / 2);
        
        // Draw the image on the rotated bitmap
        g.DrawImage(originalImage, new Point(0, 0));
    }
    
    // Save the rotated image
    rotatedImage.Save("rotated-image.jpg", ImageFormat.Jpeg);
    
    // Now you can send this rotated image to Aspose.OCR Cloud
    // ...
}
```

### 4. Combining Deskew with Other Preprocessing Techniques

For optimal results, deskewing is often combined with other preprocessing techniques. Here's a recommended sequence:

1. Apply contrast correction (if needed)
2. Perform deskewing
3. Apply binarization
4. Use upsampling (if dealing with small text)

#### Try it yourself - Complete Preprocessing Pipeline

Here's how to implement a complete preprocessing pipeline with deskewing:

```csharp
// Set up recognition settings with multiple preprocessing steps
OCRSettingsRecognizeImage settings = new OCRSettingsRecognizeImage
{
    Language = "English",
    MakeContrastCorrection = true,
    MakeSkewCorrect = true,
    MakeBinarization = true,
    ResultType = "Text"
};
```

## Measuring Improvement

To evaluate the effectiveness of deskewing:

1. Run OCR on your original skewed image
2. Run OCR on the same image with deskew correction enabled
3. Compare the recognition results

Here's a sample comparison table:

| Skew Angle | Without Deskewing | With Deskewing |
|------------|-------------------|----------------|
| 5° | 85% accuracy | 97% accuracy |
| 10° | 72% accuracy | 96% accuracy |
| 15° | 58% accuracy | 94% accuracy |
| >15° | <40% accuracy | Manual rotation required |

*Note: Results vary depending on image quality and content.*

## Troubleshooting Common Issues

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| Automatic deskewing not working | Skew angle too large | Use manual rotation for angles >15° |
| Text appearing sideways after deskewing | Multi-orientation text in image | Process different text regions separately |
| Poor recognition despite deskewing | Other image quality issues | Apply additional preprocessing (contrast, binarization) |
| Rotated image has black borders | Rotation creates areas with no data | Crop image after rotation or use appropriate background fill |

## What You've Learned

In this tutorial, you've learned:
- How skew affects OCR accuracy
- How to use Aspose.OCR Cloud's automatic skew correction
- How to handle severely skewed images with manual rotation
- How to combine deskewing with other preprocessing techniques
- Best practices for improving OCR results on skewed images

## Further Practice

To reinforce your learning:
1. Test automatic deskewing on images with various skew angles
2. Create a tool to measure skew angle in an image
3. Build a preprocessing pipeline that determines when to use automatic vs. manual deskewing
4. Compare recognition results with and without deskewing for different document types

## Next Steps

Continue your learning journey with our tutorial on [Tutorial: How to Dewarp Document Images](/preprocess-image/dewarp-image/) to learn how to handle curved pages and camera lens distortion.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them in the comments section below or on our [support forum](https://forum.aspose.cloud/c/ocr/12/).
