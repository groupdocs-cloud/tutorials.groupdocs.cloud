---
title: How to Fetch PDF Recognition Results from Aspose.OCR Cloud Tutorial
weight: 30
url: /recognize-pdf/fetch-recognition-result/
description: Learn how to retrieve and process OCR results from scanned PDF documents submitted to Aspose.OCR Cloud in this step-by-step developer tutorial.
---

# Tutorial: How to Fetch PDF Recognition Results

## Learning Objectives

By the end of this tutorial, you will be able to:
- Retrieve OCR results for previously submitted PDF documents
- Interpret the recognition response structure
- Handle different task statuses effectively
- Process and use the extracted text data
- Implement a polling mechanism for checking result availability

## Prerequisites

Before starting this tutorial, make sure you have:
- Completed the [Tutorial: Sending PDFs for Recognition](/recognize-pdf/send-for-recognition/)
- A valid task ID from a previously submitted PDF document
- An active access token for Aspose.OCR Cloud API
- Basic knowledge of REST APIs and JSON parsing

## Understanding the Process

After submitting a PDF for recognition, the processing happens asynchronously in the Aspose.OCR Cloud queue. This tutorial focuses on how to properly retrieve and handle the recognition results once processing is complete.

## Step 1: Prepare Your Request

To fetch recognition results, you'll need two key pieces of information:
1. Your access token (obtained during authentication)
2. The task ID returned when you submitted the PDF

## Step 2: Send a Request to Fetch Results

Use the following endpoint to retrieve the recognition results:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/RecognizePdf?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the actual task ID you received when submitting the PDF, and `YOUR_ACCESS_TOKEN` with your valid authentication token.

### Try it yourself:
Create and send a request using your own task ID and access token. If you're using a programming language instead of cURL, implement the equivalent HTTP GET request with the proper headers.

## Step 3: Understanding the Response Structure

The API response will be a JSON object with the following structure:

```json
{
    "id": "db03b9ea-3eed-4954-a1d4-b2712773bbe",
    "taskStatus": "Completed",
    "responseStatusCode": "Ok",
    "results": [
        {
            "type": "Pdf",
            "data": "JVBERi0xLjQKJfbk...HJlZgo0NTU2NQolJUVPRgo="
        }
    ],
    "error": null
}
```

Let's understand each field:

| Field | Description |
|-------|-------------|
| `id` | The unique identifier of your recognition task |
| `taskStatus` | Current state of the recognition task in the queue |
| `responseStatusCode` | Overall status of the recognition operation |
| `results` | Array containing the recognition output |
| `error` | Any error messages or warnings (null if no errors) |

The `results` array contains objects with:
- `type`: Format of the result (Text, Pdf, etc.)
- `data`: Base64 encoded content of the recognition result

## Step 4: Handling Different Task Statuses

The `taskStatus` field can have several values, each requiring different handling:

| Status | Meaning | What to do |
|--------|---------|------------|
| `Pending` | Still in queue | Wait and try again later |
| `Processing` | Currently being recognized | Wait and check again |
| `Completed` | Recognition finished | Process the results |
| `Error` | Recognition failed | Check error messages |
| `NotExist` | Invalid task ID or expired | Resubmit the PDF |

### Learning Checkpoint:
If you receive a `Pending` status, what should your application do next?

<details>
<summary>Answer</summary>
Your application should wait for a short period (e.g., 1-2 seconds) and then make another GET request using the same task ID to check if the status has changed.
</details>

## Step 5: Implementing a Polling Mechanism

For production applications, you'll want to implement a polling mechanism that checks the status periodically until the task is complete:

```javascript
// Pseudocode for polling mechanism
function checkResults(taskId, maxAttempts = 10, interval = 2000) {
  let attempts = 0;
  
  function poll() {
    attempts++;
    
    // Make API request to check status
    const response = fetchResults(taskId);
    
    if (response.taskStatus === "Completed") {
      // Process the completed results
      processResults(response.results);
      return;
    }
    
    if (response.taskStatus === "Error") {
      // Handle error condition
      handleError(response.error);
      return;
    }
    
    if (attempts >= maxAttempts) {
      // Maximum attempts reached
      handleTimeout();
      return;
    }
    
    if (response.taskStatus === "Pending" || response.taskStatus === "Processing") {
      // Status still indicates work in progress, wait and try again
      setTimeout(poll, interval);
    }
  }
  
  // Start polling
  poll();
}
```

### Try it yourself:
Implement a simple polling mechanism in your preferred programming language that checks for results every 2 seconds, up to a maximum of 10 attempts.

## Step 6: Processing the Results

Once the task status is `Completed`, you need to process the Base64 encoded data in the results:

```javascript
// Pseudocode for processing results
function processResults(results) {
  results.forEach(result => {
    // Decode the Base64 data
    const decodedData = atob(result.data);
    
    // Handle the data based on result type
    if (result.type === "Text") {
      // Process as plain text
      displayOrSaveText(decodedData);
    } else if (result.type === "Pdf") {
      // Process as PDF
      saveAsPdf(decodedData);
    } else if (result.type === "Json") {
      // Process as JSON
      processJsonData(JSON.parse(decodedData));
    }
  });
}
```

### Base64 Decoding Examples:

JavaScript:
```javascript
const decodedText = atob(base64String);
```

Python:
```python
import base64
decoded_text = base64.b64decode(base64_string).decode('utf-8')
```

Java:
```java
byte[] decodedBytes = Base64.getDecoder().decode(base64String);
String decodedText = new String(decodedBytes, StandardCharsets.UTF_8);
```

C#:
```csharp
byte[] decodedBytes = Convert.FromBase64String(base64String);
string decodedText = Encoding.UTF8.GetString(decodedBytes);
```

## Troubleshooting Tips

- Result Not Found: Remember that results are only stored for 24 hours. If you're trying to fetch results for an older task, you'll need to resubmit the PDF.
- Authorization Errors: Make sure your access token is still valid. Tokens expire after a certain period.
- Empty or Incorrect Results: Verify that your original PDF was properly submitted and that the recognition settings were appropriate for your document.

## What You've Learned

Congratulations! In this tutorial, you've learned how to:
- Make API calls to retrieve OCR results
- Interpret the different task statuses
- Implement a polling mechanism for checking result availability
- Process and decode the Base64 encoded results
- Handle potential errors and edge cases

## Next Steps

Now that you know how to send PDFs for recognition and retrieve the results, you might want to simplify your workflow by using the Aspose.OCR Cloud SDK. Continue your learning journey with our next tutorial:

[Tutorial: Working with the PDF Recognition SDK](/recognize-pdf/recognition-sdk/)

## Further Practice

To reinforce what you've learned, try these exercises:
1. Create a complete script that both submits a PDF and retrieves results with proper error handling
2. Implement different waiting strategies for polling (exponential backoff, etc.)
3. Process different result types (Text, PDF, JSON) and compare their contents

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post in our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance!
