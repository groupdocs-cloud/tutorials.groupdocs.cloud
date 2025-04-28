---
title: How to Correct Image Contrast Tutorial
weight: 20
url: /preprocess-image/correct-contrast/
description: Learn how to automatically adjust image contrast to improve OCR recognition results with Aspose.OCR Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Correct Image Contrast for OCR

## Learning Objectives

In this tutorial, you'll learn:
- Why contrast correction is essential for OCR accuracy
- How to implement automatic contrast adjustment using Aspose.OCR Cloud API
- When to apply contrast correction in your OCR workflow
- How to combine contrast correction with other preprocessing techniques

## Prerequisites

- Basic understanding of REST APIs
- An [Aspose Cloud account](https://dashboard.aspose.cloud/) with an active subscription
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
- Familiarity with your preferred programming language (cURL, Python, C#, or Java)
- Sample low-contrast images with text for testing

## Understanding Image Contrast in OCR

Low contrast is one of the most common issues affecting OCR accuracy, especially in:
- Images taken with smartphone cameras in poor lighting conditions
- Faded or weathered documents
- Documents with colored backgrounds
- Photocopies of photocopies (degraded documents)

When contrast is low, the distinction between text and background becomes blurred, making it difficult for OCR algorithms to identify character boundaries accurately.

Contrast correction enhances the difference between light and dark areas, making text stand out more clearly against the background.

## Tutorial Steps

### 1. The Problem with Low Contrast Images

Let's start by understanding why contrast matters for OCR. Consider these two images:

![Contrast Example](contrast-example.png)

Left: Low contrast image. Right: Same image with corrected contrast.

The image on the right has much clearer separation between text and background, which significantly improves OCR accuracy.

### 2. Using Automatic Contrast Correction with Aspose.OCR Cloud

Aspose.OCR Cloud provides an automatic contrast correction feature that can be enabled through the `makeContrastCorrection` setting. This setting is available in all image recognition methods.

#### Try it yourself - Basic Contrast Correction

Here's a cURL example to enable contrast correction during recognition:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeImage' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeContrastCorrection": true,
    "resultType": "Text"
  }
}'
```

And here's the same operation using the .NET SDK:

```csharp
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;
using System;
using System.IO;

namespace ContrastCorrectionExample
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
                byte[] imageData = File.ReadAllBytes("low-contrast-sample.jpg");
                
                // Set up recognition settings with contrast correction enabled
                OCRSettingsRecognizeImage settings = new OCRSettingsRecognizeImage
                {
                    Language = "English",
                    MakeContrastCorrection = true,
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
                Console.WriteLine("\nRecognized Text:");
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

Learning Checkpoint: This approach automatically corrects the contrast during the recognition process. Note that you cannot directly access the contrast-corrected image as a file - it's an internal preprocessing step.

### 3. When to Use Contrast Correction

Contrast correction is particularly useful in the following scenarios:

1. Smartphone photos of documents
   - Improves readability of text in images taken with smartphones, especially in poor lighting

2. Aging or faded documents
   - Brings out faint text in old, weathered, or faded documents

3. Documents with colored backgrounds
   - Helps separate text from colored backgrounds that might be too close in grayscale values

4. Low-quality scans or photocopies
   - Improves clarity in documents that have gone through multiple generations of copying

### 4. Combining Contrast Correction with Other Preprocessing

For optimal results, contrast correction often works best when combined with other preprocessing techniques. Here's a recommended workflow:

1. Apply contrast correction first
2. Follow with binarization
3. Apply geometric corrections (deskew, dewarping) last

#### Try it yourself - Combined Preprocessing

Here's how to enable multiple preprocessing options together:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeImage' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeContrastCorrection": true,
    "makeBinarization": true,
    "makeSkewCorrect": true,
    "resultType": "Text"
  }
}'
```

And with the .NET SDK:

```csharp
// Set up recognition settings with multiple preprocessing steps
OCRSettingsRecognizeImage settings = new OCRSettingsRecognizeImage
{
    Language = "English",
    MakeContrastCorrection = true,
    MakeBinarization = true,
    MakeSkewCorrect = true,
    ResultType = "Text"
};
```

## Measuring Improvement

To evaluate the effectiveness of contrast correction:

1. Run OCR on your original image without contrast correction
2. Run OCR on the same image with contrast correction enabled
3. Compare the recognition results for accuracy

For a more scientific approach, you can calculate:
- Character error rate (CER)
- Word error rate (WER)
- Overall accuracy percentage

### Sample Comparison

| Scenario | Character Error Rate | Word Accuracy |
|----------|----------------------|---------------|
| Without contrast correction | 12.5% | 85.3% |
| With contrast correction | 5.2% | 94.7% |

*Note: Results vary depending on image quality and content.*

## Troubleshooting Common Issues

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| Over-contrasted results | Image already has good contrast | Disable contrast correction for high-quality images |
| Contrast correction not helping | Extremely poor original image | Try other preprocessing methods or improve image capture |
| Text disappearing after contrast | Very faint text on dark background | Try upsampling before contrast correction |

## What You've Learned

In this tutorial, you've learned:
- How contrast affects OCR accuracy
- How to enable automatic contrast correction in Aspose.OCR Cloud
- When to use contrast correction in your OCR workflow
- How to combine contrast correction with other preprocessing techniques

## Further Practice

To reinforce your learning:
1. Test contrast correction on various types of low-quality images
2. Create a comparison table showing recognition results with and without contrast correction
3. Experiment with different combinations of preprocessing techniques
4. Develop a simple application that automatically determines when contrast correction is needed

## Next Steps

Continue your learning journey with our tutorial on [Learn to Binarize Images for Improved OCR](/preprocess-image/binarize-image/) to further enhance your image preprocessing skills.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them in the comments section below or on our [support forum](https://forum.aspose.cloud/c/ocr/12/).
