---
title: Introduction to Image Scaling with Aspose.CAD Cloud Tutorial
url: /change-scale-of-an-image/introduction/
weight: 10
description: Learn the fundamentals of scaling CAD drawings using Aspose.CAD Cloud API in this beginner-friendly tutorial
---

# Tutorial: Introduction to Image Scaling with Aspose.CAD Cloud

## Learning Objectives

In this tutorial, you'll learn:
- What image scaling means in the context of CAD files
- The available API endpoints for scaling operations in Aspose.CAD Cloud
- How to authenticate with the Aspose.CAD Cloud API
- Basic concepts you'll need for implementing scaling operations

## Prerequisites

Before starting this tutorial, you should have:
- An Aspose Cloud account ([sign up for free](https://dashboard.aspose.cloud/#/apps))
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- Basic understanding of RESTful APIs
- Familiarity with JSON data format

## Introduction to CAD Image Scaling

When working with CAD drawings, scaling refers to changing the dimensions of an image while maintaining or adjusting its proportions. Unlike simple image resizing in photo editing, CAD scaling often needs to preserve precise measurements and aspect ratios to ensure the technical accuracy of the drawing.

Aspose.CAD Cloud provides powerful APIs that enable you to:
- Change the dimensions of CAD drawings to specific width and height
- Convert between different CAD formats while scaling
- Maintain aspect ratios during scaling if needed
- Process CAD files without requiring CAD software installation

## Understanding Aspose.CAD Cloud API Structure

Aspose.CAD Cloud offers two primary endpoints for scaling operations:

1. GET /cad/{name}/resize - For resizing existing drawings stored in the cloud storage
2. POST /cad/resize - For uploading and resizing drawings in a single operation

Both endpoints accept similar parameters:

- `outputFormat` - The format of the output file (PDF, PNG, JPEG, etc.)
- `newWidth` - The desired width of the output image in pixels
- `newHeight` - The desired height of the output image in pixels

## Authentication with Aspose.CAD Cloud

Before you can use any Aspose.CAD Cloud API, you need to obtain an access token. Let's learn how to authenticate:

### Step 1: Obtain Your Credentials

1. Log in to the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
2. Create a new application or use an existing one
3. Note your Client ID (App SID) and Client Secret

### Step 2: Request an Access Token

To get an access token, you need to make a POST request to the Aspose Cloud authentication endpoint:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

### Step 3: Use the Access Token

The response will contain a JWT token that you'll use in the Authorization header for all subsequent API calls:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

## Key Concepts for CAD Image Scaling

Before implementing scaling operations, let's understand some key concepts:

### 1. CAD File Storage

Aspose.CAD Cloud can work with files from:
- Aspose Cloud Storage (default)
- Third-party cloud storage (Amazon S3, Google Drive, etc.)
- Direct file uploads in API requests

### 2. Output Formats

When scaling CAD drawings, you can simultaneously convert them to various formats:
- Raster formats: JPG, PNG, BMP, TIFF, GIF
- Vector formats: SVG, WMF
- Document formats: PDF

### 3. Scaling Parameters

The essential parameters for scaling include:
- `newWidth` - Target width in pixels
- `newHeight` - Target height in pixels
- `outputFormat` - Desired output format

## Try It Yourself: Authentication

Let's practice obtaining an authentication token:

1. Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` in the following command with your actual credentials:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

2. Run the command in your terminal
3. Copy the obtained access token for use in future tutorials

## Learning Checkpoint

Test your understanding of the concepts covered:

1. What are the two main API endpoints for scaling CAD images?
2. What parameter would you use to specify the desired height of the scaled image?
3. What authorization type is used with Aspose.CAD Cloud API?
4. Name three possible output formats when scaling a CAD drawing.

## What You've Learned

In this tutorial, you've learned:
- The fundamentals of CAD image scaling
- The available Aspose.CAD Cloud API endpoints for scaling operations
- How to authenticate with the Aspose.CAD Cloud API
- Key concepts for implementing scaling operations

## Next Steps

Now that you understand the basics, you're ready to implement your first scaling operation!

Continue to the next tutorial: [Basic Image Resizing Using GET Method](/change-scale-of-an-image/basic-get-method/) to learn how to scale CAD drawings with simple parameters using HTTP GET requests.

## Further Practice

To reinforce your understanding:
- Explore the [Aspose.CAD Cloud API Reference](https://reference.aspose.cloud/cad/) to see all available endpoints
- Try obtaining an authentication token using different programming languages
- Familiarize yourself with the various output formats supported by Aspose.CAD Cloud

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cad/)
- [Documentation](https://docs.aspose.cloud/cad/)
- [Live Demo](https://products.aspose.app/cad/family)
- [API Reference](https://reference.aspose.cloud/cad/)
- [Blog](https://blog.aspose.cloud/category/cad/)
- [Free Support](https://forum.aspose.cloud/c/cad/28/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Post them on the [Aspose.CAD Cloud Forum](https://forum.aspose.cloud/c/cad/28/) for quick assistance!
