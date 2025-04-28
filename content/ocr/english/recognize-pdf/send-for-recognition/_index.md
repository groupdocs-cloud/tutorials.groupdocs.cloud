---
title: How to Send PDFs for Recognition with Aspose.OCR Cloud Tutorial
weight: 20
url: /recognize-pdf/send-for-recognition/
description: Learn step-by-step how to submit scanned PDF documents to Aspose.OCR Cloud API for text extraction in this beginner-friendly tutorial.
---

# Tutorial: How to Send PDFs for Recognition

## Learning Objectives

By the end of this tutorial, you will be able to:
- Properly format a PDF recognition request
- Submit a scanned PDF document to Aspose.OCR Cloud API
- Configure recognition settings to optimize text extraction
- Handle the API response for further processing

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account with an active subscription or free trial
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
- Basic knowledge of REST APIs and HTTP requests
- An API client like cURL, Postman, or your programming language's HTTP library
- A sample scanned PDF document to use for testing

## Understanding the Process

When working with scanned PDF documents, the first step is to submit them to the Aspose.OCR Cloud API for processing. This tutorial focuses specifically on how to properly send your PDF files to the recognition service.

## Step 1: Obtain an Access Token

Before sending any PDF for recognition, you need to authenticate your request with an access token.

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
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

## Step 2: Prepare Your PDF File

For this tutorial, you'll need to convert your PDF file to a Base64 string. Here's how you can do it:

### Using Command Line:

Linux/macOS:
```bash
base64 -i your_pdf_file.pdf | tr -d '\n'
```

Windows (PowerShell):
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("your_pdf_file.pdf"))
```

### Try it yourself:
Convert a small PDF document to Base64 using the method appropriate for your operating system. For large files, consider using a programming language that can handle the conversion more efficiently.

## Step 3: Configure Recognition Settings

Aspose.OCR Cloud allows you to customize how your PDF is processed. Let's look at the available settings:

```json
{
  "image": "YOUR_BASE64_PDF_STRING",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "rotate": 0,
    "makeBinarization": false,
    "makeContrastCorrection": true,
    "makeUpsampling": false,
    "makeSpellCheck": false,
    "dsrMode": "Regions",
    "dsrConfidence": "Default",
    "resultType": "Text"
  }
}
```

Let's understand these settings:

| Setting | Purpose | Recommended Value |
|---------|---------|-------------------|
| `language` | Recognition language | Choose based on your document's language |
| `makeSkewCorrect` | Fix tilted pages | `true` for slightly skewed documents |
| `rotate` | Rotate page if needed | `0` for normal orientation |
| `makeBinarization` | Convert to black and white | `false` for most documents |
| `makeContrastCorrection` | Improve contrast | `true` for low contrast scans |
| `makeUpsampling` | Enhance small text | `true` for documents with small fonts |
| `makeSpellCheck` | Fix recognition errors | `true` for improved accuracy |
| `dsrMode` | Document structure analysis | `Regions` for most documents |
| `dsrConfidence` | Content block filtering | `Default` for balanced results |
| `resultType` | Output format | `Text`, `Pdf`, `TextAndPdf` or `Json` |

### Learning Checkpoint:
What setting would you adjust if your PDF has small text that's difficult to read?

<details>
<summary>Answer</summary>
You would set <code>makeUpsampling</code> to <code>true</code> to intellectually upscale the content for better recognition of small fonts.
</details>

## Step 4: Send the PDF for Recognition

Now that you have your access token and have prepared your PDF file, you can send it for recognition:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizePdf' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "JVBERi0xLjUNJeLjz9...YOUR_BASE64_PDF_STRING...g0xMTYNJSVFT0YN",
  "settings": {
    "language": "English",
    "makeSpellCheck": true,
    "resultType": "Text"
  }
}'
```

### Try it yourself:
Create a complete request with your own access token and PDF file. Start with basic settings and gradually experiment with different parameters to see how they affect recognition quality.

## Step 5: Understanding the Response

If your request is successful, the API will return a unique task ID (GUID) as a plain text response:

```
db03b9ea-3eed-4954-a1d4-b2712773bbe
```

This ID is crucial as it will be used to retrieve the recognition results once processing is complete.

### Troubleshooting Tips:

- 413 Request Entity Too Large: Your Base64 encoded PDF might be too large. Try splitting your document into smaller parts or using a different approach to submit the file.
- 401 Unauthorized: Check that your access token is valid and properly formatted in the Authorization header.
- 400 Bad Request: Verify your JSON structure, especially when encoding the Base64 string.

## What You've Learned

Congratulations! In this tutorial, you've learned how to:
- Authenticate with the Aspose.OCR Cloud API
- Convert a PDF file to Base64 format
- Configure optimal recognition settings for your document
- Submit a PDF document for OCR processing
- Handle the task ID response for later retrieval

## Next Steps

Now that you know how to send PDFs for recognition, the next logical step is to learn how to retrieve and process the recognition results. Continue your learning journey with our next tutorial:

[Tutorial: Fetching PDF Recognition Results](/recognize-pdf/fetch-recognition-result/)

## Further Practice

To reinforce what you've learned, try these exercises:
1. Submit the same PDF with different recognition settings and compare the results
2. Create a script in your preferred programming language that automates the PDF submission process
3. Process a multi-page PDF document and analyze the response

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post in our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance!
