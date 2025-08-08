---
title: How to Get Document Information with GroupDocs.Parser Cloud API Tutorial
url: /data-operations/get-document-information/
weight: 2
description: Learn to extract document metadata including file format, size, and page count using GroupDocs.Parser Cloud API in this developer tutorial
---

# Tutorial: How to Get Document Information with GroupDocs.Parser Cloud API

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve basic document metadata using GroupDocs.Parser Cloud API
- Extract information such as file format, size, and page count
- Handle standard and password-protected documents
- Implement this functionality in multiple programming languages

## Prerequisites

Before starting this tutorial, you should have:

1. A GroupDocs.Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret credentials from the [dashboard](https://dashboard.groupdocs.cloud/#/apps)
3. Basic understanding of REST API concepts
4. A document uploaded to your GroupDocs storage (or you can use the sample documents provided in the API)
5. Familiarity with your chosen programming language (C#, Java, or cURL)

## Real-World Scenario

Imagine you're developing a document management system that needs to display metadata for each uploaded file. Before processing any document, you want to retrieve basic information like file type, size, and page count to help users identify their documents and to plan processing resources. This tutorial shows you how to implement this functionality.

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

## Step 2: Prepare the Request

To get document information, you'll need to call the `/info` endpoint with the path to your document in storage.

### Understanding the Request Parameters

| Name | Description | Comment |
|---|---|---|
| FileInfo.FilePath | The path of the document in storage | Required |
| FileInfo.StorageName | Storage name | Optional (omit for default storage) |
| FileInfo.Password | The password to open file | Required only for password-protected documents |
| ContainerItemInfo.RelativePath | The relative path of the container | Required only for container files |
| ContainerItemInfo.Password | Password for container items | Required only for password-protected container items |

## Step 3: Call the Get Document Information API

Now that you have your authentication token, you can call the API endpoint to get document information.

### cURL Example

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{ \"FileInfo\": { \"FilePath\": \"/words/four-pages.docx\" } }"
```

### Response

The API returns a JSON response with the document information:

```json
{
    "fileType": {
        "fileFormat": "Microsoft Word Open XML Document",
        "extension": ".docx"
    },
    "size": 8651,
    "pageCount": 4
}
```

## Step 4: Handle Password-Protected Documents

If you need to get information from a password-protected document, you'll need to include the password in your request.

### cURL Example for Password-Protected Document

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/info" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-d "{ \"FileInfo\": { \"FilePath\": \"/words/protected-document.docx\", \"Password\": \"password123\" } }"
```

## Step 5: Implement in Your Application

Let's implement this functionality using SDK examples in different programming languages.

### C# Example

```csharp
using System;
using GroupDocs.Parser.Cloud.Sdk.Api;
using GroupDocs.Parser.Cloud.Sdk.Client;
using GroupDocs.Parser.Cloud.Sdk.Model;
using GroupDocs.Parser.Cloud.Sdk.Model.Requests;

namespace GetDocumentInformation
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
                // Create request object
                var fileInfo = new FileInfo
                {
                    FilePath = "/words/four-pages.docx"
                    // Add Password property if the document is password-protected
                    // Password = "password123"
                };
                
                var request = new GetInfoRequest(fileInfo);
                
                // Get document information
                var response = infoApi.GetInfo(request);
                
                // Display document information
                Console.WriteLine($"File Format: {response.FileType.FileFormat}");
                Console.WriteLine($"Extension: {response.FileType.Extension}");
                Console.WriteLine($"Size: {response.Size} bytes");
                Console.WriteLine($"Page Count: {response.PageCount}");
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
import com.groupdocs.parser.cloud.sdk.model.FileInfo;
import com.groupdocs.parser.cloud.sdk.model.InfoResult;
import com.groupdocs.parser.cloud.sdk.model.requests.GetInfoRequest;

public class GetDocumentInformation {
    public static void main(String[] args) {
        // Configure API client
        Configuration configuration = new Configuration();
        configuration.setAppSid("YOUR_CLIENT_ID");
        configuration.setAppKey("YOUR_CLIENT_SECRET");
        
        try {
            // Create InfoApi instance
            InfoApi infoApi = new InfoApi(configuration);
            
            // Create request object
            FileInfo fileInfo = new FileInfo();
            fileInfo.setFilePath("/words/four-pages.docx");
            // Add password if the document is password-protected
            // fileInfo.setPassword("password123");
            
            GetInfoRequest request = new GetInfoRequest(fileInfo);
            
            // Get document information
            InfoResult response = infoApi.getInfo(request);
            
            // Display document information
            System.out.println("File Format: " + response.getFileType().getFileFormat());
            System.out.println("Extension: " + response.getFileType().getExtension());
            System.out.println("Size: " + response.getSize() + " bytes");
            System.out.println("Page Count: " + response.getPageCount());
        } catch (ApiException e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Learning Checkpoint

Let's verify what you've learned so far:

1. What endpoint is used to retrieve document information?
2. What parameters must be included in the request?
3. How do you handle password-protected documents?
4. What information does the API return about a document?

## Troubleshooting Common Issues

1. File Not Found: If you receive a 404 error, check that the document path is correct and that the file exists in your storage.

2. Authentication Errors: If you receive a 401 Unauthorized error, ensure your access token is valid and hasn't expired.

3. Password Required: If you receive an error indicating that a password is required, ensure you've included the correct password in your request for protected documents.

4. Unsupported Format: If you receive an error about an unsupported format, check if the file type is supported by GroupDocs.Parser Cloud. You can use the [Get Supported File Types](/data-operations/get-supported-file-types/) API to verify.

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the GroupDocs.Parser Cloud API
- Make API calls to retrieve document information
- Process responses to extract metadata like file format, size, and page count
- Handle password-protected documents
- Implement this functionality using C# and Java SDKs

This knowledge is essential for building document management systems that need to process and validate documents before performing more complex operations.

## Further Practice

To reinforce your learning:
1. Try retrieving information for different document types (PDF, Excel, images)
2. Create a simple application that displays document thumbnails along with metadata
3. Implement error handling for various scenarios (file not found, unsupported format, etc.)


## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
