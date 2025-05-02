---
title: Working with Convert Options Tutorial
weight: 30
url: /data-organization/convert-options/
description: Learn how to configure format-specific conversion parameters for precise output control with this hands-on tutorial for GroupDocs.Conversion Cloud API
---

# Tutorial: Working with Convert Options

## Learning Objectives

In this tutorial, you'll learn:
- What ConvertOptions are and their role in controlling conversion output
- How to configure format-specific conversion parameters
- Techniques for optimizing conversion quality for different target formats
- Best practices for advanced conversion scenarios

## Prerequisites

Before starting this tutorial, you should have:
- A [GroupDocs.Conversion Cloud account](https://dashboard.groupdocs.cloud/#/apps)
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with JSON data structures
- Completion of the [LoadOptions tutorial](/data-organization/load-options) (recommended)
- A development environment set up for your preferred language (examples provided in cURL, Python, Java, and C#)

## What are ConvertOptions?

ConvertOptions control how GroupDocs.Conversion Cloud API generates output files during the conversion process. While LoadOptions focus on reading the input document, ConvertOptions determine how the output document is created and formatted.

Different target formats support different conversion options. Using the right ConvertOptions ensures your converted documents meet your specific requirements for quality, appearance, and functionality.

## Why ConvertOptions Matter

Consider these scenarios where ConvertOptions make a significant difference:

1. Image quality control: When converting to image formats, options like width, height, and quality are essential
2. Page selection: Convert only specific pages from multi-page documents
3. PDF optimization: Control PDF version, compression, and security settings
4. Document appearance: Adjust margins, zoom levels, and other visual aspects
5. Watermarking: Add text or image watermarks during conversion

Let's explore how to implement ConvertOptions for different target formats.

## Step 1: Understanding Common ConvertOptions Properties

Some ConvertOptions properties are available across multiple target formats:

### Page Selection

Most formats support selecting specific pages for conversion:

```python
# Tutorial Code Example: Page Selection ConvertOptions
conversion_settings = {
    "Format": "pdf",
    "FilePath": "documents/large_report.docx",
    "ConvertOptions": {
        "FromPage": 5,      # Start conversion from page 5
        "PagesCount": 10,   # Convert only 10 pages
        "Pages": [2, 4, 6]  # OR specify exact pages to convert (takes precedence)
    },
    "OutputPath": "converted/"
}
```

The page selection options work as follows:
- `FromPage`: The starting page number (1-based index)
- `PagesCount`: The number of pages to convert starting from `FromPage`
- `Pages`: An array of specific page numbers to convert (if provided, overrides `FromPage` and `PagesCount`)

### Using PDF as Intermediate Format

Many formats support conversion through an intermediate PDF format:

```python
# Tutorial Code Example: Using PDF as Intermediate Format
conversion_settings = {
    "Format": "docx",
    "FilePath": "presentations/quarterly_update.pptx",
    "ConvertOptions": {
        "UsePdf": True  # Use PDF as intermediate conversion format
    },
    "OutputPath": "converted/"
}
```

Using PDF as an intermediate format can improve conversion quality in some scenarios, particularly when converting between very different format types.

### Watermarking

Most output formats support adding watermarks during conversion:

```python
# Tutorial Code Example: Adding Watermarks
conversion_settings = {
    "Format": "pdf",
    "FilePath": "documents/contract.docx",
    "ConvertOptions": {
        "WatermarkOptions": {
            "Text": "CONFIDENTIAL",
            "Font": "Arial",
            "Color": "red",
            "Width": 500,
            "Height": 100,
            "Top": 400,
            "Left": 200,
            "RotationAngle": 45,
            "Transparency": 0.5,
            "Background": True,  # Place watermark in background
            "Image": None        # Use text watermark (could use image instead)
        }
    },
    "OutputPath": "converted/"
}
```

### Try it yourself

Create ConvertOptions for a PowerPoint file where you want to convert only slides 2-5 and add a "DRAFT" watermark:

```python
# Exercise: Create ConvertOptions with page selection and watermark
# Your code here:
conversion_settings = {
    "Format": "pdf",
    "FilePath": "presentations/product_launch.pptx",
    "ConvertOptions": {
        "FromPage": 2,
        "PagesCount": 4,
        "WatermarkOptions": {
            "Text": "DRAFT",
            "Font": "Calibri",
            "Color": "blue",
            "Width": 400,
            "Height": 80,
            "RotationAngle": 30,
            "Transparency": 0.7
        }
    },
    "OutputPath": "converted/drafts/"
}
```

## Step 2: Format-Specific ConvertOptions

Let's explore ConvertOptions for specific target formats:

### Image Conversion Options (PNG, JPG, TIFF, etc.)

When converting to image formats, you can control dimensions, resolution, and other visual properties:

```python
# Tutorial Code Example: Image ConvertOptions
conversion_settings = {
    "Format": "jpeg",
    "FilePath": "documents/brochure.pdf",
    "ConvertOptions": {
        "Width": 1920,               # Image width in pixels
        "Height": 1080,              # Image height in pixels
        "HorizontalResolution": 96,  # Horizontal DPI
        "VerticalResolution": 96,    # Vertical DPI
        "Quality": 95,               # JPEG quality (1-100)
        "Grayscale": False,          # Convert to grayscale
        "RotateAngle": 0,            # Rotation angle in degrees
        "BackgroundColor": "white"   # Background color
    },
    "OutputPath": "converted/images/"
}
```

Different image formats support additional specific options:

#### TIFF-Specific Options

```python
# Tutorial Code Example: TIFF-Specific ConvertOptions
conversion_settings = {
    "Format": "tiff",
    "FilePath": "documents/manual.pdf",
    "ConvertOptions": {
        "Width": 1240,
        "Height": 1754,
        "HorizontalResolution": 150,
        "VerticalResolution": 150,
        "Compression": "lzw",  # TIFF compression method (lzw, ccitt4, etc.)
        "Grayscale": True
    },
    "OutputPath": "converted/images/"
}
```

#### WebP-Specific Options

```python
# Tutorial Code Example: WebP-Specific ConvertOptions
conversion_settings = {
    "Format": "webp",
    "FilePath": "images/diagram.png",
    "ConvertOptions": {
        "Width": 800,
        "Height": 600,
        "Lossless": True,  # Use lossless compression
        "Grayscale": False
    },
    "OutputPath": "converted/web_images/"
}
```

### PDF Conversion Options

When converting to PDF, you have extensive control over the output document:

```python
# Tutorial Code Example: PDF ConvertOptions
conversion_settings = {
    "Format": "pdf",
    "FilePath": "documents/report.docx",
    "ConvertOptions": {
        "Width": 612,              # Page width in points
        "Height": 792,             # Page height in points
        "Dpi": 300,                # Document DPI
        "Password": "secure-pdf",  # Set password on output PDF
        "MarginTop": 0.5,          # Top margin in inches
        "MarginBottom": 0.5,       # Bottom margin in inches
        "MarginLeft": 0.5,         # Left margin in inches
        "MarginRight": 0.5,        # Right margin in inches
        "PdfFormat": "v1.7",       # PDF format version
        "RemovePdfACompliance": False,  # Keep PDF/A compliance
        "Zoom": 100,               # Zoom level (percentage)
        "Linearize": True,         # Optimize for web viewing
        "LinkDuplicateStreams": True,  # Optimize duplicate content
        "RemoveUnusedObjects": True,   # Remove unused objects
        "RemoveUnusedStreams": True,   # Remove unused streams
        "CompressImages": True,        # Compress images
        "ImageQuality": 95,            # Image quality after compression
        "UnembedFonts": False,         # Keep embedded fonts
        "Grayscale": False,            # Convert to grayscale
    },
    "OutputPath": "converted/pdfs/"
}
```

### Word Processing Conversion Options (DOCX, RTF, etc.)

When converting to Word formats, you can control page setup and other options:

```python
# Tutorial Code Example: Word Processing ConvertOptions
conversion_settings = {
    "Format": "docx",
    "FilePath": "documents/article.pdf",
    "ConvertOptions": {
        "Width": 8.5,    # Page width in inches
        "Height": 11,    # Page height in inches
        "Dpi": 300,      # Document DPI
        "Password": "",  # Set password on output document
        "Zoom": 100,     # Zoom level
        "PdfRecognitionMode": "Textbox",  # Mode for PDF conversion (Textbox or Flow)
        "PageSize": "Letter",  # Page size (Letter, A4, etc.)
        "PageOrientation": "Portrait"  # Page orientation
    },
    "OutputPath": "converted/word/"
}
```

### Spreadsheet Conversion Options (XLSX, CSV, etc.)

When converting to spreadsheet formats, options are more limited but still important:

```python
# Tutorial Code Example: Spreadsheet ConvertOptions
conversion_settings = {
    "Format": "xlsx",
    "FilePath": "documents/data_table.pdf",
    "ConvertOptions": {
        "Password": "",  # Set password on output spreadsheet
        "Zoom": 100      # Zoom level
    },
    "OutputPath": "converted/excel/"
}
```

### Try it yourself

Create ConvertOptions for converting a Word document to a high-quality PDF with specific page dimensions and security settings:

```python
# Exercise: Create PDF ConvertOptions with security settings
# Your code here:
conversion_settings = {
    "Format": "pdf",
    "FilePath": "documents/confidential_report.docx",
    "ConvertOptions": {
        "Width": 8.5,
        "Height": 11,
        "Dpi": 300,
        "Password": "secure-pdf-2023",
        "MarginTop": 0.75,
        "MarginBottom": 0.75,
        "MarginLeft": 1.0,
        "MarginRight": 1.0,
        "PdfFormat": "v1.7",
        "CompressImages": True,
        "ImageQuality": 90,
        "Linearize": True,
        "RemoveUnusedObjects": True
    },
    "OutputPath": "converted/secure_pdfs/"
}
```

## Step 3: Advanced ConvertOptions Scenarios

### Format-Specific Conversion Paths

Some conversions perform better with specific options or intermediate formats:

```python
# Tutorial Code Example: Optimized Conversion Path
conversion_settings = {
    "Format": "pdf",
    "FilePath": "diagrams/flowchart.svg",
    "ConvertOptions": {
        "UsePdf": True,  # Use PDF as intermediate format
        "Width": 1200,
        "Height": 900,
        "Grayscale": False
    },
    "OutputPath": "converted/diagrams/"
}
```

### E-book Conversion Options (EPUB)

When converting to EPUB, you have specific options for e-book formatting:

```python
# Tutorial Code Example: EPUB ConvertOptions
conversion_settings = {
    "Format": "epub",
    "FilePath": "documents/manuscript.docx",
    "ConvertOptions": {
        "PageSize": "A5",
        "PageOrientation": "Portrait",
        "WatermarkOptions": {
            "Text": "Sample",
            "Font": "Times New Roman",
            "Color": "gray",
            "Transparency": 0.7
        }
    },
    "OutputPath": "converted/ebooks/"
}
```

## Implementation Examples

Let's see how to implement ConvertOptions with the API:

### cURL Example

```bash
# Tutorial Code Example: Using ConvertOptions with cURL
curl -X POST "https://api.groupdocs.cloud/v2.0/conversion" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "Format": "jpeg",
    "FilePath": "documents/diagram.pdf",
    "ConvertOptions": {
      "Width": 1920,
      "Height": 1080,
      "Quality": 95,
      "FromPage": 1,
      "PagesCount": 3
    },
    "OutputPath": "converted/"
  }'
```

### Python Example

```python
# Tutorial Code Example: Using ConvertOptions with Python SDK
import groupdocs_conversion_cloud

# Configure the API client
configuration = groupdocs_conversion_cloud.Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)
api_client = groupdocs_conversion_cloud.ApiClient(configuration)
api = groupdocs_conversion_cloud.ConvertApi(api_client)

# Create conversion settings
settings = groupdocs_conversion_cloud.ConvertSettings()
settings.file_path = "documents/diagram.pdf"
settings.format = "jpeg"

# Configure JPEG-specific convert options
convert_options = groupdocs_conversion_cloud.JpegConvertOptions()
convert_options.width = 1920
convert_options.height = 1080
convert_options.quality = 95
convert_options.from_page = 1
convert_options.pages_count = 3
settings.convert_options = convert_options

settings.output_path = "converted/"

# Execute conversion
result = api.convert(settings)
print(f"Document converted successfully! Results: {result}")
```

### C# Example

```csharp
// Tutorial Code Example: Using ConvertOptions with C# SDK
using GroupDocs.Conversion.Cloud.Sdk;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

// Configure the API client
var configuration = new Configuration 
{
    ClientId = "YOUR_CLIENT_ID",
    ClientSecret = "YOUR_CLIENT_SECRET"
};
var apiInstance = new ConvertApi(configuration);

// Create conversion settings
var settings = new ConvertSettings 
{
    FilePath = "documents/diagram.pdf",
    Format = "jpeg",
    ConvertOptions = new JpegConvertOptions 
    {
        Width = 1920,
        Height = 1080,
        Quality = 95,
        FromPage = 1,
        PagesCount = 3
    },
    OutputPath = "converted/"
};

// Execute conversion
var result = apiInstance.Convert(new ConvertRequest(settings));
Console.WriteLine($"Document converted successfully! Results: {result.Count} files");
```

### Java Example

```java
// Tutorial Code Example: Using ConvertOptions with Java SDK
import com.groupdocs.conversion.cloud.api.ConvertApi;
import com.groupdocs.conversion.cloud.api.model.*;
import com.groupdocs.conversion.cloud.api.model.requests.ConvertRequest;

// Configure the API client
Configuration configuration = new Configuration();
configuration.setClientId("YOUR_CLIENT_ID");
configuration.setClientSecret("YOUR_CLIENT_SECRET");
ConvertApi apiInstance = new ConvertApi(configuration);

// Create conversion settings
ConvertSettings settings = new ConvertSettings();
settings.setFilePath("documents/diagram.pdf");
settings.setFormat("jpeg");

// Configure JPEG-specific convert options
JpegConvertOptions convertOptions = new JpegConvertOptions();
convertOptions.setWidth(1920);
convertOptions.setHeight(1080);
convertOptions.setQuality(95);
convertOptions.setFromPage(1);
convertOptions.setPagesCount(3);
settings.setConvertOptions(convertOptions);

settings.setOutputPath("converted/");

// Execute conversion
List<StoredConvertedResult> result = apiInstance.convert(new ConvertRequest(settings));
System.out.println("Document converted successfully! Results: " + result.size() + " files");
```

## Tips and Best Practices

### 1. Match ConvertOptions to Target Format

Always use the correct ConvertOptions type for your target format:
- Use `JpegConvertOptions` for JPEG output
- Use `PdfConvertOptions` for PDF output
- Use `WordProcessingConvertOptions` for DOCX, DOC, and other word formats

Using mismatched ConvertOptions types will result in errors or unexpected behavior.

### 2. Optimize for Specific Use Cases

Tailor your conversion options based on your specific needs:
- For high-quality printing: Use higher DPI, resolution, and quality settings
- For web viewing: Optimize file size and use web-friendly formats
- For editable documents: Focus on content structure preservation

### 3. Balance Quality and Size

Higher quality typically means larger file sizes:
- For images, the `Quality` parameter directly affects file size
- For PDFs, options like `CompressImages` and `ImageQuality` help balance quality and size
- Consider your audience's needs and bandwidth constraints

### 4. Use Page Selection Strategically

Page selection can improve efficiency:
- Convert only the pages you need instead of entire documents
- For large documents, consider converting in batches
- The `Pages` array lets you select non-sequential pages

### 5. Test Different Options

Conversion quality can vary based on document complexity:
- Test multiple option combinations for optimal results
- Consider using `UsePdf` for complex format conversions
- Compare different conversion paths for the same document

## Troubleshooting Common Issues

### Issue 1: Unexpected Image Dimensions
If images don't have the expected dimensions:
- Remember that some format conversions maintain aspect ratio
- Set both `Width` and `Height` for precise control
- Check if the input document has special page sizes

### Issue 2: Poor Image Quality
If image quality is lower than expected:
- Increase the `Quality` parameter for lossy formats like JPEG
- Use higher `HorizontalResolution` and `VerticalResolution` values
- For PDF to image conversions, set higher DPI values

### Issue 3: Missing Pages
If pages are missing from your conversion:
- Verify `FromPage` and `PagesCount` values are correct
- Remember that page counting starts at 1, not 0
- Check if you've inadvertently set a restrictive page range

### Issue 4: Password Protection Problems
If password protection doesn't work:
- Ensure the target format supports password protection
- Check if special characters in passwords are properly encoded
- Verify minimum password length requirements for the format

## What You've Learned

In this tutorial, you've learned:
- The purpose and importance of ConvertOptions in document conversion
- How to configure format-specific conversion parameters
- Techniques for optimizing conversion quality for different target formats
- Best practices for advanced conversion scenarios
- Implementation examples in multiple programming languages

## Further Practice

To reinforce your understanding:
1. Try converting the same document to different formats with various ConvertOptions
2. Experiment with image conversion settings to balance quality and file size
3. Create a simple application that allows users to specify custom ConvertOptions
4. Test watermarking options with different text, placement, and transparency settings

## Next Steps

Ready to expand your knowledge? Continue to our next tutorial: [Tutorial: Processing Conversion Results](/data-organization/conversion-result/) to learn how to efficiently handle and organize conversion output.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [Live Demo](https://products.groupdocs.app/conversion/family)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.conversion-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

## Feedback

Have questions about working with ConvertOptions? Need clarification on any part of this tutorial? We'd love to hear from you! Please visit our [support forum](https://forum.groupdocs.cloud/c/conversion/11) with any questions or feedback.