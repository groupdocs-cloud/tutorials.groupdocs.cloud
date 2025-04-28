---
title: How to Send Images for Region Detection Tutorial
weight: 10
url: /detect-regions/send-for-detection/
description: Learn how to properly send images to Aspose.OCR Cloud API for region detection in this step-by-step developer tutorial.
---

# Tutorial: How to Send Images for Region Detection

## Learning Objectives

In this tutorial, you'll learn how to:
- Prepare an image for region detection
- Configure optimal region detection settings
- Send an image to the Aspose.OCR Cloud API
- Handle the API response correctly

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
- Basic understanding of REST API calls
- A tool for making HTTP requests (cURL, Postman, or your preferred programming language)

## Introduction

Region detection is a powerful capability that allows you to identify specific areas of interest in an image, such as text blocks, tables, and other content regions. This tutorial will guide you through the process of sending an image to the Aspose.OCR Cloud API for region detection, which is the first critical step in the overall region detection workflow.

## Step 1: Obtain an Access Token

Before sending an image for region detection, you need to authenticate your request with an access token.

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded"
```

This will return a JSON response containing your access token:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

Save this access token for use in the next steps.

## Step 2: Prepare Your Image

For region detection, you need to encode your image as a Base64 string. Here's how to do it:

### Using cURL and Base64 Command

```bash
BASE64_IMAGE=$(base64 -i your_image.png)
```

### Using Python

```python
import base64

with open("your_image.png", "rb") as image_file:
    encoded_string = base64.b64encode(image_file.read()).decode('utf-8')
```

### Using JavaScript

```javascript
const fs = require('fs');

const image = fs.readFileSync('your_image.png');
const base64Image = image.toString('base64');
```

### Using C#

```csharp
using System;
using System.IO;

string imagePath = "your_image.png";
byte[] imageBytes = File.ReadAllBytes(imagePath);
string base64Image = Convert.ToBase64String(imageBytes);
```

## Step 3: Configure Detection Settings

The region detection API accepts various settings to optimize the detection process. Let's explore the key settings:

| Setting | Description | Recommended Value |
|---------|-------------|-------------------|
| `language` | The language of the text in the image | Use "English" for English text |
| `makeSkewCorrect` | Automatically correct image tilt | `true` for most documents |
| `rotate` | Rotate image by specified degrees | `0` unless image is rotated |
| `makeBinarization` | Convert image to black and white | `false` unless dealing with low contrast |
| `makeContrastCorrection` | Increase image contrast | `true` for most documents |
| `makeUpsampling` | Intellectually upscale image | `false` unless detecting dense lines |
| `dsrMode` | Document structure analysis algorithm | "Regions" for general use |
| `dsrConfidence` | Threshold for filtering content blocks | "Default" for most cases |

## Step 4: Create the Request JSON

Now, create a JSON object that includes your Base64-encoded image and detection settings:

```json
{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "rotate": 0,
    "makeBinarization": false,
    "makeContrastCorrection": true,
    "makeUpsampling": false,
    "dsrMode": "Regions",
    "dsrConfidence": "Default"
  }
}
```

## Step 5: Send the API Request

Send your image for region detection using a POST request to the Aspose.OCR Cloud API:

### Using cURL

```bash
curl --location 'https://api.aspose.cloud/v5.0/ocr/DetectRegions' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data '{
  "image": "YOUR_BASE64_ENCODED_IMAGE",
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "rotate": 0,
    "makeBinarization": false,
    "makeContrastCorrection": true,
    "makeUpsampling": false,
    "dsrMode": "Regions",
    "dsrConfidence": "Default"
  }
}'
```

### Using Python

```python
import requests
import json
import base64

# Your access token and image
access_token = "YOUR_ACCESS_TOKEN"
image_path = "your_image.png"

# Encode image to Base64
with open(image_path, "rb") as image_file:
    encoded_string = base64.b64encode(image_file.read()).decode('utf-8')

# Create request payload
payload = {
    "image": encoded_string,
    "settings": {
        "language": "English",
        "makeSkewCorrect": True,
        "rotate": 0,
        "makeBinarization": False,
        "makeContrastCorrection": True,
        "makeUpsampling": False,
        "dsrMode": "Regions",
        "dsrConfidence": "Default"
    }
}

# Set headers
headers = {
    "Accept": "text/plain",
    "Content-Type": "application/json",
    "Authorization": f"Bearer {access_token}"
}

# Send request
response = requests.post(
    "https://api.aspose.cloud/v5.0/ocr/DetectRegions",
    headers=headers,
    data=json.dumps(payload)
)

# Print response
print(response.text)
```

### Using C#

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json;

public class RegionDetectionExample
{
    public static async Task SendForDetection()
    {
        // Your access token
        string accessToken = "YOUR_ACCESS_TOKEN";
        
        // Read and encode image
        string imagePath = "your_image.png";
        byte[] imageBytes = File.ReadAllBytes(imagePath);
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
                makeContrastCorrection = true,
                makeUpsampling = false,
                dsrMode = "Regions",
                dsrConfidence = "Default"
            }
        };
        
        // Convert payload to JSON
        string jsonPayload = JsonConvert.SerializeObject(payload);
        
        // Create HTTP client
        using (HttpClient client = new HttpClient())
        {
            // Set headers
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("text/plain"));
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
            
            // Create request content
            StringContent content = new StringContent(jsonPayload, Encoding.UTF8, "application/json");
            
            // Send request
            HttpResponseMessage response = await client.PostAsync(
                "https://api.aspose.cloud/v5.0/ocr/DetectRegions",
                content
            );
            
            // Get response
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

## Step 6: Handle the API Response

If your request is successful, the API will return a unique identifier (GUID) for your region detection task. This identifier will look something like:

```
a371d027-4b0d-4d86-8825-c8d818dd4ed9
```

Save this identifier, as you'll need it in the next tutorial to fetch the detected regions.

## Common Issues and Troubleshooting

### Issue: Base64 String Length Errors

When using cURL in a shell command, you might encounter errors due to the length of the Base64-encoded image:

Solution: Use a request body file instead:

```bash
echo '{
  "image": "'$BASE64_IMAGE'",
  "settings": {
    "language": "English",
    "dsrMode": "Regions"
  }
}' > request.json

curl --location 'https://api.aspose.cloud/v5.0/ocr/DetectRegions' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data @request.json
```

### Issue: Authorization Errors

If you receive a 401 Unauthorized error:

Solution: Verify your access token is correct and hasn't expired. Access tokens typically expire after one hour, so you may need to generate a new one.

## Try It Yourself!

Now it's your turn to practice sending an image for region detection:

1. Choose a sample image containing text blocks or tables
2. Encode it to Base64
3. Configure the detection settings based on your image's characteristics
4. Send the request to the API
5. Save the returned task ID for use in the next tutorial

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the Aspose.OCR Cloud API
- Prepare an image by encoding it to Base64
- Configure region detection settings for optimal results
- Send an image for region detection
- Retrieve and save the task ID for further processing

## Next Steps

In the next tutorial, you'll learn how to [fetch the detected regions](/detect-regions/fetch-regions/) using the task ID you received in this tutorial.

## Further Practice

To reinforce your understanding:
- Try sending images with different content (text-heavy documents, tables, mixed content)
- Experiment with different detection settings to see how they affect the results
- Send the same image with and without automatic corrections (contrast, skew) to compare results

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Please visit our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance.
