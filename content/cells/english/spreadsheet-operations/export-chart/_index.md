---
title: Exporting Excel Charts to Different Formats with Aspose.Cells Cloud API Tutorial
second_title: Complete Developer Guide

url: /spreadsheet-operations/export-chart/
description: Learn how to export Excel charts to various image formats using Aspose.Cells Cloud REST API in this step-by-step tutorial with code examples.
weight: 10
keywords: Excel chart export, Aspose.Cells tutorial, REST API Excel, export chart to image, data visualization
---

# Tutorial: Exporting Excel Charts to Different Formats with Aspose.Cells Cloud API

## Prerequisites

Before starting this tutorial, make sure you have:
1. An Aspose Cloud account with an active subscription or free trial
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
3. An Excel file containing charts for testing
4. Basic understanding of REST APIs and your preferred programming language

## Introduction to Excel Chart Export

Excel charts are powerful visual representations of data that can be useful outside of Excel itself. You might want to extract charts to:
- Include them in web pages or applications
- Create presentations or reports
- Use them in marketing materials
- Share visualizations without sharing the entire spreadsheet

The Aspose.Cells Cloud API makes it easy to extract charts in various formats including PNG, JPEG, SVG, TIFF, and more.

## Understanding Chart Export Options

When exporting charts from Excel files, you have several options to consider:

### Supported Output Formats

Aspose.Cells Cloud supports exporting charts to multiple formats:

- Raster formats: PNG, JPEG, GIF, BMP, TIFF
- Vector formats: SVG, EMF, WMF
- Document formats: PDF

Each format has its own advantages:
- PNG: Good for web use with transparency support
- JPEG: Smaller file size, best for photographs
- SVG: Vector format ideal for responsive web design
- PDF: Perfect for document inclusion with high quality
- TIFF: High quality, supports multiple pages

### API Endpoints

The primary endpoint for exporting Excel charts is:

```
POST /cells/export
```

This endpoint accepts multipart/form-data content with:
- The Excel file(s) to process
- Configuration parameters

## Exporting Charts to TIFF Format

Let's start with a complete example showing how to export all charts from an Excel file to TIFF format.

### Try It Yourself: Using cURL

```bash
curl -X POST "https://api.aspose.cloud/v3.0/cells/export?objectType=chart&format=tiff" \
-H "accept: multipart/form-data" \
-H "Content-Type: multipart/form-data" \
-H "Authorization: Bearer <your_jwt_token>" \
-F "file=@YourExcelFile.xlsx"
```

Replace `<your_jwt_token>` with your actual authentication token. The response will contain all charts from your Excel file as TIFF images.

### Understanding the Response

The API response contains a JSON array of file objects, each with:
- Filename: Automatically generated name including the worksheet and chart index
- FileSize: Size in bytes
- FileContent: Base64-encoded image data

Example response structure:

```json
{
    "Files": [
        {
            "Filename": "Book1_xlsx_Sheet4_Charts_0.tif",
            "FileSize": 10040,
            "FileContent": "base64-encoded-data"
        },
        {
            "Filename": "Book1_xlsx_Sheet4_Charts_1.tif",
            "FileSize": 12978,
            "FileContent": "base64-encoded-data"
        }
    ]
}
```

### Implementation in C#

```csharp
// Initialize the API with your client credentials
CellsApi cellsApi = new CellsApi(clientId, clientSecret);

// Prepare the file to upload
var fileStream = File.OpenRead("Book1.xlsx");
var files = new Dictionary<string, Stream> { { "Book1.xlsx", fileStream } };

// Specify export options
string objectType = "chart";
string format = "tiff";

// Execute the export
var response = cellsApi.PostExport(
    files,
    objectType,
    format
);

// Process the response
Console.WriteLine($"Successfully exported {response.Files.Count} charts:");

// Save each chart to a file
foreach (var file in response.Files)
{
    // Decode the base64 content
    byte[] imageBytes = Convert.FromBase64String(file.FileContent);
    
    // Save to disk
    string outputPath = file.Filename;
    File.WriteAllBytes(outputPath, imageBytes);
    
    Console.WriteLine($"  - Saved {file.Filename} ({file.FileSize} bytes)");
}
```

### Implementation in PHP

```php
<?php
require_once(__DIR__ . '/vendor/autoload.php');

// Configure API key authorization
$config = new \Aspose\Cells\Configuration();
$config->setAppSid('your_client_id');
$config->setAppKey('your_client_secret');

$apiInstance = new \Aspose\Cells\Api\CellsApi(
    new GuzzleHttp\Client(),
    $config
);

// Read the file
$fileContents = file_get_contents("Book1.xlsx");

// Prepare the request
$files = array(
    "Book1.xlsx" => $fileContents
);

// Specify export options
$objectType = "chart";
$format = "tiff";

try {
    // Execute the export
    $result = $apiInstance->postExport($files, $objectType, $format);
    
    // Process the response
    echo "Successfully exported " . count($result->getFiles()) . " charts:\n";
    
    // Save each chart to a file
    foreach ($result->getFiles() as $file) {
        // Decode the base64 content
        $imageData = base64_decode($file->getFileContent());
        
        // Save to disk
        file_put_contents($file->getFilename(), $imageData);
        
        echo "  - Saved " . $file->getFilename() . " (" . $file->getFileSize() . " bytes)\n";
    }
} catch (Exception $e) {
    echo "Exception when calling CellsApi->postExport: " . $e->getMessage() . "\n";
}
?>
```

## Exporting Charts to SVG Format

SVG is a vector format that scales perfectly for any screen size, making it ideal for web applications.

### Try It Yourself: Using cURL

```bash
curl -X POST "https://api.aspose.cloud/v3.0/cells/export?objectType=chart&format=svg" \
-H "accept: multipart/form-data" \
-H "Content-Type: multipart/form-data" \
-H "Authorization: Bearer <your_jwt_token>" \
-F "file=@YourExcelFile.xlsx"
```

### Implementation in C#

```csharp
// Similar to the TIFF example, but change the format
string format = "svg";

// Execute the export
var response = cellsApi.PostExport(
    files,
    objectType,
    format
);

// Process the response
foreach (var file in response.Files)
{
    // For SVG, you can also save as text since it's XML-based
    string svgContent = Encoding.UTF8.GetString(
        Convert.FromBase64String(file.FileContent)
    );
    
    // Save to disk
    string outputPath = file.Filename;
    File.WriteAllText(outputPath, svgContent);
    
    Console.WriteLine($"  - Saved {file.Filename} ({file.FileSize} bytes)");
}
```

## Exporting Specific Charts

If you have multiple charts in your Excel file but only want to export specific ones, you need to:

1. First extract all charts using the API
2. Then filter the results based on worksheet name or chart index

### Example: Filtering Charts by Worksheet Name

```csharp
// Execute the export
var response = cellsApi.PostExport(
    files,
    objectType,
    format
);

// Filter charts from a specific worksheet
string worksheetName = "Sheet4";
var filteredCharts = response.Files
    .Where(file => file.Filename.Contains(worksheetName))
    .ToList();

Console.WriteLine($"Found {filteredCharts.Count} charts in worksheet '{worksheetName}'");

// Process only those charts
foreach (var file in filteredCharts)
{
    // Process as before
    byte[] imageBytes = Convert.FromBase64String(file.FileContent);
    File.WriteAllBytes(file.Filename, imageBytes);
}
```

## Processing Chart Images

After exporting, you may want to process the chart images before using them:

### Example: Resizing Images in C#

```csharp
// Requires System.Drawing or another image processing library
using System.Drawing;
using System.Drawing.Imaging;

// ... Export code from earlier examples ...

foreach (var file in response.Files)
{
    // Decode the base64 content
    byte[] imageBytes = Convert.FromBase64String(file.FileContent);
    
    // Load into an image
    using (var memoryStream = new MemoryStream(imageBytes))
    using (var originalImage = Image.FromStream(memoryStream))
    {
        // Resize to 50% of original
        int newWidth = originalImage.Width / 2;
        int newHeight = originalImage.Height / 2;
        
        using (var resizedImage = new Bitmap(newWidth, newHeight))
        using (var graphics = Graphics.FromImage(resizedImage))
        {
            graphics.InterpolationMode = System.Drawing.Drawing2D.InterpolationMode.HighQualityBicubic;
            graphics.DrawImage(originalImage, 0, 0, newWidth, newHeight);
            
            // Save resized image
            string outputPath = "resized_" + file.Filename;
            resizedImage.Save(outputPath, ImageFormat.Tiff);
            
            Console.WriteLine($"  - Saved resized image: {outputPath}");
        }
    }
}
```

## Programmatically Using Exported Charts

Once you've exported charts, here are some common ways to use them:

### Example: Embedding in an HTML Page

```csharp
// After exporting charts to SVG
foreach (var file in response.Files)
{
    // Decode SVG content
    string svgContent = Encoding.UTF8.GetString(
        Convert.FromBase64String(file.FileContent)
    );
    
    // Create an HTML file that embeds this SVG
    string htmlContent = $@"
        <!DOCTYPE html>
        <html>
        <head>
            <title>Excel Chart: {file.Filename}</title>
        </head>
        <body>
            <h1>Chart from Excel</h1>
            <div class='chart-container'>
                {svgContent}
            </div>
        </body>
        </html>
    ";
    
    // Save HTML file
    string htmlFilename = Path.ChangeExtension(file.Filename, "html");
    File.WriteAllText(htmlFilename, htmlContent);
    
    Console.WriteLine($"  - Created HTML page with embedded chart: {htmlFilename}");
}
```

## Troubleshooting Common Issues

### Issue: Empty Response or No Charts
Solution: Verify your Excel file actually contains charts. Try opening it in Excel and checking if charts are present and visible.

### Issue: Poor Image Quality
Solution: For raster formats like PNG or JPEG, try using higher resolution or switch to vector formats like SVG for scalable quality.

### Issue: Authentication Errors
Solution: Double check your Client ID, Client Secret, and JWT token. Ensure your token hasn't expired.

## What You've Learned

In this tutorial, you've learned:
- How to export Excel charts to various image formats using Aspose.Cells Cloud API
- How to implement chart export in C# and PHP
- How to filter and process specific charts from Excel files
- How to resize and use the exported charts in applications
- Common troubleshooting techniques for chart export

## Further Practice

To reinforce your learning, try these exercises:
1. Export charts to different formats (PNG, JPEG, PDF) and compare the results
2. Create a web application that allows users to upload Excel files and view extracted charts
3. Build a workflow that automatically extracts charts from Excel files and sends them via email

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cells/)
- [Documentation](https://docs.aspose.cloud/cells/)
- [API Reference](https://reference.aspose.cloud/cells/)
- [Free Support Forum](https://forum.aspose.cloud/c/cells/7)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/cells/7)
