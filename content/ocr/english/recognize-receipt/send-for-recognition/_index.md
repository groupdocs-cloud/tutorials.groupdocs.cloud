---
title: How to Send Receipts for Recognition with Aspose.OCR Cloud Tutorial
weight: 20
url: /recognize-receipt/send-for-recognition/
description: Learn how to submit receipt images to Aspose.OCR Cloud API for text extraction in this hands-on developer tutorial with code examples.
---

# Tutorial: How to Send Receipts for Recognition

## Learning Objectives

In this tutorial, you'll learn:
- How to prepare receipt images for optimal recognition
- How to construct and send an API request to process receipts
- How to configure recognition settings for best results
- How to handle the API response and task ID

## Prerequisites

Before starting this tutorial, ensure you have:
- An [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps) (free trial available)
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- Basic understanding of REST API concepts
- A tool for making HTTP requests (like cURL, Postman, or your preferred programming language)
- A receipt image in a supported format (JPG, PNG, etc.)

## Introduction

Receipt recognition is the process of extracting structured text from scanned or photographed receipts. This is a critical first step in automating expense reporting, reimbursement workflows, and financial record-keeping. In this tutorial, we'll focus on how to submit receipt images to the Aspose.OCR Cloud API for recognition.

## Practical Scenario

Imagine you're developing an expense reporting application for a company. Employees need to submit receipts for reimbursement, but manually entering receipt data is time-consuming and error-prone. By implementing receipt recognition, you can automatically extract the relevant information, improving both accuracy and user experience.

## Step-by-Step Guide

### 1. Obtain an Access Token

Before sending a receipt for recognition, you need to authorize your request with an access token.

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
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

Save this token as you'll need it for subsequent API calls.

### 2. Prepare Your Receipt Image

For optimal recognition results:
- Ensure good lighting and contrast in the receipt image
- Avoid shadows and glare
- Keep the receipt flat and unwrinkled
- Make sure all text is visible and within frame

### 3. Convert the Image to Base64

The API requires your receipt image to be Base64 encoded. Here's how to convert it:

```bash
# Linux/macOS
base64 -i receipt.jpg | tr -d '\n'

# Windows PowerShell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("receipt.jpg"))
```

### 4. Configure Recognition Settings

Aspose.OCR Cloud offers several settings to optimize recognition. For the best results with receipts, consider the following:

```json
{
  "language": "English",
  "makeSkewCorrect": true,
  "makeContrastCorrection": true,
  "makeSpellCheck": true,
  "resultType": "Text"
}
```

Settings explained:
- `language`: The primary language of the receipt text
- `makeSkewCorrect`: Corrects tilted images automatically
- `makeContrastCorrection`: Enhances contrast for better recognition
- `makeSpellCheck`: Corrects common OCR errors
- `resultType`: Specifies the format of the result (Text, JSON, etc.)

### 5. Send the Receipt for Recognition

Now, construct your API request to send the receipt for recognition:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeReceipt' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "makeContrastCorrection": true,
    "makeSpellCheck": true,
    "resultType": "Text"
  }
}'
```

### 6. Handle the Response

If successful, the API will return a unique task ID, which you'll use to fetch the recognition results:

```
3f030db3-de56-4acb-8469-d696be9dc9a2
```

This ID represents your recognition task in the Aspose.OCR Cloud queue. Save this ID as you'll need it to retrieve the recognition results.

## Try It Yourself

Now it's your turn to practice what you've learned:

1. Obtain your access token using your Aspose Cloud credentials
2. Take a photo of a receipt or use a sample receipt image
3. Convert the image to Base64
4. Prepare your API request with appropriate recognition settings
5. Send the request and note the task ID

## SDK Implementation

For those who prefer working with SDKs, Aspose.OCR Cloud provides libraries for various programming languages. Here's how to send a receipt for recognition using the .NET SDK:

```csharp
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;
using System.Text;

// Authorize your requests
RecognizeReceiptApi recognizeReceiptApi = new RecognizeReceiptApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Read receipt image
byte[] receipt = File.ReadAllBytes("receipt.png");

// Specify recognition settings
OCRSettingsRecognizeReceipt recognitionSettings = new OCRSettingsRecognizeReceipt {
    Language = Language.English,
    MakeSkewCorrect = true,
    MakeContrastCorrection = true,
    MakeSpellCheck = true,
    ResultType = ResultType.Text
};

// Send receipt for recognition
OCRRecognizeReceiptBody source = new OCRRecognizeReceiptBody(receipt, recognitionSettings);
string taskID = recognizeReceiptApi.PostRecognizeReceipt(source);

Console.WriteLine($"Receipt submitted for recognition. Task ID: {taskID}");
```

## Troubleshooting

Common issues and solutions:

### Error: "Base64 encoded file is too large"
- Solution: Try reducing the image size or using a more efficient image format

### Error: "Invalid token"
- Solution: Ensure your access token is valid and not expired; obtain a new token if necessary

### No response or timeout
- Solution: Check your internet connection or try again later; the API might be experiencing high load

## What You've Learned

In this tutorial, you've learned:
- How to obtain an authentication token for Aspose.OCR Cloud API
- How to prepare and encode receipt images for recognition
- How to configure recognition settings for optimal results
- How to send an API request for receipt recognition
- How to handle the API response and task ID

## Next Steps

Now that you know how to send receipts for recognition, the next step is to learn how to fetch and process the recognition results. Continue to our next tutorial: [Tutorial: Fetching and Processing Recognition Results](/recognize-receipt/fetch-recognition-result/).

## Further Practice

To reinforce your learning:
1. Try sending receipts with different languages and compare results
2. Experiment with different recognition settings
3. Build a simple script that automates the entire receipt submission process
4. Try with receipts of varying quality to understand the system's limitations

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Visit our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance.
