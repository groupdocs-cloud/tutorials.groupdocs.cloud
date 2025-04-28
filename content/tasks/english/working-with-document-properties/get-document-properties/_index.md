---
title: How to Get Document Properties of a Project Tutorial
description: Learn how to retrieve document properties from MS Project files using Aspose.Tasks Cloud API in this step-by-step tutorial for developers.
url: /working-with-document-properties/get-document-properties/
weight: 10
---

## Learning Objectives

In this tutorial, you'll learn how to retrieve all document properties from an MS Project file using Aspose.Tasks Cloud API. By the end of this guide, you'll be able to:

- Make API requests to fetch all document properties
- Parse and utilize document property data in your applications
- Implement this functionality in various programming languages

## Prerequisites

Before starting this tutorial, ensure you have:

1. An Aspose Cloud account with an active subscription or [free trial](https://dashboard.aspose.cloud/#/apps)
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
3. A basic understanding of REST APIs and your preferred programming language
4. An MS Project file ready for testing (you can use sample files from the Aspose repository)

## Understanding Document Properties

Document properties in MS Project files contain metadata about the project, such as title, author, company, creation date, and more. Accessing these properties programmatically can be valuable for:

- Cataloging and organizing project files
- Extracting metadata for project management systems
- Generating reports with document information
- Validating file metadata for compliance purposes

## Tutorial Steps

### Step 1: Understand the API Endpoint

The Aspose.Tasks Cloud API provides a dedicated endpoint for retrieving document properties:

```
GET /tasks/{name}/documentproperties
```

Where `{name}` is the name of your MS Project file stored in the Aspose Cloud Storage.

### Step 2: Prepare Your API Request

To make this request, you'll need:

1. Your project file uploaded to Aspose Cloud Storage
2. Authentication using your Client ID and Client Secret
3. The appropriate API endpoint URL

### Step 3: Execute the API Request

Let's see how to retrieve document properties using different approaches:

#### Using cURL

Try it yourself with this cURL command:

```bash
curl -X GET "https://api.aspose.cloud/v3.0/tasks/YourProjectFile.mpp/documentproperties" \
  -H "accept: application/json" \
  -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Note: Replace `YourProjectFile.mpp` with your actual project filename and `YOUR_ACCESS_TOKEN` with your authentication token.

#### Using Python

Here's a complete Python example:

```python
# Tutorial Code Example - Get Document Properties in Python
import requests
import json
import base64

# Authentication credentials
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Get access token
auth_url = "https://api.aspose.cloud/connect/token"
auth_data = {
    "grant_type": "client_credentials",
    "client_id": client_id,
    "client_secret": client_secret
}
auth_headers = {
    "Content-Type": "application/x-www-form-urlencoded",
    "Accept": "application/json"
}

auth_response = requests.post(auth_url, data=auth_data, headers=auth_headers)
access_token = auth_response.json().get("access_token")

# API endpoint for document properties
file_name = "Home_move_plan.mpp"  # Your project file name
api_endpoint = f"https://api.aspose.cloud/v3.0/tasks/{file_name}/documentproperties"

# Request headers
headers = {
    "Authorization": f"Bearer {access_token}",
    "Accept": "application/json"
}

# Execute request
response = requests.get(api_endpoint, headers=headers)

# Process and display results
if response.status_code == 200:
    properties = response.json().get("properties", {}).get("list", [])
    print(f"Found {len(properties)} document properties:")
    for prop in properties:
        print(f"{prop.get('name')}: {prop.get('value')}")
else:
    print(f"Error: {response.status_code}")
    print(response.text)
```

#### Using C#

```csharp
// Tutorial Code Example - Get Document Properties in C#
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Text.Json;
using System.Threading.Tasks;

namespace AsposeTasksCloudTutorial
{
    class Program
    {
        // Your Aspose Cloud credentials
        private const string ClientId = "YOUR_CLIENT_ID";
        private const string ClientSecret = "YOUR_CLIENT_SECRET";
        
        static async Task Main(string[] args)
        {
            // Get the access token
            var token = await GetAccessToken();
            
            // Get document properties
            await GetDocumentProperties(token);
        }
        
        static async Task<string> GetAccessToken()
        {
            using (var client = new HttpClient())
            {
                // Prepare the request to get the access token
                var requestContent = new FormUrlEncodedContent(new[]
                {
                    new KeyValuePair<string, string>("grant_type", "client_credentials"),
                    new KeyValuePair<string, string>("client_id", ClientId),
                    new KeyValuePair<string, string>("client_secret", ClientSecret)
                });
                
                // Make the request
                var response = await client.PostAsync("https://api.aspose.cloud/connect/token", requestContent);
                response.EnsureSuccessStatusCode();
                
                // Parse the response
                var responseContent = await response.Content.ReadAsStringAsync();
                using (JsonDocument document = JsonDocument.Parse(responseContent))
                {
                    return document.RootElement.GetProperty("access_token").GetString();
                }
            }
        }
        
        static async Task GetDocumentProperties(string accessToken)
        {
            using (var client = new HttpClient())
            {
                // Set the authorization header
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
                
                // Make the request to get document properties
                string fileName = "Home_move_plan.mpp"; // Your project file name
                var response = await client.GetAsync($"https://api.aspose.cloud/v3.0/tasks/{fileName}/documentproperties");
                response.EnsureSuccessStatusCode();
                
                // Parse and display the response
                var responseContent = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Document Properties:");
                Console.WriteLine(responseContent);
                
                // You can further parse the JSON to extract individual properties
                using (JsonDocument document = JsonDocument.Parse(responseContent))
                {
                    var properties = document.RootElement.GetProperty("properties").GetProperty("list");
                    foreach (var property in properties.EnumerateArray())
                    {
                        string name = property.GetProperty("name").GetString();
                        string value = property.GetProperty("value").GetString();
                        Console.WriteLine($"{name}: {value}");
                    }
                }
            }
        }
    }
}
```

#### Using Java

```java
// Tutorial Code Example - Get Document Properties in Java
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.StandardCharsets;
import java.util.Base64;
import org.json.JSONArray;
import org.json.JSONObject;

public class GetDocumentPropertiesTutorial {

    private static final String CLIENT_ID = "YOUR_CLIENT_ID";
    private static final String CLIENT_SECRET = "YOUR_CLIENT_SECRET";
    private static final HttpClient httpClient = HttpClient.newHttpClient();

    public static void main(String[] args) throws IOException, InterruptedException {
        // Get access token
        String accessToken = getAccessToken();
        
        // Get document properties
        getDocumentProperties(accessToken);
    }

    private static String getAccessToken() throws IOException, InterruptedException {
        // Prepare the request to get the access token
        String authData = "grant_type=client_credentials&client_id=" + CLIENT_ID + "&client_secret=" + CLIENT_SECRET;
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.aspose.cloud/connect/token"))
                .header("Content-Type", "application/x-www-form-urlencoded")
                .POST(HttpRequest.BodyPublishers.ofString(authData))
                .build();

        // Make the request
        HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
        
        // Parse the response
        JSONObject jsonResponse = new JSONObject(response.body());
        return jsonResponse.getString("access_token");
    }

    private static void getDocumentProperties(String accessToken) throws IOException, InterruptedException {
        // Prepare the request to get document properties
        String fileName = "Home_move_plan.mpp"; // Your project file name
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.aspose.cloud/v3.0/tasks/" + fileName + "/documentproperties"))
                .header("Authorization", "Bearer " + accessToken)
                .GET()
                .build();

        // Make the request
        HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
        
        // Parse and display the response
        JSONObject jsonResponse = new JSONObject(response.body());
        JSONArray properties = jsonResponse.getJSONObject("properties").getJSONArray("list");
        
        System.out.println("Document Properties:");
        for (int i = 0; i < properties.length(); i++) {
            JSONObject property = properties.getJSONObject(i);
            String name = property.getString("name");
            String value = property.getString("value");
            System.out.println(name + ": " + value);
        }
    }
}
```

### Step 4: Parse the Response

When successful, the API returns a JSON response containing all document properties. Here's an example response structure:

```json
{
  "code": 0,
  "status": "OK",
  "properties": {
    "link": {
      "href": "https://api.aspose.cloud/v3.0/tasks/Home_move_plan.mpp/documentproperties",
      "rel": "self",
      "type": "text/json",
      "title": "Document Properties"
    },
    "list": [
      {
        "link": {
          "href": "https://api.aspose.cloud/v3.0/tasks/Home_move_plan.mpp/documentproperties/Title",
          "rel": "self",
          "type": "text/json",
          "title": "Title"
        },
        "name": "Title",
        "value": "Home move plan"
      },
      {
        "link": {
          "href": "https://api.aspose.cloud/v3.0/tasks/Home_move_plan.mpp/documentproperties/Author",
          "rel": "self",
          "type": "text/json",
          "title": "Author"
        },
        "name": "Author",
        "value": "John Doe"
      }
      // Additional properties...
    ]
  }
}
```

### Step 5: Utilize the Document Properties

Once you've retrieved the document properties, you can:

- Display them in your application's user interface
- Store them in a database for cataloging purposes
- Use specific properties for filtering or organization
- Process metadata for reporting or analytics

## Troubleshooting Tips

If you encounter issues while following this tutorial, check these common problems:

- 401 Unauthorized: Verify your Client ID and Client Secret are correct and that your subscription is active
- 404 Not Found: Ensure your project file exists in the specified storage location
- Parameter Errors: Check that the filename is correctly URL-encoded in your requests
- Empty Properties List: Some project files might have limited document properties set

## What You've Learned

In this tutorial, you've learned how to:

1. Authenticate with the Aspose.Tasks Cloud API
2. Make requests to retrieve document properties from MS Project files
3. Parse and process the returned JSON data
4. Implement this functionality in various programming languages

## Further Practice

To reinforce your learning:

1. Try retrieving properties from different project files
2. Create a simple application that displays document properties in a user-friendly format
3. Extend the code to filter properties by specific criteria (e.g., only custom properties)
4. Compare document properties across multiple project files

## Next Steps

Now that you can retrieve all document properties, the next tutorial will show you how to [get a specific document property by name](/working-with-document-properties/get-document-property-by-name/), allowing for more targeted metadata access.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/) for assistance!
