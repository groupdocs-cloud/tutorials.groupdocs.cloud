---
title: How to Extract Images from a Document Inside a Container Tutorial
url: /parse-operations/extract-images-document-inside-container/
weight: 3
description: Learn how to extract images from documents stored within containers like ZIP archives, emails, or PDF portfolios using GroupDocs.Parser Cloud API
---

# Tutorial: How to Extract Images from a Document Inside a Container

## Learning Objectives

In this tutorial, you'll learn how to:
- Extract images from documents stored within container formats
- Access and process images from nested documents
- Handle password-protected containers and documents
- Download and use images extracted from container documents

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Parser Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret (available from the [dashboard](https://dashboard.groupdocs.cloud/#/apps))
3. A container file (ZIP archive, email, or PDF portfolio) with embedded documents containing images uploaded to your cloud storage

## The Practical Scenario

Imagine you're developing an application that needs to:
- Process images from email attachments without saving them to disk
- Extract visuals from documents in compressed archives
- Access images in PDF portfolios that contain multiple embedded documents

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

To extract images from a document inside a container, you'll make a POST request to the images endpoint with the following parameters:

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
        \"FilePath\": \"pdf/PDF with attachements.pdf\",
        \"StorageName\": \"\",
        \"Password\": \"password\"
    },
    \"ContainerItemInfo\": {
        \"RelativePath\": \"template-document.pdf\",
        \"Password\": \"password\"
    }
}"
```

The key parameters here are:
- `FileInfo`: Information about the container file
  - `FilePath`: The path to the container file in your storage
  - `Password`: If the container is password-protected (optional)
- `ContainerItemInfo`: Information about the document inside the container
  - `RelativePath`: The relative path to the document inside the container
  - `Password`: If the embedded document is password-protected (optional)
- `StartPageNumber` and `CountPagesToExtract`: Optional parameters if you want to extract images from specific pages of the embedded document

## Step 3: Execute the Request and Process the Response

When you execute the request, the API will return a JSON response containing information about the extracted images, organized by page:

```json
{
    "pages": [
        {
            "pageIndex": 1,
            "images": [
                {
                    "path": "parser/images/template-document_pdf/page_1/image_0.jpeg",
                    "downloadUrl": "https://api.groupdocs.cloud/v1.0/parser/storage/file/parser/images/template-document_pdf/page_1/image_0.jpeg"
                }
            ]
        },
        {
            "pageIndex": 2,
            "images": [
                {
                    "path": "parser/images/template-document_pdf/page_2/image_0.jpeg",
                    "downloadUrl": "https://api.groupdocs.cloud/v1.0/parser/storage/file/parser/images/template-document_pdf/page_2/image_0.jpeg"
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

Now it's your turn to try extracting images from a document inside a container:

1. Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials
2. Update the `FilePath` parameter to point to a container file in your storage
3. Update the `RelativePath` parameter to specify the document inside the container
4. Add passwords if necessary
5. Execute the curl command and observe the response
6. Try downloading some of the extracted images using the provided `downloadUrl`

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
            
            // Extract images from a document inside a container
            await ExtractImagesFromContainer(
                token, 
                "pdf/PDF with attachements.pdf", 
                "template-document.pdf", 
                1,                 // Start page (optional)
                2,                 // Page count (optional)
                "container_pass",  // Container password if needed
                "document_pass"    // Document password if needed
            );
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
        
        static async Task ExtractImagesFromContainer(
            string token, 
            string containerPath, 
            string documentPath, 
            int? startPage = null,
            int? pageCount = null,
            string containerPassword = null, 
            string documentPassword = null)
        {
            using (var client = new HttpClient())
            {
                // Prepare request
                client.DefaultRequestHeaders.Add("Authorization", $"Bearer {token}");
                
                // Create request object
                var requestObj = new Dictionary<string, object>();
                
                // Add FileInfo
                var fileInfo = new Dictionary<string, object>
                {
                    { "FilePath", containerPath },
                    { "StorageName", "" }
                };
                
                if (!string.IsNullOrEmpty(containerPassword))
                {
                    fileInfo.Add("Password", containerPassword);
                }
                
                requestObj.Add("FileInfo", fileInfo);
                
                // Add ContainerItemInfo
                var containerItemInfo = new Dictionary<string, object>
                {
                    { "RelativePath", documentPath }
                };
                
                if (!string.IsNullOrEmpty(documentPassword))
                {
                    containerItemInfo.Add("Password", documentPassword);
                }
                
                requestObj.Add("ContainerItemInfo", containerItemInfo);
                
                // Add page range parameters if specified
                if (startPage.HasValue)
                {
                    requestObj.Add("StartPageNumber", startPage.Value);
                }
                
                if (pageCount.HasValue)
                {
                    requestObj.Add("CountPagesToExtract", pageCount.Value);
                }
                
                var content = new StringContent(JsonConvert.SerializeObject(requestObj), Encoding.UTF8, "application/json");
                
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
                            string fileName = $"container_image_page_{page.PageIndex}_image_0.jpeg";
                            await DownloadImage(client, imageUrl, fileName);
                        }
                    }
                }
                else
                {
                    Console.WriteLine("No images found in the specified document inside the container.");
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

public class ExtractImagesFromContainerTutorial {
    
    private static final String BASE_URL = "https://api.groupdocs.cloud/v1.0/parser";
    private static final String AUTH_URL = "https://api.groupdocs.cloud/connect/token";
    
    public static void main(String[] args) throws IOException {
        // Get your ClientID and ClientSecret from https://dashboard.groupdocs.cloud
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Get JWT token
        String token = getAuthToken(clientId, clientSecret);
        
        // Extract images from a document inside a container
        extractImagesFromContainer(
            token, 
            "pdf/PDF with attachements.pdf", 
            "template-document.pdf",
            1,                // Start page (optional)
            2,                // Page count (optional)
            "container_pass", // Container password if needed
            "document_pass"   // Document password if needed
        );
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
    
    private static void extractImagesFromContainer(
        String token, 
        String containerPath, 
        String documentPath,
        Integer startPage,
        Integer pageCount,
        String containerPassword, 
        String documentPassword) throws IOException {
        
        URL url = new URL(BASE_URL + "/images");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setRequestProperty("Accept", "application/json");
        conn.setRequestProperty("Authorization", "Bearer " + token);
        conn.setDoOutput(true);
        
        // Create request JSON
        JSONObject requestJson = new JSONObject();
        
        // Add FileInfo
        JSONObject fileInfo = new JSONObject();
        fileInfo.put("FilePath", containerPath);
        fileInfo.put("StorageName", "");
        if (containerPassword != null && !containerPassword.isEmpty()) {
            fileInfo.put("Password", containerPassword);
        }
        requestJson.put("FileInfo", fileInfo);
        
        // Add ContainerItemInfo
        JSONObject containerItemInfo = new JSONObject();
        containerItemInfo.put("RelativePath", documentPath);
        if (documentPassword != null && !documentPassword.isEmpty()) {
            containerItemInfo.put("Password", documentPassword);
        }
        requestJson.put("ContainerItemInfo", containerItemInfo);
        
        // Add page range parameters if specified
        if (startPage != null) {
            requestJson.put("StartPageNumber", startPage);
        }
        
        if (pageCount != null) {
            requestJson.put("CountPagesToExtract", pageCount);
        }
        
        String requestBody = requestJson.toString();
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
                        String fileName = "container_image_page_" + pageIndex + "_image_0.jpeg";
                        downloadImage(token, downloadUrl, fileName);
                    }
                }
            } else {
                System.out.println("No images found in the specified document inside the container.");
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

## Understanding Container Types and Document Paths

Different container types require different approaches for specifying the relative path:

### ZIP Archives

For ZIP archives, the relative path is the path of the file inside the archive:

```json
"ContainerItemInfo": {
    "RelativePath": "folder/document.docx"
}
```

### Email Attachments

For emails, the relative path is the name of the attachment:

```json
"ContainerItemInfo": {
    "RelativePath": "attachment.pdf"
}
```

### PDF Portfolios

For PDF portfolios, the relative path is the name of the embedded document:

```json
"ContainerItemInfo": {
    "RelativePath": "embedded-document.pdf"
}
```

## Working with Password-Protected Documents

If either the container or the embedded document is password-protected, you need to provide the passwords:

```json
"FileInfo": {
    "FilePath": "archives/protected-archive.zip",
    "Password": "container-password"
},
"ContainerItemInfo": {
    "RelativePath": "protected-document.pdf",
    "Password": "document-password"
}
```

## Practical Use Cases

### Email Attachment Processing

One common application is to process images from email attachments without saving them to disk:

```csharp
// C# example: Process images from email attachments
async Task ProcessEmailAttachmentImages(string emailFilePath)
{
    // Get list of attachments in the email (using other API methods)
    var attachments = await GetEmailAttachments(emailFilePath);
    
    foreach (var attachment in attachments)
    {
        // Check if attachment is a document that might contain images
        if (IsDocumentWithPotentialImages(attachment.FileName))
        {
            // Extract images from this attachment
            await ExtractImagesFromContainer(
                token,
                emailFilePath,      // Email as container
                attachment.FileName // Attachment as document inside container
            );
        }
    }
}
```

### Batch Processing ZIP Archives

Another application is to extract images from all documents in a ZIP archive:

```java
// Java example: Process all documents in a ZIP archive
void processAllDocumentsInZip(String zipFilePath) throws IOException {
    // Get list of files in the ZIP (using other API methods)
    List<String> zipContents = getZipContents(zipFilePath);
    
    for (String filePath : zipContents) {
        // Check if file is a document that might contain images
        if (isDocumentWithPotentialImages(filePath)) {
            // Extract images from this document
            extractImagesFromContainer(
                token,
                zipFilePath, // ZIP as container
                filePath     // Document inside ZIP
            );
        }
    }
}
```

## Learning Checkpoint

Take a moment to test your understanding:

1. What is the purpose of the `ContainerItemInfo` parameter?
2. How do you handle password-protected containers and documents?
3. What types of containers are supported by the API?
4. How would you extract images from a specific page range of a document inside a container?

## Common Issues and Troubleshooting

- Invalid Relative Path: Ensure the relative path to the document inside the container is correct. The path is case-sensitive.
- Password Issues: If the container or document is password-protected, make sure you're providing the correct passwords.
- Unsupported Container Types: Not all container formats may be supported. Check the documentation for a complete list of supported formats.
- No Images Found: The document inside the container might not have any embedded images. Try with a different document.

## What You've Learned

In this tutorial, you've learned:
- How to extract images from documents stored inside containers
- How to handle password-protected containers and documents
- How to extract images from specific page ranges of documents inside containers
- How to download and process the extracted images

## Further Practice

Try creating an application that:
1. Monitors an email inbox for new messages with attachments
2. Automatically extracts all images from documents in attachments
3. Organizes the extracted images by email sender, date, and document name
4. Creates a searchable database of all extracted images with metadata

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
