---
title: Learn to Work with Color Codes in OMR Forms
weight: 10
url: /appendices/colors/
description: Tutorial on implementing font and fill colors in Aspose.OMR Cloud API. Learn to enhance form readability and design with proper color implementation.
---

# Learn to Work with Color Codes in OMR Forms

## Tutorial Overview

In this tutorial, you'll learn how to effectively implement font colors and fill (background) colors in your Aspose.OMR Cloud forms. Proper use of colors can significantly enhance form readability, user experience, and even improve recognition results.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Understand the color codes supported by Aspose.OMR Cloud
- Implement font colors in various form elements
- Apply background colors to sections and elements
- Create visually distinctive forms while maintaining recognition accuracy
- Troubleshoot common color-related issues

## Prerequisites

Before starting this tutorial:
1. Set up an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps)
2. Obtain your API credentials (Client ID and Client Secret)
3. Basic understanding of OMR form structure
4. Familiarity with API requests (via curl or an SDK)

## Understanding Supported Colors

Aspose.OMR Cloud supports a specific set of colors for both font and background (fill) elements. Working within this supported color palette ensures optimal form recognition.

The following table lists all supported colors with their visual representation:

| Color | Code |
|-------|------|
| Aqua | Aqua |
| Aquamarine | Aquamarine |
| Black | Black |
| Blue | Blue |
| BlueViolet | BlueViolet |
| Crimson | Crimson |
| DarkBlue | DarkBlue |
| DarkGreen | DarkGreen |
| DarkOrange | DarkOrange |
| DarkSalmon | DarkSalmon |
| Fuchsia | Fuchsia |
| Gray | Gray |
| Indigo | Indigo |
| LightGray | LightGray |
| Lime | Lime |
| Red | Red |
| Teal | Teal |
| White | White |

## Step-by-Step Implementation

### 1. Setting Font Colors in Text Elements

Font colors can make important text stand out or create visual hierarchies in your forms. Here's how to implement them:

```
?text=Please fill in all required fields&fontColor=Red
```

This markup creates text in red color, highlighting its importance.

#### Try it yourself:
Create a text element with blue font color to indicate optional fields.

### 2. Applying Background Colors to Elements

Background colors can group related items or highlight specific sections:

```
?container=Section Title
&fontColor=White
&backgroundColor=DarkBlue
```

This creates a section header with white text on a dark blue background.

#### Try it yourself:
Create a container with a light gray background for secondary information.

### 3. Implementing Color Contrast for Accessibility

Good color contrast improves form usability for all users:

```
?text=WARNING: Do not fold this form
&fontColor=White
&backgroundColor=Crimson
```

This creates a high-contrast warning message.

### 4. Creating a Colored Header

Here's how to create a distinctive form header:

```
?container=EMPLOYEE EVALUATION FORM
&fontColor=White
&backgroundColor=DarkGreen
&fontStyle=Bold
&fontSize=16
```

### 5. Using Colors with Bubbles and Checkboxes

Colors can help organize answer options:

```
?answer=Strongly Agree|Agree|Neutral|Disagree|Strongly Disagree
&bubbleColor=DarkBlue
```

## Implementation Examples

Let's see how to implement colors in real API calls:

### Using cURL

```bash
curl -X POST "https://api.aspose.cloud/v3.0/omr/template" \
  -H "accept: application/json" \
  -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: multipart/form-data" \
  -F "markup=@template_with_colors.txt" \
  -F "settings={\"fontFamily\":\"Arial\",\"fontStyle\":\"Regular\",\"fontColor\":\"Black\"}"
```

### Using Python SDK

```python
# Tutorial Code Example: Using colors with Aspose.OMR Cloud in Python
import aspose.omr as omr
from aspose.omr.api.omr_api import OmrApi
from aspose.omr.model.template_settings import TemplateSettings

# Configuration
configuration = omr.Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
api = OmrApi(configuration)

# Define base template settings
template_settings = TemplateSettings()
template_settings.font_family = "Arial"
template_settings.font_style = "Regular"
template_settings.font_color = "Black"  # Default font color

# Read markup file with color specifications
with open("template_with_colors.txt", "r") as file:
    markup_content = file.read()

# Generate template
result = api.post_generate_template(markup_content, template_settings)

# Process result
if result.error_code == 0:
    print("Template created successfully!")
    # Save template to file
    with open("colored_template.omr", "wb") as template_file:
        template_file.write(result.payload)
else:
    print(f"Error: {result.error_message}")
```

### Using C# SDK

```csharp
// Tutorial Code Example: Using colors with Aspose.OMR Cloud in C#
using System;
using System.IO;
using Aspose.OMR.Cloud.SDK.Api;
using Aspose.OMR.Cloud.SDK.Model;

namespace AsposeTutorials
{
    class ColorImplementationExample
    {
        static void Main(string[] args)
        {
            // Configure API client
            var config = new Configuration
            {
                ClientId = "YOUR_CLIENT_ID",
                ClientSecret = "YOUR_CLIENT_SECRET"
            };
            
            var api = new OmrApi(config);
            
            // Create template settings
            var settings = new TemplateSettings
            {
                FontFamily = "Arial",
                FontStyle = "Regular",
                FontColor = "Black" // Default font color
            };
            
            // Read markup file with color specifications
            string markupContent = File.ReadAllText("template_with_colors.txt");
            
            try
            {
                // Generate template
                var result = api.PostGenerateTemplate(markupContent, settings);
                
                if (result.ErrorCode == 0)
                {
                    Console.WriteLine("Template created successfully!");
                    // Save template to file
                    File.WriteAllBytes("colored_template.omr", result.Payload);
                }
                else
                {
                    Console.WriteLine($"Error: {result.ErrorMessage}");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
            }
        }
    }
}
```

## Common Issues and Troubleshooting

### Issue: Colors Not Rendering in Generated PDF

Solution: Ensure you're using exact color names from the supported list. Color names are case-sensitive. For example, use "DarkBlue" instead of "darkblue" or "dark blue".

### Issue: Poor Recognition Due to Color Choices

Solution: Avoid using light colors for bubbles or elements that need to be recognized. Stick to darker colors like Black, DarkBlue, or Indigo for elements that require recognition.

### Issue: Text Visibility Problems

Solution: Always ensure high contrast between font color and background color. For example, avoid light text on light backgrounds.

## What You've Learned

In this tutorial, you've learned:
- The specific color codes supported by Aspose.OMR Cloud
- How to implement font colors for text elements
- Techniques for applying background colors
- Best practices for using colors in forms
- Troubleshooting common color-related issues

## Further Practice

To reinforce your knowledge:
1. Create a complete form template with at least three different font colors
2. Implement background colors for different sections
3. Try creating a form with a color-coded rating system (e.g., Red for negative, Yellow for neutral, Green for positive)

## Next Steps

Now that you understand color implementation, proceed to our next tutorial: [Tutorial: How to Use Font Families in OMR Documents](/appendices/fonts/) to learn about typography in your OMR forms.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/omr/)
- [Documentation](https://docs.aspose.cloud/omr/)
- [Live Demo](https://products.aspose.app/omr/family)
- [API Reference](https://reference.aspose.cloud/omr/)
- [Blog](https://blog.aspose.cloud/category/omr/)
- [Free Support](https://forum.aspose.cloud/c/omr/8/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
