---
title: Learn to Delete Embedded Fonts Tutorial
url: /working-with-fonts/delete-embedded-fonts/
description: Learn how to delete embedded fonts from PowerPoint presentations to reduce file size using Aspose.Slides Cloud API in this step-by-step tutorial.
weight: 80
---

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret
- A PowerPoint presentation with embedded fonts
- Basic understanding of REST API concepts
- Familiarity with font embedding in PowerPoint

## Why Deleting Embedded Fonts Matters

Embedding fonts ensures that your presentation appears consistent across different systems, but there are situations when you might want to remove these embedded fonts:

1. Reducing file size: Embedded fonts can significantly increase presentation size
2. Updating fonts: Remove old embedded fonts before replacing them with new ones
3. Font license compliance: Remove fonts that shouldn't be distributed
4. Standardization: Clean up presentations to use only system fonts
5. Performance optimization: Improve loading and saving times for large presentations

## Step 1: Deleting Embedded Fonts from a Presentation in Storage

Let's learn how to delete an embedded font from a presentation that's stored in your cloud storage.

### API Details

|API|Type|Description|Resource|
| :- | :- | :- | :- |
|/slides/{name}/fonts/embedded/{fontName}|DELETE|Removes specified embedded font and returns presentation fonts info.|[DeleteEmbeddedFont](https://apireference.aspose.cloud/slides/#/Fonts/DeleteEmbeddedFont)|

### Understanding Parameters

- name: The name of your presentation file in storage
- fontName: The name of the embedded font to delete

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

Now, delete an embedded font from your presentation:

```bash
curl -X DELETE "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/fonts/embedded/Calibri%20Light" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This will remove the embedded "Calibri Light" font from your presentation and return information about the remaining fonts.

## Step 2: Implementing in Different Programming Languages

Let's see how to implement font deletion in various programming languages:

### C# Example

```csharp
// Tutorial Code Example - Deleting Embedded Fonts in C#
using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Api;
using Aspose.Slides.Cloud.Sdk.Model;
using System;

namespace AsposeSlidesDeleteFont
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create API instance with your credentials
            SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Name of the presentation in storage
            string presentationName = "MyPresentation.pptx";
            
            // Name of the embedded font to delete
            string fontName = "Calibri Light";
            
            try
            {
                // First, let's check the presentation's size before removing the font
                var presentationInfo = api.GetSlidesDocument(presentationName);
                long sizeBeforeDeletion = presentationInfo.SelfSize.Value;
                
                Console.WriteLine($"Presentation size before font removal: {sizeBeforeDeletion} bytes");
                
                // Get list of fonts before deletion
                FontsData fontsBefore = api.GetFonts(presentationName);
                Console.WriteLine("\nEmbedded fonts before deletion:");
                int embeddedCount = 0;
                foreach (var font in fontsBefore.List)
                {
                    if (font.IsEmbedded.HasValue && font.IsEmbedded.Value)
                    {
                        Console.WriteLine($"- {font.FontName}");
                        embeddedCount++;
                    }
                }
                Console.WriteLine($"Total embedded fonts: {embeddedCount}");
                
                // Delete the embedded font from the presentation
                FontsData response = api.DeleteEmbeddedFont(presentationName, fontName);
                
                // Check the size after font removal
                presentationInfo = api.GetSlidesDocument(presentationName);
                long sizeAfterDeletion = presentationInfo.SelfSize.Value;
                
                Console.WriteLine($"\nPresentation size after font removal: {sizeAfterDeletion} bytes");
                Console.WriteLine($"Size reduction: {sizeBeforeDeletion - sizeAfterDeletion} bytes ({(1 - (double)sizeAfterDeletion / sizeBeforeDeletion) * 100:F2}%)");
                
                // Display information about the remaining embedded fonts
                Console.WriteLine("\nEmbedded fonts after deletion:");
                embeddedCount = 0;
                foreach (var font in response.List)
                {
                    if (font.IsEmbedded.HasValue && font.IsEmbedded.Value)
                    {
                        Console.WriteLine($"- {font.FontName}");
                        embeddedCount++;
                    }
                }
                Console.WriteLine($"Total embedded fonts: {embeddedCount}");
                
                Console.WriteLine($"\nFont '{fontName}' has been successfully removed from the presentation.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error removing font: {ex.Message}");
            }
        }
    }
}
```

### Python Example

```python
# Tutorial Code Example - Deleting Embedded Fonts in Python
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

# Name of the embedded font to delete
font_name = "Calibri Light"

try:
    # First, let's check the presentation's size before removing the font
    presentation_info = api.get_slides_document(presentation_name)
    size_before_deletion = presentation_info.self_size
    
    print(f"Presentation size before font removal: {size_before_deletion} bytes")
    
    # Get list of fonts before deletion
    fonts_before = api.get_fonts(presentation_name)
    print("\nEmbedded fonts before deletion:")
    embedded_count = 0
    for font in fonts_before.list:
        if hasattr(font, 'is_embedded') and font.is_embedded:
            print(f"- {font.font_name}")
            embedded_count += 1
    print(f"Total embedded fonts: {embedded_count}")
    
    # Delete the embedded font from the presentation
    response = api.delete_embedded_font(presentation_name, font_name)
    
    # Check the size after font removal
    presentation_info = api.get_slides_document(presentation_name)
    size_after_deletion = presentation_info.self_size
    
    print(f"\nPresentation size after font removal: {size_after_deletion} bytes")
    size_reduction = size_before_deletion - size_after_deletion
    percentage = (1 - (size_after_deletion / size_before_deletion)) * 100
    print(f"Size reduction: {size_reduction} bytes ({percentage:.2f}%)")
    
    # Display information about the remaining embedded fonts
    print("\nEmbedded fonts after deletion:")
    embedded_count = 0
    for font in response.list:
        if hasattr(font, 'is_embedded') and font.is_embedded:
            print(f"- {font.font_name}")
            embedded_count += 1
    print(f"Total embedded fonts: {embedded_count}")
    
    print(f"\nFont '{font_name}' has been successfully removed from the presentation.")
    
except Exception as ex:
    print(f"Error removing font: {str(ex)}")
```

### Java Example

```java
// Tutorial Code Example - Deleting Embedded Fonts in Java
import com.aspose.slides.cloud.sdk.api.SlidesApi;
import com.aspose.slides.cloud.sdk.model.FontsData;
import com.aspose.slides.cloud.sdk.model.FontData;
import com.aspose.slides.cloud.sdk.model.DocumentInfo;

public class DeleteEmbeddedFontExample {
    public static void main(String[] args) {
        try {
            // Create API instance with your credentials
            SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Name of the presentation in storage
            String presentationName = "MyPresentation.pptx";
            
            // Name of the embedded font to delete
            String fontName = "Calibri Light";
            
            // First, let's check the presentation's size before removing the font
            DocumentInfo presentationInfo = api.getSlidesDocument(presentationName, null, null);
            long sizeBeforeDeletion = presentationInfo.getSelfSize();
            
            System.out.println("Presentation size before font removal: " + sizeBeforeDeletion + " bytes");
            
            // Get list of fonts before deletion
            FontsData fontsBefore = api.getFonts(presentationName, null, null, null);
            System.out.println("\nEmbedded fonts before deletion:");
            int embeddedCount = 0;
            for (FontData font : fontsBefore.getList()) {
                if (font.getIsEmbedded() != null && font.getIsEmbedded()) {
                    System.out.println("- " + font.getFontName());
                    embeddedCount++;
                }
            }
            System.out.println("Total embedded fonts: " + embeddedCount);
            
            // Delete the embedded font from the presentation
            FontsData response = api.deleteEmbeddedFont(presentationName, fontName, null, null, null);
            
            // Check the size after font removal
            presentationInfo = api.getSlidesDocument(presentationName, null, null);
            long sizeAfterDeletion = presentationInfo.getSelfSize();
            
            System.out.println("\nPresentation size after font removal: " + sizeAfterDeletion + " bytes");
            
            long sizeReduction = sizeBeforeDeletion - sizeAfterDeletion;
            double percentage = (1 - (double)sizeAfterDeletion / sizeBeforeDeletion) * 100;
            System.out.println("Size reduction: " + sizeReduction + " bytes (" + String.format("%.2f", percentage) + "%)");
            
            // Display information about the remaining embedded fonts
            System.out.println("\nEmbedded fonts after deletion:");
            embeddedCount = 0;
            for (FontData font : response.getList()) {
                if (font.getIsEmbedded() != null && font.getIsEmbedded()) {
                    System.out.println("- " + font.getFontName());
                    embeddedCount++;
                }
            }
            System.out.println("Total embedded fonts: " + embeddedCount);
            
            System.out.println("\nFont '" + fontName + "' has been successfully removed from the presentation.");
        } catch (Exception ex) {
            System.out.println("Error removing font: " + ex.getMessage());
        }
    }
}
```

## Step 3: Working with Presentations in Request Body

Sometimes you need to delete embedded fonts from a presentation that isn't stored in the cloud. Aspose.Slides Cloud API provides an endpoint for this purpose:

### API Details

|API|Type|Description|Resource|
| :- | :- | :- | :- |
|/slides/fonts/embedded/{fontName}/delete|POST|Removes specified embedded font and returns presentation.|[DeleteEmbeddedFontOnline](https://apireference.aspose.cloud/slides/#/Fonts/DeleteEmbeddedFontOnline)|

### C# Example for Online Mode

```csharp
// Tutorial Code Example - Deleting Embedded Fonts Online in C#
using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Api;
using System;
using System.IO;

namespace AsposeSlidesOnlineFontDeletion
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create API instance with your credentials
            SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Local path to the presentation file
            string filePath = "MyPresentation.pptx";
            
            // Name of the embedded font to delete
            string fontName = "Calibri Light";
            
            try
            {
                // Get the file size before deletion
                FileInfo fileInfo = new FileInfo(filePath);
                long sizeBeforeDeletion = fileInfo.Length;
                
                Console.WriteLine($"File size before font removal: {sizeBeforeDeletion} bytes");
                
                // Open the presentation file
                using (Stream file = File.OpenRead(filePath))
                {
                    // Delete the embedded font and get the updated presentation
                    using (Stream updatedPresentation = api.DeleteEmbeddedFontOnline(file, fontName))
                    {
                        // Save the updated presentation
                        string outputPath = "Updated_Presentation.pptx";
                        using (FileStream outputStream = File.Create(outputPath))
                        {
                            updatedPresentation.CopyTo(outputStream);
                        }
                        
                        // Get the file size after deletion
                        FileInfo updatedFileInfo = new FileInfo(outputPath);
                        long sizeAfterDeletion = updatedFileInfo.Length;
                        
                        Console.WriteLine($"File size after font removal: {sizeAfterDeletion} bytes");
                        Console.WriteLine($"Size reduction: {sizeBeforeDeletion - sizeAfterDeletion} bytes ({(1 - (double)sizeAfterDeletion / sizeBeforeDeletion) * 100:F2}%)");
                    }
                }
                
                Console.WriteLine($"Font '{fontName}' has been successfully removed and the presentation has been saved.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error removing font: {ex.Message}");
            }
        }
    }
}
```

## When to Delete Embedded Fonts

Deciding when to delete embedded fonts depends on several factors:

### Good Candidates for Deletion

1. Standard system fonts: Fonts like Arial, Calibri, Times New Roman are available on most systems
2. Fonts no longer used: If you've replaced text that used a particular font
3. Redundant fonts: Multiple similar fonts when one would suffice
4. Temporary embedded fonts: Fonts you embedded for specific editing purposes

### When to Keep Embedded Fonts

1. Custom or rare fonts: Fonts that aren't likely to be available on recipient's systems
2. Corporate branding fonts: Fonts that are essential to maintaining brand identity
3. Special character fonts: Fonts needed for specific symbols or languages
4. Presentations for printing or conversion: When you need to preserve exact appearance

## Best Practices for Font Management

1. Audit before deleting: Review which fonts are embedded and why before removing them
2. Test after deletion: Check the presentation on different systems to ensure it still looks correct
3. Document font changes: Keep track of which fonts were removed for future reference
4. Consider embedding subset: Instead of deleting fonts, consider [compressing them](/working-with-fonts/compress-embedded-fonts/) to reduce file size while maintaining appearance

## Common Issues and Troubleshooting

### Font Still in Use

If you get an error that the font is still in use:
1. Make sure all text using that font has been changed to a different font
2. Check for hidden elements that might be using the font
3. Consider replacing the font with a system font before deleting it

### Text Appearance Changes

If text appearance changes after deleting a font:
1. The system is falling back to a substitute font with different metrics
2. Re-embed the font or choose a more suitable replacement
3. Adjust text formatting as needed to accommodate the new font

### Unexpected File Size

If you don't see significant file size reduction:
1. The font might not have been fully embedded in the first place
2. The font might be small compared to other content (images, videos)
3. Multiple instances of the same font family might be embedded separately

## What You've Learned

In this tutorial, you've learned:
- How to delete embedded fonts from a presentation to reduce file size
- How to implement font deletion for presentations in storage and in request bodies
- When to delete embedded fonts and when to keep them
- Best practices for font management in presentations
- How to troubleshoot common issues with font deletion

## Further Practice

To reinforce your learning:
1. Create a font auditing tool that identifies which embedded fonts could be safely removed
2. Develop a batch process to remove standard fonts from multiple presentations
3. Build a workflow that combines font replacement and deletion to optimize presentations

## Next Steps

Now that you've completed the Working with Fonts tutorial series, you're ready to apply these skills to your real-world presentation management needs. Consider exploring other Aspose.Slides Cloud API features for comprehensive presentation management.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
