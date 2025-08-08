---
title: How to Extract Images from the Whole Document Tutorial
url: /parse-operations/extract-images-whole-document/
weight: 1
description: Learn how to extract all images from documents using GroupDocs.Parser Cloud API in this step-by-step tutorial for developers
---

# Tutorial: How to Extract Images from the Whole Document

## Learning Objectives

In this tutorial, you'll learn how to:
- Set up the GroupDocs.Parser Cloud API for image extraction
- Extract all images from a document with a simple API call
- Download and process the extracted images in different programming languages

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Parser Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret (available from the [dashboard](https://dashboard.groupdocs.cloud/#/apps))
3. A document with images uploaded to your cloud storage

## The Practical Scenario

Imagine you need to extract all images from a document to:
- Create a gallery of document images
- Process images for OCR or other image analysis
- Archive document visual assets separately

This tutorial will show you how to implement this functionality step by step.

## Step 1: Obtain Authorization Token

Before making any API calls, you need to authenticate with the GroupDocs API using your Client ID and Client Secret.

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

This will return a JWT token that you'll use in subsequent requests.

## Step 2: Prepare Your API Request

To extract images from a document, you'll make a POST request to the images endpoint with the following parameters:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/images" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"FileInfo\": {
        \"FilePath\": \"containers/archive/zip-eml-jpg-pdf.zip\",
        \"StorageName\": \"\"
    }
}"
```

Note that you only need to specify the `FilePath` parameter to extract images from the entire document.

## Step 3: Execute the Request and Process the Response

When you execute the request, the API will return a JSON response containing information about the extracted images:

```json
{
    "images": [
        {
            "path": "parser/images/containers/archive/zip-eml-jpg-pdf_zip/template-document_pdf/image_0.jpeg",
            "downloadUrl": "https://api.groupdocs.cloud/v1.0/parser/storage/file/parser/images/containers/archive/zip-eml-jpg-pdf_zip/template-document_pdf/image_0.jpeg"
        },
        {
            "path": "parser/images/containers/archive/zip-eml-jpg-pdf_zip/template-document_pdf/image_1.jpeg",
            "downloadUrl": "https://api.groupdocs.cloud/v1.0/parser/storage/file/parser/images/containers/archive/zip-eml-jpg-pdf_zip/template-document_pdf/image_1.jpeg"
        },
        {
            "path": "parser/images/containers/archive/zip-eml-jpg-pdf_zip/template-document_pdf/image_2.jpeg",
            "downloadUrl": "https://api.groupdocs.cloud/v1.0/parser/storage/file/parser/images/containers/archive/zip-eml-jpg-pdf_zip/template-document_pdf/image_2.jpeg"
        },
        {
            "path": "parser/images/containers/archive/zip-eml-jpg-pdf_zip/template-document_pdf/image_3.jpeg",
            "downloadUrl": "https://api.groupdocs.cloud/v1.0/parser/storage/file/parser/images/containers/archive/zip-eml-jpg-pdf_zip/template-document_pdf/image_3.jpeg"
        }
    ]
}
```

The response includes:
- `path`: The storage path where the extracted image is saved
- `downloadUrl`: A direct URL to download the extracted image

## Try It Yourself

Now it's your turn to try extracting images from your own document:

1. Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials
2. Update the `FilePath` parameter to point to a document with images in your storage
3. Execute the curl command and observe the response
4. Try downloading one of the extracted images using the provided `downloadUrl`

## Implementation in Different Languages

### C# Example

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json;

namespace GroupDocsParserCloudTutorial
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // Get your ClientID and ClientSecret from https://dashboard.groupdocs.cloud
            string clientId = "YOUR_CLIENT_ID";
            string clientSecret = "YOUR_CLIENT_SECRET";
            
            // Get JWT token
            string token = await GetAuthToken(clientId, clientSecret);
            
            // Extract images from the document
            await ExtractImages(token, "documents/document-with-images.pdf");
        }
        
        static async Task<string> GetAuthToken(string clientId, string clientSecret)
        {
            using (var client = new HttpClient())
            {
                // Prepare request
                var requestBody = $"grant_type=client_credentials&client_id={clientId}&client_secret={clientSecret}";
                var content = new StringContent(requestBody, Encoding.UTF8, "application/x-www-form-urlencoded");
                
                // Send request
                var response = await client.PostAsync("https://api.groupdocs.cloud/connect/token", content);
                
                // Process response
                var jsonString = await response.Content.ReadAsStringAsync();
                var token = JsonConvert.DeserializeObject<Dictionary<string, string>>(jsonString);
                
                return token["access_token"];
            }
        }
        
        static async Task ExtractImages(string token, string filePath)
        {
            using (var client = new HttpClient())
            {
                // Prepare request
                client.DefaultRequestHeaders.Add("Authorization", $"Bearer {token}");
                
                var requestBody = new
                {
                    FileInfo = new
                    {
                        FilePath = filePath,
                        StorageName = ""
                    }
                };
                
                var content = new StringContent(JsonConvert.SerializeObject(requestBody), Encoding.UTF8, "application/json");
                
                // Send request
                var response = await client.PostAsync("https://api.groupdocs.cloud/v1.0/parser/images", content);
                
                // Process response
                var jsonString = await response.Content.ReadAsStringAsync();
                Console.WriteLine("API Response:");
                Console.WriteLine(jsonString);
                
                // Parse the response
                var result = JsonConvert.DeserializeObject<ImageExtractionResponse>(jsonString);
                
                if (result.Images != null && result.Images.Count > 0)
                {
                    Console.WriteLine($"Extracted {result.Images.Count} images.");
                    
                    // Download the first image as an example
                    if (result.Images.Count > 0)
                    {
                        var imageUrl = result.Images[0].DownloadUrl;
                        await DownloadImage(client, imageUrl, "extracted_image_0.jpeg");
                    }
                }
                else
                {
                    Console.WriteLine("No images found in the document.");
                }
            }
        }
        
        static async Task DownloadImage(HttpClient client, string imageUrl, string localPath)
        {
            Console.WriteLine($"Downloading image from: {imageUrl}");
            
            var imageBytes = await client.GetByteArrayAsync(imageUrl);
            File.WriteAllBytes(localPath, imageBytes);
            
            Console.WriteLine($"Image downloaded to: {localPath}");
        }
        
        class ImageExtractionResponse
        {
            public List<ImageInfo> Images { get; set; }
        }
        
        class ImageInfo
        {
            public string Path { get; set; }
            public string DownloadUrl { get; set; }
        }
    }
}
```

### Java Example

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.util.Scanner;
import org.json.JSONArray;
import org.json.JSONObject;

public class ExtractImagesTutorial {
    
    private static final String BASE_URL = "https://api.groupdocs.cloud/v1.0/parser";
    private static final String AUTH_URL = "https://api.groupdocs.cloud/connect/token";
    
    public static void main(String[] args) throws IOException {
        // Get your ClientID and ClientSecret from https://dashboard.groupdocs.cloud
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Get JWT token
        String token = getAuthToken(clientId, clientSecret);
        
        // Extract images from the document
        extractImages(token, "documents/document-with-images.pdf");
    }
    
    private static String getAuthToken(String clientId, String clientSecret) throws IOException {
        URL url = new URL(AUTH_URL);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        conn.setDoOutput(true);
        
        String requestBody = "grant_type=client_credentials&client_id=" + clientId + "&client_secret=" + clientSecret;
        try (OutputStream os = conn.getOutputStream()) {
            os.write(requestBody.getBytes(StandardCharsets.UTF_8));
        }
        
        try (Scanner scanner = new Scanner(conn.getInputStream(), StandardCharsets.UTF_8.name())) {
            String jsonResponse = scanner.useDelimiter("\\A").next();
            JSONObject jsonObject = new JSONObject(jsonResponse);
            return jsonObject.getString("access_token");
        }
    }
    
    private static void extractImages(String token, String filePath) throws IOException {
        URL url = new URL(BASE_URL + "/images");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setRequestProperty("Accept", "application/json");
        conn.setRequestProperty("Authorization", "Bearer " + token);
        conn.setDoOutput(true);
        
        String requestBody = "{\"FileInfo\":{\"FilePath\":\"" + filePath + "\",\"StorageName\":\"\"}}";
        try (OutputStream os = conn.getOutputStream()) {
            os.write(requestBody.getBytes(StandardCharsets.UTF_8));
        }
        
        try (Scanner scanner = new Scanner(conn.getInputStream(), StandardCharsets.UTF_8.name())) {
            String jsonResponse = scanner.useDelimiter("\\A").next();
            System.out.println("API Response:");
            System.out.println(jsonResponse);
            
            // Parse the response
            JSONObject responseObj = new JSONObject(jsonResponse);
            
            if (responseObj.has("images")) {
                JSONArray images = responseObj.getJSONArray("images");
                System.out.println("Extracted " + images.length() + " images.");
                
                // Download the first image as an example
                if (images.length() > 0) {
                    JSONObject firstImage = images.getJSONObject(0);
                    String downloadUrl = firstImage.getString("downloadUrl");
                    downloadImage(token, downloadUrl, "extracted_image_0.jpeg");
                }
            } else {
                System.out.println("No images found in the document.");
            }
        }
    }
    
    private static void downloadImage(String token, String imageUrl, String localPath) throws IOException {
        System.out.println("Downloading image from: " + imageUrl);
        
        URL url = new URL(imageUrl);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");
        conn.setRequestProperty("Authorization", "Bearer " + token);
        
        try (InputStream is = conn.getInputStream();
             FileOutputStream fos = new FileOutputStream(localPath)) {
            
            byte[] buffer = new byte[4096];
            int bytesRead;
            
            while ((bytesRead = is.read(buffer)) != -1) {
                fos.write(buffer, 0, bytesRead);
            }
        }
        
        System.out.println("Image downloaded to: " + localPath);
    }
}
```

## What You Can Do with Extracted Images

Once you've extracted the images, you can:

1. Display them in your application: Use the download URLs to display the images in your web or mobile application.

2. Process them for analysis: Download the images and process them using image analysis libraries for tasks like OCR, object detection, or facial recognition.

3. Store them separately: Save the images to a dedicated image storage system or database for better organization and retrieval.

4. Create image galleries: Build image galleries that showcase all the visuals contained in a document.

## Common Issues and Troubleshooting

- No Images Extracted: If no images are returned, ensure that the document actually contains embedded images. Some documents might have vector graphics or other non-extractable visual elements.

- Image Quality: The extracted images are saved as JPEG files, which might result in some quality loss for certain types of images. If you need lossless extraction, you might need to use a different approach.

- Access Denied: Ensure that you're using a valid token and that you have the necessary permissions to access the document in storage.

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with the GroupDocs.Parser Cloud API
- How to extract all images from a document
- How to download and process the extracted images
- How to implement this functionality in C# and Java

## Next Steps

Now that you've mastered extracting images from an entire document, you can:
- Follow our tutorial on [Extracting Images by Page Number Range](/parse-operations/extract-images-page-number-range/) to learn how to extract images from specific pages
- Learn about combining [Text Extraction](/parse-operations/extract-images-whole-document) with image extraction for comprehensive document processing

## Further Practice

Try extracting images from various document formats (PDF, DOCX, PPTX, etc.) to understand how the API handles different document structures. Experiment with combining multiple API operations to build a more comprehensive document processing solution.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
