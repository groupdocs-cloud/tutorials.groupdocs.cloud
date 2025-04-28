---
title: How to Get Diagram Document Information Tutorial
url: /convert-diagram-file/get-diagram-information/
weight: 10
description: Learn how to retrieve diagram document information using Aspose.Diagram Cloud API in this step-by-step tutorial for developers.
---

# Tutorial: How to Get Diagram Document Information

## Introduction

In this tutorial, you'll learn how to retrieve essential information from diagram files using Aspose.Diagram Cloud API. Understanding the structure and metadata of diagram files is a crucial first step before performing conversions or modifications.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Authenticate with the Aspose.Diagram Cloud API
- Send requests to retrieve diagram document information
- Parse and understand the diagram document structure
- Access page information and shape details

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account ([get a free trial here](https://dashboard.aspose.cloud/#/apps))
- Your Client ID and Client Secret from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
- A basic understanding of REST APIs
- A diagram file uploaded to your Aspose Cloud Storage (see [Storage tutorial](/convert-diagram-file/working-with-files-storage/) if needed)

## Step-by-Step Guide

### 1. Authentication

The first step in any operation with Aspose.Diagram Cloud is to authenticate and obtain an access token.

#### Using cURL

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Make sure to replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

The response will include an access token that you'll use in subsequent API calls:

```json
{
  "access_token": "eyJhbGci...",
  "expires_in": 86400,
  "token_type": "Bearer"
}
```

### 2. Getting Diagram Information

Now, let's retrieve information about a diagram file. We'll use the GET operation on the `/diagram/{name}` endpoint.

#### Using cURL

```bash
curl -v "https://api.aspose.cloud/v1.1/diagram/YOUR_DIAGRAM_FILE.vdx" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_DIAGRAM_FILE.vdx` with your actual diagram filename and `YOUR_ACCESS_TOKEN` with the token obtained in step 1.

#### Response Structure

The API will return a JSON response with the diagram structure:

```json
{
  "diagramModel": {
    "FileName": null,
    "Pages": [
      {
        "PageName": "paghe1",
        "Sharps": [
          {
            "Name": "Shape1"
          }
        ]
      }
    ]
  },
  "Code": 200,
  "Status": "OK"
}
```

### 3. Understanding the Response

The response provides valuable information about your diagram:
- `Pages`: An array of pages in your diagram
- `PageName`: The name of each page
- `Sharps`: The shapes contained on each page
- `Name`: The name of each shape

This information is particularly useful when you need to:
- Understand diagram complexity before conversion
- Target specific pages or shapes for processing
- Verify diagram structure after upload

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
                // Get diagram information
                var response = diagramApi.GetDiagram("sample_diagram.vdx");
                
                // Process diagram information
                Console.WriteLine("Diagram retrieved successfully!");
                Console.WriteLine($"Number of pages: {response.DiagramModel.Pages.Count}");
                
                // Display page information
                foreach (var page in response.DiagramModel.Pages)
                {
                    Console.WriteLine($"Page name: {page.PageName}");
                    Console.WriteLine($"Number of shapes: {page.Sharps.Count}");
                    
                    foreach (var shape in page.Sharps)
                    {
                        Console.WriteLine($"  Shape name: {shape.Name}");
                    }
                }
            }
            catch (Exception e)
            {
                Debug.Print("Exception when getting diagram information: " + e.Message);
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

public class GetDiagramInfoTutorial {
    public static void main(String[] args) {
        try {
            // Configure API credentials
            ApiClient apiClient = new ApiClient();
            apiClient.setClientId("YOUR_CLIENT_ID");
            apiClient.setClientSecret("YOUR_CLIENT_SECRET");
            
            // Create API instance
            DiagramFileApi diagramApi = new DiagramFileApi(apiClient);
            
            // Get diagram information
            DiagramResponse response = diagramApi.getDiagram("sample_diagram.vdx", null, null);
            
            // Process diagram information
            System.out.println("Diagram retrieved successfully!");
            System.out.println("Number of pages: " + response.getDiagramModel().getPages().size());
            
            // Display page information
            for (Page page : response.getDiagramModel().getPages()) {
                System.out.println("Page name: " + page.getPageName());
                System.out.println("Number of shapes: " + page.getSharps().size());
                
                for (SharpModel shape : page.getSharps()) {
                    System.out.println("  Shape name: " + shape.getName());
                }
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
    # Get diagram information
    response = api_instance.get_diagram("sample_diagram.vdx")
    
    # Process diagram information
    print("Diagram retrieved successfully!")
    print(f"Number of pages: {len(response.diagram_model.pages)}")
    
    # Display page information
    for page in response.diagram_model.pages:
        print(f"Page name: {page.page_name}")
        print(f"Number of shapes: {len(page.sharps)}")
        
        for shape in page.sharps:
            print(f"  Shape name: {shape.name}")
            
except Exception as e:
    print(f"Exception when getting diagram information: {e}")
```

## Try It Yourself

Now it's your turn! Follow these steps to practice what you've learned:

1. Upload a Visio diagram to your Aspose Cloud Storage
2. Use one of the examples above to retrieve information about your diagram
3. Modify the code to extract specific information you're interested in
4. Try with different diagram files to see how the structure varies

## Common Issues and Troubleshooting

- Authentication errors: Double-check your Client ID and Client Secret
- File not found errors: Ensure the file exists in your cloud storage
- Access denied errors: Verify your authentication token is valid and not expired

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the Aspose.Diagram Cloud API
- Retrieve diagram file information
- Understand the structure of a diagram document
- Extract page and shape information

This knowledge forms the foundation for more advanced operations like diagram conversion, which we'll explore in the next tutorials.

## Next Steps

Now that you understand how to retrieve diagram information, you're ready to move on to creating diagram files in our next tutorial:

- [Tutorial: How to Create a New Diagram File](/convert-diagram-file/create-diagram-file/)

## Related Resources

- [Product Page](https://products.aspose.cloud/diagram/)
- [Documentation](https://docs.aspose.cloud/diagram/)
- [API Reference](https://reference.aspose.cloud/diagram/)
- [Free Support](https://forum.aspose.cloud/c/diagram/27/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to ask in the [Aspose.Diagram Cloud forum](https://forum.aspose.cloud/c/diagram/27/).
