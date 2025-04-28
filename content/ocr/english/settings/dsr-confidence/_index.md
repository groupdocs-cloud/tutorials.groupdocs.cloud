---
title: Mastering DSR Confidence Settings for Challenging Documents Tutorial
weight: 40
url: /settings/dsr-confidence/
description: Learn how to fine-tune OCR confidence thresholds in Aspose.OCR Cloud API to improve recognition accuracy for challenging, dim, or blurry document images.
---

# Tutorial: Mastering DSR Confidence Settings for Challenging Documents

## Learning Objectives

In this tutorial, you'll learn:
- What DSR confidence settings are and how they affect OCR accuracy
- How to configure confidence thresholds for challenging documents
- Techniques for optimizing recognition of dim, blurry, or low-contrast text
- Best practices for balancing accuracy and completeness in OCR results

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription or free trial
- Basic familiarity with REST API concepts
- Your client credentials (Client ID and Client Secret)
- Completed the [Document Structure Analysis Tutorial](/settings/structure-analysis/) or have equivalent knowledge

## Understanding DSR Confidence Settings

Document Structure Recognition (DSR) confidence settings control how strictly the OCR engine evaluates whether a detected pattern is actually text. These settings are particularly important when working with:

- Low-quality scans
- Faded or dim text
- Blurry images
- Documents with background noise
- Low-contrast text

By adjusting the confidence threshold, you can tell the OCR engine whether to:
- Be more strict (higher confidence) - reducing errors but potentially missing some text
- Be more lenient (lower confidence) - capturing more text but potentially introducing more errors

## The dsrConfidence Parameter in Aspose.OCR Cloud

Aspose.OCR Cloud provides the `dsrConfidence` parameter to control recognition confidence. The value ranges from 0 to 1:

- Higher values (closer to 1): More strict recognition, fewer mistakes but may miss difficult text
- Lower values (closer to 0): More lenient recognition, captures more text but may introduce errors
- Default value: 0.5 (balanced approach)

## Step-by-Step Guide to Configuring DSR Confidence

### Step 1: Authenticate with Aspose.OCR Cloud API

First, obtain your access token:

```bash
# Request an access token
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded"
```

Save the access token from the response for use in subsequent requests.

### Step 2: Process a Standard Document with Default Confidence

First, let's recognize a document with default confidence settings as a baseline:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@faded_document.jpg" \
-F "settings={\"language\": \"English\", \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 3: Lower Confidence to Capture Faded Text

For documents with faded or low-contrast text, try a lower confidence value:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@faded_document.jpg" \
-F "settings={\"language\": \"English\", \"dsrConfidence\": 0.3, \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 4: Increase Confidence for Higher Accuracy

When accuracy is critical and you want to minimize errors:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@noisy_document.jpg" \
-F "settings={\"language\": \"English\", \"dsrConfidence\": 0.7, \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 5: Combine with Appropriate DSR Mode

For best results, combine confidence settings with the appropriate DSR mode:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@old_newspaper.jpg" \
-F "settings={\"language\": \"English\", \"dsrMode\": \"Document\", \"dsrConfidence\": 0.4, \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Try It Yourself

1. Download sample challenging documents from [our repository](https://github.com/aspose-ocr-cloud/aspose-ocr-cloud-examples)
2. Process each document with different confidence values (0.3, 0.5, 0.7)
3. Compare the results to see how the confidence setting affects recognition
4. Find the optimal confidence value for each document type

## SDK Examples

### Python SDK Example

```python
# Tutorial Code Example: DSR Confidence Settings with Python SDK
import asposeocrcloud
from asposeocrcloud.apis.ocr_api import OcrApi
from asposeocrcloud.models.ocr_settings import OcrSettings

# Configure the API client
configuration = asposeocrcloud.Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create an instance of the OcrApi
api_instance = OcrApi(asposeocrcloud.ApiClient(configuration))

# Function to process image with different confidence levels
def process_with_confidence(image_path, confidence_level):
    with open(image_path, "rb") as image_file:
        settings = OcrSettings(
            language="English",
            dsr_confidence=confidence_level,
            result_type="Text"
        )
        
        # Make the API call
        result = api_instance.post_recognize_image(image_file, settings)
        return result.text

# Process a faded document with different confidence levels
image_path = "faded_receipt.jpg"

print("=== Processing with low confidence (0.3) ===")
low_confidence_result = process_with_confidence(image_path, 0.3)
print(low_confidence_result)

print("\n=== Processing with default confidence (0.5) ===")
default_confidence_result = process_with_confidence(image_path, 0.5)
print(default_confidence_result)

print("\n=== Processing with high confidence (0.7) ===")
high_confidence_result = process_with_confidence(image_path, 0.7)
print(high_confidence_result)

# Advanced example - combining with DSR mode for an old document
with open("old_manuscript.jpg", "rb") as image_file:
    settings = OcrSettings(
        language="English",
        dsr_mode="Document",      # Document layout analysis
        dsr_confidence=0.4,       # Slightly lower confidence for old text
        result_type="Text"
    )
    
    # Make the API call
    result = api_instance.post_recognize_image(image_file, settings)
    
    print("\n=== Processing old document with optimized settings ===")
    print(result.text)
```

### Java SDK Example

```java
// Tutorial Code Example: DSR Confidence Settings with Java SDK
import com.aspose.ocr.cloud.api.OcrApi;
import com.aspose.ocr.cloud.auth.*;
import com.aspose.ocr.cloud.model.*;
import java.io.File;

public class DsrConfidenceExample {
    public static void main(String[] args) {
        try {
            // Configure API client with your credentials
            ApiClient defaultClient = Configuration.getDefaultApiClient();
            ClientCredentials credentials = new ClientCredentials();
            credentials.setClientId("YOUR_CLIENT_ID");
            credentials.setClientSecret("YOUR_CLIENT_SECRET");
            OAuth oAuth = new OAuth(credentials);
            defaultClient.setAuthentication("JWT", oAuth);

            // Create OCR API instance
            OcrApi apiInstance = new OcrApi(defaultClient);
            
            // Helper method to process image with different confidence levels
            File imageFile = new File("blurry_document.jpg");
            
            System.out.println("=== Processing with low confidence (0.3) ===");
            processWithConfidence(apiInstance, imageFile, 0.3);
            
            System.out.println("\n=== Processing with default confidence (0.5) ===");
            processWithConfidence(apiInstance, imageFile, 0.5);
            
            System.out.println("\n=== Processing with high confidence (0.7) ===");
            processWithConfidence(apiInstance, imageFile, 0.7);
            
            // Advanced example - low contrast historical document
            File historicalDoc = new File("historical_document.jpg");
            OcrSettings settings = new OcrSettings();
            settings.setLanguage("English");
            settings.setDsrMode(DsrMode.DOCUMENT);     // Document layout analysis
            settings.setDsrConfidence(0.35);           // Low threshold for faded text
            settings.setResultType(ResultType.TEXT);
            
            RecognitionResult result = apiInstance.postRecognizeImage(historicalDoc, settings);
            System.out.println("\n=== Processing historical document with optimized settings ===");
            System.out.println(result.getText());
            
        } catch (Exception e) {
            System.err.println("Exception when calling OcrApi:");
            e.printStackTrace();
        }
    }
    
    private static void processWithConfidence(OcrApi apiInstance, File imageFile, double confidenceLevel) {
        try {
            OcrSettings settings = new OcrSettings();
            settings.setLanguage("English");
            settings.setDsrConfidence(confidenceLevel);
            settings.setResultType(ResultType.TEXT);
            
            RecognitionResult result = apiInstance.postRecognizeImage(imageFile, settings);
            System.out.println(result.getText());
        } catch (Exception e) {
            System.err.println("Error processing with confidence " + confidenceLevel + ": " + e.getMessage());
        }
    }
}
```

### C# SDK Example

```csharp
// Tutorial Code Example: DSR Confidence Settings with C# SDK
using System;
using System.IO;
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;

namespace AsposeTutorials
{
    class DsrConfidenceExample
    {
        static void Main(string[] args)
        {
            try
            {
                // Configure API client with your credentials
                var config = new Configuration();
                config.ClientId = "YOUR_CLIENT_ID";
                config.ClientSecret = "YOUR_CLIENT_SECRET";
                
                // Create OCR API instance
                var apiInstance = new OcrApi(config);
                
                var imagePath = "low_contrast_document.jpg";
                
                Console.WriteLine("=== Processing with low confidence (0.3) ===");
                ProcessWithConfidence(apiInstance, imagePath, 0.3);
                
                Console.WriteLine("\n=== Processing with default confidence (0.5) ===");
                ProcessWithConfidence(apiInstance, imagePath, 0.5);
                
                Console.WriteLine("\n=== Processing with high confidence (0.7) ===");
                ProcessWithConfidence(apiInstance, imagePath, 0.7);
                
                // Advanced example - fax document with noise
                using (var faxImage = new FileStream("fax_document.jpg", FileMode.Open))
                {
                    var settings = new OcrSettings
                    {
                        Language = "English",
                        DsrMode = "TextDetector",   // Text detector mode for simple layouts
                        DsrConfidence = 0.45,       // Slightly lower confidence for noisy fax
                        ResultType = "Text"
                    };
                    
                    // Make the API call
                    var result = apiInstance.PostRecognizeImage(faxImage, settings);
                    
                    Console.WriteLine("\n=== Processing fax document with optimized settings ===");
                    Console.WriteLine(result.Text);
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception when calling OcrApi: " + e.Message);
            }
        }
        
        static void ProcessWithConfidence(OcrApi apiInstance, string imagePath, double confidenceLevel)
        {
            try
            {
                using (var imageFile = new FileStream(imagePath, FileMode.Open))
                {
                    var settings = new OcrSettings
                    {
                        Language = "English",
                        DsrConfidence = confidenceLevel,
                        ResultType = "Text"
                    };
                    
                    // Make the API call
                    var result = apiInstance.PostRecognizeImage(imageFile, settings);
                    Console.WriteLine(result.Text);
                }
            }
            catch (Exception e)
            {
                Console.WriteLine($"Error processing with confidence {confidenceLevel}: {e.Message}");
            }
        }
    }
}
```

## Optimal Confidence Settings by Document Type

Finding the right confidence level is crucial for optimal OCR results. Here are recommended starting points for different document types:

| Document Type | Recommended Confidence | Reasoning |
|---------------|------------------------|-----------|
| High-quality printed text | 0.6 - 0.7 | Clean text allows for strict confidence without losing content |
| Standard office documents | 0.5 | The default balanced approach works well |
| Newspapers/magazines | 0.4 - 0.5 | Various fonts and layouts need balanced settings |
| Faded or old documents | 0.3 - 0.4 | Lower threshold to capture dim text |
| Handwritten notes | 0.3 - 0.4 | More lenient to handle variations in handwriting |
| Faxes or photocopies | 0.4 | Slightly lower threshold for handling noise |
| Low-resolution scans | 0.3 - 0.4 | Lower threshold to capture blurry characters |

## Fine-Tuning Guidelines for Challenging Documents

### For Faded Text:
1. Start with a confidence value of 0.3-0.4
2. Use the "Document" DSR mode for better layout analysis
3. If too many errors appear, gradually increase confidence

### For Blurry Images:
1. Try a confidence value of 0.4
2. Consider preprocessing the image before OCR
3. Use the appropriate DSR mode for the document type

### For Noisy Backgrounds:
1. Start with a higher confidence (0.6-0.7) to filter out noise
2. If important text is missed, gradually decrease the confidence
3. Combine with appropriate DSR mode for best results

## Balancing Quality and Completeness

When working with challenging documents, you'll often need to balance two competing goals:

1. Accuracy: Higher confidence settings reduce errors but may miss some content
2. Completeness: Lower confidence captures more text but may introduce errors

The optimal approach often depends on your specific use case:
- For legal documents: Prioritize accuracy (higher confidence)
- For general information extraction: Balance both factors (medium confidence)
- For maximum text capture: Prioritize completeness (lower confidence)

## Troubleshooting Tips

1. Too Much Noise/Errors: If the OCR result contains many non-text elements or errors, increase the confidence setting (try 0.6-0.8).

2. Missing Text: If important text is not being recognized, especially in dim areas, decrease the confidence setting (try 0.3-0.4).

3. Inconsistent Recognition: Try different combinations of DSR modes and confidence settings to find the optimal configuration.

4. Poor Results Despite Adjustments: Consider preprocessing the image (improving contrast, removing noise) before OCR.

5. Language-Specific Issues: Ensure you're using the correct language setting along with appropriate confidence levels.

## What You've Learned

In this tutorial, you've learned:
- How DSR confidence settings affect OCR accuracy and completeness
- Techniques for adjusting confidence thresholds for challenging documents
- How to combine confidence settings with appropriate DSR modes
- Best practices for different document types and quality levels
- How to balance accuracy and completeness based on your use case

## Further Practice

1. Create a test script that processes the same document with incremental confidence values (0.1, 0.2, ..., 0.9) to find the optimal setting
2. Build a preprocessing and OCR pipeline that automatically adjusts confidence based on document quality assessment
3. Compare results between different combinations of DSR modes and confidence settings
4. Create a document classification system that selects the appropriate confidence setting based on document type

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Visit our [support forums](https://forum.aspose.cloud/c/ocr/12/) for assistance.
