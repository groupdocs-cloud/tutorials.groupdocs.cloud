---
title: How to Recognize Barcodes Without Cloud Storage Tutorial
description: Learn to implement barcode recognition from URLs, local files, and request bodies without using cloud storage in this step-by-step developer tutorial.
weight: 30
url: /managing-and-optimizing-barcode-recognition/recognize-barcodes-without-cloud-storage/
---

# Tutorial: How to Recognize Barcodes Without Cloud Storage

## Learning Objectives

In this tutorial, you'll learn:
- How to recognize barcodes directly from external URLs
- Methods to process barcodes from local images without storing them in the cloud
- Techniques to read barcodes from request body content
- How to implement barcode recognition with checksum validation

## Prerequisites

Before starting this tutorial, make sure you have:
- An active Aspose Cloud account with Client ID and Client Secret
- A development environment with one of the supported languages (C#, Java, PHP, Python, Node.js, or Go)
- Sample barcode images for testing (local files or URLs)
- Basic understanding of REST API concepts

## Understanding Direct Barcode Recognition

While using cloud storage for barcode recognition offers advantages for batch processing and centralized storage, there are scenarios where you might want to recognize barcodes directly from:

- External URLs (e.g., images hosted on your website)
- Local files (without uploading them to cloud storage first)
- Raw image data in request bodies

Aspose.BarCode for Cloud provides APIs to handle all these scenarios, offering flexibility and convenience for different use cases.

## Tutorial Scenario

In this tutorial, we'll build a barcode recognition system that can:
1. Read barcodes from external image URLs
2. Process barcodes from local image files
3. Recognize barcodes from raw image data in request bodies
4. Validate barcodes with checksum options

## Step 1: Recognizing Barcodes from External URLs

Let's start by learning how to recognize barcodes from images available at external URLs.

### Using cURL

```bash
# First get Access Token
curl -v "https://api.aspose.cloud/oauth2/token" \
     -X POST \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "Accept: application/json"

# Recognize barcode from an external URL
curl -v "https://api.aspose.cloud/v3.0/barcode/recognize?Type=Code128&ChecksumValidation=On&url=http://www.barcoding.com/images/Barcodes/code93.gif" \
     -X POST \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

### Using SDK (Python Example)

```python
# Tutorial Code Example: Recognizing barcodes from external URLs
import aspose_barcode_cloud
from aspose_barcode_cloud.apis.barcode_api import BarcodeApi
from aspose_barcode_cloud.api_client import ApiClient
from aspose_barcode_cloud.configuration import Configuration
from aspose_barcode_cloud.models.preset import Preset

# Configure API credentials
configuration = Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API client
api_client = ApiClient(configuration)
barcode_api = BarcodeApi(api_client)

# Specify parameters
url = "http://www.barcoding.com/images/Barcodes/code93.gif"  # URL of the image
barcode_type = "ALL_SUPPORTED_TYPES"  # Scan for all barcode types

try:
    # Perform recognition
    response = barcode_api.post_barcode_recognize_from_url_or_content(
        url=url,
        type=barcode_type,
        checksum_validation="Default",
        preset=None,
        rect_x=None,
        rect_y=None,
        rect_width=None,
        rect_height=None,
        strip_fnc=None,
        timeout=None,
        median_smoothing_window_size=None,
        allow_median_smoothing=None,
        allow_detect_color_inverted=None,
        allow_data_matrix_industrial_barcodes=None,
        allow_qr_micro_qr=None,
        allow_one_d_wiped_bars=None,
        allow_one_d_fast_barcodes=None,
        australian_post_encoding_table=None,
        ignore_ending_filling_patterns_for_c_table=None,
        file=None
    )
    
    # Process results
    print(f"Recognition Status: {response.status}")
    print(f"Total barcodes found: {len(response.barcodes)}")
    
    for i, barcode in enumerate(response.barcodes, 1):
        print(f"\nBarcode {i}:")
        print(f"  Value: {barcode.barcode_value}")
        print(f"  Type: {barcode.barcode_type}")
        if barcode.region:
            print(f"  Region: {', '.join(barcode.region)}")
        if barcode.checksum:
            print(f"  Checksum: {barcode.checksum}")
            
except Exception as e:
    print(f"Error: {str(e)}")
```

## Try It Yourself!

Find a barcode image URL online or host one yourself, and try to recognize it using the code examples above. Experiment with different barcode types and recognition parameters.

## Step 2: Reading Barcodes from Local Files

Now, let's learn how to recognize barcodes from local image files without first uploading them to cloud storage.

### Using SDK (C# Example)

```csharp
// Tutorial Code Example: Reading barcodes from local files
using Aspose.BarCode.Cloud.Sdk.Api;
using Aspose.BarCode.Cloud.Sdk.Client;
using Aspose.BarCode.Cloud.Sdk.Model;
using System;
using System.IO;

// Configure API credentials
var config = new Configuration
{
    ClientId = "YOUR_CLIENT_ID",
    ClientSecret = "YOUR_CLIENT_SECRET"
};

// Create API client
var apiClient = new ApiClient(config);
var barcodeApi = new BarcodeApi(apiClient);

// Specify parameters
string localFilePath = @"C:\path\to\your\barcode.png";
string barcodeType = "Code128";  // Type of barcode to recognize

try
{
    // Read image file bytes
    byte[] fileBytes = File.ReadAllBytes(localFilePath);
    
    // Perform recognition
    var response = barcodeApi.PostBarcodeRecognizeFromUrlOrContent(
        file: fileBytes,
        type: barcodeType,
        checksumValidation: "Default",
        preset: null,
        rectX: null,
        rectY: null,
        rectWidth: null,
        rectHeight: null,
        stripFnc: null,
        timeout: null,
        medianSmoothingWindowSize: null,
        allowMedianSmoothing: null,
        allowDetectColorInverted: null,
        allowDataMatrixIndustrialBarcodes: null,
        allowQRMicroQR: null,
        allowOneDWipedBars: null,
        allowOneDFastBarcodes: null,
        australianPostEncodingTable: null,
        ignoreEndingFillingPatternsForCTable: null,
        url: null
    );

    // Process results
    Console.WriteLine($"Recognition Status: {response.Status}");
    Console.WriteLine($"Number of barcodes found: {response.Barcodes.Count}");

    foreach (var barcode in response.Barcodes)
    {
        Console.WriteLine($"\nBarcode Value: {barcode.BarcodeValue}");
        Console.WriteLine($"Barcode Type: {barcode.BarcodeType}");
        if (barcode.Region != null)
        {
            Console.WriteLine($"Region: {string.Join(", ", barcode.Region)}");
        }
        if (!string.IsNullOrEmpty(barcode.Checksum))
        {
            Console.WriteLine($"Checksum: {barcode.Checksum}");
        }
    }
}
catch (Exception ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
```

## Step 3: Reading Barcodes from Request Body

You can also process barcodes by directly passing the image data in the request body.

### Using cURL

```bash
# Recognize barcode from request body
curl -v "https://api.aspose.cloud/v3.0/barcode/recognize?Type=Code128" \
     -X POST \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/octet-stream" \
     -H "Accept: application/json" \
     --data-binary @/path/to/your/barcode.png
```

### Using SDK (Java Example)

```java
// Tutorial Code Example: Reading barcodes from request body
import com.aspose.barcode.cloud.api.BarcodeApi;
import com.aspose.barcode.cloud.client.ApiClient;
import com.aspose.barcode.cloud.model.BarcodeResponseList;
import java.io.File;
import java.io.IOException;
import java.nio.file.Files;

public class ReadBarcodeFromRequestBody {

    public static void main(String[] args) {
        try {
            // Configure API credentials
            ApiClient apiClient = new ApiClient("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            BarcodeApi barcodeApi = new BarcodeApi(apiClient);
            
            // Specify parameters
            String localFilePath = "/path/to/your/barcode.png";
            String barcodeType = "Code128";  // Type of barcode to recognize
            
            // Read image file bytes
            File file = new File(localFilePath);
            byte[] fileBytes = Files.readAllBytes(file.toPath());
            
            // Perform recognition
            BarcodeResponseList response = barcodeApi.postBarcodeRecognizeFromUrlOrContent(
                null, // url
                barcodeType,
                "Default", // checksumValidation
                null, // preset
                null, // rectX
                null, // rectY
                null, // rectWidth
                null, // rectHeight
                null, // stripFnc
                null, // timeout
                null, // medianSmoothingWindowSize
                null, // allowMedianSmoothing
                null, // allowDetectColorInverted
                null, // allowDataMatrixIndustrialBarcodes
                null, // allowQRMicroQR
                null, // allowOneDWipedBars
                null, // allowOneDFastBarcodes
                null, // australianPostEncodingTable
                null, // ignoreEndingFillingPatternsForCTable
                fileBytes // file
            );
            
            // Process results
            System.out.println("Recognition Status: " + response.getStatus());
            System.out.println("Number of barcodes found: " + response.getBarcodes().size());
            
            response.getBarcodes().forEach(barcode -> {
                System.out.println("\nBarcode Value: " + barcode.getBarcodeValue());
                System.out.println("Barcode Type: " + barcode.getBarcodeType());
                if (barcode.getRegion() != null) {
                    System.out.println("Region: " + String.join(", ", barcode.getRegion()));
                }
                if (barcode.getChecksum() != null && !barcode.getChecksum().isEmpty()) {
                    System.out.println("Checksum: " + barcode.getChecksum());
                }
            });
            
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Try It Yourself!

Select a local barcode image and try to recognize it using the code examples above. Compare the results with the previous methods to ensure consistency.

## Step 4: Recognizing Barcodes with Checksum Validation

Some barcode types include checksum mechanisms for data integrity verification. Let's explore how to use checksum validation in barcode recognition.

### Using SDK (PHP Example)

```php
<?php
// Tutorial Code Example: Recognizing barcodes with checksum validation

require_once(__DIR__ . '/vendor/autoload.php');

use Aspose\BarCode\Cloud\Api\BarcodeApi;
use Aspose\BarCode\Cloud\Api\Configuration;
use GuzzleHttp\Client;

// Configure API credentials
$config = new Configuration();
$config->setClientId('YOUR_CLIENT_ID');
$config->setClientSecret('YOUR_CLIENT_SECRET');

// Create API client
$apiClient = new Client();
$barcodeApi = new BarcodeApi($apiClient, $config);

try {
    // Specify parameters
    $url = 'http://example.com/path/to/barcode-image.png';
    $barcodeType = 'Code128';
    $checksumValidation = 'On';  // Enable checksum validation (options: Default, On, Off)
    
    // Perform recognition with checksum validation
    $response = $barcodeApi->postBarcodeRecognizeFromUrlOrContent(
        $url,
        $barcodeType,
        $checksumValidation,
        null, // preset
        null, // rectX
        null, // rectY
        null, // rectWidth
        null, // rectHeight
        null, // stripFnc
        null, // timeout
        null, // medianSmoothingWindowSize
        null, // allowMedianSmoothing
        null, // allowDetectColorInverted
        null, // allowDataMatrixIndustrialBarcodes
        null, // allowQRMicroQR
        null, // allowOneDWipedBars
        null, // allowOneDFastBarcodes
        null, // australianPostEncodingTable
        null, // ignoreEndingFillingPatternsForCTable
        null  // file
    );
    
    // Process results
    echo "Recognition Status: " . $response->getStatus() . "\n";
    echo "Number of barcodes found: " . count($response->getBarcodes()) . "\n";
    
    foreach ($response->getBarcodes() as $barcode) {
        echo "\nBarcode Value: " . $barcode->getBarcodeValue() . "\n";
        echo "Barcode Type: " . $barcode->getBarcodeType() . "\n";
        
        if ($barcode->getRegion() !== null) {
            echo "Region: " . implode(", ", $barcode->getRegion()) . "\n";
        }
        
        // With checksum validation enabled, we can access checksum information
        if ($barcode->getChecksum() !== null && $barcode->getChecksum() !== '') {
            echo "Checksum: " . $barcode->getChecksum() . "\n";
        }
    }
} catch (Exception $e) {
    echo "Error: " . $e->getMessage() . "\n";
}
```

## Understanding Checksum Validation Options

When working with barcode recognition, you have three options for checksum validation:

1. Default: Uses the default setting for the specified barcode type
   - For barcodes with obligatory checksum: Checksum is validated
   - For barcodes with optional checksum: Checksum is not validated

2. On: Forces checksum validation for all barcode types (when applicable)
   - Provides higher accuracy but may reject valid barcodes with incorrect checksums

3. Off: Disables checksum validation
   - May allow reading damaged barcodes but with potential data inaccuracies

## Step 5: Working with Multiple Recognition Methods in One Application

In real-world applications, you might need to support multiple barcode recognition methods. Let's create a simple example that demonstrates how to implement a flexible barcode recognition system.

### Using SDK (Node.js Example)

```javascript
// Tutorial Code Example: Flexible barcode recognition system
const fs = require('fs');
const { Configuration, BarcodeApi } = require('aspose-barcode-cloud');

// Configure API credentials
const config = new Configuration({
    clientId: "YOUR_CLIENT_ID",
    clientSecret: "YOUR_CLIENT_SECRET"
});

// Create API client
const barcodeApi = new BarcodeApi(config);

// Function to recognize barcode from URL
async function recognizeBarcodeFromUrl(url, barcodeType = "ALL_SUPPORTED_TYPES") {
    try {
        const response = await barcodeApi.postBarcodeRecognizeFromUrlOrContent(
            url,
            barcodeType,
            "Default", // checksumValidation
            null, // preset
            null, // rectX
            null, // rectY
            null, // rectWidth
            null, // rectHeight
            null, // stripFnc
            null, // timeout
            null, // medianSmoothingWindowSize
            null, // allowMedianSmoothing
            null, // allowDetectColorInverted
            null, // allowDataMatrixIndustrialBarcodes
            null, // allowQRMicroQR
            null, // allowOneDWipedBars
            null, // allowOneDFastBarcodes
            null, // australianPostEncodingTable
            null, // ignoreEndingFillingPatternsForCTable
            null  // file
        );
        
        return response;
    } catch (error) {
        console.error("Error recognizing barcode from URL:", error);
        throw error;
    }
}

// Function to recognize barcode from local file
async function recognizeBarcodeFromFile(filePath, barcodeType = "ALL_SUPPORTED_TYPES") {
    try {
        // Read file as buffer
        const fileBuffer = fs.readFileSync(filePath);
        
        const response = await barcodeApi.postBarcodeRecognizeFromUrlOrContent(
            null, // url
            barcodeType,
            "Default", // checksumValidation
            null, // preset
            null, // rectX
            null, // rectY
            null, // rectWidth
            null, // rectHeight
            null, // stripFnc
            null, // timeout
            null, // medianSmoothingWindowSize
            null, // allowMedianSmoothing
            null, // allowDetectColorInverted
            null, // allowDataMatrixIndustrialBarcodes
            null, // allowQRMicroQR
            null, // allowOneDWipedBars
            null, // allowOneDFastBarcodes
            null, // australianPostEncodingTable
            null, // ignoreEndingFillingPatternsForCTable
            fileBuffer // file
        );
        
        return response;
    } catch (error) {
        console.error("Error recognizing barcode from file:", error);
        throw error;
    }
}

// Function to process recognition results
function processResults(response) {
    console.log(`Recognition Status: ${response.status}`);
    console.log(`Total barcodes found: ${response.barcodes.length}`);
    
    response.barcodes.forEach((barcode, index) => {
        console.log(`\nBarcode ${index + 1}:`);
        console.log(`  Value: ${barcode.barcodeValue}`);
        console.log(`  Type: ${barcode.barcodeType}`);
        
        if (barcode.region) {
            console.log(`  Region: ${barcode.region.join(', ')}`);
        }
        
        if (barcode.checksum) {
            console.log(`  Checksum: ${barcode.checksum}`);
        }
    });
}

// Example usage
async function main() {
    try {
        // Example 1: Recognize from URL
        console.log("RECOGNIZING FROM URL:");
        const urlResponse = await recognizeBarcodeFromUrl(
            "http://www.barcoding.com/images/Barcodes/code93.gif", 
            "Code93"
        );
        processResults(urlResponse);
        
        // Example 2: Recognize from local file
        console.log("\nRECOGNIZING FROM LOCAL FILE:");
        const fileResponse = await recognizeBarcodeFromFile(
            "./barcode-sample.png", 
            "QR"
        );
        processResults(fileResponse);
    } catch (error) {
        console.error("Error in main function:", error);
    }
}

// Run the example
main();
```

## Try It Yourself!

Use the flexible barcode recognition system example to build a small application that can handle different barcode recognition scenarios based on user input. Add support for various barcode types and test with different images.

## Troubleshooting Common Issues

### Issue: "No Barcode Found" Error

If you receive a "no barcode found" error:
- Verify that the image actually contains a barcode
- Check if you're specifying the correct barcode type
- Ensure the image quality is sufficient for recognition
- Try using a different image format (PNG tends to work better than JPEG for barcodes)

### Issue: Incorrect Values in Recognition Results

If the recognized values are incorrect:
- Enable checksum validation with the "On" option
- Try specifying the exact barcode type instead of using "ALL_SUPPORTED_TYPES"
- Check if the barcode image has sufficient contrast and resolution
- Ensure there's adequate quiet zone around the barcode

### Issue: Slow Recognition Performance

If recognition is taking too long:
- Specify the exact barcode type instead of scanning for all types
- Optimize the image size (large images take longer to process)
- Define target regions if you know where the barcode is located in the image
- Use more efficient image formats that maintain barcode clarity

## What You've Learned

In this tutorial, you've learned:
- How to recognize barcodes directly from external URLs
- Methods to process barcodes from local images without storing them in the cloud
- Techniques to read barcodes from request body content
- How to implement barcode recognition with checksum validation
- Ways to build a flexible system that handles multiple recognition scenarios

## Barcode Recognition Method Comparison

| Method | Advantages | Best Used When |
|--------|------------|----------------|
| From URL | - No need to download the image<br>- Works with any publicly accessible image | - Images are already hosted online<br>- Working with third-party images |
| From Local File | - Works offline<br>- No dependency on image hosting<br>- More secure for sensitive barcodes | - Processing offline images<br>- Building desktop applications |
| From Request Body | - More flexibility in data handling<br>- Works with dynamically generated images | - Integrating with other systems<br>- Working with images from memory |

## Next Steps

Ready to learn more about optimizing your barcode recognition? Check out the next tutorial in this series:

[Tutorial: Optimizing Barcode Recognition Settings](/managing-and-optimizing-barcode-recognition/recognition-settings/)

## Further Practice

To reinforce what you've learned, try these exercises:
1. Create a web application that allows users to upload images or provide URLs for barcode recognition
2. Build a batch processing system that can handle multiple barcode images from different sources
3. Develop a solution that compares the results of different recognition methods for the same barcode

## Helpful Resources

- [Product Page](https://products.aspose.cloud/barcode/)
- [Documentation](https://docs.aspose.cloud/barcode/)
- [Live Demo](https://products.aspose.app/barcode/family)
- [API Reference](https://reference.aspose.cloud/barcode/)
- [Blog](https://blog.aspose.cloud/category/barcode/)
- [Free Support](https://forum.aspose.cloud/c/barcode/6/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

## Feedback

Did you find this tutorial helpful? Do you have questions about recognizing barcodes without cloud storage? We'd love to hear from you! Please leave your questions and feedback in our [support forum](https://forum.aspose.cloud/c/barcode/6/).