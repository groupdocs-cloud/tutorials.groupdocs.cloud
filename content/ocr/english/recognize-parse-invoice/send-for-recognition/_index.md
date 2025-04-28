---
title: Learn to Send Invoices for Recognition with Aspose.OCR Cloud
weight: 10
url: /recognize-parse-invoice/send-for-recognition/
description: This tutorial teaches you how to submit invoice images to Aspose.OCR Cloud API for processing and extract valuable business data.
---

# Tutorial: How to Send Invoices for Recognition

## Learning Objectives

In this tutorial, you'll learn how to:
- Prepare invoice images for optimal recognition
- Structure a proper API request to Aspose.OCR Cloud
- Configure recognition settings for different invoice types
- Handle the initial API response

## Prerequisites

Before starting this tutorial, you should have:
- An [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps) with active subscription or free trial
- Basic understanding of REST APIs
- A tool for making HTTP requests (cURL, Postman, or your preferred programming language)
- Sample invoice images for testing (preferably in PNG or JPG format)

## Understanding the Invoice Recognition Process

When working with Aspose.OCR Cloud, invoice recognition follows a specific workflow:

1. Authenticate to receive an access token
2. Send the invoice image for processing
3. Receive a task ID for the queued recognition job
4. Use the task ID to fetch results once processing is complete

In this tutorial, we'll focus on step 2: sending the invoice for recognition.

## Step 1: Prepare Your Invoice Image

For the best recognition results, follow these preparation guidelines:

1. Ensure your invoice is clearly visible with good lighting and contrast
2. Use a minimum resolution of 300 DPI for scanned documents
3. Save your image in PNG, JPG, TIFF, or PDF format
4. Keep the file size reasonable (under 20MB) for faster processing

### Try it yourself:
Examine your invoice image. Is it clear and readable? If not, consider rescanning or taking a better photo before proceeding.

## Step 2: Convert Your Invoice Image to Base64

The Aspose.OCR Cloud API requires your invoice image to be Base64 encoded in the request body. Here's how to convert your image:

Using cURL:
```bash
base64 -i invoice.png > invoice_base64.txt
```

Using Python:
```python
import base64

# Read the image file
with open("invoice.png", "rb") as image_file:
    encoded_string = base64.b64encode(image_file.read()).decode('utf-8')
    
# Now 'encoded_string' contains your Base64 encoded image
```

### Try it yourself:
Convert one of your sample invoices to Base64 format using the method appropriate for your environment.

## Step 3: Obtain an Authorization Token

Before sending your invoice, you need to authenticate:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded"
```

Save the `access_token` value from the response for use in the next step.

## Step 4: Send Your Invoice for Recognition

Now, you'll submit your invoice to the Aspose.OCR Cloud API using the following endpoint:

```
POST https://api.aspose.cloud/v5.0/ocr/RecognizeAndParseInvoice
```

Here's a complete example using cURL:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeAndParseInvoice' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data-raw '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "rotate": 0,
    "makeBinarization": false,
    "makeUpsampling": false,
    "makeSpellCheck": true
  }
}'
```

Let's understand the recognition settings:

| Setting | Purpose | When to Use |
|---------|---------|-------------|
| language | Specifies the invoice language | Set to the primary language in your invoice |
| makeSkewCorrect | Corrects tilted images | Enable for photos or skewed scans |
| rotate | Manually rotates the image | Use if image is significantly rotated |
| makeBinarization | Converts to black and white | Enable for color or low contrast invoices |
| makeUpsampling | Improves small text recognition | Enable for poor quality scans |
| makeSpellCheck | Corrects common misspellings | Recommended for better accuracy |

### Try it yourself:
Send a real invoice image to the API and observe the response. You should receive a task ID (GUID) that you'll use to fetch results.

## Step 5: Understand the API Response

If your request is successful, you'll receive a task ID in the form of a GUID string:

```
39b37b24-86e8-4e91-9a99-6c2574853eb5
```

This ID is crucial as you'll use it to fetch your recognition results once processing is complete. Save this ID for the next tutorial.

### Learning Checkpoint:
What happens if your invoice is rotated upside down? Which settings should you adjust?

<details>
<summary>Answer</summary>
If your invoice is upside down (rotated 180 degrees), you should set <code>rotate: 180</code> in your settings. The <code>makeSkewCorrect</code> setting only works for images tilted by 15 degrees or less.
</details>

## Troubleshooting Common Issues

| Problem | Possible Solution |
|---------|------------------|
| "Request entity too large" error | Compress your image or reduce its resolution |
| Authentication errors | Ensure your access token is valid and correctly formatted |
| Timeout errors | Try reducing the image size or splitting large documents |
| Base64 encoding errors | Make sure your image is properly encoded without line breaks |

## Example Code in Different Languages

### Python Example
```python
import requests
import base64
import json

# Authentication
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
token = auth_response.json().get("access_token")

# Read and encode the invoice image
with open("invoice.png", "rb") as image_file:
    encoded_string = base64.b64encode(image_file.read()).decode('utf-8')

# Send invoice for recognition
url = "https://api.aspose.cloud/v5.0/ocr/RecognizeAndParseInvoice"
headers = {
    "Accept": "text/plain",
    "Content-Type": "application/json",
    "Authorization": f"Bearer {token}"
}
payload = {
    "image": encoded_string,
    "settings": {
        "language": "English",
        "makeSkewCorrect": True,
        "rotate": 0,
        "makeBinarization": False,
        "makeUpsampling": False,
        "makeSpellCheck": True
    }
}

response = requests.post(url, headers=headers, data=json.dumps(payload))
task_id = response.text

print(f"Task ID: {task_id}")
```

### C# Example
```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Text;
using System.Text.Json;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        // Authentication
        var authClient = new HttpClient();
        var authContent = new StringContent(
            "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET",
            Encoding.UTF8,
            "application/x-www-form-urlencoded");
        
        var authResponse = await authClient.PostAsync("https://api.aspose.cloud/connect/token", authContent);
        var authResult = await authResponse.Content.ReadAsStringAsync();
        var token = JsonDocument.Parse(authResult).RootElement.GetProperty("access_token").GetString();
        
        // Read and encode the invoice image
        byte[] imageBytes = File.ReadAllBytes("invoice.png");
        string base64Image = Convert.ToBase64String(imageBytes);
        
        // Create request payload
        var payload = new
        {
            image = base64Image,
            settings = new
            {
                language = "English",
                makeSkewCorrect = true,
                rotate = 0,
                makeBinarization = false,
                makeUpsampling = false,
                makeSpellCheck = true
            }
        };
        
        // Send invoice for recognition
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Authorization", $"Bearer {token}");
        
        var content = new StringContent(
            JsonSerializer.Serialize(payload),
            Encoding.UTF8,
            "application/json");
            
        var response = await client.PostAsync(
            "https://api.aspose.cloud/v5.0/ocr/RecognizeAndParseInvoice", 
            content);
            
        var taskId = await response.Content.ReadAsStringAsync();
        
        Console.WriteLine($"Task ID: {taskId}");
    }
}
```

## What You've Learned

In this tutorial, you've learned how to:

- Prepare and encode invoice images for OCR processing
- Authenticate with the Aspose.OCR Cloud API
- Configure optimal recognition settings for invoices
- Submit an invoice for recognition and receive a task ID
- Handle various image quality issues using API settings

## Further Practice

To reinforce your learning:

1. Try sending invoices with different recognition settings to see how they affect results
2. Process invoices in different languages by changing the language setting
3. Test with both good and poor quality images to understand the limitations
4. Use the available preprocessing options to improve recognition of problematic invoices

## Next Steps

Now that you've successfully submitted an invoice for recognition, proceed to the next tutorial: [How to Fetch Parsed Invoice Data](/recognize-parse-invoice/fetch-recognition-result/) to learn how to retrieve and work with the structured data extracted from your invoice.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post in our [community forum](https://forum.aspose.cloud/c/ocr/12/) for assistance.
