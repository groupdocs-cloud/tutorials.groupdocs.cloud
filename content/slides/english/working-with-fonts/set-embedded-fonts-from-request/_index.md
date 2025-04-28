---
title: How to Set Embedded Fonts from a Request Tutorial
url: /working-with-fonts/set-embedded-fonts-from-request/
weight: 50
description: Learn how to embed custom font files directly into PowerPoint presentations using Aspose.Slides Cloud API in this practical step-by-step tutorial.
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Upload and embed a custom font file directly into a presentation
- Embed a custom font in a presentation stored in cloud storage
- Apply the same technique to a presentation in the request body
- Optimize embedded fonts for file size

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret
- A PowerPoint presentation uploaded to your Aspose Cloud Storage
- A font file (TTF or OTF format) that you want to embed
- Basic understanding of REST API concepts

## Why Embedding Custom Fonts Matters

While the standard [SetEmbeddedFont](/working-with-fonts/set-embedded-fonts/) approach works well for fonts that are already installed on the server, there are situations where you need to embed a custom font that isn't available on the system:

1. Custom corporate fonts that aren't widely distributed
2. Specialized or unique fonts that enhance your presentation's visual appeal
3. Self-contained presentations that preserve all design elements
4. Ensuring consistent rendering across different platforms and devices

## Step 1: Embedding a Custom Font File in a Presentation in Storage

Let's learn how to embed a custom font file directly into a presentation that's stored in your cloud storage.

### API Details

|API|Type|Description|Resource|
| :- | :- | :- | :- |
|/slides/{name}/fonts/embedded|POST|Embeds font from request and returns presentation fonts info.|[SetEmbeddedFontFromRequest](https://apireference.aspose.cloud/slides/#/Fonts/SetEmbeddedFontFromRequest)|

### Understanding Parameters

- name: The name of your presentation file in storage
- fontFile: The TTF or OTF font file to embed
- onlyUsed: A boolean parameter that determines whether to embed only the characters used in the presentation (true) or the entire font (false)

### Try It Yourself

#### Using cURL

First, authenticate to get your access token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
     -X POST \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "Accept: application/json"
```

Now, embed a custom font file in your presentation:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/fonts/embedded" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -F "file=@calibri.ttf"
```

The response will include information about all fonts in the presentation, including your newly embedded font.

## Step 2: Implementing in Different Programming Languages

Let's see how to implement custom font embedding in various programming languages:

### C# Example

```csharp
// Tutorial Code Example - Embedding Custom Font Files in Java
import com.aspose.slides.cloud.sdk.api.SlidesApi;
import com.aspose.slides.cloud.sdk.model.FontsData;
import com.aspose.slides.cloud.sdk.model.FontData;

import java.nio.file.Files;
import java.nio.file.Paths;

public class CustomFontEmbeddingExample {
    public static void main(String[] args) {
        try {
            // Create API instance with your credentials
            SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Name of the presentation in storage
            String presentationName = "MyPresentation.pptx";
            
            // Path to the font file
            String fontFilePath = "calibri.ttf";
            
            // Whether to embed only used characters (true) or the entire font (false)
            boolean onlyUsed = false;
            
            // Read the font file
            byte[] fontFile = Files.readAllBytes(Paths.get(fontFilePath));
            
            // Embed the font in the presentation
            FontsData response = api.setEmbeddedFontFromRequest(fontFile, presentationName, onlyUsed, null, null, null);
            
            // Display success message
            System.out.println("Custom font has been successfully embedded in the presentation.");
            
            // Display information about the embedded fonts
            System.out.println("\nEmbedded fonts in the presentation:");
            System.out.println("-----------------------------------");
            
            for (FontData font : response.getList()) {
                if (font.getIsEmbedded() != null && font.getIsEmbedded()) {
                    System.out.println("Font Name: " + font.getFontName());
                }
            }
        } catch (Exception ex) {
            System.out.println("Error embedding font: " + ex.getMessage());
        }
    }
}
```
## Step 3: Working with Presentations in Request Body

In some scenarios, you might need to embed a custom font in a presentation that isn't stored in the cloud. Aspose.Slides Cloud API provides an endpoint for this purpose:

### API Details

|API|Type|Description|Resource|
| :- | :- | :- | :- |
|/slides/fonts/embedded|POST|Embeds font from request and returns presentation.|[SetEmbeddedFontFromRequestOnline](https://apireference.aspose.cloud/slides/#/Fonts/SetEmbeddedFontFromRequestOnline)|

### C# Example for Online Mode

```csharp
// Tutorial Code Example - Embedding Custom Font Files Online in C#
using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Api;
using System;
using System.IO;

namespace AsposeSlidesOnlineCustomFontEmbedding
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create API instance with your credentials
            SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Local paths to the files
            string presentationPath = "MyPresentation.pptx";
            string fontFilePath = "calibri.ttf";
            
            // Whether to embed only used characters (true) or the entire font (false)
            bool onlyUsed = false;
            
            try
            {
                // Open the presentation file
                using (Stream presentationFile = File.OpenRead(presentationPath))
                {
                    // Open the font file
                    using (Stream fontFile = File.OpenRead(fontFilePath))
                    {
                        // Embed the font and get the updated presentation
                        using (Stream updatedPresentation = api.SetEmbeddedFontFromRequestOnline(presentationFile, fontFile, onlyUsed))
                        {
                            // Save the updated presentation
                            using (FileStream outputStream = File.Create("UpdatedPresentation.pptx"))
                            {
                                updatedPresentation.CopyTo(outputStream);
                            }
                        }
                    }
                }
                
                Console.WriteLine("Custom font has been successfully embedded and the presentation has been saved.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error embedding font: {ex.Message}");
            }
        }
    }
}
```

### Python Example

```python
# Tutorial Code Example - Embedding Custom Font Files in Python
import asposeslidescloud
from asposeslidescloud.configuration import Configuration
from asposeslidescloud.apis.slides_api import SlidesApi

# Configure the API client
configuration = Configuration()
configuration.app_sid = 'YOUR_CLIENT_ID'
configuration.app_key = 'YOUR_CLIENT_SECRET'
api = SlidesApi(configuration)

# Name of the presentation in storage
presentation_name = "MyPresentation.pptx"

# Path to the font file
font_file_path = "calibri.ttf"

# Whether to embed only used characters (True) or the entire font (False)
only_used = False

try:
    # Read the font file
    with open(font_file_path, 'rb') as f:
        font_file = f.read()
    
    # Embed the font in the presentation
    response = api.set_embedded_font_from_request(font_file, presentation_name, only_used)
    
    # Display success message
    print("Custom font has been successfully embedded in the presentation.")
    
    # Display information about the embedded fonts
    print("\nEmbedded fonts in the presentation:")
    print("-----------------------------------")
    
    for font in response.list:
        if hasattr(font, 'is_embedded') and font.is_embedded:
            print(f"Font Name: {font.font_name}")
            
except Exception as ex:
    print(f"Error embedding font: {str(ex)}")
```

## Step 4: Understanding Font File Requirements

When embedding custom fonts, it's important to understand the requirements and limitations:

### Supported Font Formats

Aspose.Slides Cloud API supports the following font formats:
- TrueType Fonts (.ttf)
- OpenType Fonts (.otf)

### Font File Considerations

1. File Size: Larger font files will increase your presentation size, especially when embedding the entire font.
2. Character Sets: Complex fonts with large character sets will consume more space.
3. Font License: Ensure you have the proper license to embed the font in your presentations.

## Best Practices for Custom Font Embedding

1. Use onlyUsed=true for most cases: This embeds only the characters used in the presentation, which can significantly reduce file size.

2. Consider font subsetting: If you're embedding custom fonts in multiple presentations, embedding only the used characters is more efficient.

3. Test the presentation: After embedding a custom font, open the presentation on a system without the font installed to ensure it displays correctly.

4. Document the fonts used: Keep track of which custom fonts you've embedded in your presentations for future reference.

## Common Issues and Troubleshooting

### Invalid Font File

If you receive an error about an invalid font file:
1. Verify that the font file is a valid TTF or OTF format
2. Check that the file isn't corrupted
3. Try a different font file to see if the issue persists

### Large File Size

If your presentation file becomes too large after embedding fonts:
1. Use `onlyUsed=true` to embed only the characters used in the presentation
2. Consider using a font with fewer glyphs
3. Use [CompressEmbeddedFonts](/working-with-fonts/compress-embedded-fonts/) to optimize the embedded fonts

### Font Not Displaying Correctly

If the embedded font doesn't display correctly:
1. Verify that the font file contains all the required glyphs
2. Check if the font has special rendering requirements
3. Try using a more standard font format

## What You've Learned

In this tutorial, you've learned:
- How to embed a custom font file directly into a presentation
- How to apply this to presentations in cloud storage and in request bodies
- Best practices for custom font embedding
- How to optimize embedded fonts for file size
- Troubleshooting common issues with custom font embedding

## Further Practice

To reinforce your learning:
1. Create a font management utility that maintains a library of custom fonts for your presentations
2. Build a tool that checks a presentation for specific fonts and automatically embeds them if needed
3. Experiment with different font formats and see how they affect file size and rendering quality

## Next Steps

Now that you know how to embed custom fonts, learn how to [compress embedded fonts](/working-with-fonts/compress-embedded-fonts/) to optimize your presentation file size.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)

