---
title: How to Extract Text from the Whole Document Tutorial
url: /parse-operations/extract-text-whole-document/
weight: 1
description: Learn how to extract all text content from documents using GroupDocs.Parser Cloud API in this step-by-step tutorial for developers
---

# Tutorial: How to Extract Text from the Whole Document

## Learning Objectives

In this tutorial, you'll learn how to:
- Set up the GroupDocs.Parser Cloud API for text extraction
- Extract text from an entire document with a simple API call
- Process the extracted text in different programming languages

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Parser Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret (available from the [dashboard](https://dashboard.groupdocs.cloud/#/apps))
3. At least one document uploaded to your cloud storage

## The Practical Scenario

Imagine you need to extract all text from a document to:
- Create a searchable database of document content
- Perform analysis on document text
- Enable full-text search functionality in your application

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

To extract text from a document, you'll make a POST request to the text endpoint with the following parameters:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/text" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"FileInfo\": {
        \"FilePath\": \"words/docx/document.docx\"
    }
}"
```

Note that you only need to specify the `FilePath` parameter to extract text from the entire document.

## Step 3: Execute the Request and Process the Response

When you execute the request, the API will return a JSON response containing the extracted text:

```json
{
    "text": "First Page\r\r\f"
}
```

The text is returned in a simple format, with page breaks represented by the `\f` character.

## Try It Yourself

Now it's your turn to try extracting text from your own document:

1. Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials
2. Update the `FilePath` parameter to point to a document in your storage
3. Execute the curl command and observe the response

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
            
            // Extract text from the document
            await ExtractText(token, "words/docx/document.docx");
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
        
        static async Task ExtractText(string token, string filePath)
        {
            using (var client = new HttpClient())
            {
                // Prepare request
                client.DefaultRequestHeaders.Add("Authorization", $"Bearer {token}");
                
                var requestBody = new
                {
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
            }
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
import org.json.JSONObject;

public class ExtractTextTutorial {
    
    private static final String BASE_URL = "https://api.groupdocs.cloud/v1.0/parser";
    private static final String AUTH_URL = "https://api.groupdocs.cloud/connect/token";
    
    public static void main(String[] args) throws IOException {
        // Get your ClientID and ClientSecret from https://dashboard.groupdocs.cloud
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Get JWT token
        String token = getAuthToken(clientId, clientSecret);
        
        // Extract text from the document
        extractText(token, "words/docx/document.docx");
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
    
    private static void extractText(String token, String filePath) throws IOException {
        URL url = new URL(BASE_URL + "/text");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setRequestProperty("Accept", "application/json");
        conn.setRequestProperty("Authorization", "Bearer " + token);
        conn.setDoOutput(true);
        
        String requestBody = "{\"FileInfo\":{\"FilePath\":\"" + filePath + "\"}}";
        try (OutputStream os = conn.getOutputStream()) {
            os.write(requestBody.getBytes(StandardCharsets.UTF_8));
        }
        
        try (Scanner scanner = new Scanner(conn.getInputStream(), StandardCharsets.UTF_8.name())) {
            String jsonResponse = scanner.useDelimiter("\\A").next();
            System.out.println(jsonResponse);
        }
    }
}
```

## Common Issues and Troubleshooting

- Authentication Error: If you receive a 401 Unauthorized error, check that your Client ID and Client Secret are correct and that your token hasn't expired.
- File Not Found: Ensure the specified file path is correct and the file exists in your storage.
- Empty Text Response: Some document formats may not contain extractable text. Try with a different document format.

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with the GroupDocs.Parser Cloud API
- How to extract text from an entire document
- How to implement this functionality in C# and Java

## Next Steps

Now that you've mastered extracting text from an entire document, you can:
- Follow our tutorial on [Extracting Text by Page Number Range](/parse-operations/extract-text-page-number-range) to learn how to extract text from specific pages
- Explore how to [Extract Formatted Text](/parse-operations/extract-formatted-text) to preserve document formatting
## Further Practice

Try extracting text from various document formats (PDF, DOCX, XLSX, etc.) to understand how the API handles different document structures. Experiment with combining multiple API operations to build a more comprehensive document processing solution.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
