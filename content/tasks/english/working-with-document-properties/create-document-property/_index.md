---
title: How to Create Document Properties in Project Files Tutorial
description: Learn how to create new document properties in MS Project files using Aspose.Tasks Cloud API in this comprehensive step-by-step tutorial.
url: /working-with-document-properties/create-document-property/
weight: 30
---

## Learning Objectives

In this tutorial, you'll learn how to create new document properties in MS Project files using Aspose.Tasks Cloud API. By the end of this guide, you'll be able to:

- Add standard and custom document properties to your project files
- Structure API requests for creating document properties
- Implement this functionality across different programming languages
- Verify successful property creation

## Prerequisites

Before starting this tutorial, ensure you have:

1. An Aspose Cloud account with an active subscription or [free trial](https://dashboard.aspose.cloud/#/apps)
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
3. Basic understanding of REST APIs and your preferred programming language
4. An MS Project file uploaded to your Aspose Cloud Storage

## Why Create Document Properties?

Document properties are valuable for organizing and managing project files. Creating properties allows you to:

- Add missing metadata to standardize your project files
- Include custom information specific to your business needs
- Implement document management workflows with metadata-based automation
- Ensure compliance with organizational documentation standards
- Improve searchability and discoverability of project files

## Tutorial Steps

### Step 1: Understand the API Endpoint

The Aspose.Tasks Cloud API provides a dedicated endpoint for creating document properties:

```
POST /tasks/{name}/documentproperties/{propertyName}
```

Where:
- `{name}` is the name of your MS Project file stored in the Aspose Cloud Storage
- `{propertyName}` is the name of the document property you want to create

### Step 2: Prepare the Request Body

To create a document property, you need to send a JSON payload with the following structure:

```json
{
  "name": "PropertyName",
  "value": "PropertyValue"
}
```

The `name` should match the `propertyName` in the URL path, and `value` contains the data you want to store in that property.

### Step 3: Execute the API Request

Let's see how to create document properties using different approaches:

#### Using cURL

Try it yourself with this cURL command:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/tasks/YourProjectFile.mpp/documentproperties/Title" \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d "{ \"link\": { \"href\": \"\", \"rel\": \"\", \"type\": \"\", \"title\": \"\" }, \"name\": \"Title\", \"value\": \"New Project Title\"}"
```

Note: Replace `YourProjectFile.mpp` with your actual project filename, `Title` with your desired property name, and `YOUR_ACCESS_TOKEN` with your authentication token.

#### Using Python

Here's a complete Python example:

```python
# Tutorial Code Example - Create Document Property in Python
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

# API endpoint for creating document property
file_name = "Home_move_plan.mpp"  # Your project file name
property_name = "Title"  # Property you want to create or update
api_endpoint = f"https://api.aspose.cloud/v3.0/tasks/{file_name}/documentproperties/{property_name}"

# Request headers
headers = {
    "Authorization": f"Bearer {access_token}",
    "Content-Type": "application/json",
    "Accept": "application/json"
}

# Request body
property_data = {
    "link": {
        "href": "",
        "rel": "",
        "type": "",
        "title": ""
    },
    "name": property_name,
    "value": "New Project Title"  # The value you want to set
}

# Execute request
response = requests.post(api_endpoint, headers=headers, json=property_data)

# Process and display results
if response.status_code == 200 or response.status_code == 201:
    property_info = response.json().get("property", {})
    print(f"Property created successfully:")
    print(f"Name: {property_info.get('name')}")
    print(f"Value: {property_info.get('value')}")
else:
    print(f"Error: {response.status_code}")
    print(response.text)
```

#### Using C#

```csharp
// Tutorial Code Example - Create Document Property in C#
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
            
            // Create document property
            await CreateDocumentProperty(token, "Home_move_plan.mpp", "Title", "New Project Title");
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
        
        static async Task CreateDocumentProperty(string accessToken, string fileName, string propertyName, string propertyValue)
        {
            using (var client = new HttpClient())
            {
                // Set the authorization header
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
                
                // Prepare the request body
                var propertyData = new
                {
                    link = new { href = "", rel = "", type = "", title = "" },
                    name = propertyName,
                    value = propertyValue
                };
                
                var content = new StringContent(
                    JsonSerializer.Serialize(propertyData),
                    Encoding.UTF8,
                    "application/json");
                
                // Make the request to create document property
                var response = await client.PostAsync(
                    $"https://api.aspose.cloud/v3.0/tasks/{fileName}/documentproperties/{propertyName}", 
                    content);
                
                response.EnsureSuccessStatusCode();
                
                // Parse and display the response
                var responseContent = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Document Property Created:");
                
                // Extract property information from the JSON response
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
// Tutorial Code Example - Create Document Property in Java
import java.io.IOException;
import java.net.URI;
import java.net.URLEncoder;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.StandardCharsets;
import org.json.JSONObject;

public class CreateDocumentPropertyTutorial {

    private static final String CLIENT_ID = "YOUR_CLIENT_ID";
    private static final String CLIENT_SECRET = "YOUR_CLIENT_SECRET";
    private static final HttpClient httpClient = HttpClient.newHttpClient();

    public static void main(String[] args) throws IOException, InterruptedException {
        // Get access token
        String accessToken = getAccessToken();
        
        // Create document property
        String fileName = "Home_move_plan.mpp";
        String propertyName = "Title";
        String propertyValue = "New Project Title";
        createDocumentProperty(accessToken, fileName, propertyName, propertyValue);
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

    private static void createDocumentProperty(String accessToken, String fileName, String propertyName, String propertyValue) 
            throws IOException, InterruptedException {
        // URL encode the file name and property name
        String encodedFileName = URLEncoder.encode(fileName, StandardCharsets.UTF_8);
        String encodedPropertyName = URLEncoder.encode(propertyName, StandardCharsets.UTF_8);
        
        // Create the property JSON payload
        JSONObject linkObject = new JSONObject();
        linkObject.put("href", "");
        linkObject.put("rel", "");
        linkObject.put("type", "");
        linkObject.put("title", "");
        
        JSONObject propertyObject = new JSONObject();
        propertyObject.put("link", linkObject);
        propertyObject.put("name", propertyName);
        propertyObject.put("value", propertyValue);
        
        // Prepare the request to create document property
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.aspose.cloud/v3.0/tasks/" + encodedFileName + 
                               "/documentproperties/" + encodedPropertyName))
                .header("Authorization", "Bearer " + accessToken)
                .header("Content-Type", "application/json")
                .POST(HttpRequest.BodyPublishers.ofString(propertyObject.toString()))
                .build();

        // Make the request
        HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
        
        // Parse and display the response
        JSONObject jsonResponse = new JSONObject(response.body());
        
        if (response.statusCode() == 200 || response.statusCode() == 201) {
            JSONObject property = jsonResponse.getJSONObject("property");
            System.out.println("Document Property Created:");
            System.out.println(property.getString("name") + ": " + property.getString("value"));
        } else {
            System.out.println("Error: " + response.statusCode());
            System.out.println(jsonResponse.toString(2));
        }
    }
}
```

### Step 4: Understand the Response

When successful, the API returns a JSON response confirming the creation of the document property. Here's an example response structure:

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
    "value": "New Project Title"
  }
}
```

The key information is in the `property` object, which contains:
- `name`: The name of the created property
- `value`: The value you set for the property
- `link`: Reference information for the property

### Step 5: Verify the Property Creation

To confirm that your document property was successfully created, you can:

1. Use the "Get Document Property by Name" API to retrieve the newly created property
2. Use the "Get Document Properties" API to list all properties, including your new one

This verification ensures that the property was properly stored in the project file.

## Try It Yourself

Now it's your turn to practice! Try creating these common document properties in your project file:

1. Set the "Title" to a meaningful project name
2. Add "Author" information with your name or organization
3. Create a "Company" property with your company name
4. Add "Comments" with notes about the project purpose

Challenge: Try creating a custom property with a name that doesn't exist in standard document properties.

## Working with Different Property Types

Document properties can hold different types of data:

### Text Properties

Most properties (like Title, Author, etc.) are text-based. You can set any string value as shown in the examples above.

### Date Properties

For date properties (like CreatedDate), provide the date in ISO format:

```json
{
  "name": "CreatedDate",
  "value": "2025-04-23T12:00:00Z"
}
```

### Numeric Properties

For numeric properties, provide the value as a string that contains only numbers:

```json
{
  "name": "RevisionNumber",
  "value": "42"
}
```

## Troubleshooting Tips

If you encounter issues while following this tutorial, check these common problems:

- 400 Bad Request: Verify your JSON payload is properly formatted
- 404 Not Found: Ensure your project file exists in the specified storage location
- Property Already Exists: If the property already exists, you should use PUT (update) instead of POST (create)
- Read-Only Properties: Some document properties may be read-only and cannot be created or modified

## What You've Learned

In this tutorial, you've learned how to:

1. Construct API requests to create new document properties
2. Structure the JSON payload for property creation
3. Handle different property data types
4. Verify successful property creation
5. Implement this functionality in various programming languages

## Further Practice

To reinforce your learning:

1. Create a batch of common document properties in a single program run
2. Create custom properties with different data types (text, numbers, dates)
3. Build a utility that standardizes document properties across multiple project files
4. Create properties with special characters or spaces in their names

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/) for assistance!
