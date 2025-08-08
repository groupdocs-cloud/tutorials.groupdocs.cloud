---
title: How to Extract Formatted Text Tutorial
url: /parse-operations/extract-formatted-text/
weight: 3
description: Learn how to extract formatted text from documents with preserved HTML, Markdown, or plain text formatting using GroupDocs.Parser Cloud API
---

# Tutorial: How to Extract Formatted Text

## Learning Objectives

In this tutorial, you'll learn how to:
- Extract text from documents while preserving formatting
- Use different text formatting modes (HTML, Markdown, Plain Text)
- Process formatted text in different programming languages

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Parser Cloud account (if you don't have one, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret (available from the [dashboard](https://dashboard.groupdocs.cloud/#/apps))
3. A formatted document (e.g., a DOCX file with various formatting) uploaded to your cloud storage

## The Practical Scenario

Imagine you're developing an application that needs to:
- Convert documents to web content while preserving formatting
- Export document text as HTML for rendering in a browser
- Preserve document structure including headings, lists, tables, and text styling

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

To extract formatted text, you'll make a POST request to the text endpoint with the following parameters:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/text" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"FormattedTextOptions\": {
        \"Mode\": \"Html\"
    },
    \"FileInfo\": {
        \"FilePath\": \"words/docx/formatted-document.docx\"
    }
}"
```

The key parameter here is `FormattedTextOptions.Mode`, which can be set to one of the following values:
- `Html`: Extracts text with HTML formatting
- `Markdown`: Extracts text with Markdown formatting
- `PlainText`: Extracts text without formatting (default)

## Step 3: Execute the Request and Process the Response

When you execute the request, the API will return a JSON response containing the formatted text:

```json
{
    "text": "
<p>
<b>Bold text
</b>
</p>
<p>
<i>Italic text
</i>
</p>
<ol>
<li>
<i>First element
</i>
</li>
<li>
<i>Second element
</i>
</li>
<li>
<i>Third element
</i>
</li>
</ol>
<h1>Heading 1
</h1>
<p>
<a href=\"http://targetwebsite.domain\">Hyperlink 
</a>targetwebsite.domain
</p>
<table border=\"1\">
<tr>
<td>
<p>table
</p>
</td>
<td>
<p>Cell 1
</p>
</td>
<td>
<p>Cell 2
</p>
</td>
</tr>
<tr>
<td>
<p>Cell 3
</p>
</td>
<td>
<p>Cell 4
</p>
</td>
<td>
<p>Cell 5
</p>
</td>
</tr>
</table>
<p>\f
</p>
<p>
<b>Second page bold text
</b>
</p>
<h1>Second page heading
</h1>"
}
```

Notice that the HTML response preserves:
- Paragraph structure with `<p>` tags
- Text styling with `<b>` and `<i>` tags
- Lists with `<ol>` and `<li>` tags
- Headings with `<h1>` tags
- Links with `<a>` tags
- Tables with `<table>`, `<tr>`, and `<td>` tags
- Page breaks with `\f` character

## Try It Yourself

Now it's your turn to try extracting formatted text:

1. Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials
2. Update the `FilePath` parameter to point to a formatted document in your storage
3. Try different values for `Mode` (Html, Markdown, PlainText) and observe how the response changes
4. Execute the curl command and analyze the formatted output

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
            
            // Extract formatted text (HTML mode)
            await ExtractFormattedText(token, "words/docx/formatted-document.docx", "Html");
            
            // You can also try other modes
            // await ExtractFormattedText(token, "words/docx/formatted-document.docx", "Markdown");
            // await ExtractFormattedText(token, "words/docx/formatted-document.docx", "PlainText");
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
        
        static async Task ExtractFormattedText(string token, string filePath, string mode)
        {
            using (var client = new HttpClient())
            {
                // Prepare request
                client.DefaultRequestHeaders.Add("Authorization", $"Bearer {token}");
                
                var requestBody = new
                {
                    FormattedTextOptions = new
                    {
                        Mode = mode
                    },
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
                var result = JsonConvert.DeserializeObject<FormattedTextResponse>(jsonString);
                
                Console.WriteLine($"Extracted text in {mode} format:");
                Console.WriteLine(result.Text);
                
                // If extracting HTML, you can save it to an HTML file
                if (mode == "Html")
                {
                    System.IO.File.WriteAllText("extracted.html", result.Text);
                    Console.WriteLine("HTML content saved to extracted.html");
                }
            }
        }
        
        class FormattedTextResponse
        {
            public string Text { get; set; }
        }
    }
}
```

### Java Example

```java
import java.io.FileWriter;
import java.io.IOException;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;
import java.util.Scanner;
import org.json.JSONObject;

public class ExtractFormattedTextTutorial {
    
    private static final String BASE_URL = "https://api.groupdocs.cloud/v1.0/parser";
    private static final String AUTH_URL = "https://api.groupdocs.cloud/connect/token";
    
    public static void main(String[] args) throws IOException {
        // Get your ClientID and ClientSecret from https://dashboard.groupdocs.cloud
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Get JWT token
        String token = getAuthToken(clientId, clientSecret);
        
        // Extract formatted text (HTML mode)
        extractFormattedText(token, "words/docx/formatted-document.docx", "Html");
        
        // You can also try other modes
        // extractFormattedText(token, "words/docx/formatted-document.docx", "Markdown");
        // extractFormattedText(token, "words/docx/formatted-document.docx", "PlainText");
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
    
    private static void extractFormattedText(String token, String filePath, String mode) throws IOException {
        URL url = new URL(BASE_URL + "/text");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Content-Type", "application/json");
        conn.setRequestProperty("Accept", "application/json");
        conn.setRequestProperty("Authorization", "Bearer " + token);
        conn.setDoOutput(true);
        
        String requestBody = String.format(
            "{\"FormattedTextOptions\":{\"Mode\":\"%s\"},\"FileInfo\":{\"FilePath\":\"%s\"}}",
            mode, filePath
        );
        
        try (OutputStream os = conn.getOutputStream()) {
            os.write(requestBody.getBytes(StandardCharsets.UTF_8));
        }
        
        try (Scanner scanner = new Scanner(conn.getInputStream(), StandardCharsets.UTF_8.name())) {
            String jsonResponse = scanner.useDelimiter("\\A").next();
            JSONObject responseObj = new JSONObject(jsonResponse);
            String extractedText = responseObj.getString("text");
            
            System.out.println("Extracted text in " + mode + " format:");
            System.out.println(extractedText);
            
            // If extracting HTML, you can save it to an HTML file
            if (mode.equals("Html")) {
                try (FileWriter writer = new FileWriter("extracted.html")) {
                    writer.write(extractedText);
                }
                System.out.println("HTML content saved to extracted.html");
            }
        }
    }
}
```

## Learning Checkpoint

Take a moment to test your understanding:

1. What are the three available modes for formatted text extraction?
2. How does the HTML mode preserve document structure compared to plain text?
3. What types of formatting elements are preserved in the HTML output?

## Using the Extracted Formatted Text

Here are some practical uses for the formatted text:

### HTML Mode
The HTML-formatted output can be directly embedded in a webpage or application. You might need to add some CSS styling to make it look better:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Extracted Document</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
        }
        table {
            border-collapse: collapse;
            width: 100%;
        }
        td, th {
            border: 1px solid #ddd;
            padding: 8px;
        }
        h1 {
            color: #2c3e50;
        }
        a {
            color: #3498db;
        }
    </style>
</head>
<body>
    <!-- Insert the extracted HTML content here -->
    [EXTRACTED_HTML_CONTENT]
</body>
</html>
```

### Markdown Mode
The Markdown-formatted output can be used in Markdown editors, GitHub README files, or converted to other formats using Markdown processors.

## Common Issues and Troubleshooting

- Missing Formatting: Some complex formatting may not be preserved exactly as in the original document. The API focuses on preserving the most common formatting elements.
- Special Characters: Special characters in HTML might need to be escaped when you display the content. Most modern frameworks handle this automatically.
- Rendering Differences: Different browsers or Markdown renderers might display the formatted content slightly differently.

## What You've Learned

In this tutorial, you've learned:
- How to extract text with preserved formatting from documents
- How to specify different formatting modes (HTML, Markdown, Plain Text)
- How to process and use the formatted text in your applications

## Next Steps

Now that you know how to extract formatted text, you can:
- Combine this with [Page Range Extraction](/parse-operations/extract-images-page-number-range/) to extract formatted text from specific pages
- Learn about [Extracting Images](/parse-operations/extract-images-whole-document/) to complement your formatted text with images

## Further Practice

Try creating an application that:
1. Extracts formatted text from a document
2. Processes the HTML to enhance it (e.g., adding custom CSS classes)
3. Renders the formatted content in a web application
4. Allows the user to toggle between HTML and Markdown views

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
