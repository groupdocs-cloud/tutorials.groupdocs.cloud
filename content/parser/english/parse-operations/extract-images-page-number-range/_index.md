---
title: How to Extract Images by a Page Number Range Tutorial
url: /parse-operations/extract-images-page-number-range/
weight: 2
description: Learn how to extract images from specific pages in documents using GroupDocs.Parser Cloud API in this step-by-step tutorial for developers
---

# Tutorial: How to Extract Images by a Page Number Range

## Learning Objectives

In this tutorial, you'll learn how to:
- Extract images from specific pages in a document
- Define page ranges for targeted image extraction
- Process and save page-specific images in different programming languages

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Parser Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret (available from the [dashboard](https://dashboard.groupdocs.cloud/#/apps))
3. A multi-page document with images uploaded to your cloud storage

## The Practical Scenario

Imagine you're building an application that needs to:
- Extract images only from specific sections of a document
- Process only visual elements from relevant pages instead of the entire document
- Create visual presentations focused on particular document pages

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

To extract images from specific pages, you'll make a POST request to the images endpoint with the following parameters:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/images" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"StartPageNumber\": 1,
    \"CountPagesToExtract\": 2,
    \"FileInfo\": {
        \"FilePath\": \"pdf/template-document.pdf\",
        \"StorageName\": \"\"
    }
}"
```

The key parameters here are:
- `StartPageNumber`: The zero-based index of the first page to extract images from (1 = second page)
- `CountPagesToExtract`: The number of pages to extract images from, starting from the start page

## Step 3: Execute the Request and Process the Response

When you execute the request, the API will return a JSON response containing information about the extracted images, organized by page:

```json
{
    "pages": [
        {
            "pageIndex": 1,
            "images": [
                {
                    "path": "parser/images/pdf/template-document_pdf/page_1/image_0.jpeg",
                    "downloadUrl": "https://api.groupdocs.cloud/v1.0/parser/storage/file/parser/images/pdf/template-document_pdf/page_1/image_0.jpeg"
                }
            ]
        },
        {
            "pageIndex": 2,
            "images": [
                {
                    "path": "parser/images/pdf/template-document_pdf/page_2/image_0.jpeg",
                    "downloadUrl": "https://api.groupdocs.cloud/v1.0/parser/storage/file/parser/images/pdf/template-document_pdf/page_2/image_0.jpeg"
                }
            ]
        }
    ]
}
```

The response includes:
- `pages`: An array of page objects, each containing:
  - `pageIndex`: The index of the page
  - `images`: An array of images found on that page, each containing:
    - `path`: The storage path where the extracted image is saved
    - `downloadUrl`: A direct URL to download the extracted image

## Try It Yourself

Now it's your turn to try extracting images from specific pages:

1. Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials
2. Update the `FilePath` parameter to point to a multi-page document with images in your storage
3. Adjust the `StartPageNumber` and `CountPagesToExtract` parameters to extract images from different page ranges
4. Execute the curl command and observe how the response changes
5. Try downloading some of the extracted images using the provided `downloadUrl`

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
            
            // Extract images from specific pages
            await ExtractImagesByPageRange(token, "pdf/template-document.pdf", 1, 2);
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
        
        static async Task ExtractImagesByPageRange(string token, string filePath, int startPage, int pageCount)
        {
            using (var client = new HttpClient())
            {
                // Prepare request
                client.DefaultRequestHeaders.Add("Authorization", $"Bearer {token}");
                
                var requestBody = new
                {
                    StartPageNumber = startPage,
                    CountPagesToExtract = pageCount,
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
                var result = JsonConvert.DeserializeObject<PagedImageExtractionResponse>(jsonString);
                
                if (result.Pages != null && result.Pages.Count > 0)
                {
                    Console.WriteLine($"Extracted images from {result.Pages.Count} pages.");
                    
                    // Process each page
                    foreach (var page in result.Pages)
                    {
                        Console.WriteLine($"Page {page.PageIndex} has {page.Images.Count} image(s).");
                        
                        // Download the first image from each page (if available)
                        if (page.Images.Count > 0)
                        {
                            var imageUrl = page.Images[0].DownloadUrl;
                            string fileName = $"page_{page.PageIndex}_image_0.jpeg";
                            await DownloadImage(client, imageUrl, fileName);
                        }
                    }
                }
                else
                {
                    Console.WriteLine("No images found in the specified page range.");
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
        
        class PagedImageExtractionResponse
        {
            public List<PageImageInfo> Pages { get; set; }
        }
        
        class PageImageInfo
        {
            public int PageIndex { get; set; }
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

public class ExtractImagesByPageRangeTutorial {
    
    private static final String BASE_URL = "https://api.groupdocs.cloud/v1.0/parser";
    private static final String AUTH_URL = "https://api.groupdocs.cloud/connect/token";
    
    public static void main(String[] args) throws IOException {
        // Get your ClientID and ClientSecret from https://dashboard.groupdocs.cloud
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Get JWT token
        String token = getAuthToken(clientId, clientSecret);
        
        // Extract images from specific pages
        extractImagesByPageRange(token, "pdf/template-document.pdf", 1, 2);
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
    
    private static void extractImagesByPageRange(String token, String filePath, int startPage, int pageCount) throws IOException {
        URL url = new URL(BASE_URL + "/images");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setRequestProperty("Accept", "application/json");
        conn.setRequestProperty("Authorization", "Bearer " + token);
        conn.setDoOutput(true);
        
        String requestBody = String.format(
            "{\"StartPageNumber\":%d,\"CountPagesToExtract\":%d,\"FileInfo\":{\"FilePath\":\"%s\",\"StorageName\":\"\"}}",
            startPage, pageCount, filePath
        );
        
        try (OutputStream os = conn.getOutputStream()) {
            os.write(requestBody.getBytes(StandardCharsets.UTF_8));
        }
        
        try (Scanner scanner = new Scanner(conn.getInputStream(), StandardCharsets.UTF_8.name())) {
            String jsonResponse = scanner.useDelimiter("\\A").next();
            System.out.println("API Response:");
            System.out.println(jsonResponse);
            
            // Parse the response
            JSONObject responseObj = new JSONObject(jsonResponse);
            
            if (responseObj.has("pages")) {
                JSONArray pages = responseObj.getJSONArray("pages");
                System.out.println("Extracted images from " + pages.length() + " pages.");
                
                // Process each page
                for (int i = 0; i < pages.length(); i++) {
                    JSONObject page = pages.getJSONObject(i);
                    int pageIndex = page.getInt("pageIndex");
                    JSONArray images = page.getJSONArray("images");
                    
                    System.out.println("Page " + pageIndex + " has " + images.length() + " image(s).");
                    
                    // Download the first image from each page (if available)
                    if (images.length() > 0) {
                        JSONObject firstImage = images.getJSONObject(0);
                        String downloadUrl = firstImage.getString("downloadUrl");
                        String fileName = "page_" + pageIndex + "_image_0.jpeg";
                        downloadImage(token, downloadUrl, fileName);
                    }
                }
            } else {
                System.out.println("No images found in the specified page range.");
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

## Understanding Page Indexing

It's important to remember that page indexing in the API is zero-based:
- Page 1 of the document is represented as index 0
- Page 2 of the document is represented as index 1
- And so on...

When setting the `StartPageNumber` parameter, make sure to use the zero-based index of the page you want to start from.

## Learning Checkpoint

Take a moment to test your understanding:

1. If you want to extract images from pages 3-5 of a document, what values should you use for `StartPageNumber` and `CountPagesToExtract`?
2. How would you modify the request to extract images only from the last page of a document if you don't know how many pages it has?
3. What's the difference between the response structure of extracting images from the whole document vs. extracting images by page range?

## Practical Use Cases

Here are some practical applications for page-specific image extraction:

### Document Previews
Create preview images for specific pages of documents in a document management system.

```javascript
// Frontend code example (using extracted image URLs)
function createDocumentPreview(pageImages) {
    const previewContainer = document.getElementById('preview-container');
    
    pageImages.forEach(page => {
        const pageDiv = document.createElement('div');
        pageDiv.className = 'page-preview';
        
        page.images.forEach(image => {
            const img = document.createElement('img');
            img.src = image.downloadUrl;
            img.alt = `Image from page ${page.pageIndex}`;
            pageDiv.appendChild(img);
        });
        
        previewContainer.appendChild(pageDiv);
    });
}
```

### Targeted Image Processing
Extract and process images from only the relevant sections of a document, saving time and resources.

```python
# Python example for image processing
import requests
from PIL import Image
from io import BytesIO

def process_document_images(download_urls):
    processed_images = []
    
    for url in download_urls:
        # Download the image
        response = requests.get(url)
        img = Image.open(BytesIO(response.content))
        
        # Process the image (example: convert to grayscale)
        processed = img.convert('L')
        
        processed_images.append(processed)
    
    return processed_images
```

## Common Issues and Troubleshooting

- Page Range Out of Bounds: If you specify a page range that's outside the document's boundaries, you'll receive an error. Make sure your `StartPageNumber` and `CountPagesToExtract` parameters are valid for your document.

- No Images on Specified Pages: If the specified pages don't contain any images, the API will return empty image arrays for those pages. Always check if the `images` array has elements before trying to access them.

- Different Page Counts: Remember that page numbering starts from 0 in the API, but may start from 1 in the document viewer. This can cause confusion when specifying page ranges.

## What You've Learned

In this tutorial, you've learned:
- How to extract images from specific pages in a document
- How to specify page ranges using `StartPageNumber` and `CountPagesToExtract`
- How to process the page-specific images in your application
- How to download and use the extracted images

## Next Steps

Now that you know how to extract images from specific pages, you can:
- Learn about [Extracting Images from Documents in Containers](url: /parse-operations/extract-images-document-inside-container/) for more advanced scenarios
- Combine text and image extraction techniques to create comprehensive document processing workflows

## Further Practice

Try creating an application that:
1. Extracts images from each page of a presentation (PPTX) file
2. Creates a thumbnail gallery of all images organized by page
3. Adds page number overlays to each extracted image
4. Provides a UI to navigate through the document images by page

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
