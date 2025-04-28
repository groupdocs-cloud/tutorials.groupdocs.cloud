---
title: How to Fetch Font Identification Results Tutorial
weight: 30

url: /identify-fonts/fetch-recognition-result/
description: Learn how to retrieve and interpret font identification results from the Aspose.OCR Cloud API in this hands-on developer tutorial.
---

# Tutorial: How to Fetch Font Identification Results

## Learning Objectives
In this tutorial, you'll learn:
- How to query the Aspose.OCR Cloud API for font identification results
- How to interpret the JSON response structure
- How to handle different task statuses
- How to decode and use the font identification information


## Prerequisites
- Completed the previous tutorial on [sending images for font identification](/identify-fonts/send-for-recognition/)
- A valid task ID from a previous font identification request
- Basic understanding of JSON data structures

## Understanding the Font Identification Queue

When you submit an image for font identification, Aspose.OCR Cloud processes it in a queue to ensure stable performance even under high load. The font identification process may take several seconds depending on:

- The size and complexity of your image
- Current system load
- The preprocessing options you selected

Because of this queue-based architecture, you need to query the API to check the status of your task and retrieve the results when they're ready.

## Step 1: Construct the Request

To retrieve font identification results, you'll send a GET request to the following endpoint:

```
GET https://api.aspose.cloud/v5.0/ocr/IdentifyFont
```

You'll need to include:
- The task ID as a query parameter
- Your access token in the Authorization header

Let's build the request:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/IdentifyFont?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the ID you received when submitting the image, and `YOUR_ACCESS_TOKEN` with your valid access token.

## Step 2: Understand Task Statuses

The API response includes a `taskStatus` field that indicates the current state of your font identification task. Here are the possible statuses:

| Status | Description | Next Step |
|--------|-------------|-----------|
| Pending | Task is queued but not yet processed | Wait and retry in a few seconds |
| Processing | Task is currently being processed | Wait and retry in a few seconds |
| Completed | Font identification is complete | Retrieve and decode the results |
| Error | An error occurred during processing | Check the error messages |
| NotExist | Task ID doesn't exist or results expired | Verify the ID or resubmit the image |

### Learning Checkpoint
What would you do if you receive a "Processing" status? And what if you receive a "NotExist" status? Think about the appropriate actions before continuing.

## Step 3: Handle the API Response

Let's look at a sample successful response:

```json
{
    "id": "c11c975d-5124-4555-9561-af40fb95ba07",
    "responseStatusCode": "Ok",
    "taskStatus": "Completed",
    "results": [
        {
            "type": "Text",
            "data": "eydzdHlsZSc6ICdyZWd1bGFyJywgJ2ZvbnQnOiBbJ3RhaG9tYScsICd0aW1lcycsICd2ZXJkYW5hJ119"
        }
    ],
    "error": null
}
```

Important fields to note:
- `id`: Matches your request ID
- `taskStatus`: Current state of the task
- `responseStatusCode`: Overall status of the request
- `results`: Contains the font identification data as a Base64-encoded string
- `error`: Contains error messages if any occurred

## Step 4: Decode the Results

The font detection results in the `results` array are Base64-encoded. You need to decode this data to get the actual font information:

### Try It Yourself: Decode Base64 Results

<details>
<summary>Using cURL</summary>

```bash
echo "eydzdHlsZSc6ICdyZWd1bGFyJywgJ2ZvbnQnOiBbJ3RhaG9tYScsICd0aW1lcycsICd2ZXJkYW5hJ119" | base64 --decode
# On Windows PowerShell, use:
# [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("eydzdHlsZSc6ICdyZWd1bGFyJywgJ2ZvbnQnOiBbJ3RhaG9tYScsICd0aW1lcycsICd2ZXJkYW5hJ119"))
```
</details>

<details>
<summary>Python</summary>

```python
import base64

encoded_data = "eydzdHlsZSc6ICdyZWd1bGFyJywgJ2ZvbnQnOiBbJ3RhaG9tYScsICd0aW1lcycsICd2ZXJkYW5hJ119"
decoded_data = base64.b64decode(encoded_data).decode('utf-8')
print(decoded_data)
```
</details>

<details>
<summary>C#</summary>

```csharp
using System;
using System.Text;

string encodedData = "eydzdHlsZSc6ICdyZWd1bGFyJywgJ2ZvbnQnOiBbJ3RhaG9tYScsICd0aW1lcycsICd2ZXJkYW5hJ119";
byte[] data = Convert.FromBase64String(encodedData);
string decodedData = Encoding.UTF8.GetString(data);
Console.WriteLine(decodedData);
```
</details>

After decoding, you'll get JSON data that looks like this:

```json
{'style': 'regular', 'font': ['tahoma', 'times', 'verdana']}
```

This shows:
- `style`: The most common text style in the image (regular, bold, italic)
- `font`: A list of detected fonts in order of best matching

## Step 5: Implement Polling for Results

Since the font identification process is asynchronous, you might need to poll the API until the results are ready. Here's a simple approach:

```javascript
// Pseudocode for polling
function getFontIdentificationResults(taskId, maxRetries = 10, delayMs = 1000) {
  let retries = 0;
  
  function checkStatus() {
    // Make API request to get results
    const response = fetchResults(taskId);
    
    if (response.taskStatus === "Completed") {
      // Process completed results
      const encodedData = response.results[0].data;
      const decodedData = decodeBase64(encodedData);
      return JSON.parse(decodedData);
    } else if (response.taskStatus === "Error") {
      // Handle error
      throw new Error(`Font identification failed: ${response.error.messages}`);
    } else if (response.taskStatus === "NotExist") {
      // Task doesn't exist
      throw new Error("Task ID not found or expired");
    } else if (retries < maxRetries) {
      // Still processing or pending, retry after delay
      retries++;
      setTimeout(checkStatus, delayMs);
    } else {
      // Max retries reached
      throw new Error("Maximum polling attempts reached");
    }
  }
  
  return checkStatus();
}
```

## Step 6: Complete Working Example

Let's put everything together in a complete cURL example:

```bash
#!/bin/bash

# Variables
ACCESS_TOKEN="your_access_token"
TASK_ID="your_task_id"
API_ENDPOINT="https://api.aspose.cloud/v5.0/ocr/IdentifyFont"
MAX_RETRIES=10
RETRY_DELAY=2

# Function to fetch and process results
fetch_results() {
  echo "Fetching results for task: $TASK_ID"
  
  RESPONSE=$(curl --silent --request GET --location "$API_ENDPOINT?id=$TASK_ID" \
    --header "Accept: text/plain" \
    --header "Content-Type: application/json" \
    --header "Authorization: Bearer $ACCESS_TOKEN")
  
  # Extract task status
  TASK_STATUS=$(echo $RESPONSE | grep -o '"taskStatus":"[^"]*"' | cut -d'"' -f4)
  
  echo "Current status: $TASK_STATUS"
  
  if [ "$TASK_STATUS" = "Completed" ]; then
    # Extract Base64 data
    BASE64_DATA=$(echo $RESPONSE | grep -o '"data":"[^"]*"' | cut -d'"' -f4)
    
    # Decode Base64 data
    DECODED_DATA=$(echo $BASE64_DATA | base64 --decode)
    
    echo "Font identification results:"
    echo $DECODED_DATA
  elif [ "$TASK_STATUS" = "Error" ]; then
    echo "Error occurred during font identification"
    echo $RESPONSE
  elif [ "$TASK_STATUS" = "NotExist" ]; then
    echo "Task ID not found or expired"
  else
    echo "Task still processing, will retry in $RETRY_DELAY seconds..."
    sleep $RETRY_DELAY
    return 1  # Indicate need to retry
  fi
  
  return 0  # Indicate completion
}

# Main polling loop
RETRY_COUNT=0
while [ $RETRY_COUNT -lt $MAX_RETRIES ]; do
  fetch_results
  
  if [ $? -eq 0 ]; then
    break  # Task completed or failed
  fi
  
  RETRY_COUNT=$((RETRY_COUNT + 1))
done

if [ $RETRY_COUNT -eq $MAX_RETRIES ]; then
  echo "Maximum retry attempts reached"
fi
```

## Important Considerations

### Result Expiration
Font identification results are stored in Aspose Cloud for 24 hours after the image was submitted. Make sure to retrieve and store your results within this timeframe.

### Error Handling
Even when results are returned successfully, check the `error` field for non-fatal warnings that might affect the accuracy of the font identification.

## What You've Learned
In this tutorial, you've learned:
- How to query the Aspose.OCR Cloud API for font identification results
- How to interpret different task statuses and response fields
- How to decode Base64-encoded font identification data
- How to implement polling to efficiently retrieve results

## Next Steps
Continue to the next tutorial: [Implementing Font Identification with SDK](/identify-fonts/sdk-implementation/) to learn how to streamline the font identification process using the provided software development kit.

## Further Practice
- Create a script that automatically polls for results with exponential backoff
- Build a simple application that displays font identification results in a user-friendly format
- Experiment with different images and compare the font detection accuracy

## Helpful Resources
- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
