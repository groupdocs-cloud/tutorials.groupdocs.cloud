---
title: How to Create a New Diagram File Tutorial
url: /convert-diagram-file/create-diagram-file/
weight: 20
description: Learn how to create new diagram files using Aspose.Diagram Cloud API in this beginner-friendly tutorial for developers.
---

# Tutorial: How to Create a New Diagram File

## Introduction

In this tutorial, you'll learn how to create a new, empty diagram file using Aspose.Diagram Cloud API. Creating diagram files programmatically is a fundamental skill that serves as a foundation for generating diagrams dynamically in your applications.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Authenticate with the Aspose.Diagram Cloud API
- Create a new, empty diagram file
- Verify the successful creation of the diagram
- Implement this functionality in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account ([get a free trial here](https://dashboard.aspose.cloud/#/apps))
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
- A basic understanding of REST APIs

## Step-by-Step Guide

### 1. Authentication

The first step is to authenticate with the Aspose.Diagram Cloud API to obtain an access token.

#### Using cURL

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

The response will include an access token that you'll use in subsequent API calls:

```json
{
  "access_token": "eyJhbGci...",
  "expires_in": 86400,
  "token_type": "Bearer"
}
```

### 2. Creating a New Diagram File

Now, let's create a new diagram file using the PUT operation on the `/diagram/{name}` endpoint.

#### Using cURL

```bash
curl -v "https://api.aspose.cloud/v1.1/diagram/new_diagram.vdx?IsOverwrite=true" \
-X PUT \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Content-Length: 0" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with the token obtained in step 1, and modify `new_diagram.vdx` to your desired file name.

#### Response

If successful, the API will return a JSON response indicating success:

```json
{
  "Code": 200,
  "Status": "OK"
}
```

### 3. Verifying the File Creation

To ensure that the file was created successfully, you can use the GET method to retrieve information about the newly created diagram.

```bash
curl -v "https://api.aspose.cloud/v1.1/diagram/new_diagram.vdx" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

This should return basic information about the empty diagram file that was created.

### 4. Implementing with SDKs

#### C# Example

```csharp
using System;
using System.Diagnostics;
using Aspose.Diagram.Cloud.Sdk.Api;
using Aspose.Diagram.Cloud.Sdk.Model;

namespace DiagramTutorials
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API key authorization
            var config = new Configuration
            {
                ClientId = "YOUR_CLIENT_ID",
                ClientSecret = "YOUR_CLIENT_SECRET" 
            };

            var diagramApi = new DiagramFileApi(config);
            
            try
            {
                // Create a new diagram
                string filename = "new_diagram.vdx";
                bool isOverwrite = true; // Set to true to overwrite if file already exists
                
                // Call the API to create a new diagram
                var response = diagramApi.PutCreate(filename, isOverwrite);
                
                // Check result
                if (response.Status == "OK")
                {
                    Console.WriteLine($"Diagram '{filename}' created successfully!");
                    
                    // Optionally, verify by getting diagram info
                    var diagramInfo = diagramApi.GetDiagram(filename);
                    Console.WriteLine("Verification successful - diagram info retrieved.");
                }
                else
                {
                    Console.WriteLine($"Failed to create diagram. Status: {response.Status}");
                }
            }
            catch (Exception e)
            {
                Debug.Print("Exception when creating diagram: " + e.Message);
                Console.WriteLine("Error occurred: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
import com.aspose.diagram.cloud.api.DiagramFileApi;
import com.aspose.diagram.cloud.model.*;
import com.aspose.diagram.cloud.auth.*;

public class CreateDiagramTutorial {
    public static void main(String[] args) {
        try {
            // Configure API credentials
            ApiClient apiClient = new ApiClient();
            apiClient.setClientId("YOUR_CLIENT_ID");
            apiClient.setClientSecret("YOUR_CLIENT_SECRET");
            
            // Create API instance
            DiagramFileApi diagramApi = new DiagramFileApi(apiClient);
            
            // Create a new diagram
            String filename = "new_diagram.vdx";
            Boolean isOverwrite = true; // Set to true to overwrite if file already exists
            
            // Call the API to create a new diagram
            ResultResponse response = diagramApi.putCreate(filename, isOverwrite);
            
            // Check result
            if (response.getStatus().equals("OK")) {
                System.out.println("Diagram '" + filename + "' created successfully!");
                
                // Optionally, verify by getting diagram info
                DiagramResponse diagramInfo = diagramApi.getDiagram(filename, null, null);
                System.out.println("Verification successful - diagram info retrieved.");
            } else {
                System.out.println("Failed to create diagram. Status: " + response.getStatus());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import asposediagramcloud
from asposediagramcloud.apis.diagram_file_api import DiagramFileApi
from asposediagramcloud.api_client import ApiClient
from asposediagramcloud.configuration import Configuration

# Configure API key authorization
configuration = Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API instance
api_instance = DiagramFileApi(ApiClient(configuration))

try:
    # Create a new diagram
    filename = "new_diagram.vdx"
    is_overwrite = True  # Set to True to overwrite if file already exists
    
    # Call the API to create a new diagram
    response = api_instance.put_create(filename, is_overwrite)
    
    # Check result
    if response.status == "OK":
        print(f"Diagram '{filename}' created successfully!")
        
        # Optionally, verify by getting diagram info
        diagram_info = api_instance.get_diagram(filename)
        print("Verification successful - diagram info retrieved.")
    else:
        print(f"Failed to create diagram. Status: {response.status}")
        
except Exception as e:
    print(f"Exception when creating diagram: {e}")
```

### 5. Understanding Diagram Formats

When creating a new diagram file, you need to specify the file extension, which determines the format:

- `.vsdx` - Modern Visio format (Visio 2013 and later)
- `.vsd` - Classic Visio format
- `.vdx` - XML-based Visio format
- `.vss` - Visio stencil file
- `.vst` - Visio template

Choose the format that best fits your requirements and is compatible with your target systems.

## Try It Yourself

Now it's your turn! Follow these steps to practice what you've learned:

1. Use one of the examples above to create a new diagram file
2. Try creating diagrams with different file extensions (.vsdx, .vsd, etc.)
3. Verify the creation by retrieving information about your diagram using the method from the previous tutorial
4. Modify the code to add error handling for common issues

## Common Issues and Troubleshooting

- Authentication errors: Double-check your Client ID and Client Secret
- File already exists: Use the `isOverwrite` parameter set to `true` to replace existing files
- Invalid file format: Ensure you're using a supported file extension

## What You've Learned

In this tutorial, you've learned how to:
- Create a new, empty diagram file using Aspose.Diagram Cloud API
- Verify the successful creation of the diagram
- Implement diagram creation in multiple programming languages
- Understand different diagram file formats

This skill is fundamental for applications that need to generate diagrams dynamically or provide diagram creation functionality to users.

## Related Resources

- [Product Page](https://products.aspose.cloud/diagram/)
- [Documentation](https://docs.aspose.cloud/diagram/)
- [API Reference](https://reference.aspose.cloud/diagram/)
- [Free Support](https://forum.aspose.cloud/c/diagram/27/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to ask in the [Aspose.Diagram Cloud forum](https://forum.aspose.cloud/c/diagram/27/).
