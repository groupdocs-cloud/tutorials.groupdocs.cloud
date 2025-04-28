---
title: Learn to Fetch Label Recognition Results from Aspose.OCR Cloud API Tutorial
weight: 30
url: /recognize-label/fetch-recognition-result/
description: Learn how to retrieve, decode, and process text extracted from labels and signs after recognition by the Aspose.OCR Cloud API in this developer tutorial.
---

# Tutorial: Learn to Fetch Label Recognition Results from Aspose.OCR Cloud API

## Learning Objectives

In this tutorial, you'll learn:
- How to check the status of a label recognition task
- How to retrieve the recognition results using the task ID
- How to interpret the JSON response from the API
- How to decode the Base64-encoded recognition results
- How to handle different task statuses and potential errors

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [Tutorial: How to Send Labels for Recognition](/recognize-label/send-for-recognition/)
- A valid task ID from a previously submitted label recognition request
- An active access token for the Aspose.OCR Cloud API
- Basic understanding of REST APIs and JSON parsing

## Practical Scenario

Continuing with our travel translation app example, after submitting a photo of a foreign street sign or product label for recognition, we now need to retrieve the extracted text. This step is crucial for enabling the subsequent translation of the text for the user.

## Tutorial Steps

### 1. Understanding the Recognition Workflow

Before diving into the code, it's important to understand how the recognition process works in Aspose.OCR Cloud:

1. When a label is submitted for recognition, it enters a queue
2. The image is processed when resources are available
3. Results are stored for up to 24 hours
4. You need to poll the API to check if results are ready

This queueing system ensures stable API performance even under high load.

### 2. Creating the GET Request

To fetch the recognition results, you'll need to make a GET request to the API endpoint with your task ID:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeLabel?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the ID you received after submitting the image, and `YOUR_ACCESS_TOKEN` with your valid authentication token.

### 3. Interpreting the Response

The API will return a JSON response with the following structure:

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

Let's examine each field in the response:

| Field | Description |
|-------|-------------|
| `id` | The unique identifier of your recognition task |
| `taskStatus` | The current status of the recognition process |
| `responseStatusCode` | The overall status of the API response |
| `results` | An array containing the recognition results (if available) |
| `error` | Error messages (if any) |

### 4. Understanding Task Statuses

The `taskStatus` field can have one of several values:

| Status | Description | What to Do |
|--------|-------------|------------|
| `Pending` | Your image is in the queue, not yet processed | Wait and try again in a few seconds |
| `Processing` | Your image is currently being recognized | Wait and try again in a few seconds |
| `Completed` | Recognition is finished, results are available | Parse and use the results |
| `Error` | An error occurred during recognition | Check the `error` field for details |
| `NotExist` | The task ID doesn't exist or results expired | Verify your task ID or submit the image again |

> Tip: Recognition typically takes a few seconds depending on image size and queue load. If you're building a user-facing application, implement a polling mechanism that checks every 1-2 seconds until the status is either `Completed` or `Error`.

### 5. Decoding the Results

When the `taskStatus` is `Completed`, the recognition results are provided in the `results` array as Base64-encoded strings. You need to decode them to get the actual text:

JavaScript (Browser):
```javascript
function decodeBase64Result(base64String) {
    try {
        // Convert base64 to binary, then to string
        const binaryString = atob(base64String);
        return decodeURIComponent(escape(binaryString));
    } catch (error) {
        console.error('Error decoding Base64 string:', error);
        return null;
    }
}

// Example usage
const recognitionResult = response.results[0].data;
const decodedText = decodeBase64Result(recognitionResult);
console.log('Recognized text:', decodedText);
```

Python:
```python
import base64

def decode_base64_result(base64_string):
    try:
        # Decode base64 string to bytes, then to UTF-8 string
        decoded_bytes = base64.b64decode(base64_string)
        decoded_text = decoded_bytes.decode('utf-8')
        return decoded_text
    except Exception as e:
        print(f"Error decoding Base64 string: {e}")
        return None

# Example usage
recognition_result = response['results'][0]['data']
decoded_text = decode_base64_result(recognition_result)
print(f"Recognized text: {decoded_text}")
```

### 6. Implementing a Complete Solution

Here's a complete example in JavaScript that polls the API until the results are available:

```javascript
async function fetchRecognitionResult(taskId, accessToken) {
    const endpoint = `https://api.aspose.cloud/v5.0/ocr/RecognizeLabel?id=${taskId}`;
    
    // Function to decode Base64 results
    function decodeBase64Result(base64String) {
        try {
            const binaryString = atob(base64String);
            return decodeURIComponent(escape(binaryString));
        } catch (error) {
            console.error('Error decoding Base64 string:', error);
            return null;
        }
    }
    
    // Function to check status
    async function checkStatus() {
        try {
            const response = await fetch(endpoint, {
                method: 'GET',
                headers: {
                    'Accept': 'text/plain',
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${accessToken}`
                }
            });
            
            if (!response.ok) {
                throw new Error(`HTTP error! Status: ${response.status}`);
            }
            
            const data = await response.json();
            
            switch (data.taskStatus) {
                case 'Completed':
                    // Success! Decode and return results
                    if (data.results && data.results.length > 0) {
                        const decodedText = decodeBase64Result(data.results[0].data);
                        return {
                            status: 'completed',
                            text: decodedText
                        };
                    } else {
                        return {
                            status: 'error',
                            message: 'No results found in the response'
                        };
                    }
                    
                case 'Error':
                    // Recognition error
                    return {
                        status: 'error',
                        message: data.error ? data.error.messages.join(', ') : 'Unknown error'
                    };
                    
                case 'NotExist':
                    // Task ID doesn't exist
                    return {
                        status: 'notfound',
                        message: 'Task ID not found or results expired'
                    };
                    
                default:
                    // Still processing, return null to continue polling
                    return null;
            }
        } catch (error) {
            return {
                status: 'error',
                message: error.message
            };
        }
    }
    
    // Poll every 1.5 seconds for up to 30 seconds
    const maxAttempts = 20;
    let attempts = 0;
    
    return new Promise((resolve, reject) => {
        const poll = async () => {
            if (attempts >= maxAttempts) {
                reject(new Error('Maximum polling attempts reached. Recognition taking too long.'));
                return;
            }
            
            attempts++;
            const result = await checkStatus();
            
            if (result === null) {
                // Still processing, poll again after delay
                setTimeout(poll, 1500);
            } else {
                // Processing complete or error occurred
                resolve(result);
            }
        };
        
        // Start polling
        poll();
    });
}

// Example usage
async function recognizeAndFetchLabel() {
    try {
        const taskId = 'c4b60313-4f78-45f8-b708-069eb98dc22e'; // From previous step
        const accessToken = 'YOUR_ACCESS_TOKEN';
        
        const result = await fetchRecognitionResult(taskId, accessToken);
        
        if (result.status === 'completed') {
            console.log('Recognition successful!');
            console.log('Extracted text:', result.text);
            
            // Do something with the text, like translation
            // translateText(result.text);
        } else {
            console.error('Recognition failed:', result.message);
        }
    } catch (error) {
        console.error('Error:', error.message);
    }
}

// Run the example
recognizeAndFetchLabel();
```

## Try It Yourself

Now it's your turn to practice fetching label recognition results:

1. Take the task ID you received in the previous tutorial
2. Create a GET request to fetch the results
3. Implement a polling mechanism to check the status
4. Decode the Base64 results when available
5. Display or process the extracted text

## Troubleshooting Common Issues

| Issue | Possible Solution |
|-------|-------------------|
| Always getting `Pending` status | The API might be experiencing high load; continue polling with increased intervals |
| `NotExist` status | Verify your task ID is correct; remember results expire after 24 hours |
| Empty results array | The image might not contain any recognizable text; try a clearer image |
| Garbled text after decoding | Ensure you're handling the encoding/decoding correctly across different character sets |

## What You've Learned

In this tutorial, you've learned:
- How to check the status of a label recognition task
- How to poll the API until results are available
- How to interpret the different task statuses
- How to decode Base64-encoded recognition results
- How to implement a complete solution with error handling

## Next Steps

Now that you've mastered the basics of sending and retrieving label recognition results, you're ready to streamline your development process with the Aspose.OCR Cloud SDK. Continue to our next tutorial:

[Tutorial: Using Aspose.OCR Cloud SDK for Label Recognition](/recognize-label/sdk-implementation/)

## Further Practice

To reinforce your learning:
1. Create a simple web interface that displays the recognition progress and results
2. Implement proper error handling and user feedback for all possible task statuses
3. Experiment with different polling intervals to find the optimal balance between responsiveness and API load

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance!
