---
title: Learn to Configure and Use Different OCR Result Formats Tutorial
weight: 30
url: /settings/result-format/
description: Learn how to configure and work with various OCR result formats in Aspose.OCR Cloud API, including text, PDF, hOCR, JSON, and spreadsheet formats.
---

# Tutorial: Learn to Configure and Use Different OCR Result Formats

## Learning Objectives

In this tutorial, you'll learn:
- How to specify different result formats in Aspose.OCR Cloud API
- When to use each format based on your application requirements
- How to process and work with each result format
- Techniques for requesting multiple formats in a single API call

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription or free trial
- Basic familiarity with REST API concepts
- Your client credentials (Client ID and Client Secret)
- Completed the [Language Settings Tutorial](/settings/languages/) or have equivalent knowledge

## Understanding Result Format Options

Aspose.OCR Cloud provides flexibility in how you receive recognition results. Depending on your application's needs, you can choose from various formats:

- Plain Text: Simple text output
- PDF: Searchable PDF with text layer
- hOCR: HTML-based format with positioning information
- JSON: Structured data with text and positioning
- Excel/CSV: Tabular formats ideal for recognized tables
- WAV: Audio output for text-to-speech
- PNG: Preprocessed image output

## Step-by-Step Guide to Using Different Result Formats

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

### Step 2: Get Plain Text Results

The most basic format is plain text:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@sample_document.png" \
-F "settings={\"language\": \"English\", \"resultType\": \"Text\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 3: Generate a Searchable PDF

For archiving or document management:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@sample_document.png" \
-F "settings={\"language\": \"English\", \"resultType\": \"Pdf\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 4: Get Result in hOCR Format

For applications that need positional information:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@sample_document.png" \
-F "settings={\"language\": \"English\", \"resultType\": \"Hocr\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 5: Request Multiple Formats in a Single Call

You can request multiple formats in one API call:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@sample_document.png" \
-F "settings={\"language\": \"English\", \"resultType\": \"TextAndPdfAndHocr\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 6: Process Table Recognition Results

For tables, you can get results in Excel or CSV format:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize-table" \
-X POST \
-F "image=@sample_table.png" \
-F "settings={\"language\": \"English\", \"resultTypeTable\": \"CsvAndExcel\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Step 7: Working with JSON Results

For structured data that's easy to parse in applications:

```bash
curl -v "https://api.aspose.cloud/v5.0/ocr/recognize" \
-X POST \
-F "image=@sample_document.png" \
-F "settings={\"language\": \"English\", \"resultType\": \"JSON\"}" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Try It Yourself

1. Download a sample document from [our repository](https://github.com/aspose-ocr-cloud/aspose-ocr-cloud-examples)
2. Process it using different result formats
3. Compare the outputs and understand when each format is most useful
4. Try decoding the Base64 encoded responses to view the actual content

## Decoding Base64 Results

Most results are returned as Base64 encoded strings. Here's how to decode them:

### Python Example
```python
import base64

# Assuming 'response_data' contains the Base64 encoded string
decoded_bytes = base64.b64decode(response_data)
decoded_string = decoded_bytes.decode('utf-8')  # For text-based results
# Or save to file for binary formats like PDF
with open("result.pdf", "wb") as file:
    file.write(decoded_bytes)
```

## SDK Examples

### Python SDK Example

```python
# Tutorial Code Example: Working with Different Result Formats (Python SDK)
import os
import base64
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

# Prepare image file
image_file = open("business_document.jpg", "rb")

# Example 1: Get text and PDF in one request
settings = OcrSettings(
    language="English",
    result_type="TextAndPdf"  # Request both text and PDF
)

# Make the API call
result = api_instance.post_recognize_image(image_file, settings)

# Process the results
print("Text recognition result:")
print(result.text)

# Save the PDF result
pdf_bytes = base64.b64decode(result.pdf)
with open("recognized_document.pdf", "wb") as pdf_file:
    pdf_file.write(pdf_bytes)
print("PDF saved as recognized_document.pdf")

# Example 2: Get JSON format for more detailed information
image_file.seek(0)  # Reset file pointer
settings.result_type = "JSON"

# Make the API call
json_result = api_instance.post_recognize_image(image_file, settings)

# Process the JSON result
print("JSON recognition result (structure):")
for block in json_result.json.text_areas:
    print(f"Block at position ({block.x}, {block.y}) - Size: {block.width}x{block.height}")
    print(f"Text: {block.text}")
    print("---")
```

### Java SDK Example

```java
// Tutorial Code Example: Working with Different Result Formats (Java SDK)
import com.aspose.ocr.cloud.api.OcrApi;
import com.aspose.ocr.cloud.auth.*;
import com.aspose.ocr.cloud.model.*;
import java.io.File;
import java.io.FileOutputStream;
import java.util.Base64;

public class ResultFormatExample {
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
            
            // Prepare image file
            File imageFile = new File("invoice.jpg");
            
            // Example 1: Get hOCR output
            OcrSettings settings = new OcrSettings();
            settings.setLanguage("English");
            settings.setResultType(ResultType.HOCR);
            
            // Make the API call
            RecognitionResult result = apiInstance.postRecognizeImage(imageFile, settings);
            
            // Process and save the hOCR result
            byte[] hocrBytes = Base64.getDecoder().decode(result.getHocr());
            try (FileOutputStream fos = new FileOutputStream("result.hocr")) {
                fos.write(hocrBytes);
            }
            System.out.println("hOCR saved as result.hocr");
            
            // Example 2: Process a table and get Excel output
            File tableImage = new File("data_table.jpg");
            TableRecognitionSettings tableSettings = new TableRecognitionSettings();
            tableSettings.setLanguage("English");
            tableSettings.setResultTypeTable(ResultTypeTable.CSV_AND_EXCEL);
            
            // Make the table recognition call
            TableRecognitionResult tableResult = apiInstance.postRecognizeTable(tableImage, tableSettings);
            
            // Save the Excel result
            byte[] excelBytes = Base64.getDecoder().decode(tableResult.getExcel());
            try (FileOutputStream fos = new FileOutputStream("table_data.xlsx")) {
                fos.write(excelBytes);
            }
            System.out.println("Excel table saved as table_data.xlsx");
            
            // Save the CSV result
            byte[] csvBytes = Base64.getDecoder().decode(tableResult.getCsv());
            try (FileOutputStream fos = new FileOutputStream("table_data.csv")) {
                fos.write(csvBytes);
            }
            System.out.println("CSV table saved as table_data.csv");
            
        } catch (Exception e) {
            System.err.println("Exception when calling OcrApi:");
            e.printStackTrace();
        }
    }
}
```

### C# SDK Example

```csharp
// Tutorial Code Example: Working with Different Result Formats (C# SDK)
using System;
using System.IO;
using System.Text;
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;

namespace AsposeTutorials
{
    class ResultFormatExample
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
                
                // Example 1: Get multiple formats in one request
                using (var imageFile = new FileStream("contract.jpg", FileMode.Open))
                {
                    var settings = new OcrSettings
                    {
                        Language = "English",
                        ResultType = "TextAndPdfAndHocr" // Request text, PDF and hOCR
                    };
                    
                    // Make the API call
                    var result = apiInstance.PostRecognizeImage(imageFile, settings);
                    
                    // Process the results
                    Console.WriteLine("Text recognition result:");
                    Console.WriteLine(result.Text);
                    
                    // Save the PDF result
                    var pdfBytes = Convert.FromBase64String(result.Pdf);
                    File.WriteAllBytes("result.pdf", pdfBytes);
                    Console.WriteLine("PDF saved as result.pdf");
                    
                    // Save the hOCR result
                    var hocrBytes = Convert.FromBase64String(result.Hocr);
                    File.WriteAllBytes("result.hocr", hocrBytes);
                    Console.WriteLine("hOCR saved as result.hocr");
                }
                
                // Example 2: Get JSON format for more detailed information
                using (var imageFile = new FileStream("receipt.jpg", FileMode.Open))
                {
                    var settings = new OcrSettings
                    {
                        Language = "English",
                        ResultType = "JSON"
                    };
                    
                    // Make the API call
                    var result = apiInstance.PostRecognizeImage(imageFile, settings);
                    
                    // Process the JSON result
                    Console.WriteLine("JSON recognition result (structure):");
                    foreach (var block in result.Json.TextAreas)
                    {
                        Console.WriteLine($"Block at position ({block.X}, {block.Y}) - Size: {block.Width}x{block.Height}");
                        Console.WriteLine($"Text: {block.Text}");
                        Console.WriteLine("---");
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

## Troubleshooting Tips

1. Base64 Decoding Issues: Ensure you're properly decoding the Base64 encoded results before saving or displaying them.

2. Empty Results: If you receive empty results, check if your image has sufficient quality or if the language setting is appropriate.

3. PDF Text Layer Issues: If the text in your PDF isn't selectable, verify that the OCR process completed successfully and that the image quality was sufficient.

4. Table Format Problems: For table recognition, ensure your image contains clearly defined table boundaries for better results.

5. JSON Structure Understanding: When working with JSON results, explore the object structure carefully to access all available information.

## Choosing the Right Format for Your Application

| Format | Best for |
|--------|----------|
| Text | Simple text extraction, search, and basic processing |
| PDF | Document archiving, sharing, and maintaining original layout |
| hOCR | Applications needing positional data (e.g., highlighting words) |
| JSON | Web applications and APIs needing structured data |
| Excel/CSV | Data analysis, spreadsheets, database import |

## What You've Learned

In this tutorial, you've learned:
- How to specify different result formats in your OCR requests
- Techniques for processing Base64 encoded results
- When to use each format based on your application requirements
- How to request multiple formats in a single API call
- Working with table-specific formats like Excel and CSV

## Further Practice

1. Create a small application that extracts text from an image and saves it in multiple formats
2. Build a document processing pipeline that uses different formats for different purposes
3. Experiment with table recognition and compare the CSV and Excel outputs
4. Create a searchable PDF archive from a collection of images

## Next Tutorial in Learning Path

Continue your learning journey with the [Tutorial: How to Optimize Document Structure Analysis Settings](/settings/structure-analysis/) to understand how to improve recognition accuracy through proper document structure analysis.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Visit our [support forums](https://forum.aspose.cloud/c/ocr/12/) for assistance.
