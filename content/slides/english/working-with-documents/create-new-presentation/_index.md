---
title: How to Create a New Presentation from Scratch Tutorial
url: /working-with-documents/create-new-presentation/
description: Learn how to create PowerPoint presentations from scratch using Aspose.Slides Cloud API with REST and SDKs. Step-by-step guide for beginners.
weight: 10
---

## Prerequisites

Before starting this tutorial, you should:

1. Have an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps) with active credentials
2. Be familiar with making REST API calls
3. Have your preferred development environment set up (for SDK examples)
4. Understand basic PowerPoint document concepts

## Introduction

Creating presentations programmatically is a common requirement for many business applications. Whether you're generating reports, building a presentation creation tool, or automating document workflows, the ability to create PowerPoint presentations from scratch is essential.

This tutorial demonstrates how to use Aspose.Slides Cloud API to create brand new, empty presentations that can later be populated with content. We'll start with the simplest approach and build on that foundation.

## Understanding the API

The API we'll be using is:

| API | Method | Description | Resource |
| --- | --- | --- | --- |
| /slides/{name} | POST | Creates a new empty presentation | [Create Presentation](https://apireference.aspose.cloud/slides/#/Document/CreatePresentation) |

The file format is determined based on the file extension:
- Use .ppt or .pot for Office 97-2003 compatible presentations
- Use .pptx, .pptm, .potx, .potm, .ppsx, or .ppsm for Office 2007-2010 compatible presentations

## Step 1: Setting Up Authentication

Before making API calls, you need to authenticate. Aspose.Slides Cloud API uses OAuth 2.0 client credentials flow.

### Try it yourself

First, request an access token:

```bash
curl -X POST "https://api.aspose.cloud/connect/token" \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded"
```

The response will include an access token like this:

```json
{
  "access_token": "your-access-token",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

Save this token to use in the following API calls.

## Step 2: Creating an Empty Presentation

Now let's create a new empty presentation:

### Try it yourself with cURL

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx?folder=MyFolder&storage=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: multipart/form-data" \
     -d ""
```

### What's happening?

In this request:
1. We're making a POST request to create a presentation named "MyPresentation.pptx"
2. We're specifying a folder "MyFolder" in the "MyStorage" storage where the presentation will be saved
3. We're sending an empty payload to create an empty presentation

### Response

Upon success, you'll receive a JSON response containing information about the newly created presentation:

```json
{
  "documentProperties": {
    "href": "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/documentProperties?folder=MyFolder",
    "relation": "self"
  },
  "viewProperties": {
    "href": "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/viewProperties?folder=MyFolder",
    "relation": "self"
  },
  "slides": {
    "href": "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides?folder=MyFolder",
    "relation": "self"
  },
  /* Additional properties */
  "selfUri": {
    "href": "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx?folder=MyFolder",
    "relation": "self"
  }
}
```

## Step 3: Implementing with SDKs

Let's see how to implement this functionality using SDKs for different programming languages:

### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/aspose-Slides-cloud/aspose-Slides-cloud-dotnet

using System;
using Aspose.Slides.Cloud.Sdk;

class CreateEmptyPresentation
{
    static void Main()
    {
        // Setup client credentials
        SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Create a new empty presentation
        try {
            var response = api.CreatePresentation("MyPresentation.pptx", folder: "MyFolder");
            Console.WriteLine("Presentation created successfully!");
            Console.WriteLine("Access URL: " + response.SelfUri.Href);
        }
        catch (Exception ex) {
            Console.WriteLine("Error creating presentation: " + ex.Message);
        }
    }
}
```

### Python Example

```python
# For complete examples and data files, please go to https://github.com/aspose-Slides-cloud/aspose-Slides-cloud-python

import asposeslidescloud
from asposeslidescloud.apis.slides_api import SlidesApi

# Setup client credentials
slides_api = SlidesApi(None, "YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET")

try:
    # Create a new empty presentation
    response = slides_api.create_presentation("MyPresentation.pptx", folder="MyFolder")
    print("Presentation created successfully!")
    print(f"Access URL: {response.self_uri.href}")
except Exception as e:
    print(f"Error creating presentation: {str(e)}")
```

### Java Example

```java
// For complete examples and data files, please go to https://github.com/aspose-Slides-cloud/aspose-Slides-cloud-java

import com.aspose.slides.api.SlidesApi;
import com.aspose.slides.ApiException;

public class CreateEmptyPresentation {
    public static void main(String[] args) {
        // Setup client credentials
        SlidesApi slidesApi = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        try {
            // Create a new empty presentation
            Document response = slidesApi.createPresentation("MyPresentation.pptx", null, null, null, "MyFolder", "MyStorage");
            System.out.println("Presentation created successfully!");
            System.out.println("Access URL: " + response.getSelfUri().getHref());
        } catch (ApiException e) {
            System.err.println("Error creating presentation: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Step 4: Creating from Source Document

You can also create a presentation based on an existing presentation:

### Try it yourself with cURL

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/MyNewPresentation.pptx?folder=MyFolder&storage=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: multipart/form-data" \
     -F "file=@source.pptx"
```

Here, we're uploading "source.pptx" as the basis for our new presentation.

## Troubleshooting Tips

Here are some common issues you might encounter:

1. Authentication Errors: Ensure your client credentials are correct and the access token is valid and not expired.

2. Storage or Folder Not Found: Verify that the specified storage and folder exist. If not specified, default storage is used.

3. File Already Exists: The API will overwrite existing files with the same name. If you want to avoid this, check if the file exists first.

4. API Rate Limiting: If you're making many requests, you might hit API rate limits. Implement throttling in your application.

## What You've Learned

In this tutorial, you've learned how to:

- Authenticate with the Aspose.Slides Cloud API
- Create empty PowerPoint presentations
- Create presentations from existing files
- Implement this functionality in different programming languages
- Handle common issues that might arise

## Further Practice

To reinforce your learning, try these exercises:

1. Create a presentation with a specific PowerPoint version format (.ppt vs .pptx)
2. Create a presentation template (.potx) instead of a regular presentation
3. Create a presentation in a new folder that doesn't exist yet (hint: you'll need to create the folder first)
4. Create a presentation and then immediately add a slide to it (combine with another API call)

## Next Steps

Now that you've learned how to create presentations, you might want to learn how to:

- [Create a Presentation from a Template](/working-with-documents/use-document-template/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
