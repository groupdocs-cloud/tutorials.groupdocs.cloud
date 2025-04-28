---
title: How to Fetch Recognition Results from Aspose.OCR Cloud API Tutorial
weight: 20
url: /recognize-image/fetch-recognition-result/
description: Learn to retrieve and process OCR results from the Aspose.OCR Cloud queue in this step-by-step tutorial for developers implementing text extraction.
---

# Tutorial: How to Fetch Recognition Results from Aspose.OCR Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve OCR results from the Aspose.OCR Cloud queue using a task ID
- Interpret different task statuses and handle them appropriately
- Process the JSON response to extract the recognized text
- Decode Base64-encoded results to usable text format
- Implement polling strategies for pending tasks

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [Send Images for Recognition](/recognize-image/send-for-recognition/) tutorial
- A valid task ID from a previous image recognition request
- An access token for Aspose.OCR Cloud API authorization
- Basic understanding of REST APIs and JSON

## Introduction

When you [submit an image for recognition](/recognize-image/send-for-recognition/) to Aspose.OCR Cloud API, the processing happens asynchronously. The service returns a task ID, which you then use to fetch the recognition results once processing is complete. This tutorial focuses on this crucial second step - retrieving and processing the results.

## Understanding the Recognition Workflow

Aspose.OCR Cloud uses a queue-based system to ensure stability even under high load. Here's how the workflow operates:

1. You submit an image for recognition and receive a task ID
2. The image enters a processing queue
3. The OCR engine processes the image when resources are available
4. You retrieve the results using the task ID

This queue-based approach allows the API to handle many requests efficiently while maintaining consistent performance.

## Step 1: The Results Endpoint

To fetch recognition results, you'll use the following endpoint:

```
https://api.aspose.cloud/v5.0/ocr/RecognizeImage
```

This requires a GET request with your task ID as a query parameter, along with your authorization token.

## Step 2: Create Your API Request

Here's how to construct your API request:

### cURL Example

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeImage?id=c4b60313-4f78-45f8-b708-069eb98dc22e' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_ACCESS_TOKEN` with your actual access token and `c4b60313-4f78-45f8-b708-069eb98dc22e` with your specific task ID.

## Step 3: Understanding the Response

The API returns a JSON response with the following structure:

```json
{
    "id": "c4b60313-4f78-45f8-b708-069eb98dc22e",
    "taskStatus": "Completed",
    "responseStatusCode": "Ok",
    "results": [
        {
            "type": "Text",
            "data": "QWxsIG1lbiBsaXZlIGVudmVsb3BlZCBpbiB3aGFsZS1saW5lcy4="
        }
    ],
    "error": null
}
```

Let's break down the key components:

| Field | Description |
|-------|-------------|
| `id` | The unique identifier of your recognition task |
| `taskStatus` | Current state of the recognition task in the queue |
| `responseStatusCode` | Recognition response status |
| `results` | Array of recognition results (depends on the `resultType` specified) |
| `error/messages` | Error messages, if any occurred during processing |

## Step 4: Interpreting Task Statuses

The `taskStatus` field indicates where your request is in the processing pipeline:

| Status | Meaning | Action |
|--------|---------|--------|
| `Pending` | In queue, not yet processed | Wait and try again in a few seconds |
| `Processing` | Currently being processed | Wait and try again shortly |
| `Completed` | Processing finished successfully | Extract results from response |
| `Error` | Processing failed | Check error messages for details |
| `NotExist` | Invalid task ID or expired result | Verify ID or resubmit image |

## Try It Yourself

1. Use the task ID from your previous image submission
2. Send a GET request to the endpoint with your task ID
3. Check the `taskStatus` field to confirm completion
4. If `Completed`, proceed to decode the results

### Handling Non-Completed Tasks

If your task is not yet completed (status is `Pending` or `Processing`), implement a polling strategy:

```python
# Python example of a simple polling strategy
import requests
import time
import base64
import json

def fetch_results(task_id, access_token, max_attempts=10, delay=2):
    url = f"https://api.aspose.cloud/v5.0/ocr/RecognizeImage?id={task_id}"
    headers = {
        "Accept": "text/plain",
        "Content-Type": "application/json",
        "Authorization": f"Bearer {access_token}"
    }
    
    for attempt in range(max_attempts):
        response = requests.get(url, headers=headers)
        result = response.json()
        
        if result["taskStatus"] == "Completed":
            # Decode the Base64 result
            text_data = base64.b64decode(result["results"][0]["data"]).decode('utf-8')
            return text_data
        elif result["taskStatus"] == "Error":
            raise Exception(f"Recognition error: {result.get('error', {}).get('messages')}")
        elif result["taskStatus"] == "NotExist":
            raise Exception("Task ID not found or results expired")
        else:
            # Still pending or processing, wait and try again
            print(f"Status: {result['taskStatus']}. Waiting {delay} seconds...")
            time.sleep(delay)
            
    raise Exception("Maximum polling attempts reached")
```

## Step 5: Decoding the Results

The recognition results in the `data` field are Base64 encoded. You need to decode them to get the actual text:

```javascript
// JavaScript example
const base64Result = "QWxsIG1lbiBsaXZlIGVudmVsb3BlZCBpbiB3aGFsZS1saW5lcy4=";
const decodedText = atob(base64Result);
console.log(decodedText);
// Output: "All men live enveloped in whale-lines."
```

```csharp
// C# example
using System;
using System.Text;

string base64Result = "QWxsIG1lbiBsaXZlIGVudmVsb3BlZCBpbiB3aGFsZS1saW5lcy4=";
byte[] data = Convert.FromBase64String(base64Result);
string decodedText = Encoding.UTF8.GetString(data);
Console.WriteLine(decodedText);
// Output: "All men live enveloped in whale-lines."
```

```java
// Java example
import java.util.Base64;

String base64Result = "QWxsIG1lbiBsaXZlIGVudmVsb3BlZCBpbiB3aGFsZS1saW5lcy4=";
byte[] decodedBytes = Base64.getDecoder().decode(base64Result);
String decodedText = new String(decodedBytes);
System.out.println(decodedText);
// Output: "All men live enveloped in whale-lines."
```

## Note on Evaluation Mode

If you used the evaluation endpoint to submit your image, use this endpoint to fetch results:

```
https://api.aspose.cloud/v5.0/ocr/RecognizeImageTrial?id={task ID}
```

Remember that evaluation mode will mask approximately 10% of the words with asterisks.

## Troubleshooting Tips

- Empty Results: If recognition completed but no text was returned, the image might not contain readable text or the quality might be too low
- Task Not Found: Results are stored for only 24 hours - ensure you fetch them within this timeframe
- Error Status: Check the `error` field for specific error messages to diagnose issues
- Timeout Issues: For large or complex images, increase your polling interval and maximum attempts

## What You've Learned

In this tutorial, you've learned:
- How to retrieve OCR results using a task ID
- How to interpret different task statuses
- How to implement a polling strategy for pending tasks
- How to decode Base64-encoded results to usable text
- How long results are stored in the cloud (24 hours)

## Best Practices

1. Implement Exponential Backoff: For polling, start with short intervals that gradually increase
2. Add Error Handling: Always check task status and handle errors appropriately
3. Process Results Promptly: Remember that results expire after 24 hours
4. Verify Decoding: Always confirm your Base64 decoding is working correctly

## Next Steps

Now that you've successfully retrieved OCR results, proceed to the next tutorial to learn how to [Implement OCR with Language-Specific SDKs](/recognize-image/recognition-sdk/) to streamline your development process.

## Further Practice

To reinforce your learning:
1. Create a complete script that handles both submission and retrieval
2. Add robust error handling for different task statuses
3. Implement a configurable polling strategy with exponential backoff

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post on our [support forum](https://forum.aspose.cloud/c/ocr/12/)!
