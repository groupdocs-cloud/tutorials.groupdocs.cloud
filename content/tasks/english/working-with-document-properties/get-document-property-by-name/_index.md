---
title: How to Get a Document Property by Name Tutorial
description: Learn how to retrieve specific document properties by name from MS Project files in this step-by-step tutorial using Aspose.Tasks Cloud API.
url: /working-with-document-properties/get-document-property-by-name/
weight: 20
---

## Learning Objectives

In this tutorial, you'll learn how to retrieve a specific document property by its name from an MS Project file using Aspose.Tasks Cloud API. By the end of this guide, you'll be able to:

- Construct API requests to target individual document properties
- Access common and custom properties by their exact names
- Handle the response data in different programming languages

## Prerequisites

Before starting this tutorial, ensure you have:

1. An Aspose Cloud account with an active subscription or [free trial](https://dashboard.aspose.cloud/#/apps)
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
3. Basic understanding of REST APIs and your preferred programming language
4. An MS Project file uploaded to your Aspose Cloud Storage

## The Use Case for Targeting Specific Properties

While retrieving all document properties is useful, often you only need specific metadata values. For example:

- Checking the project's title or author
- Retrieving custom properties that contain specific business information
- Confirming creation or last modified dates
- Extracting company or category information

This tutorial shows how to access these properties directly by name for more efficient processing.

## Tutorial Steps

### Step 1: Understand the API Endpoint

The Aspose.Tasks Cloud API provides a dedicated endpoint for retrieving a specific document property:

```
GET /tasks/{name}/documentproperties/{propertyName}
```

Where:
- `{name}` is the name of your MS Project file stored in the Aspose Cloud Storage
- `{propertyName}` is the exact name of the document property you want to retrieve

### Step 2: Identify Common Document Properties

MS Project files typically include these standard document properties:

- Title
- Subject
- Author
- Category
- Keywords
- Comments
- Manager
- Company
- CreatedDate
- LastSavedDate

You can also access custom properties if they exist in your project file.

### Step 3: Execute the API Request

Let's see how to retrieve a specific document property using different approaches:

#### Using cURL

Try it yourself with this cURL command:

```bash
curl -X GET "https://api.aspose.cloud/v3.0/tasks/YourProjectFile.mpp/documentproperties/Title" \
  -H "accept: application/json" \
  -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Note: Replace `YourProjectFile.mpp` with your actual project filename, `Title` with your desired property name, and `YOUR_ACCESS_TOKEN` with your authentication token.

#### Using Python

Here's a complete Python example:

```python
# Tutorial Code Example - Get Document Property by Name in Python
import requests
import json

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

# API endpoint for specific document property
file_name = "Home_move_plan.mpp"  # Your project file name
property_name = "Title"  # Property you want to retrieve
api_endpoint = f"https://api.aspose.cloud/v3.0/tasks/{file_name}/documentproperties/{property_name}"

# Request headers
headers = {
    "Authorization": f"Bearer {access_token}",
    "Accept": "application/json"
}

# Execute request
response = requests.get(api_endpoint, headers=headers)

# Process and display results
if response.status_code == 200:
    property_info = response.json().get("property", {})
    print(f"Property: {property_info.get('name')}")
    print(f"Value: {property_info.get('value')}")
else:
    print(f"Error: {response.status_code}")
    print(response.text)
```

#### Using C#

```csharp
// Tutorial Code Example - Get Document Property by Name in C#
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
            
            // Get specific document property
            await GetDocumentProperty(token, "Home_move_plan.mpp", "Title");
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
        
        static async Task GetDocumentProperty(string accessToken, string fileName, string propertyName)
        {
            using (var client = new HttpClient())
            {
                // Set the authorization header
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
                
                // Make the request to get specific document property
                var response = await client.GetAsync($"https://api.aspose.cloud/v3.0/tasks/{fileName}/documentproperties/{propertyName}");
                response.EnsureSuccessStatusCode();
                
                // Parse and display the response
                var responseContent = await response.Content.ReadAsStringAsync();
                Console.WriteLine($"Document Property: {propertyName}");
                
                // Extract property value from the JSON response
                using (JsonDocument document = JsonDocument.Parse(responseContent))
                {
                    var property = document.RootElement.GetProperty("property");
                    string name = property.GetProperty("name").GetString();
                    string value = property.GetProperty("value").GetString();
                    Console.WriteLine($"{name}: {value}");
                }
            }
        }
    }
}
```

#### Using Java

```java
// Tutorial Code Example - Get Document Property by Name in Java
import java.io.IOException;
import java.net.URI;
import java.net.URLEncoder;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.StandardCharsets;
import org.json.JSONObject;

public class GetDocumentPropertyByNameTutorial {

    private static final String CLIENT_ID = "YOUR_CLIENT_ID";
    private static final String CLIENT_SECRET = "YOUR_CLIENT_SECRET";
    private static final HttpClient httpClient = HttpClient.newHttpClient();

    public static void main(String[] args) throws IOException, InterruptedException {
        // Get access token
        String accessToken = getAccessToken();
        
        // Get specific document property
        String fileName = "Home_move_plan.mpp";
        String propertyName = "Title";
        getDocumentProperty(accessToken, fileName, propertyName);
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

    private static void getDocumentProperty(String accessToken, String fileName, String propertyName) 
            throws IOException, InterruptedException {
        // URL encode the file name and property name
        String encodedFileName = URLEncoder.encode(fileName, StandardCharsets.UTF_8);
        String encodedPropertyName = URLEncoder.encode(propertyName, StandardCharsets.UTF_8);
        
        // Prepare the request to get specific document property
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.aspose.cloud/v3.0/tasks/" + encodedFileName + 
                               "/documentproperties/" + encodedPropertyName))
                .header("Authorization", "Bearer " + accessToken)
                .GET()
                .build();

        // Make the request
        HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
        
        // Parse and display the response
        JSONObject jsonResponse = new JSONObject(response.body());
        JSONObject property = jsonResponse.getJSONObject("property");
        
        System.out.println("Document Property:");
        System.out.println(property.getString("name") + ": " + property.getString("value"));
    }
}
```

### Step 4: Understand the Response

When successful, the API returns a JSON response containing the requested document property. Here's an example response structure:

```json
{
  "code": 0,
  "status": "OK",
  "property": {
    "link": {
      "href": "https://api.aspose.cloud/v3.0/tasks/Home_move_plan.mpp/documentproperties/Title",
      "rel": "self",
      "type": "text/json",
      "title": "Title"
    },
    "name": "Title",
    "value": "Home move plan"
  }
}
```

The key information is in the `property` object, which contains:
- `name`: The name of the requested property
- `value`: The actual value of the property
- `link`: Reference information for the property

### Step 5: Handle Special Cases

When working with document properties by name, consider these special cases:

#### Case-Sensitivity

Property names are case-sensitive. For example, "Title" and "title" are treated as different properties. Always use the exact property name.

#### URL Encoding

When using property names that contain spaces or special characters, ensure they are properly URL-encoded in your requests.

#### Non-Existent Properties

If you request a property that doesn't exist in the document, the API will return an appropriate error message. Your code should check for and handle these cases gracefully.

## Try It Yourself

Now it's your turn to practice! Try retrieving different document properties from your project file:

1. Common properties like "Author", "Company", or "CreatedDate"
2. Custom properties specific to your project files
3. Properties with spaces or special characters in their names (remember to URL-encode them)

## Troubleshooting Tips

If you encounter issues while following this tutorial, check these common problems:

- 404 Not Found: The property name might not exist in the document or might be misspelled
- URL Encoding Issues: Ensure property names with spaces or special characters are properly encoded
- Authentication Errors: Verify your access token is valid and not expired

## What You've Learned

In this tutorial, you've learned how to:

1. Construct API requests to target specific document properties by name
2. Handle case-sensitivity and special characters in property names
3. Process the returned property value in different programming languages
4. Handle special cases and potential errors

## Further Practice

To reinforce your learning:

1. Create a function that can retrieve multiple specified properties in a single application run
2. Build a simple utility that checks for the existence of certain required properties
3. Try accessing custom properties that might be used in your organization's project files

## Next Steps

Now that you can retrieve specific document properties by name, the next tutorial will show you how to [create new document properties](/working-with-document-properties/create-document-property/) in your MS Project files.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/) for assistance!
