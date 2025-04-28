---
title: How to Optimize Document Structure Analysis Settings Tutorial
weight: 20
url: /settings/structure-analysis/
description: Learn how to configure document structure recognition modes in Aspose.OCR Cloud API to improve OCR accuracy for different document layouts and types.
---

# Tutorial: How to Optimize Document Structure Analysis Settings

## Learning Objectives

In this tutorial, you'll learn:
- What Document Structure Recognition (DSR) is and why it matters
- How to configure different DSR modes in Aspose.OCR Cloud API
- Which DSR settings work best for different document types
- Techniques for optimizing structure analysis for challenging documents

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription or free trial
- Basic familiarity with REST API concepts
- Your client credentials (Client ID and Client Secret)
- Completed the [Language Settings Tutorial](/settings/languages/) or have equivalent knowledge

## Understanding Document Structure Recognition

Document Structure Recognition (DSR) is a critical part of the OCR process that analyzes the layout of a document to identify different elements such as:

- Text blocks
- Paragraphs
- Columns
- Tables
- Images
- Headers and footers

Proper structure analysis ensures the OCR engine processes text in the correct reading order and maintains the document's logical flow. Aspose.OCR Cloud provides different DSR modes optimized for various document types.

## DSR Modes in Aspose.OCR Cloud

Aspose.OCR Cloud offers several DSR modes that you can specify using the `dsrMode` parameter:

1. Document: Best for standard documents with typical layouts (default)
2. Spreadsheet: Optimized for documents containing tables and structured data
3. DigitalBorn: For digital-native PDFs and screenshots
4. TextDetector: For documents with simple layout, primarily text
5. DetectAreas: Special mode for recognizing text in specific regions of an image

## Step-by-Step Guide to Optimizing DSR Settings

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

### Step 2: Process a Standard Document with Default DSR Mode

For typical business documents, letters, or reports, use the default "Document" mode:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@business_letter.png" \
-F "settings={\"language\": \"English\", \"dsrMode\": \"Document\", \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 3: Process a Document with Tables Using Spreadsheet Mode

For documents with tables or structured data, use the "Spreadsheet" mode:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@financial_report.png" \
-F "settings={\"language\": \"English\", \"dsrMode\": \"Spreadsheet\", \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 4: Process a Digital-Born Document

For screenshots, digital PDFs, or documents created electronically:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@screen_capture.png" \
-F "settings={\"language\": \"English\", \"dsrMode\": \"DigitalBorn\", \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 5: Process Simple Text Documents

For documents with primarily text and simple layouts:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@plain_text.png" \
-F "settings={\"language\": \"English\", \"dsrMode\": \"TextDetector\", \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 6: Recognize Text in Specific Regions

When you need to extract text from specific areas of an image:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize-regions" \
-X POST \
-F "image=@form_image.png" \
-F "settings={\"language\": \"English\", \"dsrMode\": \"DetectAreas\", \"resultType\": \"Text\"}" \
-F "regions=[{\"x\":100, \"y\":200, \"width\":400, \"height\":100}, {\"x\":100, \"y\":350, \"width\":400, \"height\":100}]" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Try It Yourself

1. Download sample documents of different types (business letter, report with tables, form) from [our repository](https://github.com/aspose-ocr-cloud/aspose-ocr-cloud-examples)
2. Process each document with different DSR modes
3. Compare the results to see which mode produces the most accurate output for each document type

## SDK Examples

### Python SDK Example

```python
# Tutorial Code Example: Document Structure Analysis with Python SDK
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

# Example 1: Process a document with tables using Spreadsheet mode
with open("financial_report.jpg", "rb") as image_file:
    settings = OcrSettings(
        language="English",
        dsr_mode="Spreadsheet",  # Optimize for tables and structured data
        result_type="Text"
    )
    
    # Make the API call
    result = api_instance.post_recognize_image(image_file, settings)
    
    # Process and print the result
    print("Recognition result with Spreadsheet mode:")
    print(result.text)

# Example 2: Process a screenshot or digital document
with open("screenshot.png", "rb") as image_file:
    settings = OcrSettings(
        language="English",
        dsr_mode="DigitalBorn",  # Optimize for digital-native content
        result_type="Text"
    )
    
    # Make the API call
    result = api_instance.post_recognize_image(image_file, settings)
    
    # Process and print the result
    print("\nRecognition result with DigitalBorn mode:")
    print(result.text)

# Example 3: Recognize specific regions in a document
with open("form.png", "rb") as image_file:
    settings = OcrSettings(
        language="English",
        dsr_mode="DetectAreas",  # For region-based recognition
        result_type="Text"
    )
    
    # Define regions of interest
    regions = [
        {"x": 100, "y": 200, "width": 400, "height": 100},  # First field
        {"x": 100, "y": 350, "width": 400, "height": 100}   # Second field
    ]
    
    # Make the API call for regions recognition
    result = api_instance.post_recognize_regions(image_file, settings, regions)
    
    # Process and print the result
    print("\nRegion recognition results:")
    for i, region_text in enumerate(result.regions):
        print(f"Region {i+1}: {region_text}")
```

### Java SDK Example

```java
// Tutorial Code Example: Document Structure Analysis with Java SDK
import com.aspose.ocr.cloud.api.OcrApi;
import com.aspose.ocr.cloud.auth.*;
import com.aspose.ocr.cloud.model.*;
import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class DsrSettingsExample {
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
            
            // Example 1: Process a multi-column document
            File newspaperImage = new File("newspaper.jpg");
            OcrSettings settings = new OcrSettings();
            settings.setLanguage("English");
            settings.setDsrMode(DsrMode.DOCUMENT);  // Optimized for multi-column layouts
            settings.setResultType(ResultType.TEXT);
            
            // Make the API call
            RecognitionResult result = apiInstance.postRecognizeImage(newspaperImage, settings);
            
            // Process the result
            System.out.println("Recognition result with Document mode:");
            System.out.println(result.getText());
            
            // Example 2: Process a document with simple layout
            File letterImage = new File("simple_letter.jpg");
            settings.setDsrMode(DsrMode.TEXT_DETECTOR);  // For simple text layouts
            
            // Make the API call
            result = apiInstance.postRecognizeImage(letterImage, settings);
            
            // Process the result
            System.out.println("\nRecognition result with TextDetector mode:");
            System.out.println(result.getText());
            
            // Example 3: Recognize specific regions in a form
            File formImage = new File("application_form.jpg");
            settings.setDsrMode(DsrMode.DETECT_AREAS);  // For region-based recognition
            
            // Define regions of interest
            List<OCRRegion> regions = new ArrayList<>();
            OCRRegion nameField = new OCRRegion();
            nameField.setX(100);
            nameField.setY(200);
            nameField.setWidth(400);
            nameField.setHeight(50);
            regions.add(nameField);
            
            OCRRegion addressField = new OCRRegion();
            addressField.setX(100);
            addressField.setY(300);
            addressField.setWidth(400);
            addressField.setHeight(100);
            regions.add(addressField);
            
            // Make the API call for regions recognition
            RegionsRecognitionResult regionsResult = apiInstance.postRecognizeRegions(formImage, settings, regions);
            
            // Process the result
            System.out.println("\nRegion recognition results:");
            for (int i = 0; i < regionsResult.getRegions().size(); i++) {
                System.out.println("Region " + (i+1) + ": " + regionsResult.getRegions().get(i));
            }
            
        } catch (Exception e) {
            System.err.println("Exception when calling OcrApi:");
            e.printStackTrace();
        }
    }
}
```

### C# SDK Example

```csharp
// Tutorial Code Example: Document Structure Analysis with C# SDK
using System;
using System.IO;
using System.Collections.Generic;
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;

namespace AsposeTutorials
{
    class DsrSettingsExample
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
                
                // Example 1: Process a document with tables
                using (var imageFile = new FileStream("invoice_with_tables.jpg", FileMode.Open))
                {
                    var settings = new OcrSettings
                    {
                        Language = "English",
                        DsrMode = "Spreadsheet",  // Optimize for tables
                        ResultType = "Text"
                    };
                    
                    // Make the API call
                    var result = apiInstance.PostRecognizeImage(imageFile, settings);
                    
                    // Process the result
                    Console.WriteLine("Recognition result with Spreadsheet mode:");
                    Console.WriteLine(result.Text);
                }
                
                // Example 2: Process a digital-born document
                using (var imageFile = new FileStream("pdf_screenshot.png", FileMode.Open))
                {
                    var settings = new OcrSettings
                    {
                        Language = "English",
                        DsrMode = "DigitalBorn",  // For digital-native content
                        ResultType = "Text"
                    };
                    
                    // Make the API call
                    var result = apiInstance.PostRecognizeImage(imageFile, settings);
                    
                    // Process the result
                    Console.WriteLine("\nRecognition result with DigitalBorn mode:");
                    Console.WriteLine(result.Text);
                }
                
                // Example 3: Recognize specific regions in a document
                using (var imageFile = new FileStream("id_card.jpg", FileMode.Open))
                {
                    var settings = new OcrSettings
                    {
                        Language = "English",
                        DsrMode = "DetectAreas",  // For region detection
                        ResultType = "Text"
                    };
                    
                    // Define regions of interest
                    var regions = new List<OCRRegion>
                    {
                        new OCRRegion { X = 100, Y = 50, Width = 300, Height = 30 },   // Name field
                        new OCRRegion { X = 100, Y = 100, Width = 300, Height = 30 },  // ID number field
                        new OCRRegion { X = 400, Y = 50, Width = 200, Height = 150 }   // Photo area
                    };
                    
                    // Make the API call for regions recognition
                    var result = apiInstance.PostRecognizeRegions(imageFile, settings, regions);
                    
                    // Process the result
                    Console.WriteLine("\nRegion recognition results:");
                    for (int i = 0; i < result.Regions.Count; i++)
                    {
                        Console.WriteLine($"Region {i+1}: {result.Regions[i]}");
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception when calling OcrApi: " + e.Message);
            }
        }
    }
}
```

## Choosing the Right DSR Mode

| Document Type | Recommended DSR Mode | Why It Works Best |
|---------------|---------------------|-------------------|
| Business letters, reports | Document | Handles multiple columns, paragraphs, and mixed content well |
| Financial statements, reports with tables | Spreadsheet | Preserves tabular data structure and alignment |
| Screenshots, digital PDFs | DigitalBorn | Optimized for clean, digital-native content |
| Simple letters, plain text | TextDetector | Faster processing for simple layouts |
| Forms, IDs, structured documents | DetectAreas | Allows precise extraction from specific regions |

## Troubleshooting Tips

1. Incorrect Text Order: If text is recognized but in the wrong order (especially in multi-column documents), try switching to the "Document" DSR mode.

2. Tables Not Properly Recognized: For documents with tables where data alignment is important, use the "Spreadsheet" mode and consider using table-specific recognition endpoints.

3. Digital Content Issues: For screenshots or digital PDFs with rendering issues, the "DigitalBorn" mode often produces better results.

4. Performance Considerations: The "TextDetector" mode is fastest but less accurate for complex layouts. Use it for simple documents where speed is critical.

5. Form Data Extraction: When extracting data from specific areas of forms or IDs, the region-based approach with "DetectAreas" mode provides the most precise results.

## What You've Learned

In this tutorial, you've learned:
- The importance of document structure analysis in the OCR process
- How to select and configure different DSR modes in Aspose.OCR Cloud API
- Which DSR settings work best for different document types
- How to use region-based recognition for targeted text extraction
- Techniques for optimizing structure analysis based on document characteristics

## Further Practice

1. Try processing the same document with different DSR modes and compare the results
2. Create a form processing application that uses region-based recognition to extract specific fields
3. Build a processing pipeline that automatically selects the best DSR mode based on document characteristics
4. Experiment with combining DSR modes with different result formats to optimize output for your application

## Next Tutorial in Learning Path

Continue your learning journey with the [Tutorial: Mastering DSR Confidence Settings for Challenging Documents](/settings/dsr-confidence/) to learn how to fine-tune confidence thresholds for better recognition of dim or blurry text.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Visit our [support forums](https://forum.aspose.cloud/c/ocr/12/) for assistance.
