---
title: How to Send Labels for Recognition with Aspose.OCR Cloud API Tutorial
weight: 20
url: /recognize-label/send-for-recognition/
description: Learn how to properly submit photographed labels and signs to the Aspose.OCR Cloud API for text extraction in this step-by-step developer tutorial.
---

# Tutorial: How to Send Labels for Recognition with Aspose.OCR Cloud API


## Learning Objectives

In this tutorial, you'll learn:
- How to prepare a label image for submission to the OCR API
- How to structure the JSON request body with proper parameters
- How to send a POST request to the Aspose.OCR Cloud API endpoint
- How to handle the API response and prepare for result retrieval

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
- Basic understanding of REST APIs and JSON
- A tool for making API requests (like cURL, Postman, or your preferred programming language)
- An image of a label or sign that you want to recognize

## Practical Scenario

Imagine you're developing a mobile application that helps users translate text from street signs and product labels when traveling abroad. The first step is to send the captured photo to the OCR service for text extraction. This tutorial will show you how to implement this critical first step.

## Tutorial Steps

### 1. Obtain an Access Token

Before sending any label for recognition, you need to authenticate with the Aspose.OCR Cloud API by obtaining an access token.

```bash
curl -X POST "https://api.aspose.cloud/connect/token" \
  -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
  -H "Content-Type: application/x-www-form-urlencoded"
```

The response will contain your access token:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

Save this token as you'll need it for the next steps.

### 2. Prepare Your Image

You need to convert your image to a Base64 encoded string. Here's how you can do it:

For Web Applications (JavaScript):
```javascript
// Function to convert an image file to Base64
function getBase64(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.readAsDataURL(file);
    reader.onload = () => {
      // Remove the "data:image/png;base64," part from the string
      let encoded = reader.result.toString().replace(/^data:(.*,)?/, '');
      resolve(encoded);
    };
    reader.onerror = error => reject(error);
  });
}

// Example usage with a file input
document.getElementById('fileInput').addEventListener('change', async (e) => {
  const file = e.target.files[0];
  const base64String = await getBase64(file);
  console.log(base64String); // This is what you'll send to the API
});
```

For Command Line:
```bash
base64 -i your_label_image.jpg
```

> Note: The Base64 string can be quite long for high-resolution images. Some platforms have limitations on the length of command-line arguments. If you encounter issues, consider using a programming language or tool that doesn't have these limitations.

### 3. Structure Your JSON Request

Create a JSON object with your image and recognition settings:

```json
{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "makeUpsampling": false,
    "resultType": "Text"
  }
}
```

Let's understand each setting:

| Setting | Description | Recommended Value |
|---------|-------------|-------------------|
| `language` | The language of the text in the image | Currently only `English` is supported |
| `makeSkewCorrect` | Automatically correct tilted images | `true` for photos taken at an angle |
| `makeUpsampling` | Upscale image to improve recognition of small text | `true` for small or distant signs |
| `resultType` | Format of the recognition result | `Text` for plain text extraction |

> Tip: If you're dealing with distant signs or small text on labels, try setting `makeUpsampling` to `true` to improve recognition accuracy.

### 4. Send the POST Request

Now you're ready to send your label for recognition:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeLabel' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "makeUpsampling": false,
    "resultType": "Text"
  }
}'
```

### 5. Handle the Response

If your request is successful, you'll receive a unique identifier (GUID) in the response body:

```
a197aade-bba9-4c7a-92c7-46851b3dceaa
```

This identifier is crucial as it's what you'll use to fetch the recognition results once the processing is complete.

> Important: Save this GUID immediately! You'll need it to retrieve your results in the next step of the process.

## Try It Yourself

Now it's your turn to try sending a label for recognition:

1. Choose a clear image of a sign or label
2. Convert it to Base64
3. Prepare your JSON request with appropriate settings
4. Send the POST request to the API endpoint
5. Save the returned GUID for the next tutorial

## Troubleshooting Common Issues

| Issue | Possible Solution |
|-------|-------------------|
| "Invalid image format" error | Ensure your Base64 encoding is correct and the image format is supported (JPG, PNG, etc.) |
| Request timeout | Your image might be too large. Try compressing it before encoding to Base64 |
| Authentication error | Check that your access token is valid and properly formatted in the Authorization header |
| 403 Forbidden error | Verify that your subscription is active and has sufficient credits |

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with the Aspose.OCR Cloud API
- How to prepare and encode an image for recognition
- How to structure the request body with appropriate parameters
- How to send the request and handle the initial response

## Next Steps

Now that you know how to send a label for recognition, the next step is to learn how to fetch and process the recognition results. Continue to our next tutorial:

[Tutorial: Learn to Fetch Label Recognition Results](/recognize-label/fetch-recognition-result/)

## Further Practice

To reinforce your learning:
1. Try sending different types of labels (road signs, price tags, food labels)
2. Experiment with different recognition settings to see how they affect results
3. Create a simple script that automates the process of encoding and sending images

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance!
