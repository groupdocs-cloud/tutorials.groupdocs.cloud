---
title: How to Manage Barcode Text and Captions Tutorial
url: /generating-saving-barcode/manage-text/
weight: 40
description: Step-by-step tutorial on customizing barcode text, including visibility, positioning, font settings, and adding captions using Aspose.BarCode Cloud API
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Control barcode text visibility and positioning
- Adjust spacing between bars and text
- Customize barcode text font settings 
- Configure text wrapping behavior
- Add and customize top and bottom captions
- Replace text in 2D barcodes for improved readability

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account (sign up for a [free trial](https://dashboard.aspose.cloud/#/apps) if needed)
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
- Basic knowledge of REST APIs
- Completed the basic barcode generation tutorial or have equivalent knowledge
- A development environment for your preferred language (C#, Java, PHP, Python, Node.js, or Go)

## Introduction

The text elements in a barcode provide human-readable information that complements the machine-readable bars or patterns. Aspose.BarCode Cloud API offers extensive options to customize how text appears on your barcodes, enhancing both readability and visual appeal.

In this tutorial, we'll explore how to work with three different types of text elements:
1. Main barcode text (the human-readable representation of the encoded data)
2. Top caption (additional text displayed above the barcode)
3. Bottom caption (additional text displayed below the barcode)

## Understanding Barcode Text Structure

A barcode image can include the following text elements:

![Barcode Text Structure](barcode_text_scheme.png)

Each element has customizable properties like visibility, position, font, and spacing.

## Step 1: Controlling Barcode Text Visibility

You can choose whether to display the human-readable text on your barcode:

### cURL Example for Hiding Barcode Text

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/no-text-barcode.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&codeTextVisible=false" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `codeTextVisible=false`: Hides the barcode text

### cURL Example for Showing Barcode Text

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/with-text-barcode.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&codeTextVisible=true" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `codeTextVisible=true`: Shows the barcode text (this is the default)

### Try it yourself:
Generate both versions and compare their appearance and suitability for different use cases.

## Step 2: Positioning Barcode Text

Control the position of text relative to the barcode bars:

### Text Location (Vertical Position)

### cURL Example for Text Above Barcode

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/text-above.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&codeTextLocation=Above" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `codeTextLocation=Above`: Places text above the barcode

### cURL Example for Text Below Barcode

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/text-below.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&codeTextLocation=Below" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `codeTextLocation=Below`: Places text below the barcode (this is the default)

### Text Alignment (Horizontal Position)

Control the horizontal alignment of barcode text:

### cURL Example for Left-Aligned Text

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/text-left.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&codeTextAlignment=Left" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `codeTextAlignment=Left`: Aligns text to the left

### Available alignment options:
- Left
- Center (default)
- Right

### Try it yourself:
Create barcodes with different combinations of text location and alignment to see how they affect the overall appearance.

## Step 3: Adjusting Spacing Between Bars and Text

Control the gap between the barcode and its text:

### cURL Example for Custom Text Spacing

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/text-spacing.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&codeTextSpace=10" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `codeTextSpace=10`: Sets a 10-pixel gap between the barcode and text

### Note:
This parameter is not applicable to certain barcode types like EAN-13, EAN-8, UPC-E, UPC-A, ISMN, ISBN, ISSN, and UpcaGs1DatabarCoupon.

### Try it yourself:
1. Generate barcodes with different spacing values (5, 20, 40 pixels)
2. Compare how spacing affects readability and overall barcode size

## Step 4: Customizing Font Settings

Control the appearance of the text by adjusting font properties:

### cURL Example for Custom Font

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/custom-font.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&codeTextFontName=Courier&codeTextFontSize=16&codeTextFontStyle=Bold" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameters:
- `codeTextFontName=Courier`: Sets the font family
- `codeTextFontSize=16`: Sets the font size in points
- `codeTextFontStyle=Bold`: Sets the font style

### Available font styles:
- Regular
- Bold
- Italic
- Underline
- Strikeout

### Font Mode Options

Aspose.BarCode Cloud also offers automatic font sizing based on barcode dimensions:

### cURL Example for Auto Font Mode

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/auto-font.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&fontMode=Auto&width=300&height=150" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `fontMode=Auto`: Automatically determines the font size based on barcode dimensions

### Font Mode options:
- Auto: Automatically sizes the font
- Manual: Uses the explicitly provided font size

### Try it yourself:
Generate barcodes with different font settings and compare their appearance.

## Step 5: Configuring Text Wrapping

For long text that might not fit on a single line:

### cURL Example for Text Wrapping

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/wrapped-text.png/generate?text=This%20is%20a%20very%20long%20text%20string%20that%20demonstrates%20text%20wrapping%20in%20barcodes&type=Code128&format=png&codeTextNoWrap=false&width=200" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `codeTextNoWrap=false`: Allows text to wrap onto multiple lines if needed

### cURL Example for No Wrap Mode

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/no-wrap-text.png/generate?text=This%20is%20a%20very%20long%20text%20string%20that%20demonstrates%20text%20nowrap%20in%20barcodes&type=Code128&format=png&codeTextNoWrap=true&width=200" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `codeTextNoWrap=true`: Forces text to remain on a single line

### Try it yourself:
1. Generate barcodes with very long text
2. Compare the wrapped and unwrapped versions
3. Consider which is more suitable for your specific use case

## Step 6: Replacing Display Text in 2D Barcodes

For 2D barcodes, you can replace the displayed text with a more readable alternative:

### cURL Example for Custom Display Text

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/custom-display-text.png/generate?text=https%3A%2F%2Fwww.aspose.cloud%2F&type=QR&format=png&twoDDisplayText=Visit%20Aspose%20Cloud" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `twoDDisplayText=Visit%20Aspose%20Cloud`: Shows "Visit Aspose Cloud" instead of the encoded URL

### Note:
This is applicable to 2D symbologies like QR Code, Data Matrix, Aztec, PDF417, and MaxiCode.

## Step 7: Adding and Customizing Captions

Captions provide additional text information above or below the barcode:

### cURL Example for Caption Above

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/caption-above.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&captionAboveText=PRODUCT%20CODE&captionAboveVisible=true" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameters:
- `captionAboveText=PRODUCT%20CODE`: Sets the caption text
- `captionAboveVisible=true`: Makes the caption visible

### cURL Example for Caption Below

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/caption-below.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&captionBelowText=SCAN%20HERE&captionBelowVisible=true" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameters:
- `captionBelowText=SCAN%20HERE`: Sets the caption text
- `captionBelowVisible=true`: Makes the caption visible

## Step 8: Caption Alignment and Padding

Control the horizontal position and spacing of captions:

### cURL Example for Caption Alignment

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/caption-aligned.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&captionAboveText=PRODUCT%20CODE&captionAboveVisible=true&captionAboveAlignment=Left" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `captionAboveAlignment=Left`: Aligns the caption to the left

### Available alignment options:
- Left
- Center (default)
- Right

### cURL Example for Caption Padding

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/caption-padding.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&captionAboveText=PRODUCT%20CODE&captionAboveVisible=true&captionAbovePadding=20" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `captionAbovePadding=20`: Sets 20-pixel padding for the caption

## Step 9: Caption Font Customization

Customize the appearance of caption text:

### cURL Example for Caption Font

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/caption-font.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&captionAboveText=PRODUCT%20CODE&captionAboveVisible=true&captionAboveFontName=Arial&captionAboveFontSize=20&captionAboveFontStyle=Bold" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameters:
- `captionAboveFontName=Arial`: Sets the caption font family
- `captionAboveFontSize=20`: Sets the caption font size
- `captionAboveFontStyle=Bold`: Sets the caption font style

## Step 10: Caption Text Wrapping

Control text wrapping behavior for captions:

### cURL Example for Caption No Wrap

```bash
curl -v "https://api.aspose.cloud/v3.0/barcode/caption-nowrap.png/generate?text=ASPOSE-CLOUD&type=Code128&format=png&captionAboveText=This%20is%20a%20very%20long%20caption%20text%20to%20demonstrate%20wrapping%20behavior&captionAboveVisible=true&captionAboveNoWrap=true&width=200" \
     -X PUT \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json"
```

Key parameter:
- `captionAboveNoWrap=true`: Forces caption to remain on a single line

## Step 11: Implementing in Python

Let's implement text and caption customization in Python:

```python
# Tutorial Code Example: Customize barcode text and captions in Python
import requests
import json

# Replace with your actual credentials
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"
auth_url = "https://api.aspose.cloud/oauth2/token"
api_base_url = "https://api.aspose.cloud/v3.0/barcode/"

def get_access_token():
    """Get OAuth2 access token"""
    payload = {
        'grant_type': 'client_credentials',
        'client_id': client_id,
        'client_secret': client_secret
    }
    
    headers = {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Accept': 'application/json'
    }
    
    response = requests.post(auth_url, data=payload, headers=headers)
    response.raise_for_status()
    
    return response.json()['access_token']

def generate_barcode_with_text_options(access_token, file_name, params):
    """Generate barcode with custom text and caption options"""
    # Build request URL with filename
    request_url = f"{api_base_url}{file_name}/generate"
    
    headers = {
        'Authorization': f'Bearer {access_token}',
        'Content-Type': 'application/json',
        'Accept': 'application/json'
    }
    
    # Make PUT request to save to cloud
    response = requests.put(request_url, params=params, headers=headers)
    response.raise_for_status()
    
    return response.json()

if __name__ == "__main__":
    # Get access token
    token = get_access_token()
    
    # Example 1: Custom text position and alignment
    params_text_position = {
        'text': 'TEXT-POSITION',
        'type': 'Code128',
        'format': 'png',
        'codeTextLocation': 'Above',
        'codeTextAlignment': 'Left'
    }
    result1 = generate_barcode_with_text_options(
        token, "text-position-left-above.png", params_text_position
    )
    print("Generated barcode with custom text position")
    
    # Example 2: Custom font settings
    params_font = {
        'text': 'FONT-SETTINGS',
        'type': 'Code128',
        'format': 'png',
        'codeTextFontName': 'Verdana',
        'codeTextFontSize': 16,
        'codeTextFontStyle': 'Bold'
    }
    result2 = generate_barcode_with_text_options(
        token, "custom-font-barcode.png", params_font
    )
    print("Generated barcode with custom font")
    
    # Example 3: Adding captions
    params_captions = {
        'text': 'CAPTION-DEMO',
        'type': 'Code128',
        'format': 'png',
        'captionAboveText': 'SCAN THIS CODE',
        'captionAboveVisible': True,
        'captionBelowText': 'THANK YOU',
        'captionBelowVisible': True
    }
    result3 = generate_barcode_with_text_options(
        token, "barcode-with-captions.png", params_captions
    )
    print("Generated barcode with captions")
    
    # Example 4: 2D barcode with custom display text
    params_2d = {
        'text': 'https://products.aspose.cloud/barcode/',
        'type': 'QR',
        'format': 'png',
        'twoDDisplayText': 'Visit our Barcode Cloud'
    }
    result4 = generate_barcode_with_text_options(
        token, "2d-custom-text.png", params_2d
    )
    print("Generated 2D barcode with custom display text")
    
    print("All text customization examples completed successfully!")
```

## Troubleshooting Common Issues

### Issue: Caption text not appearing
- Possible cause: Caption visibility not set to true
- Solution: Ensure `captionAboveVisible=true` or `captionBelowVisible=true` is included

### Issue: Text alignment not working
- Possible cause: Using unsupported alignment value
- Solution: Use only "Left", "Center", or "Right" for alignment parameters

### Issue: Font settings not applied
- Possible cause: Font name misspelled or unsupported font requested
- Solution: Use standard, widely available fonts like Arial, Verdana, Times New Roman

### Issue: Custom display text not showing in 2D barcode
- Possible cause: Using with an unsupported barcode type
- Solution: Ensure you're using a supported 2D barcode type like QR Code or Data Matrix

## What You've Learned

Congratulations! In this tutorial, you've learned how to:
- Control barcode text visibility
- Position text above or below barcode bars
- Align text horizontally (left, center, right)
- Adjust spacing between bars and text
- Customize font settings for barcode text
- Configure text wrapping behavior
- Replace display text in 2D barcodes
- Add and customize top and bottom captions
- Control caption alignment, padding, and font
- Implement these customizations programmatically

## Further Practice

To reinforce your learning:
1. Create barcodes with different combinations of text properties
2. Create a barcode with both top and bottom captions, each with different alignment
3. Experiment with different font styles and observe how they affect readability
4. Create a QR code with a custom display text that's more user-friendly than the encoded data

## Next Steps

Now that you've mastered barcode text and captions, you might want to explore how to customize barcode colors:

[Tutorial: Customize Barcode Color Schemes](/generating-saving-barcode/manage-color/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/barcode/)
- [Documentation](https://docs.aspose.cloud/barcode/)
- [Live Demo](https://products.aspose.app/barcode/family)
- [API Reference](https://reference.aspose.cloud/barcode/)
- [Blog](https://blog.aspose.cloud/category/barcode/)
- [Free Support](https://forum.aspose.cloud/c/barcode/6/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/barcode/6/).