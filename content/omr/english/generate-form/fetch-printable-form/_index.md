---
title: How to Fetch Printable OMR Forms Tutorial
weight: 30
url: /generate-form/fetch-printable-form/
description: Learn how to retrieve generated printable OMR forms and recognition patterns from Aspose.OMR Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Fetch Printable OMR Forms

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve generated OMR forms from the Aspose.OMR Cloud API
- Extract printable form images and recognition patterns
- Decode Base64 responses into usable files
- Understand the structure of the API response
- Handle potential error messages

## Prerequisites

Before starting this tutorial, you should:
- Have an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps)
- Have completed the [Send Form Source Code tutorial](/generate-form/send-source-code/)
- Have a valid task ID from a form generation request
- Be familiar with HTTP requests and JSON responses
- Know how to decode Base64 strings

## Introduction

After [sending your OMR form source code](/generate-form/send-source-code/) for generation, the next step is to fetch the printable form and recognition pattern from the Aspose.OMR Cloud API. This tutorial will guide you through the process of retrieving your generated form and understanding the API response.

## Practical Scenario

You're developing an automated examination system for a school. You've sent your test form source code to the API and received a task ID. Now you need to retrieve the printable form that can be distributed to students and the recognition pattern file that will be used later to recognize the filled forms.

## Step 1: Understand the Queuing System

When form source code is submitted for generation, it is queued to ensure a stable response even under high load. The task ID you received in the previous tutorial is used to identify your generation request in the queue.

> Note: Generated forms are stored in the Aspose cloud and can be obtained by the task ID within 24 hours after the source code was sent for generation.

## Step 2: Prepare Your GET Request

To fetch the generated form, you'll send a GET request to the Aspose.OMR Cloud API. You'll need:

1. Your access token for authentication
2. The task ID from your previous form generation request

## Step 3: Send the GET Request

Send a GET request to the API endpoint, including your task ID as a parameter:

```bash
curl --location 'https://api.aspose.cloud/v5.0/omr/GenerateTemplate/GetGenerateTemplate?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the ID you received when sending the form source code, and `YOUR_ACCESS_TOKEN` with your valid access token.

> Try it yourself: Send a GET request using the task ID you received in the previous tutorial.

## Step 4: Understand the API Response

The API will return a JSON response containing:

```json
{
  "id": "7ec63522-e35f-4f9d-8b06-0da1876220b6",
  "responseStatusCode": "Ok",
  "results": [
    {
      "type": "Png",
      "data": "iVBORw0KGgoAAAANSUhEUgAACbAAAA20CAYAAAAuvPkNAAAACXBIWXMAAA7DAAAOwwHHb6hkAAB...N7rR2lwI/PWEAAAAAElFTkSuQmCC"
    },
    {
      "type": "Omr",
      "data": "ewoJIlZlcnNpb24iOiAiMS4wIiwKCSJOYW1lIjogIk15VGVtcGxhdGUiLAoJIlBhZ2VzIjogWwo...VyYXRlZCI6IHRydWUKfQ=="
    }
  ],
  "error": null
}
```

Let's examine the structure of this response:

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier of the generation task (matches your task ID) |
| `responseStatusCode` | string | Generation response status (e.g., "Ok") |
| `results` | array | Array of generated files |
| `error/messages` | array | Generation error messages, if any |

The `results` array will contain at least two entries:

1. Printable form pages (`"type": "Png"`) - PNG images that match the specified paper size and orientation. Each page is saved as a separate file.
2. Recognition pattern (`"type": "Omr"`) - A special .OMR format file used by Aspose.OMR recognition engine. This file is required for recognizing filled forms.

> Important: Both the printable form pages and recognition pattern are returned as Base64 encoded strings. You must decode them to save as files.

## Step 5: Process the Response Data

To save the generated files, you need to:

1. Extract the Base64 encoded strings from the response
2. Decode them into binary data
3. Save them as files with appropriate extensions

Here's a simple example using JavaScript:

```javascript
// Assuming 'response' contains the API response JSON
const printableFormBase64 = response.results.find(r => r.type === "Png").data;
const recognitionPatternBase64 = response.results.find(r => r.type === "Omr").data;

// Decode and save as files
const printableFormBinary = atob(printableFormBase64);
const recognitionPatternBinary = atob(recognitionPatternBase64);

// Now save these binary data as files
// This depends on your programming environment
```

### Python Example:

```python
import base64
import requests
import json

# Your API request here
response = requests.get(
    "https://api.aspose.cloud/v5.0/omr/GenerateTemplate/GetGenerateTemplate",
    params={"id": "YOUR_TASK_ID"},
    headers={
        "Accept": "text/plain",
        "Content-Type": "application/json",
        "Authorization": "Bearer YOUR_ACCESS_TOKEN"
    }
)

data = response.json()

# Extract and save printable form
for index, result in enumerate(data["results"]):
    if result["type"] == "Png":
        # Decode Base64 string
        image_data = base64.b64decode(result["data"])
        # Save to file
        with open(f"form_page_{index}.png", "wb") as file:
            file.write(image_data)
    elif result["type"] == "Omr":
        # Decode Base64 string
        pattern_data = base64.b64decode(result["data"])
        # Save to file
        with open("recognition_pattern.omr", "wb") as file:
            file.write(pattern_data)
```

> Learning checkpoint: Why is it important to save both the PNG image files and the OMR recognition pattern file?

## Step 6: Handle Potential Errors

Even if the form is generated successfully, you might still receive notifications and warnings about non-fatal generation errors. Check the `error/messages` array in the response for any warnings or errors.

Common issues include:
- Missing images referenced in the source code
- Unsupported font families
- Layout issues due to element size or positioning

> Troubleshooting tip: If you receive a 404 error, your task ID might be incorrect or the generated form might have expired (forms are available for 24 hours).

## What You've Learned

In this tutorial, you learned how to:
- Fetch generated OMR forms from the Aspose.OMR Cloud API
- Understand the structure of the API response
- Extract and save printable form images and recognition patterns
- Handle potential error messages

## Further Practice

Try these exercises to reinforce your learning:

1. Write a script that automatically fetches and saves all generated files from the API response.
2. Create a simple web interface that allows users to view their generated forms.
3. Build error handling into your code to address various API response scenarios.

## Next Steps

Now that you know how to generate and fetch OMR forms, you might want to explore [using the Aspose.OMR Cloud SDK](/generate-form/using-sdk/) for easier integration with your application.

## Additional Resources

- [Product Page](https://products.aspose.cloud/omr/)
- [Documentation](https://docs.aspose.cloud/omr/)
- [Live Demo](https://products.aspose.app/omr/family)
- [API Reference](https://reference.aspose.cloud/omr/)
- [Blog](https://blog.aspose.cloud/category/omr/)
- [Free Support](https://forum.aspose.cloud/c/omr/8/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [support forum](https://forum.aspose.cloud/c/omr/8/).
