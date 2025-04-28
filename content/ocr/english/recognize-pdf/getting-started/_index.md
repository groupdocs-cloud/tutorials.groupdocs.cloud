---
title: Getting Started with PDF Recognition in Aspose.OCR Cloud Tutorial
weight: 10
url: /recognize-pdf/getting-started/
description: Learn the fundamentals of extracting text from scanned PDF documents using Aspose.OCR Cloud in this beginner-friendly tutorial for developers.
---

# Tutorial: Getting Started with PDF Recognition


## Learning Objectives

By the end of this tutorial, you will be able to:
- Understand the PDF recognition workflow in Aspose.OCR Cloud
- Set up your Aspose Cloud account and obtain API credentials
- Send a basic PDF recognition request
- Retrieve and use the extracted text results
- Define the best approaches for different PDF recognition scenarios

## Prerequisites

Before starting this tutorial, make sure you have:
- A basic understanding of REST APIs
- An API client like cURL, Postman, or your programming language's HTTP library
- A sample scanned PDF document to use for testing

## Understanding PDF Recognition

PDF is one of the most common formats for scanned documents. However, scanned PDFs are essentially images embedded in a document format, which means:
- The text cannot be selected, copied, or searched
- The content cannot be indexed by search engines
- The document size is often much larger than necessary
- The content cannot be easily edited or reformatted

Optical Character Recognition (OCR) technology solves these problems by extracting the text from scanned PDFs, allowing you to:
- Convert scanned documents into searchable PDFs
- Extract plain text for further processing
- Index document content for search
- Edit and manipulate the extracted content

## Step 1: Create an Aspose Cloud Account

First, you need to create an Aspose Cloud account to access the API:

1. Visit [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
2. Click on "Register" to create a new account
3. Verify your email address
4. Log in to your dashboard

### Try it yourself:
Create your Aspose Cloud account if you don't already have one. Explore the dashboard to familiarize yourself with the interface.

## Step 2: Create a New Application

To access the Aspose.OCR Cloud API, you need to create an application and obtain credentials:

1. In your Aspose Cloud Dashboard, navigate to "Applications"
2. Click "Create New Application"
3. Provide a name for your application (e.g., "PDF OCR Tutorial")
4. Select the APIs you want to access (make sure OCR is selected)
5. Click "Create"
6. Note your Client ID and Client Secret - you'll need these for authentication

### Learning Checkpoint:
Why do you need to create an application in the Aspose Cloud Dashboard?

<details>
<summary>Answer</summary>
Creating an application provides you with a Client ID and Client Secret, which are required for authenticating your requests to the Aspose.OCR Cloud API. They serve as your credentials for accessing the service.
</details>

## Step 3: Understand the PDF Recognition Workflow

The Aspose.OCR Cloud PDF recognition process follows a 3-step workflow:

1. Authentication: Obtain an access token using your Client ID and Client Secret
2. Submission: Send your PDF file to the OCR service with recognition settings
3. Retrieval: Fetch the recognition results using the task ID returned from submission

Let's go through each step in detail.

## Step 4: Authentication - Obtaining an Access Token

Before you can use the OCR API, you need to obtain an access token:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with the values from your Aspose Cloud Dashboard.

The response will contain your access token:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

Save this token as you'll need it for subsequent API calls. Note that tokens expire after 1 hour, so you'll need to refresh them for long-running applications.

### Try it yourself:
Generate an access token using cURL or your preferred HTTP client. Make sure to save the token for the next steps.

## Step 5: Submit a PDF for Recognition

Once you have your access token, you can submit a PDF for recognition:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizePdf' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_PDF",
  "settings": {
    "language": "English",
    "resultType": "Text"
  }
}'
```

Replace `YOUR_ACCESS_TOKEN` with the token you obtained in the previous step and `YOUR_BASE64_ENCODED_PDF` with your Base64 encoded PDF file.

The response will be a task ID, which you'll use to retrieve the results:

```
db03b9ea-3eed-4954-a1d4-b2712773bbe
```

### Base64 Encoding Your PDF

To encode your PDF file to Base64, you can use various tools:

Command Line (Linux/Mac):
```bash
base64 -i your_file.pdf | tr -d '\n'
```

Command Line (Windows PowerShell):
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("your_file.pdf"))
```

Online Tools:
There are many online Base64 encoding tools available, but be cautious about uploading sensitive documents to third-party services.

## Step 6: Retrieve Recognition Results

After submitting your PDF, the recognition process is queued. To retrieve the results, you need to poll the API using the task ID:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/RecognizePdf?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the ID you received in the previous step.

The response will be a JSON object with the recognition results:

```json
{
  "id": "db03b9ea-3eed-4954-a1d4-b2712773bbe",
  "taskStatus": "Completed",
  "responseStatusCode": "Ok",
  "results": [
    {
      "type": "Text",
      "data": "SGVsbG8gd29ybGQhIFRoaXMgaXMgYSB0ZXN0IGRvY3VtZW50Lg=="
    }
  ],
  "error": null
}
```

The `results` array contains the recognition output. The `data` field is Base64 encoded, so you'll need to decode it to get the actual text:

Command Line (Linux/Mac):
```bash
echo "SGVsbG8gd29ybGQhIFRoaXMgaXMgYSB0ZXN0IGRvY3VtZW50Lg==" | base64 -d
```

Command Line (Windows PowerShell):
```powershell
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("SGVsbG8gd29ybGQhIFRoaXMgaXMgYSB0ZXN0IGRvY3VtZW50Lg=="))
```

After decoding, you'll get the extracted text from your PDF document.

### Understanding Task Status

The `taskStatus` field in the response can have several values:
- `Pending`: The PDF is queued for recognition but not yet processed
- `Processing`: The PDF is currently being recognized
- `Completed`: The PDF has been successfully recognized
- `Error`: An error occurred during recognition
- `NotExist`: The task ID doesn't exist or has expired

If the status is `Pending` or `Processing`, you should wait a few seconds and try again.

### Try it yourself:
Submit one of your own PDF documents for recognition and retrieve the results. Decode the Base64 data to see the extracted text.

## Step 7: Using Different Recognition Settings

Aspose.OCR Cloud offers various settings to optimize recognition for different types of documents:

```json
{
  "image": "YOUR_BASE64_ENCODED_PDF",
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

### Key Settings Explained:

| Setting | Description | When to Use |
|---------|-------------|-------------|
| `language` | Recognition language | When documents are in a specific language |
| `makeSkewCorrect` | Fix page tilt | When scans are slightly rotated |
| `rotate` | Manual page rotation | For sideways or upside-down pages |
| `makeBinarization` | Convert to black and white | For colored or grayscale documents |
| `makeContrastCorrection` | Improve contrast | For low-contrast scans |
| `makeUpsampling` | Enhance small text | For documents with small fonts |
| `makeSpellCheck` | Apply spell checking | To correct common OCR errors |
| `resultType` | Output format | "Text", "Pdf", "TextAndPdf", or "Json" |

### Result Type Options:

- `Text`: Returns plain text extracted from the PDF
- `Pdf`: Returns a searchable PDF with the recognized text layer
- `TextAndPdf`: Returns both plain text and searchable PDF
- `Json`: Returns JSON with detailed text position information

## What You've Learned

Congratulations! In this tutorial, you've learned:
- The basics of PDF recognition with Aspose.OCR Cloud
- How to set up your Aspose Cloud account and credentials
- The three-step workflow for PDF recognition
- How to authenticate, submit PDFs, and retrieve results
- Different settings to optimize recognition for various documents

## Next Steps

Now that you understand the basics, you can dive deeper into specific aspects of PDF recognition:

- [Tutorial: Sending PDFs for Recognition](/recognize-pdf/send-for-recognition/) - Learn advanced techniques for submitting PDFs
- [Tutorial: Fetching Recognition Results](/recognize-pdf/fetch-recognition-result/) - Master retrieving and processing results
- [Tutorial: Working with the PDF Recognition SDK](/recognize-pdf/recognition-sdk/) - Simplify your workflow with language-specific SDKs

## Further Practice

To reinforce what you've learned, try these exercises:
1. Experiment with different recognition settings to see how they affect the results
2. Process a multi-page PDF and analyze the output
3. Try different result types (Text, PDF, JSON) and compare the outputs

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post in our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance!
