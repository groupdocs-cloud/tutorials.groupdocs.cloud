---
title: How to Use Font Fallback Rules in Conversion Tutorial
url: /working-with-documents/font-fallback/
description: Learn how to implement font fallback rules when converting PowerPoint presentations with Aspose.Slides Cloud API. Ensure consistent font rendering across languages and devices using REST or SDKs.
weight: 45
---

## Prerequisites

Before starting this tutorial, you should:

1. Have an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps) with active credentials
2. Be familiar with making REST API calls
3. Have your preferred development environment set up (for SDK examples)
4. Have completed the [Converting Presentations tutorial](/working-with-documents/convert-presentations/) or have equivalent knowledge
5. Have a basic understanding of fonts and character encodings

## Introduction

When converting PowerPoint presentations, one common challenge is handling fonts that might not be available on the server or in the target format. This is particularly important for presentations containing international text or special characters.

Font fallback rules allow you to specify alternative fonts that should be used when certain characters or character ranges can't be displayed using the original font. By implementing proper font fallback strategies, you can ensure consistent text appearance across different systems and conversion formats.

In this tutorial, we'll demonstrate how to use Aspose.Slides Cloud API to implement font fallback rules when converting presentations.

## Understanding Font Fallback Rules

A font fallback rule consists of two main components:

1. Unicode Range: A range of Unicode character codes that should be handled by the rule
2. Fallback Font List: A prioritized list of font names that should be used for characters in that range

When the API encounters characters in the specified Unicode range that can't be displayed with the current font, it will try each font in the fallback list until it finds one that can display the character.

## Step 1: Setting Up Authentication

Before making API calls, we need to authenticate:

```bash
curl -X POST "https://api.aspose.cloud/connect/token" \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded"
```

Save the access token from the response for the following requests.

## Step 2: Converting with Font Fallback Rules

Let's convert a presentation to a PNG image with font fallback rules:

### Try it yourself with cURL

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/convert/png" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/octet-stream" \
     -F "file=@MyPresentation.pptx" \
     -F "data=@options.json;filename=" \
     -o MyPresentation.zip
```

Where `options.json` contains:

```json
{
    "FontFallbackRules": [
        {
            "RangeStartIndex": 0x0B80,
            "RangeEndIndex": 0x0BFF,
            "FallbackFontList": ["Arial"]
        },
        {
            "RangeStartIndex": 0x0900,
            "RangeEndIndex": 0x097F,
            "FallbackFontList": ["Mangal", "Arial Unicode MS"]
        }
    ]
}
```

### What's happening?

1. We're converting a presentation to PNG format
2. We're specifying two font fallback rules:
   - For Tamil characters (Unicode range 0x0B80-0x0BFF), use Arial
   - For Devanagari characters (Unicode range 0x0900-0x097F), try Mangal first, then Arial Unicode MS
3. The API applies these rules when it encounters characters in these ranges

## Step 3: Understanding Unicode Ranges

To create effective font fallback rules, you need to understand which Unicode ranges correspond to which scripts:

| Script | Unicode Range | Example Fonts |
| --- | --- | --- |
| Latin | 0x0000-0x007F (Basic Latin)<br>0x0080-0x00FF (Latin-1 Supplement) | Arial, Times New Roman |
| Cyrillic | 0x0400-0x04FF | Arial, Tahoma |
| Greek | 0x0370-0x03FF | Arial, Tahoma |
| Arabic | 0x0600-0x06FF | Arabic Typesetting, Tahoma |
| Hebrew | 0x0590-0x05FF | David, Tahoma |
| Devanagari (Hindi) | 0x0900-0x097F | Mangal, Arial Unicode MS |
| Tamil | 0x0B80-0x0BFF | Latha, Vijaya |
| Chinese | 0x4E00-0x9FFF | SimSun, Microsoft YaHei |
| Japanese | 0x3040-0x309F (Hiragana)<br>0x30A0-0x30FF (Katakana) | MS Gothic, Meiryo |
| Korean | 0xAC00-0xD7AF | Gulim, Batang |

When creating font fallback rules, target the specific Unicode ranges for the languages or scripts used in your presentations.

## Step 4: Implementing with SDKs

Let's see how to implement font fallback rules using different programming languages:

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/aspose-Slides-cloud/aspose-Slides-cloud-dotnet

using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Model;
using System.Collections.Generic;
using System.IO;

class ConvertWithFontFallback
{
    static void Main()
    {
        // Setup client credentials
        SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Create font fallback rules
        List<FontFallbackRule> fontRules = new List<FontFallbackRule>();
        
        // Rule for Tamil characters
        fontRules.Add(new FontFallbackRule()
        {
            RangeStartIndex = 0x0B80,
            RangeEndIndex = 0x0BFF,
            FallbackFontList = new List<string>() { "Vijaya" }
        });
        
        // Rule for Devanagari characters
        fontRules.Add(new FontFallbackRule()
        {
            RangeStartIndex = 0x0900,
            RangeEndIndex = 0x097F,
            FallbackFontList = new List<string>() { "Mangal", "Arial Unicode MS" }
        });

        // Create export options with font fallback rules
        ImageExportOptions exportOptions = new ImageExportOptions()
        {
            FontFallbackRules = fontRules
        };
        
        // Convert the presentation to PNG
        using (var presentationStream = File.OpenRead("MyPresentation.pptx"))
        using (var pngStream = api.Convert(presentationStream, ExportFormat.Png, options: exportOptions))
        {
            // Save the PNG file(s)
            using (var fileStream = File.Create("MyPresentation.zip"))
            {
                pngStream.CopyTo(fileStream);
            }
        }
        
        System.Console.WriteLine("Presentation converted successfully with font fallback rules!");
    }
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/aspose-Slides-cloud/aspose-Slides-cloud-python

import asposeslidescloud
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.font_fallback_rule import FontFallbackRule
from asposeslidescloud.models.image_export_options import ImageExportOptions
from asposeslidescloud.models.export_format import ExportFormat

# Setup client credentials
slides_api = SlidesApi(None, "YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET")

# Create font fallback rules
font_rules = []

# Rule for Tamil characters
rule1 = FontFallbackRule()
rule1.range_start_index = 0x0B80
rule1.range_end_index = 0x0BFF
rule1.fallback_font_list = ["Vijaya"]
font_rules.append(rule1)

# Rule for Devanagari characters
rule2 = FontFallbackRule()
rule2.range_start_index = 0x0900
rule2.range_end_index = 0x097F
rule2.fallback_font_list = ["Mangal", "Arial Unicode MS"]
font_rules.append(rule2)

# Create export options with font fallback rules
export_options = ImageExportOptions()
export_options.font_fallback_rules = font_rules

# Convert the presentation to PNG
with open("MyPresentation.pptx", "rb") as presentation_file:
    result = slides_api.convert(presentation_file.read(), ExportFormat.PNG, 
                               None, None, None, None, export_options)
    
    # Save the result to a file
    with open("MyPresentation.zip", "wb") as zip_file:
        zip_file.write(result)
    
    print("Presentation converted successfully with font fallback rules!")
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/aspose-Slides-cloud/aspose-Slides-cloud-java

import com.aspose.slides.ApiException;
import com.aspose.slides.api.SlidesApi;
import com.aspose.slides.model.*;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class ConvertWithFontFallback {
    public static void main(String[] args) throws IOException, ApiException {
        // Setup client credentials
        SlidesApi slidesApi = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Create font fallback rules
        List<FontFallbackRule> fontRules = new ArrayList<>();
        
        // Rule for Tamil characters
        FontFallbackRule rule1 = new FontFallbackRule();
        rule1.setRangeStartIndex(0x0B80);
        rule1.setRangeEndIndex(0x0BFF);
        rule1.setFallbackFontList(Arrays.asList("Vijaya"));
        fontRules.add(rule1);
        
        // Rule for Devanagari characters
        FontFallbackRule rule2 = new FontFallbackRule();
        rule2.setRangeStartIndex(0x0900);
        rule2.setRangeEndIndex(0x097F);
        rule2.setFallbackFontList(Arrays.asList("Mangal", "Arial Unicode MS"));
        fontRules.add(rule2);

        // Create export options with font fallback rules
        ImageExportOptions exportOptions = new ImageExportOptions();
        exportOptions.setFontFallbackRules(fontRules);
        
        // Convert the presentation to PNG
        byte[] presentationData = Files.readAllBytes(Paths.get("MyPresentation.pptx"));
        byte[] result = slidesApi.convert(presentationData, ExportFormat.PNG, 
                                         null, null, null, null, exportOptions);
        
        // Save the result to a file
        Files.write(Paths.get("MyPresentation.zip"), result);
        
        System.out.println("Presentation converted successfully with font fallback rules!");
    }
}
```

## Step 5: Using Custom Fonts

You can also use custom fonts in your font fallback rules. To do this, you need to:

1. Upload your custom fonts to a folder in storage
2. Specify this folder using the `fontsFolder` parameter when converting

### Try it yourself

First, create a folder for your fonts:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/slides/storage/folder/MyFonts" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Upload your custom fonts:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/slides/storage/file/MyFonts/CustomFont.ttf" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/octet-stream" \
     --data-binary @CustomFont.ttf
```

Convert with custom fonts:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/convert/pdf?fontsFolder=MyFonts" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/octet-stream" \
     -F "file=@MyPresentation.pptx" \
     -F "data=@options.json;filename=" \
     -o MyPresentation.pdf
```

Where `options.json` includes your font fallback rules referencing the custom font:

```json
{
    "FontFallbackRules": [
        {
            "RangeStartIndex": 0x0B80,
            "RangeEndIndex": 0x0BFF,
            "FallbackFontList": ["CustomFont", "Arial"]
        }
    ]
}
```

## Troubleshooting Tips

Here are some common issues you might encounter:

1. Font Availability: Even with fallback rules, the specified fonts must be available on the server or in your uploaded fonts folder. If a font isn't available, the rule won't work.

2. Unicode Range Precision: Be precise with your Unicode ranges. If they're too broad, you might accidentally apply fallback fonts to characters that don't need them. If they're too narrow, some characters might be missed.

3. Font Order: The order of fonts in the fallback list matters. The API tries each font in order until it finds one that can display the character. Put the most suitable fonts first.

4. Multiple Rules Overlap: If you have multiple rules with overlapping Unicode ranges, the first matching rule will be used. Ensure your rules are ordered correctly.

5. Missing Character Detection: Sometimes, the API might not detect that a character is missing from a font, especially for less common character ranges. Test your rules with actual text containing the characters you're targeting.

## Advanced Font Fallback Strategies

For more complex scenarios, consider these advanced strategies:

### 1. Script-Based Fallback

Create separate rules for each script or language in your presentation:

```csharp
// Rule for Arabic
fontRules.Add(new FontFallbackRule()
{
    RangeStartIndex = 0x0600,
    RangeEndIndex = 0x06FF,
    FallbackFontList = new List<string>() { "Arabic Typesetting", "Tahoma" }
});

// Rule for Thai
fontRules.Add(new FontFallbackRule()
{
    RangeStartIndex = 0x0E00,
    RangeEndIndex = 0x0E7F,
    FallbackFontList = new List<string>() { "Tahoma", "Leelawadee" }
});
```

### 2. Cascading Fallback

For important character ranges, provide multiple fallback options:

```csharp
// Comprehensive CJK fallback
fontRules.Add(new FontFallbackRule()
{
    RangeStartIndex = 0x4E00,
    RangeEndIndex = 0x9FFF, // CJK Unified Ideographs
    FallbackFontList = new List<string>() { 
        "SimSun", // Chinese
        "MS Gothic", // Japanese
        "Batang", // Korean
        "Arial Unicode MS" // Last resort
    }
});
```

### 3. Default Fallback

Always include a default fallback rule for any characters not covered by specific rules:

```csharp
// Default fallback for any uncovered characters
fontRules.Add(new FontFallbackRule()
{
    RangeStartIndex = 0x0000,
    RangeEndIndex = 0x10FFFF, // Entire Unicode range
    FallbackFontList = new List<string>() { "Arial Unicode MS", "Tahoma" }
});
```

## What You've Learned

In this tutorial, you've learned how to:

- Understand and implement font fallback rules for presentation conversion
- Target specific Unicode character ranges for different scripts
- Create prioritized lists of fallback fonts
- Use custom fonts in your fallback rules
- Apply font fallback strategies in different programming languages
- Troubleshoot common font fallback issues

## Further Practice

To reinforce your learning, try these exercises:

1. Convert a presentation containing multiple languages with appropriate font fallback rules
2. Create a comprehensive font fallback strategy for a multilingual organization
3. Experiment with different font orders in your fallback lists to see their effect
4. Use custom fonts as fallbacks for specialized characters or symbols

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
