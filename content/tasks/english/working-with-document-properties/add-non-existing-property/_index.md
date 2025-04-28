---
title: How to Add Non-Existing Document Properties Tutorial
description: Learn how to add custom, non-existing document properties to MS Project files using Aspose.Tasks Cloud API in this comprehensive tutorial.
url: /working-with-document-properties/add-non-existing-property/
weight: 50
---

## Learning Objectives

In this tutorial, you'll learn how to add custom, non-existing document properties to MS Project files using Aspose.Tasks Cloud API. By the end of this guide, you'll be able to:

- Create custom document properties with unique names
- Understand the differences between standard and custom properties
- Implement custom property creation in various programming languages
- Verify and utilize your custom properties

## Prerequisites

Before starting this tutorial, ensure you have:

1. An Aspose Cloud account with an active subscription or [free trial](https://dashboard.aspose.cloud/#/apps)
2. Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/#/apps)
3. Basic understanding of REST APIs and your preferred programming language
4. An MS Project file uploaded to your Aspose Cloud Storage
5. Completed the previous tutorials on retrieving and creating document properties

## The Power of Custom Document Properties

Custom document properties extend the metadata capabilities of MS Project files beyond the standard set of properties. They provide several advantages:

- Business-Specific Metadata: Add organization-specific information to project files
- Workflow Integration: Store process status, approval flags, or routing information
- Custom Categorization: Implement tagging systems specific to your project management needs
- Extended Search Capabilities: Add searchable metadata that isn't covered by standard properties
- Application Integration: Store identifiers or references to other systems

## Tutorial Steps

### Step 1: Understand the API Endpoint

The Aspose.Tasks Cloud API uses the same endpoint for creating custom properties as for standard ones:

```
POST /tasks/{name}/documentproperties/{propertyName}
```

Where:
- `{name}` is the name of your MS Project file stored in the Aspose Cloud Storage
- `{propertyName}` is the name of the custom document property you want to create

The key difference is that `propertyName` will be a name that doesn't exist in the standard set of document properties.

### Step 2: Plan Your Custom Properties

Before implementation, decide what custom properties would benefit your project management workflow. Common examples include:

- ProjectID: A unique identifier for the project in your system
- Department: The department responsible for the project
- Budget: Financial allocation information
- Priority: Custom priority rating
- ApprovalStatus: Current status in approval workflow
- LastReviewDate: Date of last review
- ClientName: Associated client

### Step 3: Execute the API Request

Let's see how to add custom document properties using different approaches:

#### Using cURL

Try it yourself with this cURL command:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/tasks/YourProjectFile.mpp/documentproperties/ProjectID" \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d "{ \"link\": { \"href\": \"\", \"rel\": \"\", \"type\": \"\", \"title\": \"\" }, \"name\": \"ProjectID\", \"value\": \"PRJ-2025-042\"}"
```

Note: Replace `YourProjectFile.mpp` with your actual project filename, `ProjectID` with your desired custom property name, and `YOUR_ACCESS_TOKEN` with your authentication token.

#### Using Python

Here's a complete Python example:

```python
# Tutorial Code Example - Add Non-Existing Document Property in Python
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

# API endpoint for creating custom document property
file_name = "Home_move_plan.mpp"  # Your project file name
property_name = "ProjectID"  # Custom property name
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
    "value": "PRJ-2025-042"  # The value for your custom property
}

# Execute request
response = requests.post(api_endpoint, headers=headers, json=property_data)

# Process and display results
if response.status_code == 200 or response.status_code == 201:
    property_info = response.json().get("property", {})
    print(f"Custom property added successfully:")
    print(f"Name: {property_info.get('name')}")
    print(f"Value: {property_info.get('value')}")
    
    # Optionally: Add more custom properties
    # This demonstrates adding multiple custom properties in sequence
    custom_properties = [
        {"name": "Department", "value": "Engineering"},
        {"name": "Priority", "value": "High"},
        {"name": "ClientName", "value": "Acme Corporation"}
    ]
    
    print("\nAdding additional custom properties:")
    for prop in custom_properties:
        prop_name = prop["name"]
        prop_endpoint = f"https://api.aspose.cloud/v3.0/tasks/{file_name}/documentproperties/{prop_name}"
        
        prop_data = {
            "link": {
                "href": "",
                "rel": "",
                "type": "",
                "title": ""
            },
            "name": prop_name,
            "value": prop["value"]
        }
        
        prop_response = requests.post(prop_endpoint, headers=headers, json=prop_data)
        if prop_response.status_code == 200 or prop_response.status_code == 201:
            prop_info = prop_response.json().get("property", {})
            print(f"- Added {prop_info.get('name')}: {prop_info.get('value')}")
        else:
            print(f"- Error adding {prop_name}: {prop_response.status_code}")
else:
    print(f"Error: {response.status_code}")
    print(response.text)

# Verify all custom properties by retrieving all document properties
print("\nVerifying all document properties:")
properties_endpoint = f"https://api.aspose.cloud/v3.0/tasks/{file_name}/documentproperties"
properties_response = requests.get(properties_endpoint, headers={"Authorization": f"Bearer {access_token}", "Accept": "application/json"})

if properties_response.status_code == 200:
    properties = properties_response.json().get("properties", {}).get("list", [])
    print(f"Found {len(properties)} document properties:")
    # Filter to show only our custom properties
    custom_props = [p for p in properties if p.get('name') in ['ProjectID', 'Department', 'Priority', 'ClientName']]
    for prop in custom_props:
        print(f"{prop.get('name')}: {prop.get('value')}")
```

#### Using C#

```csharp
// Tutorial Code Example - Add Non-Existing Document Property in C#
using System;
using System.Collections.Generic;
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
            
            // Add custom document property
            await AddCustomProperty(token, "Home_move_plan.mpp", "ProjectID", "PRJ-2025-042");
            
            // Add more custom properties to demonstrate batch processing
            var customProperties = new List<(string Name, string Value)>
            {
                ("Department", "Engineering"),
                ("Priority", "High"),
                ("ClientName", "Acme Corporation")
            };
            
            Console.WriteLine("\nAdding additional custom properties:");
            foreach (var prop in customProperties)
            {
                await AddCustomProperty(token, "Home_move_plan.mpp", prop.Name, prop.Value);
            }
            
            // Verify all properties
            Console.WriteLine("\nVerifying all custom properties:");
            await VerifyCustomProperties(token, "Home_move_plan.mpp");
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
        
        static async Task AddCustomProperty(string accessToken, string fileName, string propertyName, string propertyValue)
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
                
                // Make the request to create custom document property
                var response = await client.PostAsync(
                    $"https://api.aspose.cloud/v3.0/tasks/{fileName}/documentproperties/{propertyName}", 
                    content);
                
                response.EnsureSuccessStatusCode();
                
                // Parse and display the response
                var responseContent = await response.Content.ReadAsStringAsync();
                
                // Extract property information from the JSON response
                using (JsonDocument document = JsonDocument.Parse(responseContent))
                {
                    var property = document.RootElement.GetProperty("property");
                    string name = property.GetProperty("name").GetString();
                    string value = property.GetProperty("value").GetString();
                    Console.WriteLine($"Added custom property - {name}: {value}");
                }
            }
        }
        
        static async Task VerifyCustomProperties(string accessToken, string fileName)
        {
            using (var client = new HttpClient())
            {
                // Set the authorization header
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
                
                // Make the request to get all document properties
                var response = await client.GetAsync(
                    $"https://api.aspose.cloud/v3.0/tasks/{fileName}/documentproperties");
                
                response.EnsureSuccessStatusCode();
                
                // Parse and display the custom properties
                var responseContent = await response.Content.ReadAsStringAsync();
                using (JsonDocument document = JsonDocument.Parse(responseContent))
                {
                    var propertiesList = document.RootElement
                        .GetProperty("properties")
                        .GetProperty("list");
                    
                    var customPropertyNames = new[] { "ProjectID", "Department", "Priority", "ClientName" };
                    
                    foreach (var element in propertiesList.EnumerateArray())
                    {
                        string name = element.GetProperty("name").GetString();
                        
                        // Only display our custom properties
                        if (Array.IndexOf(customPropertyNames, name) >= 0)
                        {
                            string value = element.GetProperty("value").GetString();
                            Console.WriteLine($"{name}: {value}");
                        }
                    }
                }
            }
        }
    }
}
```

#### Using Java

```java
// Tutorial Code Example - Add Non-Existing Document Property in Java
import java.io.IOException;
import java.net.URI;
import java.net.URLEncoder;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import org.json.JSONArray;
import org.json.JSONObject;

public class AddNonExistingDocumentPropertyTutorial {

    private static final String CLIENT_ID = "YOUR_CLIENT_ID";
    private static final String CLIENT_SECRET = "YOUR_CLIENT_SECRET";
    private static final HttpClient httpClient = HttpClient.newHttpClient();

    public static void main(String[] args) throws IOException, InterruptedException {
        // Get access token
        String accessToken = getAccessToken();
        
        // Add first custom document property
        String fileName = "Home_move_plan.mpp";
        String propertyName = "ProjectID";
        String propertyValue = "PRJ-2025-042";
        addCustomProperty(accessToken, fileName, propertyName, propertyValue);
        
        // Add more custom properties to demonstrate batch processing
        System.out.println("\nAdding additional custom properties:");
        List<Property> customProperties = new ArrayList<>();
        customProperties.add(new Property("Department", "Engineering"));
        customProperties.add(new Property("Priority", "High"));
        customProperties.add(new Property("ClientName", "Acme Corporation"));
        
        for (Property prop : customProperties) {
            addCustomProperty(accessToken, fileName, prop.name, prop.value);
        }
        
        // Verify all properties
        System.out.println("\nVerifying all custom properties:");
        verifyCustomProperties(accessToken, fileName);
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

    private static void addCustomProperty(String accessToken, String fileName, String propertyName, String propertyValue) 
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
        
        // Prepare the request to create custom document property
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
            System.out.println("Added custom property - " + property.getString("name") + ": " + property.getString("value"));
        } else {
            System.out.println("Error: " + response.statusCode());
            System.out.println(jsonResponse.toString(2));
        }
    }
    
    private static void verifyCustomProperties(String accessToken, String fileName) 
            throws IOException, InterruptedException {
        // URL encode the file name
        String encodedFileName = URLEncoder.encode(fileName, StandardCharsets.UTF_8);
        
        // Prepare the request to get all document properties
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.aspose.cloud/v3.0/tasks/" + encodedFileName + "/documentproperties"))
                .header("Authorization", "Bearer " + accessToken)
                .GET()
                .build();

        // Make the request
        HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());
        
        // Parse and display the custom properties
        JSONObject jsonResponse = new JSONObject(response.body());
        
        if (response.statusCode() == 200) {
            JSONArray propertiesList = jsonResponse.getJSONObject("properties").getJSONArray("list");
            
            // List of custom property names we're interested in
            List<String> customPropertyNames = Arrays.asList("ProjectID", "Department", "Priority", "ClientName");
            
            for (int i = 0; i < propertiesList.length(); i++) {
                JSONObject property = propertiesList.getJSONObject(i);
                String name = property.getString("name");
                
                // Only display our custom properties
                if (customPropertyNames.contains(name)) {
                    String value = property.getString("value");
                    System.out.println(name + ": " + value);
                }
            }
        } else {
            System.out.println("Error verifying properties: " + response.statusCode());
            System.out.println(jsonResponse.toString(2));
        }
    }
    
    // Simple class to hold property name-value pairs
    static class Property {
        String name;
        String value;
        
        Property(String name, String value) {
            this.name = name;
            this.value = value;
        }
    }
}
```

### Step 4: Understand the Response

When successful, the API returns a JSON response similar to what you'd see when creating standard properties:

```json
{
  "code": 0,
  "status": "OK",
  "property": {
    "link": {
      "href": "https://api.aspose.cloud/v3.0/tasks/Home_move_plan.mpp/documentproperties/ProjectID",
      "rel": "self",
      "type": "text/json",
      "title": "ProjectID"
    },
    "name": "ProjectID",
    "value": "PRJ-2025-042"
  }
}
```

The key information is in the `property` object, which contains:
- `name`: The name of your custom property
- `value`: The value you set for the property
- `link`: Reference information for the property

### Step 5: Best Practices for Custom Properties

When working with custom document properties, follow these best practices:

#### Naming Conventions

- Use clear, descriptive names that indicate the property's purpose
- Avoid spaces and special characters when possible
- Follow a consistent naming pattern across all projects
- Consider prefixing organization-specific properties (e.g., "ACME_ProjectID")

#### Value Consistency

- Use consistent data formats, especially for dates and numbers
- Consider standardizing values (e.g., "High/Medium/Low" for priorities)
- Document the expected format for each custom property

#### Property Management

- Create a central registry of custom properties used in your organization
- Document the purpose and expected values for each custom property
- Implement validation for custom property values when possible
- Consider batch processing for setting multiple custom properties at once

## Data Types for Custom Properties

Custom properties can store different types of information:

### Text Properties

Most custom properties will be text-based. You can store any string value:

```json
{
  "name": "Department",
  "value": "Engineering"
}
```

### Date Properties

For date information, provide the date in ISO format:

```json
{
  "name": "ReviewDate",
  "value": "2025-04-23T12:00:00Z"
}
```

### Numeric Properties

For numeric values, provide the value as a string containing only numbers:

```json
{
  "name": "Priority",
  "value": "1"
}
```

### Boolean Properties

For boolean values, use "true" or "false" as text:

```json
{
  "name": "IsApproved",
  "value": "true"
}
```

## Try It Yourself

Now it's your turn to practice! Try adding these custom properties to your project file:

1. Add a "ProjectCode" property with a unique identifier
2. Create a "ResponsibleTeam" property with a department or team name
3. Add a "StartupCost" property with a numeric value
4. Create a "LastReviewDate" property with today's date in ISO format

For an additional challenge, try adding multiple custom properties in a single program run and then verify they were all created correctly.

## Troubleshooting Tips

If you encounter issues while following this tutorial, check these common problems:

- 400 Bad Request: Verify your JSON payload is properly formatted
- Property Already Exists: Ensure you're not trying to create a property that already exists
- Inconsistent Behavior: Some MS Project versions might handle custom properties differently
- Missing Properties After Save: Ensure the file format supports custom properties (MPP is recommended)

## What You've Learned

In this tutorial, you've learned how to:

1. Create custom, non-existing document properties in MS Project files
2. Structure the JSON payload for custom property creation
3. Implement naming conventions and best practices for custom properties
4. Use different data types for custom property values
5. Verify custom property creation
6. Process multiple custom properties in sequence

## Further Practice

To reinforce your learning:

1. Create a property management tool that adds a standard set of custom properties to all project files
2. Implement validation for custom property values before setting them
3. Build a reporting tool that extracts and summarizes custom properties from multiple project files
4. Create a migration script that adds your organization's standard custom properties to legacy project files

## Completing the Learning Path

Congratulations! You've completed the full tutorial series on working with document properties in Aspose.Tasks Cloud API. You now have the knowledge to:

1. Retrieve all document properties from project files
2. Access specific document properties by name
3. Create new standard document properties
4. Edit existing document properties
5. Add custom, non-existing document properties

These skills provide a solid foundation for implementing robust document metadata management in your project management applications.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial series? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/) for assistance!
