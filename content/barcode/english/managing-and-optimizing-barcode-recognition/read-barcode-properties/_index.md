---
title: How to Decode Barcode Properties and Metadata Tutorial
description: Step-by-step tutorial on extracting and interpreting barcode properties, metadata, and quality information from barcodes using Aspose.BarCode Cloud API.
weight: 50
url: /managing-and-optimizing-barcode-recognition/read-barcode-properties/
---

# Tutorial: How to Decode Barcode Properties and Metadata

## Learning Objectives

In this tutorial, you'll learn:
- How to extract barcode type, encoded data, and technical properties
- Methods to decode barcode data as byte streams and Unicode text
- Techniques to assess the quality and confidence of barcode recognition
- How to determine barcode position and orientation angle
- Ways to extract metadata from specific barcode types (PDF417, QR Code, DataBar, etc.)

## Prerequisites

Before starting this tutorial, make sure you have:
- Completed the earlier tutorials in this series
- An active Aspose Cloud account with Client ID and Client Secret
- A development environment with one of the supported languages (C#, Java, PHP, Python, Node.js, or Go)
- Sample barcode images of different types for testing

## Understanding Barcode Properties and Metadata

Aspose.BarCode for Cloud not only decodes the main data from barcodes but also provides access to various technical properties and metadata. This additional information can be crucial for:

- Verifying the authenticity and integrity of barcodes
- Processing specialized barcode formats
- Determining the physical characteristics of barcodes
- Assessing the quality of barcode recognition

## Tutorial Scenario

In this tutorial, we'll explore how to extract and interpret different barcode properties and metadata through practical examples. We'll learn how to:
1. Extract basic barcode type and encoded data
2. Read barcode data as byte streams and Unicode text
3. Check the quality and confidence of recognition results
4. Determine barcode position and orientation
5. Extract metadata from specialized barcode types

## Step 1: Extracting Barcode Type and Encoded Data

Let's start by learning how to extract the basic information from a barcode: its type and the encoded data.

### Using cURL

```bash
# First get Access Token
curl -v "https://api.aspose.cloud/oauth2/token" \
     -X POST \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "Accept: application/json"

# Recognize barcode and get type and data
curl -v "https://api.aspose.cloud/v3.0/barcode/sample-qrcode.png/recognize?type=QR&format=png" \
     -X GET \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

### Using SDK (C# Example)

```csharp
// Tutorial Code Example: Extracting barcode type and encoded data
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

try
{
    // Read image file bytes
    string localFilePath = @"C:\path\to\your\sample-qrcode.png";
    byte[] fileBytes = File.ReadAllBytes(localFilePath);
    
    // Perform recognition
    var response = barcodeApi.PostBarcodeRecognizeFromUrlOrContent(
        file: fileBytes,
        type: "QR",  // Specify barcode type
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

    Console.WriteLine($"Recognition Status: {response.Status}");
    Console.WriteLine($"Number of barcodes found: {response.Barcodes.Count}");

    foreach (var barcode in response.Barcodes)
    {
        // Extract barcode type
        Console.WriteLine($"\nBarcode Type: {barcode.BarcodeType}");
        
        // Extract encoded data (CodeText)
        Console.WriteLine($"Barcode Value (CodeText): {barcode.BarcodeValue}");
    }
}
catch (Exception ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
```

## Try It Yourself!

Create or download QR codes and other barcode types, then use the code examples above to extract their type and data. Compare the results between different barcode types to understand how the information is presented.

## Step 2: Reading Barcode Data as Byte Streams and Unicode Text

For more advanced scenarios, you might need to work with barcode data as raw bytes or handle Unicode text properly.

### Using SDK (Java Example)

```java
// Tutorial Code Example: Reading barcode data as byte streams and Unicode text
import com.aspose.barcode.cloud.api.BarcodeApi;
import com.aspose.barcode.cloud.client.ApiClient;
import com.aspose.barcode.cloud.model.BarcodeResponseList;
import java.io.File;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.util.Base64;

public class BarcodeBytesAndUnicode {

    public static void main(String[] args) {
        try {
            // Configure API credentials
            ApiClient apiClient = new ApiClient("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            BarcodeApi barcodeApi = new BarcodeApi(apiClient);
            
            // Specify parameters
            String localFilePath = "/path/to/unicode-qrcode.png";
            String barcodeType = "QR";
            
            // Read image file bytes
            File file = new File(localFilePath);
            byte[] fileBytes = Files.readAllBytes(file.toPath());
            
            // Perform recognition with Unicode detection enabled
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
                true, // allowDetectColorInverted (enable Unicode detection)
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
                // Get barcode value as string
                String codeText = barcode.getBarcodeValue();
                System.out.println("\nBarcode Type: " + barcode.getBarcodeType());
                System.out.println("Barcode Value (as String): " + codeText);
                
                // Convert string to byte array
                byte[] codeBytes = codeText.getBytes(StandardCharsets.UTF_8);
                System.out.println("Barcode Value (as Base64): " + Base64.getEncoder().encodeToString(codeBytes));
                
                // If working with Unicode data
                System.out.println("Barcode Value (UTF-8 bytes length): " + codeBytes.length);
                
                // Interpret as different character sets if needed
                if (barcode.getBarcodeType().equals("QR") || barcode.getBarcodeType().equals("DataMatrix")) {
                    try {
                        // Try different encodings if needed
                        System.out.println("Value as UTF-8: " + new String(codeBytes, StandardCharsets.UTF_8));
                        System.out.println("Value as UTF-16: " + new String(codeBytes, StandardCharsets.UTF_16));
                        System.out.println("Value as ISO-8859-1: " + new String(codeBytes, StandardCharsets.ISO_8859_1));
                    } catch (Exception e) {
                        System.out.println("Error decoding text: " + e.getMessage());
                    }
                }
            });
            
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Step 3: Checking Quality and Confidence of Recognition Results

When reading barcodes in production environments, it's important to assess the quality and confidence of the recognition results. This helps determine if the decoded data is reliable.

### Understanding Confidence Levels

Aspose.BarCode for Cloud assigns confidence levels to recognition results:

| Confidence Level | Quality Value | Description |
|------------------|---------------|-------------|
| NONE | 0 | Barcode might be invalid, and information may contain errors |
| MODERATE | 80 | Applies to linear and postal barcode types with weak/absent checksum; further analysis needed |
| STRONG | 100 | Applied to 2D symbologies with Reed-Solomon error correction; high accuracy |

### Using SDK (Python Example)

```python
# Tutorial Code Example: Checking quality and confidence of recognition results
import aspose_barcode_cloud
from aspose_barcode_cloud.apis.barcode_api import BarcodeApi
from aspose_barcode_cloud.api_client import ApiClient
from aspose_barcode_cloud.configuration import Configuration

# Configure API credentials
configuration = Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API client
api_client = ApiClient(configuration)
barcode_api = BarcodeApi(api_client)

# URL of the barcode image
url = "http://example.com/sample-barcode.png"

try:
    # Perform recognition
    response = barcode_api.post_barcode_recognize_from_url_or_content(
        url=url,
        type="ALL_SUPPORTED_TYPES",  # Check for all barcode types
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
    
    print(f"Recognition Status: {response.status}")
    print(f"Total barcodes found: {len(response.barcodes)}")
    
    for i, barcode in enumerate(response.barcodes, 1):
        print(f"\nBarcode {i}:")
        print(f"  Value: {barcode.barcode_value}")
        print(f"  Type: {barcode.barcode_type}")
        
        # Assess quality based on barcode type
        quality_level = "Unknown"
        confidence = "Unknown"
        
        # For 2D barcodes (QR, DataMatrix, etc.)
        if barcode.barcode_type in ["QR", "DataMatrix", "Pdf417", "MacroPdf417", "AztecCode"]:
            quality_level = "STRONG"
            confidence = 100
            print(f"  Confidence: {quality_level} ({confidence})")
            print("  Interpretation: High accuracy with Reed-Solomon error correction")
        
        # For 1D barcodes with strong checksum (EAN13, UPC, etc.)
        elif barcode.barcode_type in ["EAN13", "EAN8", "UPCA", "UPCE", "ISBN", "ISSN"]:
            quality_level = "MODERATE" if barcode.checksum else "NONE"
            confidence = 80 if barcode.checksum else 0
            print(f"  Confidence: {quality_level} ({confidence})")
            print("  Interpretation: Moderate accuracy with checksum validation")
        
        # For 1D barcodes with optional or no checksum
        else:
            quality_level = "MODERATE"
            confidence = 80
            print(f"  Confidence: {quality_level} ({confidence})")
            print("  Interpretation: Accuracy depends on image quality and barcode condition")
        
        # Check for checksum information
        if barcode.checksum:
            print(f"  Checksum: {barcode.checksum}")
            
except Exception as e:
    print(f"Error: {str(e)}")
```

## Try It Yourself!

Create or find barcodes of different types (1D, 2D, postal) and test their recognition quality using the code example above. Try modifying the images to reduce their quality (blur, resize, etc.) and observe how it affects the confidence levels.

## Step 4: Determining Barcode Position and Orientation

For applications that need to process barcode images further, it's often important to know the exact position and orientation of the detected barcode.

### Using SDK (PHP Example)

```php
<?php
// Tutorial Code Example: Determining barcode position and orientation

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
    // URL of the barcode image
    $url = 'http://example.com/barcode-image.png';
    $barcodeType = 'ALL_SUPPORTED_TYPES';
    
    // Perform recognition
    $response = $barcodeApi->postBarcodeRecognizeFromUrlOrContent(
        $url,
        $barcodeType,
        'Default', // checksumValidation
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
    
    foreach ($response->getBarcodes() as $index => $barcode) {
        echo "\nBarcode " . ($index + 1) . ":\n";
        echo "  Value: " . $barcode->getBarcodeValue() . "\n";
        echo "  Type: " . $barcode->getBarcodeType() . "\n";
        
        // Get barcode region (position)
        if ($barcode->getRegion() !== null) {
            $region = $barcode->getRegion();
            echo "  Region coordinates:\n";
            
            // Region is an array of points that form a quadrangle
            foreach ($region as $i => $point) {
                echo "    Point " . ($i + 1) . ": " . $point . "\n";
            }
            
            // Calculate bounding rectangle
            $xPoints = array();
            $yPoints = array();
            
            foreach ($region as $point) {
                list($x, $y) = explode(', ', $point);
                $xPoints[] = (int)$x;
                $yPoints[] = (int)$y;
            }
            
            $minX = min($xPoints);
            $maxX = max($xPoints);
            $minY = min($yPoints);
            $maxY = max($yPoints);
            
            $width = $maxX - $minX;
            $height = $maxY - $minY;
            
            echo "  Bounding Rectangle:\n";
            echo "    Top-Left: (" . $minX . ", " . $minY . ")\n";
            echo "    Width: " . $width . " pixels\n";
            echo "    Height: " . $height . " pixels\n";
            
            // Calculate orientation angle (simplified approximation)
            // This is a basic approximation assuming the first two points 
            // form the top edge of the barcode
            if (count($region) >= 2) {
                list($x1, $y1) = explode(', ', $region[0]);
                list($x2, $y2) = explode(', ', $region[1]);
                
                $deltaX = (int)$x2 - (int)$x1;
                $deltaY = (int)$y2 - (int)$y1;
                
                $angleRadians = atan2($deltaY, $deltaX);
                $angleDegrees = $angleRadians * 180 / M_PI;
                
                echo "  Orientation Angle (approximate): " . round($angleDegrees, 2) . " degrees\n";
            }
        }
    }
} catch (Exception $e) {
    echo "Error: " . $e->getMessage() . "\n";
}
```

## Step 5: Extracting Metadata from Specialized Barcode Types

Different barcode types can contain various metadata beyond the main barcode value. Let's explore how to extract metadata from specific barcode types.

### A. Extracting Metadata from PDF417 and Macro PDF417 Barcodes

PDF417 barcodes can contain rich metadata, especially in their macro variant which can split large data across multiple barcodes.

### Using SDK (Node.js Example)

```javascript
// Tutorial Code Example: Extracting metadata from PDF417 barcodes
const fs = require('fs');
const { Configuration, BarcodeApi } = require('aspose-barcode-cloud');

// Configure API credentials
const config = new Configuration({
    clientId: "YOUR_CLIENT_ID",
    clientSecret: "YOUR_CLIENT_SECRET"
});

// Create API client
const barcodeApi = new BarcodeApi(config);

// Function to extract PDF417 metadata
async function extractPDF417Metadata(filePath) {
    try {
        // Read file as buffer
        const fileBuffer = fs.readFileSync(filePath);
        
        // Perform recognition specifying PDF417 type
        const response = await barcodeApi.postBarcodeRecognizeFromUrlOrContent(
            null, // url
            "PDF417", // type - specifically targeting PDF417
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
        
        console.log(`Recognition Status: ${response.status}`);
        console.log(`Total PDF417 barcodes found: ${response.barcodes.length}\n`);
        
        response.barcodes.forEach((barcode, index) => {
            console.log(`PDF417 Barcode ${index + 1}:`);
            console.log(`  Value: ${barcode.barcodeValue}`);
            
            // Check if it's a MacroPDF417 barcode
            let isMacro = false;
            if (barcode.barcodeValue && 
                (barcode.barcodeValue.includes("SEGMENTINDEX:") || 
                 barcode.barcodeValue.includes("FILENAME:") ||
                 barcode.barcodeValue.includes("SEGMENTS:"))) {
                isMacro = true;
            }
            
            if (isMacro) {
                console.log("  Type: MacroPDF417");
                
                // Extract MacroPDF417 metadata from the barcode value
                // This is a simplified approach - a more robust parser should be used in production
                const lines = barcode.barcodeValue.split('\n');
                
                let fileID = null;
                let segmentIndex = null;
                let segmentsCount = null;
                let fileName = null;
                let fileSize = null;
                let timestamp = null;
                let checksum = null;
                let sender = null;
                let addressee = null;
                
                lines.forEach(line => {
                    if (line.startsWith("FILEID:")) fileID = line.substring(7).trim();
                    if (line.startsWith("SEGMENTINDEX:")) segmentIndex = line.substring(13).trim();
                    if (line.startsWith("SEGMENTS:")) segmentsCount = line.substring(9).trim();
                    if (line.startsWith("FILENAME:")) fileName = line.substring(9).trim();
                    if (line.startsWith("FILESIZE:")) fileSize = line.substring(9).trim();
                    if (line.startsWith("TIMESTAMP:")) timestamp = line.substring(10).trim();
                    if (line.startsWith("CHECKSUM:")) checksum = line.substring(9).trim();
                    if (line.startsWith("SENDER:")) sender = line.substring(7).trim();
                    if (line.startsWith("ADDRESSEE:")) addressee = line.substring(10).trim();
                });
                
                console.log("  MacroPDF417 Metadata:");
                if (fileID) console.log(`    File ID: ${fileID}`);
                if (segmentIndex) console.log(`    Segment Index: ${segmentIndex}`);
                if (segmentsCount) console.log(`    Segments Count: ${segmentsCount}`);
                if (fileName) console.log(`    File Name: ${fileName}`);
                if (fileSize) console.log(`    File Size: ${fileSize}`);
                if (timestamp) console.log(`    Timestamp: ${timestamp}`);
                if (checksum) console.log(`    Checksum: ${checksum}`);
                if (sender) console.log(`    Sender: ${sender}`);
                if (addressee) console.log(`    Addressee: ${addressee}`);
            } else {
                console.log("  Type: Standard PDF417");
            }
            
            // Extract region information
            if (barcode.region) {
                console.log("  Region: " + barcode.region.join(', '));
            }
        });
        
        return response;
    } catch (error) {
        console.error("Error extracting PDF417 metadata:", error);
        throw error;
    }
}

// Example usage
async function main() {
    try {
        await extractPDF417Metadata("./pdf417-sample.png");
    } catch (error) {
        console.error("Error in main function:", error);
    }
}

// Run the example
main();
```

### B. Extracting Metadata from QR Codes with Structured Append

QR codes can use a structured append feature to split large data across multiple QR codes.

### Using SDK (Python Example)

```python
# Tutorial Code Example: Extracting metadata from QR codes with structured append
import aspose_barcode_cloud
from aspose_barcode_cloud.apis.barcode_api import BarcodeApi
from aspose_barcode_cloud.api_client import ApiClient
from aspose_barcode_cloud.configuration import Configuration

# Configure API credentials
configuration = Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API client
api_client = ApiClient(configuration)
barcode_api = BarcodeApi(api_client)

def extract_qr_structured_append_metadata(file_path):
    """Extract metadata from QR codes with structured append"""
    
    try:
        # Read file bytes
        with open(file_path, 'rb') as file:
            file_bytes = file.read()
        
        # Perform recognition specifying QR type
        response = barcode_api.post_barcode_recognize_from_url_or_content(
            url=None,
            type="QR",  # Specifically targeting QR codes
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
            allow_qr_micro_qr=True,  # Enable detection of Micro QR codes
            allow_one_d_wiped_bars=None,
            allow_one_d_fast_barcodes=None,
            australian_post_encoding_table=None,
            ignore_ending_filling_patterns_for_c_table=None,
            file=file_bytes
        )
        
        print(f"Recognition Status: {response.status}")
        print(f"Total QR codes found: {len(response.barcodes)}\n")
        
        # Process each QR code
        for i, barcode in enumerate(response.barcodes, 1):
            print(f"QR Code {i}:")
            print(f"  Value: {barcode.barcode_value}")
            
            # Look for structured append indicators in the barcode value
            is_structured_append = False
            
            # Check if the barcode value follows a specific pattern
            # This is a simplified approach - you'd need a more robust parser for production
            value_parts = barcode.barcode_value.split('\n')
            structured_append_data = {}
            
            for part in value_parts:
                if part.startswith("SA_PARITY:"):
                    structured_append_data["parity"] = part.replace("SA_PARITY:", "").strip()
                    is_structured_append = True
                elif part.startswith("SA_INDEX:"):
                    structured_append_data["index"] = part.replace("SA_INDEX:", "").strip()
                    is_structured_append = True
                elif part.startswith("SA_TOTAL:"):
                    structured_append_data["total"] = part.replace("SA_TOTAL:", "").strip()
                    is_structured_append = True
            
            if is_structured_append:
                print("  Type: QR Code with Structured Append")
                print("  Structured Append Metadata:")
                
                if "index" in structured_append_data:
                    print(f"    Index: {structured_append_data['index']}")
                if "total" in structured_append_data:
                    print(f"    Total Codes: {structured_append_data['total']}")
                if "parity" in structured_append_data:
                    print(f"    Parity Data: {structured_append_data['parity']}")
            else:
                print("  Type: Standard QR Code")
            
            # Extract region information
            if barcode.region:
                print("  Region: " + ', '.join(barcode.region))
            
            print()  # Empty line for spacing
        
        return response
    
    except Exception as e:
        print(f"Error extracting QR code metadata: {str(e)}")
        raise e

# Example usage
if __name__ == "__main__":
    try:
        extract_qr_structured_append_metadata("./qr-structured-append.png")
    except Exception as e:
        print(f"Error in main function: {str(e)}")
```

### C. Reading Metadata from 1D Barcodes

Some 1D barcode types can separate the main data from checksum information.

### Using SDK (Java Example)

```java
// Tutorial Code Example: Reading metadata from 1D barcodes
import com.aspose.barcode.cloud.api.BarcodeApi;
import com.aspose.barcode.cloud.client.ApiClient;
import com.aspose.barcode.cloud.model.BarcodeResponseList;
import java.io.File;
import java.io.IOException;
import java.nio.file.Files;

public class OneDBarcodeSample {

    public static void main(String[] args) {
        try {
            // Configure API credentials
            ApiClient apiClient = new ApiClient("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            BarcodeApi barcodeApi = new BarcodeApi(apiClient);
            
            // Specify parameters
            String localFilePath = "/path/to/ean13-barcode.png";
            
            // Read image file bytes
            File file = new File(localFilePath);
            byte[] fileBytes = Files.readAllBytes(file.toPath());
            
            // Perform recognition specifying EAN13 type
            BarcodeResponseList response = barcodeApi.postBarcodeRecognizeFromUrlOrContent(
                null, // url
                "EAN13", // Specifically target EAN13 barcodes
                "On", // checksumValidation - enable to extract checksum information
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
                System.out.println("\nEAN13 Barcode:");
                System.out.println("  Value: " + barcode.getBarcodeValue());
                System.out.println("  Type: " + barcode.getBarcodeType());
                
                // Extract checksum information
                if (barcode.getChecksum() != null && !barcode.getChecksum().isEmpty()) {
                    String valueWithoutChecksum = barcode.getBarcodeValue().substring(0, barcode.getBarcodeValue().length() - 1);
                    String checksumDigit = barcode.getChecksum();
                    
                    System.out.println("  EAN13 Metadata:");
                    System.out.println("    Value Without Checksum: " + valueWithoutChecksum);
                    System.out.println("    Checksum Digit: " + checksumDigit);
                    
                    // Parse EAN-13 structure
                    if (barcode.getBarcodeValue().length() == 13) {
                        String gS1Prefix = barcode.getBarcodeValue().substring(0, 3);
                        String manufacturerCode = barcode.getBarcodeValue().substring(3, 7);
                        String productCode = barcode.getBarcodeValue().substring(7, 12);
                        
                        System.out.println("    GS1 Prefix: " + gS1Prefix);
                        System.out.println("    Manufacturer Code: " + manufacturerCode);
                        System.out.println("    Product Code: " + productCode);
                    }
                }
                
                // Extract region information
                if (barcode.getRegion() != null) {
                    System.out.println("  Region: " + String.join(", ", barcode.getRegion()));
                }
            });
            
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### D. Getting Raw Data from Code 128 Barcodes

Code 128 can encode data in three different ways: A, B, or C. Let's explore how to access this information.

### Using SDK (C# Example)

```csharp
// Tutorial Code Example: Getting raw data from Code 128 barcodes
using Aspose.BarCode.Cloud.Sdk.Api;
using Aspose.BarCode.Cloud.Sdk.Client;
using Aspose.BarCode.Cloud.Sdk.Model;
using System;
using System.IO;
using System.Text;

// Configure API credentials
var config = new Configuration
{
    ClientId = "YOUR_CLIENT_ID",
    ClientSecret = "YOUR_CLIENT_SECRET"
};

// Create API client
var apiClient = new ApiClient(config);
var barcodeApi = new BarcodeApi(apiClient);

try
{
    // Specify local file path
    string localFilePath = @"C:\path\to\code128-barcode.png";
    byte[] fileBytes = File.ReadAllBytes(localFilePath);
    
    // Perform recognition
    var response = barcodeApi.PostBarcodeRecognizeFromUrlOrContent(
        file: fileBytes,
        type: "Code128",
        checksumValidation: "On",
        preset: null,
        rectX: null,
        rectY: null,
        rectWidth: null,
        rectHeight: null,
        stripFnc: false, // Keep FNC symbols to analyze raw data
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

    Console.WriteLine($"Recognition Status: {response.Status}");
    Console.WriteLine($"Number of barcodes found: {response.Barcodes.Count}");

    foreach (var barcode in response.Barcodes)
    {
        Console.WriteLine($"\nCode 128 Barcode:");
        Console.WriteLine($"  Value: {barcode.BarcodeValue}");
        Console.WriteLine($"  Type: {barcode.BarcodeType}");
        
        if (barcode.BarcodeValue != null)
        {
            // Analyze Code 128 encoding mode based on content
            AnalyzeCode128Encoding(barcode.BarcodeValue);
        }
        
        // Extract region information
        if (barcode.Region != null)
        {
            Console.WriteLine($"  Region: {string.Join(", ", barcode.Region)}");
        }
    }
}
catch (Exception ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}

// Helper method to analyze Code 128 encoding mode
void AnalyzeCode128Encoding(string value)
{
    Console.WriteLine("  Code 128 Analysis:");
    
    // Check for FNC characters (indicators of different encoding modes)
    bool hasFNC1 = value.Contains("<FNC1>");
    bool hasFNC2 = value.Contains("<FNC2>");
    bool hasFNC3 = value.Contains("<FNC3>");
    bool hasFNC4 = value.Contains("<FNC4>");
    
    Console.WriteLine($"    Has FNC1: {hasFNC1}");
    Console.WriteLine($"    Has FNC2: {hasFNC2}");
    Console.WriteLine($"    Has FNC3: {hasFNC3}");
    Console.WriteLine($"    Has FNC4: {hasFNC4}");
    
    // Try to determine encoding mode based on content
    bool hasNonPrintableAscii = false;
    bool hasLowerCase = false;
    bool hasDigitsOnly = true;
    
    string valueWithoutFNC = value
        .Replace("<FNC1>", "")
        .Replace("<FNC2>", "")
        .Replace("<FNC3>", "")
        .Replace("<FNC4>", "");
    
    foreach (char c in valueWithoutFNC)
    {
        if (c < 32 || c > 126) hasNonPrintableAscii = true;
        if (c >= 'a' && c <= 'z') hasLowerCase = true;
        if (c < '0' || c > '9') hasDigitsOnly = false;
    }
    
    // Determine likely encoding mode
    string likelyMode = "Unknown";
    if (hasNonPrintableAscii && !hasLowerCase)
    {
        likelyMode = "Code 128A (control chars, upper case, digits)";
    }
    else if (hasLowerCase)
    {
        likelyMode = "Code 128B (all ASCII printable chars)";
    }
    else if (hasDigitsOnly && valueWithoutFNC.Length % 2 == 0)
    {
        likelyMode = "Code 128C (digits in pairs)";
    }
    else if (!hasNonPrintableAscii && !hasLowerCase)
    {
        likelyMode = "Code 128A or mixed modes";
    }
    
    Console.WriteLine($"    Likely Encoding Mode: {likelyMode}");
    
    // If FNC1 is in first position, this might be a GS1-128 barcode
    if (value.StartsWith("<FNC1>"))
    {
        Console.WriteLine("    Special Type: GS1-128 (starts with FNC1)");
        
        // Try to parse GS1 Application Identifiers
        string gs1Data = value.Substring(6); // Skip <FNC1>
        Console.WriteLine("    GS1 Application Identifiers:");
        
        int position = 0;
        while (position < gs1Data.Length)
        {
            // Application Identifiers are typically 2-4 digits
            if (position + 2 <= gs1Data.Length && Char.IsDigit(gs1Data[position]) && Char.IsDigit(gs1Data[position + 1]))
            {
                string ai = gs1Data.Substring(position, 2);
                position += 2;
                
                // Some AIs are 3 or 4 digits long
                if (position < gs1Data.Length && Char.IsDigit(gs1Data[position]))
                {
                    ai += gs1Data[position];
                    position++;
                    
                    if (position < gs1Data.Length && Char.IsDigit(gs1Data[position]))
                    {
                        ai += gs1Data[position];
                        position++;
                    }
                }
                
                // Find the end of the data for this AI
                int dataEnd = gs1Data.IndexOf("<FNC1>", position);
                if (dataEnd == -1) dataEnd = gs1Data.Length;
                
                string data = gs1Data.Substring(position, dataEnd - position);
                position = dataEnd + 6; // Skip past the FNC1
                
                Console.WriteLine($"      AI {ai}: {data}");
            }
            else
            {
                // Not a valid GS1 format, exit the loop
                break;
            }
        }
    }
}
```

## Try It Yourself!

Create or obtain barcodes of different specialized types (PDF417, QR with structured append, EAN-13, Code 128) and use the code examples to extract their metadata. Pay special attention to how the different properties and metadata fields vary between barcode types.

## Barcode Properties Comparison Table

| Barcode Type | Data Capacity | Metadata Available | Best Use Case |
|--------------|---------------|-------------------|---------------|
| 1D (EAN, UPC, etc.) | Low (20-40 chars) | Checksum, position | Retail, inventory |
| QR Code | High (4000+ chars) | Position, orientation, structured append | URLs, contact info, general purpose |
| PDF417 | High (1000+ chars) | Macro PDF417 fields, error correction | Government IDs, transportation |
| DataBar | Medium (up to 74 chars) | GS1 Application Identifiers | Fresh food, coupons |
| Code 128 | Medium (up to 100 chars) | FNC symbols, encoding mode | Logistics, packaging |

## Troubleshooting Common Issues

### Issue: Metadata Not Recognized

If specific metadata isn't recognized:
- Ensure you're using the correct barcode type for recognition
- Verify that the barcode actually contains the expected metadata
- Make sure the image quality is sufficient for detailed recognition
- Try enabling/disabling specific recognition options related to the metadata

### Issue: Incorrect Position Information

If barcode position information is incorrect:
- Ensure the image isn't rotated or skewed
- Try using a higher resolution image
- Check if the barcode has sufficient quiet zones (margins)
- Verify that the entire barcode is visible in the image

### Issue: Unicode Character Issues

If Unicode characters aren't decoded correctly:
- Ensure Unicode detection is enabled
- Try different character encodings when processing the result
- Verify that the barcode was properly encoded with Unicode characters
- Check if the barcode type supports the specific Unicode characters

## Further Practice

To reinforce what you've learned, try these exercises:
1. Create a program that extracts and displays all available metadata from different barcode types
2. Build a solution that processes a batch of barcodes and generates a report of their properties
3. Develop a QR code analyzer that extracts and processes structured append information
4. Create a Code 128 decoder that displays the different encoding segments (A, B, or C)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/barcode/)
- [Documentation](https://docs.aspose.cloud/barcode/)
- [Live Demo](https://products.aspose.app/barcode/family)
- [API Reference](https://reference.aspose.cloud/barcode/)
- [Blog](https://blog.aspose.cloud/category/barcode/)
- [Free Support](https://forum.aspose.cloud/c/barcode/6/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

## Feedback

Did you find this tutorial helpful? Do you have questions about reading barcode properties and metadata? We'd love to hear from you! Please leave your questions and feedback in our [support forum](https://forum.aspose.cloud/c/barcode/6/).