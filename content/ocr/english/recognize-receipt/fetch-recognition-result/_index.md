---
title: Fetching and Processing Receipt Recognition Results Tutorial
weight: 30
url: /recognize-receipt/fetch-recognition-result/
description: Learn how to retrieve, decode and handle text extraction results from Aspose.OCR Cloud API in this practical developer tutorial.
---

# Tutorial: Fetching and Processing Receipt Recognition Results

## Learning Objectives

In this tutorial, you'll learn:
- How to check the status of your receipt recognition task
- How to retrieve recognition results from the Aspose.OCR Cloud API
- How to decode Base64-encoded result data
- How to handle different result formats
- How to implement error handling for recognition tasks

## Prerequisites

Before starting this tutorial, ensure you have:
- Completed the [Tutorial: How to Send Receipts for Recognition](/recognize-receipt/send-for-recognition/)
- A task ID from a receipt recognition request
- Your access token for Aspose.OCR Cloud API
- Basic understanding of JSON and Base64 encoding

## Introduction

After submitting a receipt for recognition, the next step is to retrieve and process the results. In this tutorial, we'll explore how to fetch recognition results from the Aspose.OCR Cloud API, decode the data, and handle various response scenarios.

## Practical Scenario

Continuing with our expense reporting application example, after an employee submits a receipt image, your application needs to retrieve the extracted text data. This data can then be used to automatically populate expense report fields, reducing manual entry and improving accuracy.

## Step-by-Step Guide

### 1. Understanding the Recognition Queue

When you submit a receipt for recognition, it enters a processing queue. The recognition process typically takes a few seconds, depending on:
- The size and quality of the original image
- The current load on the Aspose.OCR Cloud service
- The complexity of the receipt content

### 2. Check Recognition Status

Before retrieving results, it's a good practice to check the status of your recognition task:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeReceipt?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the ID received when submitting the receipt, and `YOUR_ACCESS_TOKEN` with your valid access token.

### 3. Understand Task Statuses

The API response will include a `taskStatus` field with one of these values:

| Status | Description | Action |
|--------|-------------|--------|
| Pending | Task is queued but not yet processed | Wait and check again in a few seconds |
| Processing | Task is currently being processed | Wait and check again in a few seconds |
| Completed | Recognition is finished | Proceed to extract results |
| Error | An error occurred during recognition | Check error messages and troubleshoot |
| NotExist | Task ID is invalid or expired | Verify ID or resubmit the receipt |

### 4. Retrieve Recognition Results

Once the task status is `Completed`, you can retrieve the recognition results from the same response:

```json
{
	"id": "3f030db3-de56-4acb-8469-d696be9dc9a2",
	"taskStatus": "Completed",
	"responseStatusCode": "Ok",
	"results": [
		{
			"type": "Text",
			"data": "QWNtZSBDb3Jwb3JhdGlvbgo...5pcyBiYWxscyAkMw=="
		}
	],
	"error": null
}
```

### 5. Decode the Result Data

The recognition results are returned as Base64-encoded strings in the `data` field. You need to decode this data to get the actual text:

```bash
# Linux/macOS
echo "QWNtZSBDb3Jwb3JhdGlvbgo...5pcyBiYWxscyAkMw==" | base64 --decode

# Windows PowerShell
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("QWNtZSBDb3Jwb3JhdGlvbgo...5pcyBiYWxscyAkMw=="))
```

### 6. Handle Different Result Types

Depending on the `resultType` specified in your recognition request, the decoded data will be in different formats:

- Text: Plain text extracted from the receipt
- JSON: Structured data with text and position information
- XML: Structured data in XML format
- PDF: Searchable PDF document
- HOCR: HTML-based format with positional data

### 7. Implement Polling with Exponential Backoff

Since recognition takes time, implement a polling strategy with exponential backoff to efficiently check for results:

```javascript
// Pseudocode for polling with exponential backoff
async function getRecognitionResult(taskId, accessToken) {
  let maxRetries = 10;
  let waitTime = 1000; // Start with 1 second
  
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    // Wait before checking
    await new Promise(resolve => setTimeout(resolve, waitTime));
    
    // Check result
    const response = await checkRecognitionStatus(taskId, accessToken);
    
    if (response.taskStatus === "Completed") {
      return decodeResult(response.results[0].data);
    } else if (response.taskStatus === "Error") {
      throw new Error(`Recognition failed: ${response.error.messages.join(", ")}`);
    } else if (response.taskStatus === "NotExist") {
      throw new Error("Invalid or expired task ID");
    }
    
    // Exponential backoff
    waitTime = Math.min(waitTime * 2, 10000); // Double wait time, max 10 seconds
  }
  
  throw new Error("Recognition timed out");
}
```

## Try It Yourself

Now it's your turn to practice what you've learned:

1. Use the task ID from your previous recognition request
2. Implement a polling mechanism to check the status
3. Once completed, retrieve and decode the results
4. Process the extracted text according to your needs

## SDK Implementation

Here's how to fetch recognition results using the .NET SDK:

```csharp
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;
using System.Text;

// Authorize your requests
RecognizeReceiptApi recognizeReceiptApi = new RecognizeReceiptApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Fetch recognition result using the task ID
string taskId = "3f030db3-de56-4acb-8469-d696be9dc9a2"; // The ID received from submission
OCRResponse result = recognizeReceiptApi.GetRecognizeReceipt(taskId);

// Check the status
if (result.TaskStatus == TaskStatus.Completed)
{
    // Decode the Base64-encoded result
    string recognizedText = Encoding.UTF8.GetString(result.Results[0].Data);
    Console.WriteLine("Recognition result:");
    Console.WriteLine(recognizedText);
}
else if (result.TaskStatus == TaskStatus.Error)
{
    Console.WriteLine($"Recognition failed: {string.Join(", ", result.Error.Messages)}");
}
else
{
    Console.WriteLine($"Task is still {result.TaskStatus}. Try again later.");
}
```

## Advanced: Processing the Extracted Text

Once you have the extracted text, you can process it further:

1. Parse key-value pairs: Extract merchant name, date, total amount
2. Categorize expenses: Automatically categorize based on merchant or items
3. Validate data: Check for missing information or inconsistencies
4. Format for reporting: Prepare the data for integration with accounting systems

## Troubleshooting

Common issues and solutions:

### Task Status Remains "Processing" for Too Long
- Solution: Check if your image is very large or complex; try resubmitting with a smaller or clearer image

### "NotExist" Status Shortly After Submission
- Solution: Verify the task ID is correct; IDs are case-sensitive

### Empty or Incomplete Results
- Solution: The receipt might be low quality or have poor contrast; try enhancing the image and resubmitting

### Error Decoding Base64 Data
- Solution: Ensure you're using the entire Base64 string without truncation; check for special characters

## What You've Learned

In this tutorial, you've learned:
- How to check the status of a receipt recognition task
- How to implement an efficient polling strategy
- How to retrieve and decode recognition results
- How to handle different task statuses and error scenarios
- How to access the extracted text data

## Next Steps

Now that you know how to retrieve and process recognition results, you might want to learn how to integrate this functionality into your applications using language-specific SDKs. Continue to our next tutorial: [Tutorial: Working with the Aspose.OCR Cloud SDK for Receipt Recognition](/recognize-receipt/recognition-sdk/).

## Further Practice

To reinforce your learning:
1. Create a simple script that polls for results automatically
2. Implement error handling for different task statuses
3. Try processing different types of receipts and analyze the results
4. Build a simple data extraction function to pull structured information from the text

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Visit our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance.
