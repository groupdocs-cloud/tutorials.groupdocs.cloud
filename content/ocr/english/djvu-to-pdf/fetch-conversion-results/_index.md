---
title: How to Fetch PDF Conversion Results Tutorial
weight: 30
url: /djvu-to-pdf/fetch-conversion-results/
description: Learn how to retrieve converted PDF documents from the Aspose.OCR Cloud API and handle different task statuses in this developer tutorial.
---

# Tutorial: How to Fetch PDF Conversion Results

## Learning Objectives

In this tutorial, you'll learn:
- How to check the status of a conversion task
- How to retrieve a converted PDF document
- How to handle different task statuses
- How to decode the Base64-encoded PDF
- How to implement polling for asynchronous processing

## Prerequisites

Before starting this tutorial, you should have:
- Completed the previous tutorial: [How to Send DjVu Files for Conversion](/djvu-to-pdf/send-for-conversion/)
- A valid access token for the Aspose.OCR Cloud API
- A task ID from a previously submitted DjVu conversion request
- Basic understanding of asynchronous processing concepts

## Understanding the Conversion Queue

When you submit a DjVu file for conversion, it doesn't get processed immediately. Instead, it's placed in a queue to ensure stable service even under high load. This means you need to check the status of your conversion task and retrieve the result when it's ready.

### Conversion Task Lifecycle

A conversion task goes through several states:

1. Pending: The task is in the queue but hasn't started processing yet
2. Processing: The conversion is actively being performed
3. Completed: The conversion has finished successfully
4. Error: An error occurred during conversion
5. NotExist: The task ID doesn't exist or has expired

## Fetching Conversion Results

Let's learn how to check the status of your conversion task and retrieve the PDF document.

### Step 1: Send Request to Fetch Results

Use a GET request to the Aspose.OCR Cloud API with your task ID:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/djvu2pdf?id=YOUR_TASK_ID' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace:
- `YOUR_TASK_ID` with the ID you received when submitting the DjVu file
- `YOUR_ACCESS_TOKEN` with your valid access token

### Step 2: Interpret the Response

The API will return a JSON response containing the task status and, if complete, the converted PDF document:

```json
{
  "id": "c4b60313-4f78-45f8-b708-069eb98dc22e",
  "taskStatus": "Completed",
  "responseStatusCode": "Ok",
  "results": [
    {
      "type": "Pdf",
      "data": "QWxsIG1lbiBsaXZlIGVudm...Vsb3BlZCBpbiB3aGFsZS1saW5lcy4="
    }
  ],
  "error": null
}
```

Let's break down this response:

- `id`: The unique identifier of your conversion task
- `taskStatus`: The current state of the task (Pending, Processing, Completed, Error, or NotExist)
- `responseStatusCode`: Overall status of the API response
- `results`: An array containing the converted documents (when taskStatus is "Completed")
  - `type`: The format of the result (should be "Pdf")
  - `data`: The Base64-encoded PDF document
- `error`: Error information (null if no errors occurred)

### Try It Yourself

1. Use the task ID from your previous conversion request
2. Send the GET request using cURL or Postman
3. Examine the response and note the `taskStatus`

## Handling Different Task Statuses

Your code should handle each possible task status appropriately:

### Pending or Processing

If the task is still in the queue or being processed, you'll need to wait and check again later:

```json
{
  "id": "c4b60313-4f78-45f8-b708-069eb98dc22e",
  "taskStatus": "Processing",
  "responseStatusCode": "Ok",
  "results": null,
  "error": null
}
```

In this case, implement a polling mechanism with a reasonable delay (5-10 seconds) and try again.

### Completed

When the task is completed, extract the Base64-encoded PDF from the `results` array and decode it to obtain the actual PDF document.

### Error

If an error occurred, check the `error` property for details:

```json
{
  "id": "c4b60313-4f78-45f8-b708-069eb98dc22e",
  "taskStatus": "Error",
  "responseStatusCode": "Error",
  "results": null,
  "error": {
    "messages": ["The input file appears to be corrupted or is not a valid DjVu document."]
  }
}
```

### NotExist

If the task ID doesn't exist or has expired (remember that results are only kept for 24 hours):

```json
{
  "id": "c4b60313-4f78-45f8-b708-069eb98dc22e",
  "taskStatus": "NotExist",
  "responseStatusCode": "Error",
  "results": null,
  "error": {
    "messages": ["The requested conversion task was not found."]
  }
}
```

In this case, you'll need to submit the DjVu file again.

## Decoding Base64 PDF Data

Once you have a completed task, you need to decode the Base64 string to get the actual PDF document:

### Command Line (Linux/macOS)
```bash
echo "YOUR_BASE64_PDF_DATA" | base64 --decode > result.pdf
```

### PowerShell (Windows)
```powershell
$base64 = "YOUR_BASE64_PDF_DATA"
[System.IO.File]::WriteAllBytes("result.pdf", [System.Convert]::FromBase64String($base64))
```

### Programming Languages

Python
```python
import base64

base64_pdf = "YOUR_BASE64_PDF_DATA"
pdf_bytes = base64.b64decode(base64_pdf)

with open("result.pdf", "wb") as pdf_file:
    pdf_file.write(pdf_bytes)
```

JavaScript (Node.js)
```javascript
const fs = require('fs');

const base64Pdf = "YOUR_BASE64_PDF_DATA";
const pdfBuffer = Buffer.from(base64Pdf, 'base64');

fs.writeFileSync('result.pdf', pdfBuffer);
```

C#
```csharp
using System;
using System.IO;

string base64Pdf = "YOUR_BASE64_PDF_DATA";
byte[] pdfBytes = Convert.FromBase64String(base64Pdf);

File.WriteAllBytes("result.pdf", pdfBytes);
```

### Try It Yourself

1. Extract the Base64 string from the `data` field in your API response
2. Use one of the methods above to decode it to a PDF file
3. Open the resulting PDF to verify the conversion was successful

## Implementing Polling Logic

Since conversion isn't instantaneous, you should implement a polling mechanism in your code to check the status periodically until it completes:

### Python Example with Polling

```python
import requests
import base64
import time
import json

def fetch_conversion_result(task_id, access_token, max_attempts=12, delay_seconds=5):
    """
    Fetch the conversion result with polling.
    
    Args:
        task_id: The task ID from the conversion request
        access_token: Valid access token for the API
        max_attempts: Maximum number of polling attempts
        delay_seconds: Delay between polling attempts in seconds
        
    Returns:
        The PDF bytes if successful, None otherwise
    """
    url = f"https://api.aspose.cloud/v5.0/ocr/djvu2pdf?id={task_id}"
    headers = {
        "Accept": "text/plain",
        "Content-Type": "application/json",
        "Authorization": f"Bearer {access_token}"
    }
    
    for attempt in range(max_attempts):
        print(f"Polling attempt {attempt + 1}/{max_attempts}...")
        
        response = requests.get(url, headers=headers)
        if response.status_code != 200:
            print(f"Error: HTTP status {response.status_code}")
            return None
            
        result = response.json()
        task_status = result.get("taskStatus")
        
        print(f"Task status: {task_status}")
        
        if task_status == "Completed":
            # Conversion successful, extract PDF data
            pdf_base64 = result["results"][0]["data"]
            return base64.b64decode(pdf_base64)
            
        elif task_status == "Error":
            # Conversion failed
            error_messages = result.get("error", {}).get("messages", ["Unknown error"])
            print(f"Conversion error: {', '.join(error_messages)}")
            return None
            
        elif task_status == "NotExist":
            # Task not found
            print("Task ID not found or expired")
            return None
            
        elif task_status in ["Pending", "Processing"]:
            # Task still in progress, wait and try again
            print(f"Task still in progress. Waiting {delay_seconds} seconds...")
            time.sleep(delay_seconds)
            continue
            
    print(f"Maximum polling attempts ({max_attempts}) reached")
    return None

# Example usage
task_id = "YOUR_TASK_ID"
access_token = "YOUR_ACCESS_TOKEN"

pdf_bytes = fetch_conversion_result(task_id, access_token)
if pdf_bytes:
    with open("result.pdf", "wb") as pdf_file:
        pdf_file.write(pdf_bytes)
    print("PDF successfully saved to result.pdf")
```

### C# Example with Polling

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Text.Json;
using System.Threading.Tasks;

public class ConversionResult
{
    public string id { get; set; }
    public string taskStatus { get; set; }
    public string responseStatusCode { get; set; }
    public ResultItem[] results { get; set; }
    public ErrorInfo error { get; set; }
}

public class ResultItem
{
    public string type { get; set; }
    public string data { get; set; }
}

public class ErrorInfo
{
    public string[] messages { get; set; }
}

public class DjVuConverter
{
    public static async Task<byte[]> FetchConversionResultAsync(
        string taskId, 
        string accessToken, 
        int maxAttempts = 12, 
        int delaySeconds = 5)
    {
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Authorization", $"Bearer {accessToken}");
        
        string url = $"https://api.aspose.cloud/v5.0/ocr/djvu2pdf?id={taskId}";
        
        for (int attempt = 0; attempt < maxAttempts; attempt++)
        {
            Console.WriteLine($"Polling attempt {attempt + 1}/{maxAttempts}...");
            
            try
            {
                var response = await client.GetAsync(url);
                if (!response.IsSuccessStatusCode)
                {
                    Console.WriteLine($"Error: HTTP status {response.StatusCode}");
                    return null;
                }
                
                var jsonContent = await response.Content.ReadAsStringAsync();
                var result = JsonSerializer.Deserialize<ConversionResult>(jsonContent);
                
                Console.WriteLine($"Task status: {result.taskStatus}");
                
                switch (result.taskStatus)
                {
                    case "Completed":
                        // Conversion successful, extract PDF data
                        string pdfBase64 = result.results[0].data;
                        return Convert.FromBase64String(pdfBase64);
                        
                    case "Error":
                        // Conversion failed
                        string errorMessage = result.error?.messages != null && result.error.messages.Length > 0
                            ? string.Join(", ", result.error.messages)
                            : "Unknown error";
                        Console.WriteLine($"Conversion error: {errorMessage}");
                        return null;
                        
                    case "NotExist":
                        // Task not found
                        Console.WriteLine("Task ID not found or expired");
                        return null;
                        
                    case "Pending":
                    case "Processing":
                        // Task still in progress, wait and try again
                        Console.WriteLine($"Task still in progress. Waiting {delaySeconds} seconds...");
                        await Task.Delay(delaySeconds * 1000);
                        break;
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
                return null;
            }
        }
        
        Console.WriteLine($"Maximum polling attempts ({maxAttempts}) reached");
        return null;
    }
    
    // Example usage
    public static async Task Main()
    {
        string taskId = "YOUR_TASK_ID";
        string accessToken = "YOUR_ACCESS_TOKEN";
        
        byte[] pdfBytes = await FetchConversionResultAsync(taskId, accessToken);
        if (pdfBytes != null)
        {
            File.WriteAllBytes("result.pdf", pdfBytes);
            Console.WriteLine("PDF successfully saved to result.pdf");
        }
    }
}
```

### JavaScript (Node.js) Example with Polling

```javascript
const fs = require('fs');
const axios = require('axios');

async function fetchConversionResult(taskId, accessToken, maxAttempts = 12, delaySeconds = 5) {
    const url = `https://api.aspose.cloud/v5.0/ocr/djvu2pdf?id=${taskId}`;
    const headers = {
        'Accept': 'text/plain',
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${accessToken}`
    };
    
    for (let attempt = 0; attempt < maxAttempts; attempt++) {
        console.log(`Polling attempt ${attempt + 1}/${maxAttempts}...`);
        
        try {
            const response = await axios.get(url, { headers });
            const result = response.data;
            
            console.log(`Task status: ${result.taskStatus}`);
            
            if (result.taskStatus === 'Completed') {
                // Conversion successful, extract PDF data
                const pdfBase64 = result.results[0].data;
                return Buffer.from(pdfBase64, 'base64');
                
            } else if (result.taskStatus === 'Error') {
                // Conversion failed
                const errorMessages = result.error?.messages || ['Unknown error'];
                console.log(`Conversion error: ${errorMessages.join(', ')}`);
                return null;
                
            } else if (result.taskStatus === 'NotExist') {
                // Task not found
                console.log('Task ID not found or expired');
                return null;
                
            } else if (['Pending', 'Processing'].includes(result.taskStatus)) {
                // Task still in progress, wait and try again
                console.log(`Task still in progress. Waiting ${delaySeconds} seconds...`);
                await new Promise(resolve => setTimeout(resolve, delaySeconds * 1000));
                continue;
            }
            
        } catch (error) {
            console.error('Error:', error.response?.data || error.message);
            return null;
        }
    }
    
    console.log(`Maximum polling attempts (${maxAttempts}) reached`);
    return null;
}

// Example usage
async function main() {
    const taskId = 'YOUR_TASK_ID';
    const accessToken = 'YOUR_ACCESS_TOKEN';
    
    const pdfBuffer = await fetchConversionResult(taskId, accessToken);
    if (pdfBuffer) {
        fs.writeFileSync('result.pdf', pdfBuffer);
        console.log('PDF successfully saved to result.pdf');
    }
}

main();
```

## Conversion Timeline and Expiration

It's important to note that converted PDF documents are stored in the Aspose cloud and can be retrieved using the task ID for only 24 hours after the DjVu file was sent for conversion. After this period, the task ID will return a "NotExist" status, and you'll need to submit the DjVu file again.

### Recommended Practices

1. Implement proper error handling: Always check the task status and handle each case appropriately
2. Store the PDF locally: Once you successfully retrieve the PDF, save it to your local storage
3. Use appropriate polling intervals: Don't poll too frequently (to avoid unnecessary API calls) or too infrequently (to avoid user waiting)
4. Set a reasonable timeout: Define a maximum number of polling attempts to prevent infinite loops

## What You've Learned

In this tutorial, you've learned:
- How to send a request to check the status of a conversion task
- How to interpret different task statuses and respond accordingly
- How to extract and decode a Base64-encoded PDF document
- How to implement polling logic for asynchronous processing
- Best practices for handling conversion results

## Next Steps

Now that you understand how to submit DjVu files and retrieve PDF documents, consider exploring the SDK approach for easier implementation:

[Tutorial: Implementing DjVu to PDF with SDK](/djvu-to-pdf/sdk-implementation/)

## Further Practice

- Implement error recovery strategies (e.g., automatic resubmission if a task fails)
- Create a complete DjVu to PDF conversion utility with a user interface
- Add logging to track conversion status and performance
- Explore batch processing for multiple DjVu files

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post on our [support forum](https://forum.aspose.cloud/c/ocr/12/).