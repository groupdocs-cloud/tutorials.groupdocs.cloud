---
title: Fetching Form Recognition Results Tutorial
weight: 20
url: /recognize-form/fetch-results/
description: Learn in this tutorial how to retrieve and process OMR form recognition results from Aspose.OMR Cloud API with practical code examples.
---

# Tutorial: Fetching Form Recognition Results

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve OMR form recognition results using the task ID
- Parse and decode different result formats (CSV, JSON, XML)
- Handle common response scenarios and errors
- Process and utilize the recognition data in your applications

## Prerequisites

Before starting this tutorial, make sure you have:

1. Completed the [Tutorial: How to Send OMR Forms for Recognition](/recognize-form/send-for-recognition/)
2. A valid task ID from a previously submitted form
3. An active access token for the Aspose.OMR Cloud API
4. Basic knowledge of HTTP requests and data formats

## Understanding Result Retrieval

When you submit a form for recognition as learned in the previous tutorial, Aspose.OMR Cloud processes it asynchronously and provides a task ID. This approach ensures stability even under high loads. The recognition process typically takes a few seconds, after which you can fetch the results using this ID.

Recognition results are available for 24 hours after submission, after which they are automatically deleted from the cloud storage.

## Step 1: Preparing the Request

To fetch recognition results, we need to make a GET request to the results endpoint with our task ID.

### The Endpoint

```
https://api.aspose.cloud/v5.0/omr/RecognizeTemplate/GetRecognizeTemplate
```

### Required Parameters

- `id`: The task ID received when submitting the form for recognition
- Authorization header with your access token

## Step 2: Sending the Request

Let's construct and send our request to retrieve the recognition results:

### Try it yourself:

```bash
curl --location 'https://api.aspose.cloud/v5.0/omr/RecognizeTemplate/GetRecognizeTemplate?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the task ID you received from the previous tutorial, and `YOUR_ACCESS_TOKEN` with your valid access token.

## Step 3: Understanding the Response

If successful, you'll receive a JSON response containing the recognition results:

```json
{
  "id": "85bc0ca8-a76e-44f6-a7a3-e6263f7e24ee",
  "responseStatusCode": "Ok",
  "results": [
    {
      "type": "Csv",
      "data": "RWxlbWVudCBOYW1lLFZhbHVlLApBbmltYWxzOTEsIiIKQW5pbWFsczky...gpQbGFudHM5MCwiIgo="
    }
  ],
  "error": null
}
```

Let's break down this response:

- `id`: The task ID you provided in your request
- `responseStatusCode`: Status of the recognition process ("Ok" if successful)
- `results`: Array containing recognition data
  - `type`: Format of the results (CSV, JSON, or XML as requested)
  - `data`: Base64-encoded results in the specified format
- `error`: Contains error messages if any occurred (null if successful)

## Step 4: Decoding the Results

The `data` field in the response contains your recognition results encoded in Base64. Let's decode it to get the actual content:

### Try it yourself:

For Linux/macOS:
```bash
# Save the Base64 data to a file
echo "RWxlbWVudCBOYW1lLFZhbHVlLApBbmltYWxzOTEsIiIKQW5pbWFsczky...gpQbGFudHM5MCwiIgo=" > encoded_results.txt

# Decode to the appropriate format (e.g., CSV)
base64 -d encoded_results.txt > results.csv
```

For Windows (PowerShell):
```powershell
# Save the Base64 data to a variable
$encodedResults = "RWxlbWVudCBOYW1lLFZhbHVlLApBbmltYWxzOTEsIiIKQW5pbWFsczky...gpQbGFudHM5MCwiIgo="

# Decode to the appropriate format (e.g., CSV)
$decodedBytes = [System.Convert]::FromBase64String($encodedResults)
[System.IO.File]::WriteAllBytes("results.csv", $decodedBytes)
```

## Step 5: Processing Different Result Formats

Depending on the `outputFormat` you specified when submitting the form, you'll need to process the results differently:

### CSV Format

CSV results are typically structured with headers and values:

```csv
Element Name,Value,
Animals91,""
Animals92,"selected"
...
Plants90,""
```

This format is ideal for importing into spreadsheet applications or for simple data processing.

### JSON Format

JSON results provide a structured representation of the form data:

```json
{
  "Elements": [
    {
      "Name": "Animals91",
      "Value": ""
    },
    {
      "Name": "Animals92",
      "Value": "selected"
    },
    ...
    {
      "Name": "Plants90",
      "Value": ""
    }
  ]
}
```

This format is best for web applications or when working with NoSQL databases.

### XML Format

XML results offer compatibility with a wide range of systems:

```xml
<?xml version="1.0" encoding="utf-8"?>
<OmrResult xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Elements>
    <Element>
      <Name>Animals91</Name>
      <Value></Value>
    </Element>
    <Element>
      <Name>Animals92</Name>
      <Value>selected</Value>
    </Element>
    ...
    <Element>
      <Name>Plants90</Name>
      <Value></Value>
    </Element>
  </Elements>
</OmrResult>
```

XML is ideal for integration with enterprise systems or when strict data validation is required.

## Step 6: Handling Common Response Scenarios

Let's look at some common scenarios you might encounter when fetching results:

### 1. Processing Not Complete

If you request results too soon after submission, the processing might not be complete yet:

```json
{
  "id": "85bc0ca8-a76e-44f6-a7a3-e6263f7e24ee",
  "responseStatusCode": "Processing",
  "results": null,
  "error": null
}
```

In this case, wait a few seconds and try again.

### 2. Error in Processing

If there was an error during recognition:

```json
{
  "id": "85bc0ca8-a76e-44f6-a7a3-e6263f7e24ee",
  "responseStatusCode": "Error",
  "results": null,
  "error": {
    "messages": [
      "Could not find positioning markers on the image"
    ]
  }
}
```

Check the error messages to understand what went wrong.

### 3. Task Not Found

If you provide an invalid task ID or if it's been more than 24 hours:

```json
{
  "id": "invalid-task-id",
  "responseStatusCode": "NotFound",
  "results": null,
  "error": {
    "messages": [
      "Task not found"
    ]
  }
}
```

Ensure you're using the correct task ID and that it hasn't expired.

## Complete Example Using Python

Here's a complete example using Python that fetches and processes recognition results:

```python
import requests
import base64
import json
import time
import sys

# Your Aspose.OMR Cloud credentials
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"
task_id = "YOUR_TASK_ID"  # Task ID received from the form submission

# Step 1: Get access token
def get_access_token(client_id, client_secret):
    auth_url = "https://api.aspose.cloud/connect/token"
    auth_data = {
        "grant_type": "client_credentials",
        "client_id": client_id,
        "client_secret": client_secret
    }
    auth_headers = {
        "Content-Type": "application/x-www-form-urlencoded"
    }
    auth_response = requests.post(auth_url, data=auth_data, headers=auth_headers)
    
    if auth_response.status_code == 200:
        return auth_response.json()["access_token"]
    else:
        print(f"Authentication failed: {auth_response.text}")
        sys.exit(1)

# Step 2: Poll for results with retry mechanism
def fetch_recognition_results(access_token, task_id, max_retries=5, retry_interval=3):
    results_url = f"https://api.aspose.cloud/v5.0/omr/RecognizeTemplate/GetRecognizeTemplate?id={task_id}"
    results_headers = {
        "Accept": "text/plain",
        "Content-Type": "application/json",
        "Authorization": f"Bearer {access_token}"
    }
    
    retry_count = 0
    while retry_count < max_retries:
        results_response = requests.get(results_url, headers=results_headers)
        
        if results_response.status_code == 200:
            results_data = results_response.json()
            
            if results_data["responseStatusCode"] == "Ok" and results_data["results"]:
                return results_data
            elif results_data["responseStatusCode"] == "Processing":
                print(f"Processing in progress. Retrying in {retry_interval} seconds...")
                time.sleep(retry_interval)
                retry_count += 1
            elif results_data["responseStatusCode"] == "Error":
                error_msgs = results_data["error"]["messages"]
                print(f"Recognition error: {error_msgs}")
                return results_data
            else:
                print(f"Unexpected status: {results_data['responseStatusCode']}")
                return results_data
        else:
            print(f"Error fetching results: {results_response.status_code} - {results_response.text}")
            retry_count += 1
            time.sleep(retry_interval)
    
    print(f"Max retries ({max_retries}) reached. Could not fetch results.")
    return None

# Step 3: Process and save the results
def process_results(results_data):
    if not results_data or results_data["responseStatusCode"] != "Ok":
        print("No valid results to process.")
        return
    
    # Get the first result (usually there's only one)
    result = results_data["results"][0]
    result_type = result["type"]
    encoded_data = result["data"]
    
    # Decode the Base64 data
    decoded_bytes = base64.b64decode(encoded_data)
    decoded_data = decoded_bytes.decode('utf-8')
    
    # Save the result based on its format
    output_filename = f"result.{result_type.lower()}"
    with open(output_filename, "w", encoding="utf-8") as file:
        file.write(decoded_data)
    
    print(f"Results saved to {output_filename}")
    
    # Display a preview of the results
    if result_type == "Csv":
        print("\nPreview of CSV results:")
        lines = decoded_data.split('\n')
        for i, line in enumerate(lines[:5]):
            print(line)
        if len(lines) > 5:
            print("...")
    elif result_type == "Json":
        print("\nPreview of JSON results:")
        json_data = json.loads(decoded_data)
        print(json.dumps(json_data, indent=2)[:300] + "...")
    elif result_type == "Xml":
        print("\nPreview of XML results:")
        print(decoded_data[:300] + "...")

# Main execution
access_token = get_access_token(client_id, client_secret)
results_data = fetch_recognition_results(access_token, task_id)
process_results(results_data)
```

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve form recognition results using the Aspose.OMR Cloud API
- Handle different response scenarios (success, processing, error)
- Decode Base64-encoded result data
- Process different result formats (CSV, JSON, XML)
- Implement a retry mechanism for improved reliability

## Next Steps

Now that you know how to fetch and process recognition results, consider exploring these related tutorials:

- [Tutorial: Fine-Tuning Recognition Accuracy](/recognize-form/accuracy-threshold/) - Learn how to optimize recognition settings for different forms and conditions
- [Tutorial: Implementing Form Recognition with SDK](/recognize-form/recognition-sdk/) - Discover how to use language-specific SDKs for simplified integration

## Further Practice

To reinforce your learning:
1. Create a simple application that fetches and visualizes form results
2. Compare the different output formats to determine which is best for your use case
3. Implement error handling and retry logic for production-grade reliability

## Helpful Resources

- [Product Page](https://products.aspose.cloud/omr/)
- [Documentation](https://docs.aspose.cloud/omr/)
- [Live Demo](https://products.aspose.app/omr/family)
- [API Reference](https://reference.aspose.cloud/omr/)
- [Blog](https://blog.aspose.cloud/category/omr/)
- [Free Support](https://forum.aspose.cloud/c/omr/8/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Visit our [support forum](https://forum.aspose.cloud/c/omr/8/) for assistance.
