---
title: How to Get Supported File Types with GroupDocs.Parser Cloud API Tutorial
url: /data-operations/get-supported-file-types/
weight: 1
description: Learn to retrieve all supported file formats with GroupDocs.Parser Cloud API in this step-by-step tutorial for developers
---

# Tutorial: How to Get Supported File Types with GroupDocs.Parser Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Authenticate with the GroupDocs.Parser Cloud API
- Make REST API calls to retrieve supported file formats
- Implement this functionality in various programming languages
- Handle and process the response data

## Prerequisites

Before starting this tutorial, you should have:

1. A GroupDocs.Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. Basic understanding of REST API concepts
4. Familiarity with your chosen programming language (C#, Java, or cURL)

## Real-World Scenario

Imagine you're developing a document management application that needs to validate file types before processing. Before accepting user uploads, you want to check if the file format is supported by the parsing system. This tutorial shows you how to retrieve the complete list of supported formats to implement this validation.

## Step 1: Obtain an Access Token

Before making API calls, you need to authenticate with the GroupDocs.Parser Cloud API using your Client ID and Client Secret.

### Try it yourself:

Use the following cURL command to obtain a JWT token:

```bash
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Remember to replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

The response will include an access token that you'll use in subsequent API calls:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

## Step 2: Call the Get Supported File Types API

Now that you have your authentication token, you can call the API endpoint to get the list of supported file formats.

### cURL Example

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/formats" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Response

The API returns a JSON response with all supported file formats:

```json
{
  "formats": [    
    {
      "extension": ".doc",
      "fileFormat": "Microsoft Word Document"
    },
    {
      "extension": ".docm",
      "fileFormat": "Word Open XML Macro-Enabled Document"
    },
    {
      "extension": ".docx",
      "fileFormat": "Microsoft Word Open XML Document"
    },
    /* Additional formats listed here */
    {
      "extension": ".xlsx",
      "fileFormat": "Microsoft Excel Open XML Spreadsheet"
    }
  ]
}
```

## Step 3: Implement in Your Application

Let's implement this functionality using SDK examples in different programming languages.

### C# Example

```csharp
using System;
using GroupDocs.Parser.Cloud.Sdk.Api;
using GroupDocs.Parser.Cloud.Sdk.Client;
using GroupDocs.Parser.Cloud.Sdk.Model;

namespace GetSupportedFileTypes
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API client
            var configuration = new Configuration
            {
                AppSid = "YOUR_CLIENT_ID",
                AppKey = "YOUR_CLIENT_SECRET"
            };
            
            // Create API instance
            var infoApi = new InfoApi(configuration);
            
            try
            {
                // Get supported file formats
                var response = infoApi.GetSupportedFileFormats();
                
                // Display supported formats
                Console.WriteLine("Supported File Formats:");
                foreach (var format in response.Formats)
                {
                    Console.WriteLine($"Extension: {format.Extension}, Format: {format.FileFormat}");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Java Example

```java
import com.groupdocs.parser.cloud.sdk.api.InfoApi;
import com.groupdocs.parser.cloud.sdk.client.ApiException;
import com.groupdocs.parser.cloud.sdk.client.Configuration;
import com.groupdocs.parser.cloud.sdk.model.FormatsResult;
import com.groupdocs.parser.cloud.sdk.model.Format;

public class GetSupportedFileTypes {
    public static void main(String[] args) {
        // Configure API client
        Configuration configuration = new Configuration();
        configuration.setAppSid("YOUR_CLIENT_ID");
        configuration.setAppKey("YOUR_CLIENT_SECRET");
        
        try {
            // Create InfoApi instance
            InfoApi infoApi = new InfoApi(configuration);
            
            // Get supported file formats
            FormatsResult response = infoApi.getSupportedFileFormats();
            
            // Display supported formats
            System.out.println("Supported File Formats:");
            for (Format format : response.getFormats()) {
                System.out.println("Extension: " + format.getExtension() + 
                                  ", Format: " + format.getFileFormat());
            }
        } catch (ApiException e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Troubleshooting Common Issues

1. Authentication Errors: If you receive a 401 Unauthorized error, ensure your Client ID and Client Secret are correct and that your token hasn't expired.

2. Connection Issues: If you can't connect to the API, check your network connection and ensure the API endpoint URL is correct.

3. Parsing Response: If you have trouble processing the response, verify that you're correctly deserializing the JSON response according to your programming language's conventions.

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the GroupDocs.Parser Cloud API
- Make API calls to retrieve the list of supported file formats
- Process and display the formats information
- Implement this functionality using C# and Java SDKs

This knowledge forms the foundation for working with document formats in your applications using GroupDocs.Parser Cloud.

## Further Practice

To reinforce your learning:
1. Try implementing the same functionality using a different programming language
2. Create a simple file validator that checks if a given extension is supported
3. Build a user interface that displays all supported formats categorized by document type

## Next Steps

Ready to continue your learning journey? Check out our next tutorial: [How to Get Document Information](/data-operations/get-document-information/) to learn how to extract metadata from specific documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
