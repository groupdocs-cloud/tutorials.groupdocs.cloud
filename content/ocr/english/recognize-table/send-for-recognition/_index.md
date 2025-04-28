---
title: How to Send Tables for Recognition with Aspose.OCR Cloud Tutorial
weight: 10
url: /recognize-table/send-for-recognition/
description: Learn how to submit scanned or photographed tables to the Aspose.OCR Cloud API for text extraction in this beginner-friendly tutorial.
---

# Tutorial: How to Send Tables for Recognition

## Learning Objectives

In this tutorial, you'll learn how to:
- Prepare a table image for recognition
- Authenticate with the Aspose.OCR Cloud API
- Configure optimal recognition settings
- Submit a table image for OCR processing
- Handle the API response

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account with an active subscription or free trial
- Client credentials (Client ID and Client Secret) from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
- Basic knowledge of REST APIs and JSON
- A tool for making HTTP requests (cURL, Postman, or your preferred programming language)
- A scanned or photographed table image for testing

## Introduction

Extracting text from tables in scanned documents or photographs is a common requirement in many document processing applications. Aspose.OCR Cloud provides a powerful API that makes this process straightforward. In this tutorial, we'll walk through the process of sending a table image to the Aspose.OCR Cloud API for recognition.

## Real-World Scenario

Imagine you work for a financial services company that receives hundreds of scanned financial statements every day. These statements contain tables with critical data that needs to be extracted and stored in a database. Manually entering this data would be time-consuming and error-prone. By implementing table recognition with Aspose.OCR Cloud, you can automate this process.

## Step 1: Obtain an Access Token

Before sending a table for recognition, you need to authenticate with the Aspose.OCR Cloud API by obtaining an access token.

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

The response will contain an access token that you'll use in the next step:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

## Step 2: Prepare Your Table Image

For this tutorial, you'll need a scanned or photographed table. The image should be:
- Clearly visible with good contrast
- In a common format (JPEG, PNG, BMP, TIFF)
- Reasonably aligned (though the API can correct minor skew)

You'll need to convert your image to a Base64 string. Here's a simple way to do it:

### Using Linux/macOS Terminal:
```bash
base64 -i your_table_image.jpg -o base64_output.txt
```

### Using Windows PowerShell:
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("your_table_image.jpg")) | Out-File base64_output.txt
```

## Step 3: Configure Recognition Settings

Before sending your request, you need to decide on the recognition settings. Here's a breakdown of the available options:

| Setting | Description | Recommendation |
|---------|-------------|----------------|
| `language` | Recognition language | Use "English" for English text |
| `makeSkewCorrect` | Auto-correct image tilt | Enable for slightly tilted images (≤15°) |
| `rotate` | Manual rotation angle | Use if image is rotated >15° |
| `makeBinarization` | Convert to black and white | Enable for low-contrast images |
| `makeContrastCorrection` | Enhance contrast | Enable for poor quality scans |
| `makeUpsampling` | Intelligently upscale image | Enable for small fonts |
| `makeSpellCheck` | Auto-correct misspelled words | Enable for improved accuracy |
| `dsrMode` | Document structure analysis | Use "Regions" for tables |
| `dsrConfidence` | Content block filtering | Use "Default" to start |
| `resultTypeTable` | Result format | Use "Csv" for structured data |

## Step 4: Send the Table for Recognition

Now, let's submit the table image for recognition using a POST request to the Aspose.OCR Cloud API:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeTable' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_IMAGE_STRING",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "rotate": 0,
    "makeBinarization": false,
    "makeContrastCorrection": true,
    "makeUpsampling": false,
    "makeSpellCheck": true,
    "dsrMode": "Regions",
    "dsrConfidence": "Default",
    "resultTypeTable": "Csv"
  }
}'
```

### Try it yourself!

Replace `YOUR_ACCESS_TOKEN` with the token you obtained in Step 1 and `YOUR_BASE64_IMAGE_STRING` with your Base64-encoded image. Run the command and observe the response.

## Step 5: Understand the Response

If your request is successful, the API will return a unique identifier (GUID) for your recognition task, like this:

```
db212989-42b9-422c-8e0d-70acb08474a6
```

This identifier is crucial as you'll use it to fetch the recognition results once processing is complete.

## Troubleshooting Common Issues

### Error: 401 Unauthorized
- Verify that your access token is valid and hasn't expired
- Ensure you're using the correct authorization header format

### Error: 400 Bad Request
- Check that your JSON body is properly formatted
- Ensure your Base64-encoded image string is valid

### Command Line Length Limitations
When using cURL in a shell command, you might encounter errors with very large Base64-encoded strings. Use the `getconf ARG_MAX` command to check the maximum command length on your system. To work around this limitation:
- Save your request body to a file and use `@filename` in your cURL command
- Use a REST client like Postman instead of command line
- Implement the request using a programming language

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the Aspose.OCR Cloud API
- Prepare a table image for recognition
- Configure recognition settings based on your needs
- Send a table image to the API for processing
- Handle the API response

## Next Steps

Now that you've successfully submitted a table for recognition, the next step is to learn how to fetch and process the recognition results. Continue to our next tutorial:

[Tutorial: How to Fetch Table Recognition Results](/recognize-table/fetch-recognition-result/)

## Further Practice

To reinforce what you've learned:
1. Try submitting images with different quality levels to see how the settings affect recognition
2. Experiment with different recognition settings and observe the differences in results
3. Implement the table submission process in your preferred programming language


## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/ocr/12/).