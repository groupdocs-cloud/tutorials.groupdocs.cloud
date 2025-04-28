---
title: How to Send OMR Form Source Code for Generation Tutorial
weight: 20
url: /generate-form/send-source-code/
description: Learn how to send your OMR form source code to Aspose.OMR Cloud API for rendering in this step-by-step tutorial for developers.
---

# Tutorial: How to Send OMR Form Source Code for Generation

## Learning Objectives

In this tutorial, you'll learn how to:
- Prepare your OMR form source code for submission
- Send the source code to the Aspose.OMR Cloud API
- Handle images in your form
- Configure form rendering parameters
- Process the API response

## Prerequisites

Before starting this tutorial, you should:
- Have an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps)
- Understand basic REST API concepts
- Have experience with HTTP requests
- Be familiar with JSON formatting

## Introduction

Once you've designed your OMR form source code, the next step is to send it to the Aspose.OMR Cloud API for rendering. This tutorial will walk you through the process of sending your form source code to the API and understanding the response.

## Practical Scenario

Imagine you're creating an educational assessment platform that needs to generate customized test forms for different subjects. You've designed the form structure with the Aspose.OMR markup language, and now you need to convert it into a printable form that can be distributed to students.

## Step 1: Obtain an Access Token

Before sending your form source code, you need to authenticate with the Aspose.OMR Cloud API.

Get your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)

> Try it yourself: Generate an access token using your Client ID and Client Secret. This token will be used in subsequent API calls.

## Step 2: Prepare Your Request Body

The form source code and rendering parameters are provided in JSON format in the request body.

```json
{
  "MarkupFile": "Source code as Base64 string",
  "Images": {
    "image1.png": "Base64 encoded image file",
    "image2.png": "Base64 encoded image file"
  },
  "settings": {
    "FontFamily": "Arial",
    "FontStyle": "Regular",
    "FontSize": 15,
    "PaperSize": "A4",
    "BubbleColor": "Red",
    "PageMarginLeft": 50,
    "Orientation": "Vertical",
    "BubbleSize": "Normal"
  }
}
```

### Encoding the Source Code

Your form's source code must be provided as a Base64 encoded string in the `MarkupFile` property.

For example, if your source code is in a file named `myform.txt`, you can encode it using:

```bash
# Linux/Mac
base64 myform.txt

# Windows PowerShell
[Convert]::ToBase64String([System.IO.File]::ReadAllBytes("myform.txt"))
```

> Tip: Make sure your Base64 string doesn't contain any line breaks when adding it to the JSON request.

### Adding Images to Your Form

If your form contains images, they must be provided as entries in the `Images` property. The key is the image filename referenced in your source code, and the value is the Base64-encoded content of the image file.

```json
"Images": {
  "logo.png": "P3BhZ2U9Cj90ZXh0PUJpb2xvZ3kgUXVpejogUGxhbnRzCglmb...vdW50PTUKJnBhZ2U=",
  "photo.png": "P3BhZ2U9gUXVpejogUGxhbnRzCglmbpejogUGxhbnRzCglmb...vdW50PTUKJnBhZaU="
}
```

> Warning: Base64 encoded images can be very large, especially when using high-resolution photos. You may encounter command length limitations when using cURL in shell commands.

## Step 3: Configure Rendering Parameters

The `Settings` property allows you to configure the paper size, orientation, font, and other global layout settings for your form.

Here are the available settings:

| Setting | Type | Description | Options |
|---------|------|-------------|---------|
| `PaperSize` | string | Physical page size | `A4`, `Letter`, `Legal` |
| `Orientation` | string | Page orientation | `Horizontal` (landscape), `Vertical` (portrait) |
| `PageMarginLeft` | number | Left page margin in pixels | Any number |
| `FontFamily` | string | Font family for all texts | Any [supported font](/omr/appendices/fonts/) |
| `FontSize` | number | Font size in pixels | Any number |
| `FontStyle` | string | Font style | `Regular`, `Bold`, `Italic`, `Underline`, `Strikeout` |
| `BubbleColor` | string | Color of answer bubbles | Any [supported color code](/omr/appendices/colors/) |
| `BubbleSize` | string | Size of answer bubbles | `Extrasmall`, `Small`, `Normal`, `Large`, `Extralarge` |

> Learning checkpoint: Which settings would you use to create a form with blue bubbles on Letter-sized paper in landscape orientation?

## Step 4: Send the POST Request

Now that you've prepared your request body, you can send a POST request to the Aspose.OMR Cloud API.

```bash
curl --location --request POST 'https://api.aspose.cloud/v5.0/omr/GenerateTemplate/PostGenerateTemplate' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data '{
  "MarkupFile": "YOUR_BASE64_ENCODED_SOURCE_CODE",
  "Images": {},
  "Settings": {
    "PaperSize": "A4",
    "BubbleColor": "red"
  }
}'
```

## Step 5: Process the API Response

If successful, the API will return a string with a unique identifier (GUID) of the form generation request in the queue. Save this GUID as you'll need it to fetch the printable form.

Example response:
```
7ec63522-e35f-4f9d-8b06-0da1876220b6
```

If there's an error, the API will return a HTTP status code corresponding to the error.

> Troubleshooting tip: If you receive a 401 error, your access token may have expired. Generate a new token and try again.

## What You've Learned

In this tutorial, you learned how to:
- Prepare your form source code for the Aspose.OMR Cloud API
- Include images in your form request
- Configure form rendering parameters
- Send the request to the API
- Process the API response

## Further Practice

Try these exercises to reinforce your learning:

1. Create a simple OMR form with one question and multiple choice answers, then send it to the API for generation.
2. Add an image to your form and ensure it's correctly included in the request.
3. Experiment with different form settings, such as changing paper size, orientation, and bubble color.

## Next Steps

Now that you know how to send your form source code for generation, the next step is to learn how to [fetch the printable form](/generate-form/fetch-printable-form/) from the API.

## Additional Resources

- [Product Page](https://products.aspose.cloud/omr/)
- [Documentation](https://docs.aspose.cloud/omr/)
- [Live Demo](https://products.aspose.app/omr/family)
- [API Reference](https://reference.aspose.cloud/omr/)
- [Blog](https://blog.aspose.cloud/category/omr/)
- [Free Support](https://forum.aspose.cloud/c/omr/8/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [support forum](https://forum.aspose.cloud/c/omr/8/).
