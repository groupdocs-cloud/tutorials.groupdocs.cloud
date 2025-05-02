---
title: How to Use Custom Fonts in Document Conversion Tutorial
description: Learn to implement custom fonts during document conversion to maintain precise visual fidelity using GroupDocs.Conversion Cloud API in this developer tutorial.
url: /advanced-features/convert-using-custom-font/
weight: 30
---

# Tutorial: How to Use Custom Fonts in Document Conversion

## Learning Objectives

In this tutorial, you'll learn how to:
- Upload and use custom fonts during document conversion
- Maintain visual fidelity of documents with unique typography
- Configure font paths for accurate rendering
- Solve common font-related issues in document conversion
- Implement font substitution when necessary

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Conversion Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with your preferred programming language (C#, Java, Python, PHP, Ruby, or Node.js)
- A document that uses custom fonts
- The font files you want to use (.ttf, .otf, etc.)

## Why Custom Fonts Matter in Document Conversion

When converting documents that use non-standard fonts, maintaining visual fidelity can be challenging. Problems that can occur include:

- Text misalignment: When a font is substituted, text spacing and layout can change
- Character issues: Special characters may appear incorrectly or be replaced with placeholder symbols
- Aesthetic degradation: The overall look and feel of the document changes, affecting its visual impact
- Brand inconsistency: Corporate documents with custom brand fonts lose their identity

By using GroupDocs.Conversion Cloud API's custom font capabilities, you can ensure your converted documents maintain their original appearance regardless of the target format.

## Implementation Steps

### Step 1: Upload Your Custom Fonts to Cloud Storage

Before conversion, you need to upload your custom fonts to your cloud storage. You can use the Storage API for this:

```csharp
// This is a code example showing how to upload font files
// In a real implementation, you'd use the Storage API to upload your font files
```

For this tutorial, we'll assume you've already uploaded your font files to a folder named "font/ttf" in your cloud storage.

### Step 2: Set Up Your Development Environment

Ensure you have the GroupDocs.Conversion Cloud SDK installed for your language:
For .NET:
```bash
Install-Package GroupDocs.Conversion-Cloud
```
For Java:
```bash
<dependency>
    <groupId>com.groupdocs</groupId>
    <artifactId>groupdocs-conversion-cloud</artifactId>
    <version>latest-version</version>
</dependency>
```
For Python:
```bash
pip install groupdocs-conversion-cloud
```

### Step 3: Authenticate with the API

To use GroupDocs.Conversion Cloud API, authenticate using your Client ID and Client Secret:

```csharp
// Initialize the API with your credentials
string MyAppKey = "XXXX-XXXX-XXXX-XXXX"; // Your AppKey
string MyAppSid = "XXXX-XXXX-XXXX-XXXX"; // Your AppSID
var configuration = new Configuration(MyAppSid, MyAppKey);
var apiInstance = new ConvertApi(configuration);
```

### Step 4: Configure Conversion with Custom Fonts

Now, let's set up the conversion settings to use your custom fonts:

```csharp
// Prepare convert settings with custom font path
var settings = new ConvertSettings
{
    FilePath = "Presentation/uses-custom-font.pptx",
    Format = "pdf",
    FontsPath = "font/ttf", // Path to the directory containing your fonts
    OutputPath = "converted/with-custom-fonts.pdf"
};
```

The `FontsPath` property is crucial - it tells the API where to look for font files during conversion.

### Step 5: Execute the Conversion Request

Now, let's execute the conversion with custom font settings:

```csharp
// Convert document using custom fonts
var response = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings));
Console.WriteLine("Document converted successfully: " + response[0].Url);
```

## Complete Code Examples

### Using C#

```csharp
using System;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace GroupDocs.Conversion.Cloud.Examples
{
    class UseCustomFontsExample
    {
        public static void Run()
        {
            // Get your credentials from https://dashboard.groupdocs.cloud/applications
            string MyAppKey = "XXXX-XXXX-XXXX-XXXX"; // Your AppKey
            string MyAppSid = "XXXX-XXXX-XXXX-XXXX"; // Your AppSID
            
            var configuration = new Configuration(MyAppSid, MyAppKey);
            
            // Create necessary API instances
            var apiInstance = new ConvertApi(configuration);
            
            try
            {
                // Prepare convert settings with custom font path
                var settings = new ConvertSettings
                {
                    FilePath = "Presentation/uses-custom-font.pptx",
                    Format = "pdf",
                    FontsPath = "font/ttf", // Path to the directory containing your fonts
                    OutputPath = "converted/with-custom-fonts.pdf"
                };
                
                // Convert document using custom fonts
                var response = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings));
                Console.WriteLine("Document converted successfully: " + response[0].Url);
                
                // Example 2: Convert Word document with custom fonts
                var settings2 = new ConvertSettings
                {
                    FilePath = "WordProcessing/custom-typography.docx",
                    Format = "pdf",
                    FontsPath = "font/ttf", // Path to the directory containing your fonts
                    OutputPath = "converted/word-with-custom-fonts.pdf"
                };
                
                var response2 = apiInstance.ConvertDocument(new ConvertDocumentRequest(settings2));
                Console.WriteLine("Word document converted successfully: " + response2[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception when calling ConvertApi: " + e.Message);
            }
        }
    }
}
```

### Using Python

```python
# Import required modules
import groupdocs_conversion_cloud
import os

# Get your credentials from https://dashboard.groupdocs.cloud/applications
client_id = "XXXX-XXXX-XXXX-XXXX"  # Your Client ID
client_secret = "XXXXXXXXXXXXXXXX"  # Your Client Secret

# Create instance of the API
api = groupdocs_conversion_cloud.ConvertApi.from_keys(client_id, client_secret)

try:
    # Example 1: Convert presentation with custom fonts
    settings = groupdocs_conversion_cloud.ConvertSettings()
    settings.file_path = "Presentation/uses-custom-font.pptx"
    settings.format = "pdf"
    settings.fonts_path = "font/ttf"  # Path to the directory containing your fonts
    settings.output_path = "converted/with-custom-fonts.pdf"
    
    # Execute conversion
    result = api.convert_document(groupdocs_conversion_cloud.ConvertDocumentRequest(settings))
    print(f"Document converted successfully: {result[0].url}")
    
    # Example 2: Convert Word document with custom fonts
    settings2 = groupdocs_conversion_cloud.ConvertSettings()
    settings2.file_path = "WordProcessing/custom-typography.docx"
    settings2.format = "pdf"
    settings2.fonts_path = "font/ttf"  # Path to the directory containing your fonts
    settings2.output_path = "converted/word-with-custom-fonts.pdf"
    
    # Execute conversion
    result2 = api.convert_document(groupdocs_conversion_cloud.ConvertDocumentRequest(settings2))
    print(f"Word document converted successfully: {result2[0].url}")

except groupdocs_conversion_cloud.ApiException as e:
    print(f"Exception when calling ConvertApi: {e}")
```

### Using Java

```java
// Import required packages
import com.groupdocs.cloud.conversion.api.ConvertApi;
import com.groupdocs.cloud.conversion.api.Configuration;
import com.groupdocs.cloud.conversion.model.*;
import com.groupdocs.cloud.conversion.model.requests.*;
import java.util.List;

public class UseCustomFontsExample {
    public static void main(String[] args) {
        // Get your credentials from https://dashboard.groupdocs.cloud/applications
        String clientId = "XXXX-XXXX-XXXX-XXXX"; // Your Client ID
        String clientSecret = "XXXXXXXXXXXXXXXX"; // Your Client Secret
        
        // Create API instance
        Configuration configuration = new Configuration(clientId, clientSecret);
        ConvertApi apiInstance = new ConvertApi(configuration);
        
        try {
            // Example 1: Convert presentation with custom fonts
            ConvertSettings settings = new ConvertSettings();
            settings.setFilePath("Presentation/uses-custom-font.pptx");
            settings.setFormat("pdf");
            settings.setFontsPath("font/ttf"); // Path to the directory containing your fonts
            settings.setOutputPath("converted/with-custom-fonts.pdf");
            
            // Execute conversion
            List<StoredConvertedResult> result = apiInstance.convertDocument(new ConvertDocumentRequest(settings));
            System.out.println("Document converted successfully: " + result.get(0).getUrl());
            
            // Example 2: Convert Word document with custom fonts
            ConvertSettings settings2 = new ConvertSettings();
            settings2.setFilePath("WordProcessing/custom-typography.docx");
            settings2.setFormat("pdf");
            settings2.setFontsPath("font/ttf"); // Path to the directory containing your fonts
            settings2.setOutputPath("converted/word-with-custom-fonts.pdf");
            
            // Execute conversion
            List<StoredConvertedResult> result2 = apiInstance.convertDocument(new ConvertDocumentRequest(settings2));
            System.out.println("Word document converted successfully: " + result2.get(0).getUrl());
        } catch (Exception e) {
            System.err.println("Exception when calling ConvertApi: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Using cURL

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Get token from JSON response
# Now use the token to convert document with custom fonts
curl -v "https://api.groupdocs.cloud/v2.0/conversion" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FilePath': 'Presentation/uses-custom-font.pptx',
    'Format': 'pdf',
    'FontsPath': 'font/ttf',
    'OutputPath': 'converted/with-custom-fonts.pdf'
}"
```

## Advanced Font Handling Techniques

### Font Substitution with OneLoadOptions

For OneNote documents, you can use font substitution to replace specific fonts:

```csharp
// Create font substitution dictionary
var fontSubstitutes = new Dictionary<string, string>
{
    {"Tahoma", "Arial"},
    {"Times New Roman", "Arial"}
};

// Create load options with font substitution
var loadOptions = new OneLoadOptions
{
    FontSubstitutes = fontSubstitutes
};

// Prepare convert settings with custom font handling
var settings = new ConvertSettings
{
    FilePath = "Note/sample.one",
    Format = "pdf",
    LoadOptions = loadOptions,
    OutputPath = "converted/note-with-font-substitution.pdf"
};
```

### Word Processing Default Font Setting

For Word documents, you can specify a default font to use when a font is missing:

```csharp
// Set a default font to use when fonts are missing
var loadOptions = new WordProcessingLoadOptions
{
    DefaultFont = "Arial"
};

// Prepare convert settings with default font
var settings = new ConvertSettings
{
    FilePath = "WordProcessing/document.docx",
    Format = "pdf",
    LoadOptions = loadOptions,
    OutputPath = "converted/with-default-font.pdf"
};
```

## Troubleshooting Tips

### Missing Fonts
Issue: Text appears with incorrect font despite providing custom fonts.Solution: 
- Ensure the font file name matches exactly what's expected by the document
- Check that the font is in a supported format (.ttf, .otf)
- Verify the font path is correct and accessible

### Text Misalignment
Issue: Text is misaligned or runs off the page in the converted document.Solution: 
- Try different conversion options like resolution or page size settings
- If converting to PDFs, try enabling PDF optimization options

### Font Licensing
Issue: Concerns about font licensing when embedding fonts.Solution: Ensure you have the proper licenses for the fonts you're using, especially for commercial applications.

## Try It Yourself

Now that you've learned how to use custom fonts, try these exercises:

1. Convert a document that uses a custom corporate font to PDF
2. Create a font substitution map for a document with unavailable fonts
3. Compare the output quality between using and not using custom fonts
4. Try converting a document with multiple custom fonts

## What You've Learned

In this tutorial, you've learned how to:
- Configure custom font directories for document conversion
- Maintain the visual fidelity of documents with unique typography
- Implement font substitution for unavailable fonts
- Troubleshoot common font-related issues in document conversion
- Use advanced font handling techniques for different document types

## Next Steps

Now that you can use custom fonts in your conversions, you might want to explore:
- [Tutorial: Converting to PDF Formats](/advanced-features/convert-to-pdf-formats/)
- [Tutorial: How to Add Watermarks During Conversion](/advanced-features/add-watermark/)
- [Tutorial: Converting to Image Formats](/advanced-features/convert-to-image-formats/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/conversion)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)


If you have questions about this tutorial, please feel free to post them in our [support forum](https://forum.groupdocs.cloud/c/conversion).
