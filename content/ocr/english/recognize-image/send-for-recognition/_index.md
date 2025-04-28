---
title: Learn to Send Images for Recognition with Aspose.OCR Cloud API Tutorial
weight: 10
url: /recognize-image/send-for-recognition/
description: Step-by-step tutorial on how to send images to Aspose.OCR Cloud API for text extraction. Learn practical OCR implementation for developers.
---

# Tutorial: Learn to Send Images for Recognition with Aspose.OCR Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Prepare an image for submission to the Aspose.OCR Cloud API
- Configure OCR recognition settings for optimal results
- Send an API request to initiate the text extraction process
- Handle the API response and task ID for further processing

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription or free trial
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
- Basic understanding of REST APIs and JSON
- Access to a REST client like cURL, Postman, or your programming language of choice

## Introduction to Image Recognition Process

The Aspose.OCR Cloud API allows you to extract text from various image formats through a simple REST API. The process follows these basic steps:

1. Get an access token for authorization
2. Send the image for recognition
3. Fetch the recognition results using the returned task ID

In this tutorial, we'll focus on step 2 - sending an image for recognition.

## Understanding the Image Submission Endpoint

The Aspose.OCR Cloud API provides an endpoint dedicated to image recognition:

```
https://api.aspose.cloud/v5.0/ocr/RecognizeImage
```

This endpoint accepts POST requests with the image and recognition settings in the request body.

## Step 1: Prepare Your Image

First, you need to prepare the image you want to process. The API accepts images in Base64 encoded format.

Try it yourself:
1. Select an image containing text (PNG, JPEG, TIFF, etc.)
2. Convert it to Base64 using an online converter or a code snippet in your preferred language

### Sample Code to Convert an Image to Base64

Here's how you can convert an image to Base64 in different programming languages:

```python
# Python Example
import base64

with open("sample.png", "rb") as image_file:
    encoded_string = base64.b64encode(image_file.read()).decode('utf-8')
print(encoded_string)
```

```java
// Java Example
import java.util.Base64;
import java.nio.file.Files;
import java.nio.file.Paths;

byte[] fileContent = Files.readAllBytes(Paths.get("sample.png"));
String encodedString = Base64.getEncoder().encodeToString(fileContent);
System.out.println(encodedString);
```

```csharp
// C# Example
using System;
using System.IO;

byte[] imageArray = File.ReadAllBytes("sample.png");
string base64ImageRepresentation = Convert.ToBase64String(imageArray);
Console.WriteLine(base64ImageRepresentation);
```

## Step 2: Configure Recognition Settings

Next, you'll need to define your recognition settings in a JSON object. These settings control how the OCR engine processes your image.

Key settings include:
- `language`: Specifies the language of the text (default: "English")
- `makeSkewCorrect`: Automatically corrects image tilt (default: true)
- `resultType`: Specifies the output format (default: "Text")

### Recognition Settings Reference Table

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `language` | string | "English" | Recognition language |
| `makeSkewCorrect` | boolean | true | Automatically correct image tilt |
| `rotate` | integer | 0 | Manually rotate image (in degrees) |
| `makeBinarization` | boolean | false | Convert image to black and white |
| `makeContrastCorrection` | boolean | true | Increase image contrast |
| `makeUpsampling` | boolean | false | Intelligently upscale image |
| `makeSpellCheck` | boolean | false | Apply spell checking to results |
| `dsrMode` | string | "Regions" | Document structure analysis algorithm |
| `dsrConfidence` | string | "Default" | Threshold for filtering content blocks |
| `resultType` | string | "Text" | Output format |

## Step 3: Create Your API Request

Now you're ready to construct your API request:

### cURL Example

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeImage' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "makeContrastCorrection": true,
    "resultType": "Text"
  }
}'
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token and `YOUR_BASE64_ENCODED_IMAGE` with your Base64-encoded image.

### Expected Response

If successful, the API will return a UUID string which represents your task ID:

```
a197aade-bba9-4c7a-92c7-46851b3dceaa
```

This ID is crucial - you'll need it to fetch your recognition results in the next step of the OCR process.

## Note on Evaluation Mode

If you want to try the API without authentication, you can use the evaluation endpoint:

```
https://api.aspose.cloud/v5.0/ocr/RecognizeImageTrial
```

This endpoint doesn't require authentication, but please note that approximately 10% of words in the results will be masked with asterisks.

## Troubleshooting Tips

- Error 401 (Unauthorized): Check that your access token is valid and correctly formatted in the Authorization header
- Error 400 (Bad Request): Verify your JSON payload format and ensure the image is properly Base64 encoded
- Command Length Issues: Base64 encoded images can be very long. If using command line tools, you may encounter maximum length limits. Consider using a programming language instead of direct cURL commands for large images

## What You've Learned

In this tutorial, you've learned:
- How to prepare and encode an image for OCR processing
- How to configure recognition settings for optimal results
- How to send an image to the Aspose.OCR Cloud API
- How to handle the task ID response for later result retrieval

## Next Steps

Now that you've successfully sent an image for recognition, proceed to the next tutorial to learn [How to Fetch Recognition Results](/recognize-image/fetch-recognition-result/) from the Aspose.OCR Cloud API.

## Further Practice

To reinforce your learning:
1. Try sending images with different recognition settings to understand their impact
2. Compare results between different image types (scanned documents vs. photos)
3. Experiment with the evaluation endpoint to test without authentication

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post on our [support forum](https://forum.aspose.cloud/c/ocr/12/)!
