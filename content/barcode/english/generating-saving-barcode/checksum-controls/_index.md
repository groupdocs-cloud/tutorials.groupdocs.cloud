---
title: How to Add Checksum Controls to 1D Barcodes Tutorial
url: /generating-saving-barcode/checksum-controls/
weight: 30
description: Learn how to implement and configure checksum controls for error detection in 1D barcodes using Aspose.BarCode Cloud API in this step-by-step tutorial
---

# Tutorial: How to Add Checksum Controls to 1D Barcodes

## Learning Objectives

In this tutorial, you'll learn how to:
- Understand what checksum digits are and how they improve barcode reliability
- Implement checksum controls for 1D barcodes with optional checksum
- Work with barcode types that have mandatory checksums
- Configure checksum visibility options in human-readable text
- Test and verify checksum functionality

## Prerequisites

Before starting this tutorial, make sure you have:
- Completed our [Basic Barcode Generation Tutorial](/generating-saving-barcode/basic-generation/)
- An [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps) with Client ID and Client Secret
- Understanding of basic barcode concepts
- Postman, cURL, or your preferred API testing tool
- Development environment for your preferred language (for SDK examples)

## Introduction

Barcodes are widely used for data storage and quick retrieval, but what happens if a barcode is damaged or incorrectly scanned? This is where checksums come in. A checksum (or check digit) is an error detection mechanism used to verify data integrity in 1D barcodes.

In this tutorial, we'll explore how Aspose.BarCode Cloud API allows you to implement and configure checksum controls to improve the reliability of your barcodes.

## Understanding Barcode Checksums

A checksum is typically the last digit in a barcode sequence, calculated using a specific algorithm based on the other digits in the code. When a barcode is scanned, the reader:

1. Calculates a checksum based on the scanned digits
2. Compares this calculated value with the checksum digit in the barcode
3. If they match, the barcode is considered valid; if not, an error is detected

Different barcode types use different checksum algorithms and have different rules:

- Optional Checksums: Some barcode types (like Code 39) can function with or without a checksum
- Mandatory Checksums: Other types (like EAN-13) always require a checksum for valid operation

Let's explore how to work with both types.

## Step 1: Working with Optional Checksum Controls

For barcode types with optional checksums, you can enable or disable the checksum as needed. Let's see how to do this with Code 39, a common barcode type with optional checksum:

### Enabling Checksum for Code 39

```bash
curl -X POST "https://api.aspose.cloud/v3.0/barcode/generate" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{\"TypeOfBarcode\":\"Code39Standard\",\"Text\":\"CODE39-TEST\",\"Format\":\"png\",\"Parameters\":{\"EnableChecksum\":\"Yes\"}}" \
--output code39_with_checksum.png
```

### Disabling Checksum for Code 39

```bash
curl -X POST "https://api.aspose.cloud/v3.0/barcode/generate" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{\"TypeOfBarcode\":\"Code39Standard\",\"Text\":\"CODE39-TEST\",\"Format\":\"png\",\"Parameters\":{\"EnableChecksum\":\"No\"}}" \
--output code39_without_checksum.png
```

### Try it yourself

Generate both barcodes and compare them. The version with checksum enabled will have an additional character (the check digit) calculated based on the input text. This might not be visible to the naked eye, but it affects how the barcode is decoded.

## Step 2: Working with Mandatory Checksum Controls

Some barcode types, like Code 93, EAN-13, and UPC-A, have mandatory checksums. For these types, Aspose.BarCode Cloud automatically calculates and includes the checksum digit. Let's see an example with Code 93:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/barcode/generate" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{\"TypeOfBarcode\":\"Code93\",\"Text\":\"CODE93-TEST\",\"Format\":\"png\"}" \
--output code93_with_checksum.png
```

For these barcode types, the `EnableChecksum` property is set to `Yes` by default and doesn't need to be specified.

## Step 3: Controlling Checksum Visibility

For certain barcode types like Code 128, you can control whether the checksum digit appears in the human-readable text portion of the barcode. This is controlled with the `ChecksumAlwaysShow` parameter:

### Showing Checksum in Human-Readable Text

```bash
curl -X POST "https://api.aspose.cloud/v3.0/barcode/generate" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{\"TypeOfBarcode\":\"Code128\",\"Text\":\"SHOW-CHECKSUM\",\"Format\":\"png\",\"Parameters\":{\"ChecksumAlwaysShow\":true}}" \
--output code128_show_checksum.png
```

### Hiding Checksum in Human-Readable Text

```bash
curl -X POST "https://api.aspose.cloud/v3.0/barcode/generate" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{\"TypeOfBarcode\":\"Code128\",\"Text\":\"HIDE-CHECKSUM\",\"Format\":\"png\",\"Parameters\":{\"ChecksumAlwaysShow\":false}}" \
--output code128_hide_checksum.png
```

### Try it yourself

Generate both versions and compare the human-readable text at the bottom of the barcode. With `ChecksumAlwaysShow` set to true, you'll see the checksum digit in the text.

## Step 4: Barcode Types and Their Checksum Requirements

Let's look at different barcode types and their checksum requirements:

### Optional Checksum Barcode Types:
- Codabar
- Code 39 (Standard and Extended)
- Interleaved 2-of-5
- Standard 2-of-5
- Matrix 2-of-5
- MSI
- Pharmacode
- PZN

### Mandatory Checksum Barcode Types:
- Code 11
- Code 93
- Code 128
- EAN-13
- EAN-8
- UPC-A
- UPC-E
- GS1 DataBar types
- ISBN
- ISSN
- ITF-14

Understanding these requirements helps you properly implement barcode generation for your specific use case.

## SDK Examples

### Python SDK Example

```python
# Tutorial Code Example: Working with checksums in Aspose.BarCode Cloud Python SDK
import aspose_barcode_cloud
from aspose_barcode_cloud.apis.barcode_api import BarcodeApi
from aspose_barcode_cloud.api_client import ApiClient
from aspose_barcode_cloud.configuration import Configuration
from aspose_barcode_cloud.models import GeneratorParams

# Configure authorization
configuration = Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API client
api_client = ApiClient(configuration)

# Create an instance of BarcodeApi
api = BarcodeApi(api_client)

# Example 1: Generate Code 39 with optional checksum enabled
params1 = GeneratorParams(
    enable_checksum="Yes"  # Enable checksum for Code 39
)

response1 = api.get_barcode_generate(
    type="Code39Standard",
    text="CODE39-PYTHON",
    format="png",
    parameters=params1
)

# Save to file
with open("code39_with_checksum.png", "wb") as file:
    file.write(response1)

# Example 2: Generate Code 128 with visible checksum
params2 = GeneratorParams(
    checksum_always_show=True  # Make checksum visible in human-readable text
)

response2 = api.get_barcode_generate(
    type="Code128",
    text="PYTHON-CHECKSUM",
    format="png",
    parameters=params2
)

# Save to file
with open("code128_visible_checksum.png", "wb") as file:
    file.write(response2)

print("Barcodes with checksum controls generated successfully")
```

### C# SDK Example

```csharp
// Tutorial Code Example: Working with checksums in Aspose.BarCode Cloud C# SDK
using System;
using System.IO;
using Aspose.BarCode.Cloud.Sdk.Api;
using Aspose.BarCode.Cloud.Sdk.Client;
using Aspose.BarCode.Cloud.Sdk.Model;

namespace AsposeBarCodeChecksumTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure authorization
            var config = new Configuration
            {
                ClientId = "YOUR_CLIENT_ID",
                ClientSecret = "YOUR_CLIENT_SECRET"
            };

            // Create API client
            var apiClient = new ApiClient(config);
            var api = new BarcodeApi(apiClient);

            try
            {
                // Example 1: Generate Code 39 with optional checksum enabled
                var params1 = new GeneratorParams
                {
                    EnableChecksum = "Yes"  // Enable checksum for Code 39
                };

                var response1 = api.GetBarcodeGenerate(
                    "Code39Standard",
                    "CODE39-CSHARP",
                    format: "png",
                    parameters: params1
                );

                // Save to file
                File.WriteAllBytes("code39_with_checksum.png", response1);

                // Example 2: Generate Code 128 with visible checksum
                var params2 = new GeneratorParams
                {
                    ChecksumAlwaysShow = true  // Make checksum visible in human-readable text
                };

                var response2 = api.GetBarcodeGenerate(
                    "Code128",
                    "CSHARP-CHECKSUM",
                    format: "png",
                    parameters: params2
                );

                // Save to file
                File.WriteAllBytes("code128_visible_checksum.png", response2);

                Console.WriteLine("Barcodes with checksum controls generated successfully");
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error: " + ex.Message);
            }
        }
    }
}
```

## Practical Use Cases

### Scenario 1: Retail Product Barcodes

For retail applications, checksums are crucial for ensuring scanning accuracy at the point of sale:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/barcode/generate" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{\"TypeOfBarcode\":\"EAN13\",\"Text\":\"590123412345\",\"Format\":\"png\"}" \
--output retail_ean13.png
```

Note that for EAN-13, you only need to provide 12 digits - the 13th digit (checksum) is automatically calculated and added by Aspose.BarCode Cloud.

### Scenario 2: Inventory Management

For internal inventory systems where you control both barcode generation and scanning, you might choose a format like Code 39 with optional checksum:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/barcode/generate" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_ACCESS_TOKEN" \
-H "Content-Type: application/json" \
-d "{\"TypeOfBarcode\":\"Code39Standard\",\"Text\":\"INV-2023-45678\",\"Format\":\"png\",\"Parameters\":{\"EnableChecksum\":\"Yes\"}}" \
--output inventory_barcode.png
```

## Troubleshooting

### Common Issues and Solutions

1. Invalid Characters for Checksum Calculation: Some barcode types have restrictions on characters that can be used with checksums. If you encounter errors, check the allowed character set for your barcode type.

2. Incorrect Checksum Length: For barcode types with fixed lengths (like EAN-13), make sure you're providing the correct number of digits. The checksum will be calculated automatically.

3. Scanner Not Verifying Checksum: Some barcode scanners can be configured to ignore checksums. Ensure your scanner settings are properly configured to validate checksums if that's required for your application.

4. Inconsistent Results: If you're getting different scanning results with the same barcode, the checksum might be incorrectly calculated or the scanner might be malfunctioning.

## What You've Learned

In this tutorial, you've learned:
- How barcode checksums work as an error detection mechanism
- How to enable optional checksums for barcode types that support them
- How to work with barcode types that have mandatory checksums
- How to control the visibility of checksum digits in human-readable text
- How to implement checksum controls in different real-world scenarios

## Further Practice

To reinforce your learning:

1. Generate barcodes of different types with and without checksums
2. Test scanning these barcodes and observe how scanning errors are detected
3. Create a simple application that validates checksum correctness
4. Compare scanning reliability between barcodes with and without checksums under various conditions (e.g., partial damage, poor lighting)

## Next Steps

Ready to learn more? Continue your barcode journey with our next tutorial:
[Tutorial: Working with Vector Image Formats](/generating-saving-barcode/vector-formats/) to learn how to create scalable barcode images using SVG and EMF formats.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/barcode/)
- [Documentation](https://docs.aspose.cloud/barcode/)
- [Live Demo](https://products.aspose.app/barcode/family)
- [API Reference](https://reference.aspose.cloud/barcode/)
- [Blog](https://blog.aspose.cloud/category/barcode/)
- [Free Support](https://forum.aspose.cloud/c/barcode/6/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? We'd love to hear from you! Please visit our [support forum](https://forum.aspose.cloud/c/barcode/6/) to share your feedback.