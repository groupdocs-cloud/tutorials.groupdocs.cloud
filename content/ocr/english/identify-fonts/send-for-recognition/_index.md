---
title: How to Send Images for Font Identification Tutorial
weight: 20
url: /identify-fonts/send-for-recognition/
description: Learn step-by-step how to prepare and submit images to the Aspose.OCR Cloud API for font identification in this practical developer tutorial.
---

# Tutorial: How to Send Images for Font Identification

## Learning Objectives
In this tutorial, you'll learn:
- How to prepare images for font identification
- How to construct the API request for font identification
- How to configure image preprocessing settings
- How to handle the API response

## Prerequisites
- Basic understanding of REST APIs
- Familiarity with JSON
- An active Aspose Cloud account with valid credentials
- An image with clear text for testing

## Preparing Your Environment

Before we begin sending images for font identification, you need to set up your environment:

1. Sign in to your [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
2. Note your Client ID and Client Secret or generate new ones if needed
3. Ensure you have an image file ready for testing

## Understanding the Font Identification Endpoint

The Aspose.OCR Cloud API provides a dedicated endpoint for font identification:

```
POST https://api.aspose.cloud/v5.0/ocr/IdentifyFont
```

This endpoint accepts a JSON payload containing your image and configuration settings.

## Step 1: Obtain an Access Token

Before sending an image for font identification, you need to authenticate with the Aspose.OCR Cloud service. Let's see how to get an access token:

```bash
curl -X POST "https://api.aspose.cloud/connect/token" \
  -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
  -H "Content-Type: application/x-www-form-urlencoded"
```

The response will include an `access_token` that you'll use in subsequent requests.

## Step 2: Prepare Your Image

For font identification to work effectively, your image should:
- Contain clearly visible text
- Have good contrast between text and background
- Be in a common format (JPEG, PNG, BMP, etc.)

You'll need to encode your image as a Base64 string to include it in the API request. Here's how to do it in different languages:

### Try It Yourself: Encode an Image to Base64

<details>
<summary>Using cURL</summary>

```bash
base64 -i your_image.png
# On Windows PowerShell, use:
# [Convert]::ToBase64String([IO.File]::ReadAllBytes("your_image.png"))
```
</details>

<details>
<summary>Python</summary>

```python
import base64

with open("your_image.png", "rb") as image_file:
    encoded_string = base64.b64encode(image_file.read()).decode('utf-8')
    print(encoded_string)
```
</details>

<details>
<summary>C#</summary>

```csharp
using System;
using System.IO;

string filePath = "your_image.png";
byte[] imageBytes = File.ReadAllBytes(filePath);
string base64String = Convert.ToBase64String(imageBytes);
Console.WriteLine(base64String);
```
</details>

## Step 3: Configure Font Identification Settings

The API allows you to configure various preprocessing options to improve font identification accuracy. Here are the available settings:

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `language` | string | `English` | Specify a language for font identification |
| `makeSkewCorrect` | boolean | `true` | Automatically correct image tilt (deskew) |
| `rotate` | integer | `0` | Rotate an image by the specified degree |
| `makeBinarization` | boolean | `false` | Convert image to black and white |
| `makeContrastCorrection` | boolean | `true` | Increase contrast before identification |
| `makeUpsampling` | boolean | `false` | Upscale image to improve small font detection |
| `resultType` | string | `Text` | The result type (always use `Text` for font detection) |

### Image Preprocessing Order

When multiple preprocessing options are enabled, they're applied in this order:
1. Contrast correction
2. Skew correction
3. Upsampling

## Step 4: Send the Image for Font Identification

Now, let's build the request to send an image for font identification:

```json
{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "rotate": 0,
    "makeBinarization": false,
    "makeContrastCorrection": true,
    "makeUpsampling": false,
    "resultType": "Text"
  }
}
```

### Try It Yourself: Complete cURL Example

Let's put everything together in a complete cURL example:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/IdentifyFont' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "makeSkewCorrect": true,
    "resultType": "Text"
  }
}'
```

## Step 5: Handle the API Response

If your request is successful, the API will return a unique identifier (GUID) for your font identification task:

```
c11c975d-5124-4555-9561-af40fb95ba07
```

This ID will be used in the next tutorial to retrieve the font identification results.

### Troubleshooting Tips

If you encounter issues when sending images for font identification, check these common problems:

- Error 401: Your access token may be invalid or expired. Generate a new one.
- Error 400: Check your request payload for formatting issues.
- Command too long: For very large images, the Base64 string can exceed command line limits. Consider using a programming language instead of cURL.

## What You've Learned
In this tutorial, you've learned:
- How to authenticate with the Aspose.OCR Cloud API
- How to prepare and encode images for font identification
- How to configure preprocessing settings for optimal results
- How to send images to the API and handle the response

## Next Steps
Now that you've successfully submitted an image for font identification, continue to the next tutorial: [How to Fetch Font Identification Results](/identify-fonts/fetch-recognition-result/) to learn how to retrieve and interpret the results.

## Further Practice
- Try sending different types of images with various fonts
- Experiment with different preprocessing settings
- Create a script that automates the image encoding and submission process

## Helpful Resources
- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
