---
title: How to Send OMR Forms for Recognition Tutorial
weight: 10
url: /recognize-form/send-for-recognition/
description: Learn step-by-step how to send filled OMR forms to Aspose.OMR Cloud API for processing in this hands-on developer tutorial.
---

# Tutorial: How to Send OMR Forms for Recognition

## Learning Objectives

In this tutorial, you'll learn how to:
- Prepare a filled OMR form for recognition
- Authenticate with the Aspose.OMR Cloud API
- Structure a form recognition request
- Submit the form for processing
- Handle API responses

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account with active subscription or [free trial](https://dashboard.aspose.cloud/#/apps)
2. Your Client ID and Client Secret for API authentication
3. A digitized OMR form (scanned or photographed)
4. The recognition pattern (.omr) file for your form
5. Basic knowledge of REST APIs and HTTP requests

## Understanding the Process

Before diving into the code, let's understand the form recognition workflow:

1. Authentication - Get an access token using your credentials
2. Preparation - Encode your form image and recognition pattern in Base64
3. Submission - Send a POST request to the recognition endpoint
4. Tracking - Receive a task ID to follow the recognition progress

## Step 1: Obtaining an Access Token

First, we need to authenticate with the Aspose.OMR Cloud API to get an access token.

### Try it yourself:

```bash
curl --location 'https://api.aspose.cloud/connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id=YOUR_CLIENT_ID' \
--data-urlencode 'client_secret=YOUR_CLIENT_SECRET'
```

This will return a JSON response containing your access token:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

Save this token for use in subsequent API calls. Remember that tokens expire, so you'll need to refresh them periodically.

## Step 2: Preparing Your Form for Recognition

Next, we need to:
1. Convert your form image to Base64
2. Convert your recognition pattern file to Base64

### Try it yourself:

Here's how to convert your files to Base64 format:

For Linux/macOS:
```bash
# Convert the form image to Base64
base64 -i form.jpg > form_base64.txt

# Convert the recognition pattern to Base64
base64 -i form.omr > pattern_base64.txt
```

For Windows (PowerShell):
```powershell
# Convert the form image to Base64
[Convert]::ToBase64String([System.IO.File]::ReadAllBytes("form.jpg")) | Out-File form_base64.txt

# Convert the recognition pattern to Base64
[Convert]::ToBase64String([System.IO.File]::ReadAllBytes("form.omr")) | Out-File pattern_base64.txt
```

> Tip: For testing, you can use smaller images to make the Base64 strings more manageable.

## Step 3: Constructing the API Request

Now that we have our Base64-encoded files, let's construct the request to the Aspose.OMR Cloud API.

The endpoint for form recognition is:
```
https://api.aspose.cloud/v5.0/omr/RecognizeTemplate/PostRecognizeTemplate
```

We'll need to prepare a JSON payload with:
- The Base64-encoded form image
- The Base64-encoded recognition pattern
- The desired output format (CSV, JSON, or XML)
- The recognition threshold (0-100)

### Try it yourself:

Create a file named `request.json` with the following structure:

```json
{
  "Images": [
    "YOUR_BASE64_ENCODED_IMAGE"
  ],
  "omrFile": "YOUR_BASE64_ENCODED_PATTERN",
  "outputFormat": "CSV",
  "recognitionThreshold": 35
}
```

Replace the placeholders with your actual Base64-encoded strings.

> Note: The `recognitionThreshold` parameter (35 in this example) determines how filled a bubble must be to count as marked. Lower values detect lighter marks but may cause false positives. Higher values require darker, more complete marks.

## Step 4: Sending the Form for Recognition

With our request payload ready, let's send it to the API:

### Try it yourself:

```bash
curl --location 'https://api.aspose.cloud/v5.0/omr/RecognizeTemplate/PostRecognizeTemplate' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data @request.json
```

If successful, the API will return a task ID (GUID) that looks something like this:
```
85bc0ca8-a76e-44f6-a7a3-e6263f7e24ee
```

This ID is crucial as it allows you to fetch the recognition results later.

## Step 5: Handling Response Codes

Understanding the API response codes will help you troubleshoot issues:

| Status Code | Meaning |
|-------------|---------|
| 200 OK | Request successful, task ID returned |
| 401 Unauthorized | Authentication failed, check your access token |
| 400 Bad Request | Invalid request format or parameters |
| 500 Server Error | Problem on the server side |

### Troubleshooting Tips

1. Authentication errors: Ensure your access token is valid and properly formatted in the Authorization header
2. Invalid images: Verify that your image is properly formatted and all positioning markers are visible
3. Base64 encoding issues: Make sure there are no line breaks or extra spaces in your Base64 strings
4. Recognition pattern mismatch: Confirm that the pattern file matches the form design

## Complete Example Using Python

Here's a complete example using Python that performs all the steps:

```python
import requests
import base64
import json
import time

# Step 1: Get access token
auth_url = "https://api.aspose.cloud/connect/token"
auth_data = {
    "grant_type": "client_credentials",
    "client_id": "YOUR_CLIENT_ID",
    "client_secret": "YOUR_CLIENT_SECRET"
}
auth_headers = {
    "Content-Type": "application/x-www-form-urlencoded"
}
auth_response = requests.post(auth_url, data=auth_data, headers=auth_headers)
access_token = auth_response.json()["access_token"]

# Step 2: Prepare form and pattern as Base64
with open("form.jpg", "rb") as image_file:
    encoded_image = base64.b64encode(image_file.read()).decode('utf-8')

with open("pattern.omr", "rb") as pattern_file:
    encoded_pattern = base64.b64encode(pattern_file.read()).decode('utf-8')

# Step 3: Construct API request
recognition_url = "https://api.aspose.cloud/v5.0/omr/RecognizeTemplate/PostRecognizeTemplate"
recognition_headers = {
    "Accept": "text/plain",
    "Content-Type": "application/json",
    "Authorization": f"Bearer {access_token}"
}
recognition_data = {
    "Images": [encoded_image],
    "omrFile": encoded_pattern,
    "outputFormat": "CSV",
    "recognitionThreshold": 35
}

# Step 4: Send form for recognition
recognition_response = requests.post(
    recognition_url, 
    headers=recognition_headers, 
    data=json.dumps(recognition_data)
)

if recognition_response.status_code == 200:
    task_id = recognition_response.text.strip()
    print(f"Form submitted successfully! Task ID: {task_id}")
    print("Use this ID to fetch results with the next tutorial.")
else:
    print(f"Error: {recognition_response.status_code} - {recognition_response.text}")
```

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the Aspose.OMR Cloud API
- Prepare form images and recognition patterns
- Create and send a form recognition request
- Handle API responses and task IDs

## Next Steps

Now that you know how to send a form for recognition, the next tutorial will teach you how to [fetch and process the recognition results](/recognize-form/fetch-results/).

## Further Practice

To reinforce your learning:
1. Try sending different forms with various recognition thresholds (15, 35, 50) to see how it affects results
2. Experiment with different output formats (CSV, JSON, XML)
3. Create a simple application that takes a form image as input and sends it for recognition

## Helpful Resources

- [Product Page](https://products.aspose.cloud/omr/)
- [Documentation](https://docs.aspose.cloud/omr/)
- [Live Demo](https://products.aspose.app/omr/family)
- [API Reference](https://reference.aspose.cloud/omr/)
- [Blog](https://blog.aspose.cloud/category/omr/)
- [Free Support](https://forum.aspose.cloud/c/omr/8/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Visit our [support forum](https://forum.aspose.cloud/c/omr/8/) for assistance.
