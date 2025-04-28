---
title: Creating Your First Search Context Tutorial
url: /reverse-image-search/create-search-context/
weight: 10
description: Learn how to create a search context for reverse image search with this step-by-step tutorial using Aspose.Imaging Cloud API.
---

# Tutorial: Creating Your First Search Context

## Learning Objectives

In this tutorial, you'll learn:
- What a search context is and why it's essential for reverse image search
- How to create a new search context using Aspose.Imaging Cloud API
- How to work with the returned search context ID
- Best practices for managing search contexts

## Prerequisites

Before starting this tutorial, please ensure you have:
- An Aspose Cloud account ([sign up for free](https://dashboard.aspose.cloud/#/apps))
- Your Aspose Cloud credentials (Client ID and Client Secret)
- A basic understanding of REST API calls
- Familiarity with command-line tools or your preferred programming language

## What is a Search Context?

A search context is a fundamental component in the Aspose.Imaging Cloud reverse image search system. It serves as a container that:

- Stores the features index structure for fast image searching
- Maintains service information about your image collection
- Enables efficient similarity comparisons between images

Think of a search context as a specialized database optimized for visual similarity searching.

## Step 1: Obtain an Access Token

Before creating a search context, you need to authenticate with the Aspose Cloud API by obtaining an access token.

```bash
curl -v "https://api.aspose.cloud/oauth2/token" \
-X POST \
-d 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET' \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

The response will contain an access token that you'll use in subsequent API calls:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

## Step 2: Create a New Search Context

Now that you have an access token, you can create a new search context. This is done with a simple POST request:

```bash
curl -v "https://api.aspose.cloud/v3/imaging/ai/imageSearch/create" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Content-Length: 0" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

If successful, you'll receive a response with a unique search context ID:

```json
{
  "Id": "f0cf8f1f-6a96-420c-927d-f200757ffbb8",
  "SearchStatus": "Idle",
  "Code": 200,
  "Status": "OK"
}
```

Important: Save this ID carefully as you'll need it for all subsequent operations with this search context.

## Step 3: Understanding Search Context Parameters

When creating a search context, you can customize it with the following parameters:

- detector: The features detector algorithm (default is AKAZE)
- matchingAlgorithm: The algorithm used for matching features (default is RandomBinaryTree)

These parameters determine how image features are extracted and compared, affecting the accuracy and performance of your image searches.

## Try It Yourself

Now it's your turn to create a search context:

1. Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual Aspose Cloud credentials
2. Execute the curl command to obtain an access token
3. Replace `YOUR_ACCESS_TOKEN` with the token you received
4. Create your first search context using the second curl command
5. Note down the search context ID for future use

## Using SDK Libraries

### Java SDK Example

```java
import com.aspose.imaging.cloud.sdk.api.ImagingApi;
import com.aspose.imaging.cloud.sdk.api.ImagingBase;
import com.aspose.imaging.cloud.sdk.model.requests.*;
import com.aspose.imaging.cloud.sdk.model.*;

public class CreateSearchContextExample {
    
    public static void main(String[] args) {
        // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Create instance of the API
        ImagingApi api = new ImagingApi(clientId, clientSecret);
        
        try {
            // Create search context
            SearchContextStatus status = api.createImageSearch(null, null, null);
            
            // Print the search context ID
            System.out.println("Search context created with ID: " + status.getId());
            System.out.println("Status: " + status.getSearchStatus());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### .NET SDK Example

```csharp
using Aspose.Imaging.Cloud.Sdk.Api;
using Aspose.Imaging.Cloud.Sdk.Model;
using System;

namespace AsposeImagingCloudTutorials
{
    class CreateSearchContextExample
    {
        static void Main(string[] args)
        {
            // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
            string clientId = "YOUR_CLIENT_ID";
            string clientSecret = "YOUR_CLIENT_SECRET";
            
            // Create instance of the API
            ImagingApi api = new ImagingApi(clientId, clientSecret);
            
            try
            {
                // Create search context
                SearchContextStatus status = api.CreateImageSearch();
                
                // Print the search context ID
                Console.WriteLine("Search context created with ID: " + status.Id);
                Console.WriteLine("Status: " + status.SearchStatus);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error: " + ex.Message);
            }
        }
    }
}
```

## What You've Learned

In this tutorial, you've learned:
- The concept and importance of a search context
- How to obtain an authentication token for Aspose Cloud API
- How to create a new search context using REST API
- How to use the SDK libraries to create a search context programmatically

## Troubleshooting Tips

- Authentication Errors: Ensure your Client ID and Client Secret are correct.
- API Version: Make sure you're using the correct API version in the URL path.
- Token Expiration: Access tokens expire after a certain time. If you get authentication errors after working for a while, you may need to obtain a new token.

## Next Steps

Now that you've created your first search context, you're ready to move on to the next tutorial in our series:

→ [Tutorial: Understanding Search Context Status](/reverse-image-search/get-search-context-status/)

In the next tutorial, you'll learn how to check the status of your search context and understand the different states it can be in.

## Further Practice

To reinforce what you've learned:
- Create multiple search contexts with different detector and matching algorithm settings
- Delete a search context you no longer need (hint: use the DELETE endpoint)
- Write a script that creates a search context and saves the ID to a configuration file

## Helpful Resources

- [Product Page](https://products.aspose.cloud/imaging/)
- [Documentation](https://docs.aspose.cloud/imaging/)
- [API Reference](https://reference.aspose.cloud/imaging/)
- [Free Support](https://forum.aspose.cloud/c/imaging/10/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Please feel free to post your questions on our [support forum](https://forum.aspose.cloud/c/imaging/10/).
