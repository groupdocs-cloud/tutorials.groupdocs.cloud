---
title: How to Fetch Region Recognition Results Tutorial
weight: 30
url: /recognize-regions/fetch-recognition-result/
description: Learn how to retrieve, parse and process OCR results from previously submitted region recognition tasks in this step-by-step developer tutorial.
---

# Tutorial: How to Fetch Region Recognition Results

## Learning Objectives

In this tutorial, you'll learn:
- How to check the status of a region recognition task
- How to retrieve OCR results from the Aspose.OCR Cloud queue
- How to decode and process the recognition results
- How to handle different task statuses and potential errors

## Prerequisites

- Completion of the [Send Image Regions for Recognition](/recognize-regions/send-for-recognition/) tutorial
- A task ID from a previously submitted region recognition request
- Basic understanding of REST APIs and JSON
- A tool for making API requests (cURL, Postman, or a programming language of your choice)

## Practical Scenario

You've submitted ID cards or forms for region-based OCR processing and received task IDs. Now you need to retrieve the extracted text from each defined region and integrate it into your application's workflow, perhaps populating a database or validating user information.

## Step 1: Understand the Recognition Queue

When you submit an image for region recognition, Aspose.OCR Cloud processes it asynchronously:

1. The image enters a processing queue
2. Processing may take several seconds depending on image size and server load
3. Results are stored for 24 hours after submission
4. You need to poll for results using the task ID you received

This asynchronous approach ensures API stability even under high load conditions.

## Step 2: Retrieve the Recognition Results

To fetch recognition results, send a GET request to the Aspose.OCR Cloud API endpoint with your task ID:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeRegions?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the ID you received when submitting the image, and `YOUR_ACCESS_TOKEN` with your valid authentication token.

## Step 3: Understand the Response Format

The API returns a JSON response containing the recognition status and results:

```json
{
	"id": "2ce30237-86da-41ef-88e9-84f0b7acffc0",
	"taskStatus": "Completed",
	"responseStatusCode": "Ok",
	"results": [
		{
			"type": "Text",
			"data": "QWxsIG1lbiBsaXZlIGVudmVsb3BlZCBpbiB3aGFsZS1saW5lcy4="
		},
		{
			"type": "Text",
			"data": "VGhlIHdoaXRlIHdoYWxlIHN3YW0gYmVmb3JlIGhpbS4="
		}
	],
	"error": null
}
```

Key components of the response:
- `id`: The task ID (matching your request)
- `taskStatus`: Current state of the recognition task
- `responseStatusCode`: Recognition response status
- `results`: Array of recognition results, one entry per defined region
- `error`: Error messages, if any

## Step 4: Handle Different Task Statuses

The `taskStatus` field indicates the current state of your recognition task:

| Status | Description | Action |
|--------|-------------|--------|
| Pending | In queue, not yet processed | Wait and retry in a few seconds |
| Processing | Currently being recognized | Wait and retry |
| Completed | Recognition finished | Process the results |
| Error | Recognition failed | Check the error message |
| NotExist | Invalid task ID or expired result | Verify ID or resubmit |

## Try it Yourself: Poll for Results with Exponential Backoff

Here's a Python example that polls for results with an exponential backoff strategy:

```python
import requests
import json
import time
import base64

def fetch_recognition_results(task_id, access_token, max_attempts=10):
    url = f"https://api.aspose.cloud/v5.0/ocr/RecognizeRegions?id={task_id}"
    headers = {
        "Accept": "text/plain",
        "Content-Type": "application/json",
        "Authorization": f"Bearer {access_token}"
    }
    
    attempt = 1
    delay = 1  # Initial delay in seconds
    
    while attempt <= max_attempts:
        print(f"Attempt {attempt}: Checking recognition status...")
        
        response = requests.get(url, headers=headers)
        
        if response.status_code == 200:
            result = response.json()
            status = result.get("taskStatus")
            
            print(f"Current status: {status}")
            
            if status == "Completed":
                print("Recognition completed successfully!")
                return result
            elif status == "Error":
                print(f"Error occurred: {result.get('error')}")
                return result
            elif status == "NotExist":
                print("Task ID not found or results expired")
                return None
            else:  # Pending or Processing
                print(f"Waiting for {delay} seconds before next attempt...")
                time.sleep(delay)
                delay *= 2  # Exponential backoff
                attempt += 1
        else:
            print(f"Error: {response.status_code}")
            print(response.text)
            return None
    
    print("Maximum attempts reached. Please check status manually.")
    return None

# Usage
task_id = "YOUR_TASK_ID"
access_token = "YOUR_ACCESS_TOKEN"
result = fetch_recognition_results(task_id, access_token)

if result and result.get("taskStatus") == "Completed":
    # Process the results
    for i, region_result in enumerate(result.get("results", [])):
        # Decode the Base64 data
        decoded_text = base64.b64decode(region_result.get("data")).decode('utf-8')
        print(f"Region {i} text: {decoded_text}")
```

## Step 5: Decode and Process the Results

Important: All results (including plain text) are returned as Base64 encoded strings. You must decode them before using:

```python
import base64

# Assuming 'data' contains the Base64 encoded string from the results
encoded_data = "QWxsIG1lbiBsaXZlIGVudmVsb3BlZCBpbiB3aGFsZS1saW5lcy4="
decoded_text = base64.b64decode(encoded_data).decode('utf-8')
print(decoded_text)  # Outputs: "All men live enveloped in whale-lines."
```

## Working with Different Result Types

When submitting your recognition request, you can specify different result types:

| Result Type | Description | Use Case |
|-------------|-------------|----------|
| Text | Plain text | Simple text extraction |
| JSON | Structured data | When you need word positions |
| XML | XML formatted | For document structure |
| PDF | Searchable PDF | For archiving |

Each type requires slightly different processing after decoding.

## Troubleshooting Tips

Common issues and solutions:

1. Task Status Remains "Pending" or "Processing" for Too Long
   - Large images may take longer to process
   - Consider using exponential backoff when polling
   - If it never completes, try resubmitting with a smaller image

2. "NotExist" Status
   - Verify your task ID is correct
   - Results are stored for only 24 hours
   - If expired, resubmit your image

3. Decoding Errors
   - Ensure you're properly decoding the Base64 string
   - Check for transmission errors that might corrupt the Base64 data

4. Empty or Incorrect Results
   - Verify your region coordinates
   - Check image quality and text clarity
   - Try different recognition settings

## What You've Learned

In this tutorial, you've learned:
- How to retrieve OCR results from the Aspose.OCR Cloud queue
- How to handle different task statuses
- How to decode Base64 encoded recognition results
- Best practices for polling and processing results

## Next Steps

Now that you can send images for recognition and retrieve results, learn how to implement this workflow in your applications using the Aspose.OCR Cloud SDK:

[Tutorial: Building Applications with Aspose.OCR Cloud SDK for Region Recognition](/recognize-regions/recognition-sdk/)

## Further Practice

To strengthen your understanding:
- Create a complete workflow that sends images and retrieves results
- Implement error handling and retry logic
- Process multiple recognition tasks in parallel
- Store and analyze recognition results

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/ocr/12/).
