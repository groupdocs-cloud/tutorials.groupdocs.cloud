---
title: How to Send Image Regions for Recognition Tutorial
weight: 20
url: /recognize-regions/send-for-recognition/
description: Learn step-by-step how to define and send image regions for text extraction using Aspose.OCR Cloud API in this hands-on developer tutorial.
---

# Tutorial: How to Send Image Regions for Recognition

## Learning Objectives

In this tutorial, you'll learn:
- How to obtain an authentication token for Aspose.OCR Cloud API
- How to properly define regions in an image for text extraction
- How to configure recognition settings for optimal results
- How to send an image with regions for OCR processing via REST API

## Prerequisites

- Basic understanding of REST APIs
- A tool for making API requests (cURL, Postman, or programming language of your choice)
- An Aspose Cloud account ([sign up for free](https://dashboard.aspose.cloud/#/apps))
- A sample image containing text for recognition

## Practical Scenario

Imagine you're developing an application that processes ID cards or driver's licenses. Instead of extracting all text from the image, you need to target specific fields like name, date of birth, and ID number. Region recognition allows you to precisely extract only the data you need.

## Step 1: Obtain an Access Token

Before you can send images for recognition, you need to authenticate with the Aspose.OCR Cloud API:

1. Go to the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
2. Create a new application or use an existing one
3. Note your Client ID and Client Secret
4. Use these credentials to obtain an access token

Here's how to get your access token using cURL:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

The response will contain an `access_token` which you'll use for authorization in subsequent requests.

## Step 2: Prepare Your Image

For this tutorial, you should have a sample image ready for recognition. The image can be:
- A scanned document
- A photograph of a document
- Any image containing readable text

For optimal results:
- Ensure the image is clear and well-lit
- Text should be legible
- The image should be properly oriented

You'll need to convert your image to a Base64 encoded string. Many online tools can do this for you, or you can use programming libraries.

## Try it Yourself: Convert an Image to Base64

Using a programming language like Python:

```python
import base64

# Read image file
with open("your_image.jpg", "rb") as image_file:
    encoded_string = base64.b64encode(image_file.read()).decode('utf-8')
    
print(encoded_string)  # This is what you'll include in your API request
```

## Step 3: Define Your Recognition Regions

Next, you need to define the specific regions of the image from which you want to extract text. Each region is defined by:

1. The coordinates of its top-left corner (`topLeftX`, `topLeftY`)
2. The coordinates of its bottom-right corner (`bottomRightX`, `bottomRightY`)
3. An order number that determines the sequence in the results

For example, if you want to extract text from two regions of an ID card:

```json
"regions": [
  {
    "rect": {
      "topLeftX": 20,
      "topLeftY": 40,
      "bottomRightX": 300,
      "bottomRightY": 80
    },
    "order": 0
  },
  {
    "rect": {
      "topLeftX": 20,
      "topLeftY": 100,
      "bottomRightX": 300,
      "bottomRightY": 140
    },
    "order": 1
  }
]
```

## Step 4: Configure Recognition Settings

Aspose.OCR Cloud allows you to fine-tune the recognition process with various settings:

- `language`: Specify the language of the text ("English", "French", etc.)
- `makeSkewCorrect`: Automatically correct image tilt
- `rotate`: Manually rotate the image by a specific angle
- `makeBinarization`: Convert the image to black and white
- `makeContrastCorrection`: Improve image contrast
- `makeUpsampling`: Upscale image for better recognition of small text
- `makeSpellCheck`: Automatically correct common spelling errors
- `resultType`: Format of the recognition results ("Text", "JSON", etc.)

Choose settings appropriate for your image and use case.

## Step 5: Send the Image for Recognition

Now you're ready to send your image for region recognition. You'll make a POST request to the Aspose.OCR Cloud API endpoint with your image, regions, and settings:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeRegions' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data '{
	"image": "YOUR_BASE64_ENCODED_IMAGE",
	"settings": {
		"language": "English",
		"makeSkewCorrect": true,
		"makeContrastCorrection": true,
		"resultType": "Text",
		"regions": [
			{
				"order": 0,
				"rect": {
					"topLeftX": 20,
					"topLeftY": 40,
					"bottomRightX": 300,
					"bottomRightY": 80
				}
			},
			{
				"order": 1,
				"rect": {
					"topLeftX": 20,
					"topLeftY": 100,
					"bottomRightX": 300,
					"bottomRightY": 140
				}
			}
		]
	}
}'
```

## Try it Yourself: Full API Request Example

Here's a complete example using Python with the requests library:

```python
import requests
import base64
import json

# Step 1: Obtain access token (assuming you already have it)
access_token = "YOUR_ACCESS_TOKEN"

# Step 2: Prepare your image
with open("your_image.jpg", "rb") as image_file:
    encoded_image = base64.b64encode(image_file.read()).decode('utf-8')

# Step 3 & 4: Define regions and settings
payload = {
    "image": encoded_image,
    "settings": {
        "language": "English",
        "makeSkewCorrect": True,
        "makeContrastCorrection": True,
        "resultType": "Text",
        "regions": [
            {
                "order": 0,
                "rect": {
                    "topLeftX": 20,
                    "topLeftY": 40,
                    "bottomRightX": 300,
                    "bottomRightY": 80
                }
            },
            {
                "order": 1,
                "rect": {
                    "topLeftX": 20,
                    "topLeftY": 100,
                    "bottomRightX": 300,
                    "bottomRightY": 140
                }
            }
        ]
    }
}

# Step 5: Send request
headers = {
    "Accept": "text/plain",
    "Content-Type": "application/json",
    "Authorization": f"Bearer {access_token}"
}

response = requests.post(
    "https://api.aspose.cloud/v5.0/ocr/RecognizeRegions",
    headers=headers,
    data=json.dumps(payload)
)

# Check response
if response.status_code == 200:
    task_id = response.text
    print(f"Success! Task ID: {task_id}")
    print(f"Use this ID to fetch results in the next tutorial")
else:
    print(f"Error: {response.status_code}")
    print(response.text)
```

## Understanding the Response

If your request is successful, the API will return a unique task ID (GUID) that looks something like this:

```
b7162f77-bf10-407d-86de-ee255e422ca7
```

This ID is essential for retrieving your recognition results in the next step of the workflow.

## Troubleshooting Tips

If you encounter issues:

- 401 Unauthorized: Your access token may be invalid or expired. Generate a new one.
- 400 Bad Request: Check your JSON format and ensure all required parameters are included.
- Request Too Large: Base64 encoded images can be very long. If using cURL from a shell, you might hit command length limits. Consider using a programming language instead.
- Invalid Region Coordinates: Make sure your regions are within the image boundaries.

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with the Aspose.OCR Cloud API
- How to properly define regions in an image
- How to configure recognition settings for optimal results
- How to send an image for region recognition
- How to interpret the API response

## Next Steps

Now that you've successfully sent an image for region recognition, the next step is to retrieve the recognition results:

[Tutorial: How to Fetch Region Recognition Results](/recognize-regions/fetch-recognition-result/)

## Further Practice

To strengthen your understanding:
- Try defining different regions on various types of documents
- Experiment with different recognition settings
- Test the effect of image quality on recognition accuracy

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/ocr/12/).
