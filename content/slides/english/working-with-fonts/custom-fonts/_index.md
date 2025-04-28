---
title: Learn to Use Custom Fonts with fontsFolder Parameter Tutorial
url: /working-with-fonts/custom-fonts/
description: Learn how to use custom fonts in your PowerPoint presentations with Aspose.Slides Cloud API. This step-by-step tutorial shows how to specify custom font folders.
weight: 10
---

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret
- Custom font files uploaded to your Aspose Cloud Storage
- Basic understanding of REST API concepts

## The Importance of Custom Fonts

When working with presentations, font consistency is crucial for maintaining visual quality. By default, Aspose.Slides Cloud uses system fonts, but there are scenarios where you need to use custom fonts:

1. When your presentations contain specialized or branded fonts
2. When you need to ensure consistent rendering across different platforms
3. When converting presentations to formats like PDF or images

## Step 1: Upload Your Custom Fonts to Cloud Storage

Before you can use custom fonts, you need to upload them to a folder in your Aspose Cloud Storage:

1. Log in to your [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
2. Navigate to the Storage section
3. Create a new folder (e.g., "fonts" or "customFonts")
4. Upload your .ttf or .otf font files to this folder

## Step 2: Specify the fontsFolder Parameter

The fontsFolder parameter allows you to tell Aspose.Slides Cloud where to find your custom fonts. This parameter is available in several API methods, particularly those that convert presentations or their parts to other formats.

### Try it Yourself

Let's see how to use the fontsFolder parameter when converting a presentation to PDF:

#### Using cURL

First, authenticate to get your access token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
     -X POST \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "Accept: application/json"
```

Now, convert your presentation to PDF using custom fonts:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/convert?format=pdf&fontsFolder=fonts" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     --data-binary @sample.pptx \
     -o output.pdf
```

## Step 3: Implementing in Different Languages

Let's look at how to implement this functionality in various programming languages:

### C# Example

```csharp
// Tutorial Code Example - Using Custom Fonts in C#
using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Api;
using System.IO;

namespace AsposeSlidesCustomFonts
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create API instance with your credentials
            SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Open the PowerPoint file
            using Stream file = File.OpenRead("sample.pptx");
            
            // Convert to PDF with custom fonts from the "fonts" folder in your storage
            using Stream response = api.Convert(file, ExportFormat.Pdf, fontsFolder: "fonts");
            
            // Save the output file
            using FileStream outputFile = File.Create("output.pdf");
            response.CopyTo(outputFile);
            
            System.Console.WriteLine("Conversion completed successfully!");
        }
    }
}
```

### Python Example

```python
# Tutorial Code Example - Using Custom Fonts in Python
import asposeslidescloud
from asposeslidescloud.configuration import Configuration
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.export_format import ExportFormat

# Configure the API client
configuration = Configuration()
configuration.app_sid = 'YOUR_CLIENT_ID'
configuration.app_key = 'YOUR_CLIENT_SECRET'
api = SlidesApi(configuration)

# Read the input file
with open("sample.pptx", 'rb') as f:
    input_file = f.read()

# Convert to PDF using custom fonts from the "fonts" folder
result = api.convert(input_file, ExportFormat.PDF, None, None, "fonts")

# Save the output file
with open("output.pdf", 'wb') as f:
    f.write(result)
    
print('Conversion completed successfully!')
```

### Java Example

```java
// Tutorial Code Example - Using Custom Fonts in Java
import com.aspose.slides.cloud.sdk.api.SlidesApi;
import com.aspose.slides.cloud.sdk.model.ExportFormat;

import java.io.File;
import java.nio.file.Files;
import java.nio.file.Paths;

public class CustomFontsExample {
    public static void main(String[] args) throws Exception {
        // Create API instance with your credentials
        SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        // Read the input file
        byte[] file = Files.readAllBytes(Paths.get("sample.pptx"));
        
        // Convert to PDF using custom fonts from the "fonts" folder
        File response = api.convert(file, ExportFormat.PDF, null, null, "fonts", null, null);
        
        System.out.println("Conversion completed successfully!");
        System.out.println("The converted file was saved to " + response.getPath());
    }
}
```

## Common Issues and Troubleshooting

### Font Not Found Issues

If you're experiencing font-related issues, check the following:

1. Verify font filenames: Ensure the font files are properly named with the correct extension (.ttf or .otf).
2. Check font folder path: Make sure the fontsFolder parameter points to the correct folder in your storage.
3. Font compatibility: Some specialized fonts may not be fully supported. Try with standard TrueType fonts first.

### Authorization Errors

If you receive authorization errors, ensure your Client ID and Client Secret are correct and that your subscription is active.

## What You've Learned

In this tutorial, you've learned:
- How to upload custom fonts to Aspose Cloud Storage
- How to use the fontsFolder parameter to specify custom font locations
- How to implement custom font usage in different programming languages
- Troubleshooting tips for common font-related issues

## Further Practice

To reinforce your learning:
1. Try converting a presentation with custom fonts to different formats (JPEG, HTML)
2. Experiment with different font types and observe the results
3. Create a presentation that relies on custom fonts and ensure it renders correctly

## Next Steps

Now that you know how to use custom fonts, proceed to the next tutorial to learn how to [get fonts information from a presentation](/working-with-fonts/get-presentation-fonts/).

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
