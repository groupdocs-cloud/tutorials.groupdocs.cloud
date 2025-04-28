---
title: How to Fetch Table Recognition Results with Aspose.OCR Cloud Tutorial
weight: 20
url: /recognize-table/fetch-recognition-result/
description: Learn how to retrieve and process table recognition results from the Aspose.OCR Cloud API in this step-by-step tutorial for developers.
---

# Tutorial: How to Fetch Table Recognition Results

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve table recognition results from the Aspose.OCR Cloud API
- Interpret the JSON response structure
- Handle different task statuses
- Process and decode the recognition data
- Implement error handling for robust applications

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [Tutorial: How to Send Tables for Recognition](/recognize-table/send-for-recognition/)
- A valid task ID from a previously submitted table recognition request
- An access token for authentication with the Aspose.OCR Cloud API
- Basic understanding of REST APIs and JSON

## Introduction

After submitting a table for recognition to the Aspose.OCR Cloud API, the next step is to retrieve and process the results. In this tutorial, we'll walk through the process of fetching table recognition results, understanding the response structure, and handling different task statuses.

## Real-World Scenario

Continuing our example from the previous tutorial, imagine you're developing an automated system for a financial services company that processes hundreds of scanned financial statements daily. After submitting tables for recognition, you need to retrieve the extracted data, verify its accuracy, and import it into your database system.

## Step 1: Prepare Your Request

To fetch table recognition results, you'll need:
- The task ID received when you submitted the table for recognition
- Your access token for authentication

## Step 2: Send a Request to Fetch Recognition Results

Use the following cURL command to retrieve the recognition results:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeTable?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the ID you received when submitting the table, and `YOUR_ACCESS_TOKEN` with your valid authentication token.

### Try it yourself!

Copy the command above, replace the placeholders with your actual values, and run it in your terminal or command prompt. Examine the response you receive.

## Step 3: Understand the Response Structure

The API returns a JSON response with the following structure:

```json
{
    "id": "db212989-42b9-422c-8e0d-70acb08474a6",
    "taskStatus": "Completed",
    "responseStatusCode": "Ok",
    "results": [
        {
            "type": "Csv",
            "data": "MCwxCiAgICAgICAgICAgICAgICAgICAgICBTSE9QICAg...ICAgICBUSEFOSyB5byAgSSwKLAo="
        }
    ],
    "error": null
}
```

Let's break down the key components:

| Field | Description |
|-------|-------------|
| `id` | The unique identifier of your table recognition task |
| `taskStatus` | The current status of the recognition task |
| `responseStatusCode` | The status of the API response |
| `results` | An array of recognition results (Base64 encoded) |
| `error` | Error information (if any) |

## Step 4: Handle Different Task Statuses

The `taskStatus` field can have one of the following values:

| Status | Description | What to do |
|--------|-------------|------------|
| `Pending` | The table is queued for recognition | Wait and try again in a few seconds |
| `Processing` | The table is currently being recognized | Wait and try again in a few seconds |
| `Completed` | Recognition is complete | Process the results |
| `Error` | An error occurred during recognition | Check the error messages |
| `NotExist` | The task ID doesn't exist or was deleted | Verify the ID or submit again |

### Implementing a Polling Strategy

Since recognition may take several seconds, implement a polling strategy:

```bash
#!/bin/bash

# Replace with your actual values
TASK_ID="your-task-id-here"
ACCESS_TOKEN="your-access-token-here"

while true; do
    # Fetch the recognition result
    response=$(curl --silent --request GET --location "https://api.aspose.cloud/v5.0/ocr/RecognizeTable?id=$TASK_ID" \
    --header 'Accept: text/plain' \
    --header 'Content-Type: application/json' \
    --header "Authorization: Bearer $ACCESS_TOKEN")
    
    # Extract the task status
    task_status=$(echo $response | grep -o '"taskStatus":"[^"]*"' | cut -d'"' -f4)
    
    echo "Current status: $task_status"
    
    # Check if the task is complete or has an error
    if [ "$task_status" = "Completed" ]; then
        echo "Recognition complete! Processing results..."
        echo $response > result.json
        break
    elif [ "$task_status" = "Error" ]; then
        echo "Error occurred during recognition."
        echo $response > error.json
        break
    elif [ "$task_status" = "NotExist" ]; then
        echo "Task ID does not exist or result has been deleted."
        break
    fi
    
    echo "Waiting for 2 seconds before checking again..."
    sleep 2
done
```

## Step 5: Process the Recognition Results

Once the task status is `Completed`, you need to decode the Base64-encoded results:

### Bash Example:
```bash
#!/bin/bash

# Extract the Base64 encoded data from the JSON response
base64_data=$(cat result.json | grep -o '"data":"[^"]*"' | cut -d'"' -f4)

# Decode the Base64 data
echo $base64_data | base64 --decode > recognized_table.csv

echo "Table recognition results saved to recognized_table.csv"
```

### Python Example:
```python
import json
import base64

# Load the JSON response
with open('result.json', 'r') as file:
    response = json.load(file)

# Check if the task is completed
if response['taskStatus'] == 'Completed':
    # Get the Base64 encoded data
    base64_data = response['results'][0]['data']
    
    # Decode the Base64 data
    decoded_data = base64.b64decode(base64_data)
    
    # Save the decoded data to a file
    with open('recognized_table.csv', 'wb') as output_file:
        output_file.write(decoded_data)
    
    print("Table recognition results saved to recognized_table.csv")
else:
    print(f"Task status: {response['taskStatus']}")
    if response['error']:
        print(f"Error: {response['error']}")
```

## Learning Checkpoint

Before continuing, ensure you can:
- Successfully fetch recognition results using a task ID
- Understand the different task statuses and how to handle them
- Decode the Base64-encoded results into a usable format

## Troubleshooting Common Issues

### Error: 401 Unauthorized
- Your access token may have expired. Generate a new one.
- Ensure you're using the correct authorization header format.

### Error: 404 Not Found
- Verify that the task ID is correct.
- The result may have been deleted (they're stored for 24 hours only).

### Error: Garbled Text in Results
- Make sure you're properly decoding the Base64 data.
- Check if the original image was clear enough for accurate recognition.

## What You've Learned

In this tutorial, you've learned how to:
- Fetch table recognition results from the Aspose.OCR Cloud API
- Interpret the different task statuses
- Implement a polling strategy for asynchronous processing
- Decode and save the recognition results
- Handle common errors and edge cases

## Next Steps

Now that you've mastered the basics of table recognition with REST API calls, you might want to learn how to implement the same functionality using the Aspose.OCR Cloud SDKs for your preferred programming language:

[Tutorial: Implementing Table Recognition with SDKs](/recognize-table/recognition-sdk/)

## Further Practice

To reinforce what you've learned:
1. Create a script that automatically polls for results after submitting a table
2. Implement error handling to gracefully manage different task statuses
3. Build a simple application that processes and visualizes the extracted table data


## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/ocr/12/).