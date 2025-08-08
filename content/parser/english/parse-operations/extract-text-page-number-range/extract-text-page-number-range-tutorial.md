---
title: How to Extract Text by a Page Number Range Tutorial
url: /parse-operations/extract-text-page-number-range/
weight: 2
description: Learn how to extract text from specific pages in documents using GroupDocs.Parser Cloud API in this step-by-step tutorial for developers
---

# Tutorial: How to Extract Text by a Page Number Range

## Learning Objectives

In this tutorial, you'll learn how to:
- Extract text from specific pages in a document
- Define page ranges for targeted text extraction
- Process page-specific text in different programming languages

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Parser Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret (available from the [dashboard](https://dashboard.groupdocs.cloud/#/apps))
3. A multi-page document uploaded to your cloud storage

## The Practical Scenario

Imagine you're building an application that needs to:
- Extract text from specific sections of a long document
- Process only relevant pages instead of the entire document
- Allow users to navigate through document content page by page

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

To extract text from specific pages, you'll make a POST request to the text endpoint with the following parameters:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/text" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"StartPageNumber\": 0,
    \"CountPagesToExtract\": 4,
    \"FileInfo\": {
        \"FilePath\": \"pdf/pages-document.pdf\"
    }
}"
```

The key parameters here are:
- `StartPageNumber`: The zero-based index of the first page to extract (0 = first page)
- `CountPagesToExtract`: The number of pages to extract starting from the start page

## Step 3: Execute the Request and Process the Response

When you execute the request, the API will return a JSON response containing the extracted text for each page:

```json
{
    "pages": [
        {
            "pageIndex": 0,
            "text": "Text inside bookmark 0\r\n\r\n           Page 0 heading\r\nP a g e  T e x t -  P a g e  0\r\n"
        },
        {
            "pageIndex": 1,
            "text": "Text inside bookmark 1\r\n\r\n           Page 1 heading\r\nP a g e  T e x t -  P a g e  1\r\n"
        },
        {
            "pageIndex": 2,
            "text": "Text inside bookmark 2\r\n\r\n           Page 2 heading\r\nP a g e  T e x t -  P a g e  2\r\n"
        },
        {
            "pageIndex": 3,
            "text": "Text inside bookmark 3\r\n\r\n           Page 3 heading\r\nP a g e  T e x t -  P a g e  3\r\n"
        }
    ]
}
```

Notice that the response includes the `pageIndex` for each extracted page, making it easy to identify which text belongs to which page.

## Try It Yourself

Now it's your turn to try extracting text from specific pages:

1. Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials
2. Update the `FilePath` parameter to point to a multi-page document in your storage
3. Adjust the `StartPageNumber` and `CountPagesToExtract` parameters to extract different page ranges
4. Execute the curl command and observe how the response changes

## Implementation in Different Languages

### C# Example

```csharp
using System;
using System.Collections.Generic;
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
            
            // Extract text from specific pages
            await ExtractTextByPageRange(token, "pdf/pages-document.pdf", 0, 4);
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
        
        static async Task ExtractTextByPageRange(string token, string filePath, int startPage, int pageCount)
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
                        FilePath = filePath
                    }
                };
                
                var content = new StringContent(JsonConvert.SerializeObject(requestBody), Encoding.UTF8, "application/json");
                
                // Send request
                var response = await client.PostAsync("https://api.groupdocs.cloud/v1.0/parser/text", content);
                
                // Process response
                var jsonString = await response.Content.ReadAsStringAsync();
                Console.WriteLine(jsonString);
                
                // You can process each page individually
                var result = JsonConvert.DeserializeObject<PageTextResponse>(jsonString);
                foreach (var page in result.Pages)
                {
                    Console.WriteLine($"Page {page.PageIndex}:");
                    Console.WriteLine(page.Text);
                    Console.WriteLine("--------------------");
                }
            }
        }
        
        class PageTextResponse
        {
            public List<PageData> Pages { get; set; }
        }
        
        class PageData
        {
            public int PageIndex { get; set; }
            public string Text { get; set; }
        }
    }
}
```

### Java Example

```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.util.Scanner;
import org.json.JSONArray;
import org.json.JSONObject;

public class ExtractTextByPageRangeTutorial {
    
    private static final String BASE_URL = "https://api.groupdocs.cloud/v1.0/parser";
    private static final String AUTH_URL = "https://api.groupdocs.cloud/connect/token";
    
    public static void main(String[] args) throws IOException {
        // Get your ClientID and ClientSecret from https://dashboard.groupdocs.cloud
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Get JWT token
        String token = getAuthToken(clientId, clientSecret);
        
        // Extract text from specific pages
        extractTextByPageRange(token, "pdf/pages-document.pdf", 0, 4);
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
    
    private static void extractTextByPageRange(String token, String filePath, int startPage, int pageCount) throws IOException {
        URL url = new URL(BASE_URL + "/text");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setRequestProperty("Accept", "application/json");
        conn.setRequestProperty("Authorization", "Bearer " + token);
        conn.setDoOutput(true);
        
        String requestBody = String.format(
            "{\"StartPageNumber\":%d,\"CountPagesToExtract\":%d,\"FileInfo\":{\"FilePath\":\"%s\"}}",
            startPage, pageCount, filePath
        );
        
        try (OutputStream os = conn.getOutputStream()) {
            os.write(requestBody.getBytes(StandardCharsets.UTF_8));
        }
        
        try (Scanner scanner = new Scanner(conn.getInputStream(), StandardCharsets.UTF_8.name())) {
            String jsonResponse = scanner.useDelimiter("\\A").next();
            System.out.println(jsonResponse);
            
            // Process each page individually
            JSONObject responseObj = new JSONObject(jsonResponse);
            JSONArray pages = responseObj.getJSONArray("pages");
            
            for (int i = 0; i < pages.length(); i++) {
                JSONObject page = pages.getJSONObject(i);
                int pageIndex = page.getInt("pageIndex");
                String text = page.getString("text");
                
                System.out.println("Page " + pageIndex + ":");
                System.out.println(text);
                System.out.println("--------------------");
            }
        }
    }
}
```

## Learning Checkpoint

Take a moment to test your understanding:

1. What is the purpose of the `StartPageNumber` parameter?
2. If you want to extract pages 5-10 of a document, what values should you use for `StartPageNumber` and `CountPagesToExtract`?
3. How would you modify the request to extract only the last page of a document if you don't know how many pages it has?

## Common Issues and Troubleshooting

- Page Range Out of Bounds: If you specify a page range that's outside the document's boundaries, you'll receive an error. Make sure your `StartPageNumber` and `CountPagesToExtract` parameters are valid for your document.
- Zero Pages Returned: Ensure that the document actually has content on the specified pages. Some documents might have blank pages that return empty text.
- Different Page Counts: Remember that page numbering starts from 0 in the API, but may start from 1 in the document viewer.

## What You've Learned

In this tutorial, you've learned:
- How to extract text from specific pages in a document
- How to specify page ranges using `StartPageNumber` and `CountPagesToExtract`
- How to process the page-specific text in your application

## Next Steps

Now that you know how to extract text from specific pages, you can:
- Explore [Extracting Formatted Text](/parse-operations/extract-formatted-text) to preserve document formatting

## Further Practice

Try creating an application that:
1. First determines the total number of pages in a document
2. Extracts text page by page using a loop
3. Performs analysis on each page (such as word count, keyword detection, etc.)
4. Compiles a summary report of the document's content structure

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
