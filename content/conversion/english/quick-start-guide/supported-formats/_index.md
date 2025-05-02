---
title: Learn About Supported File Formats in GroupDocs.Conversion Cloud Tutorial
weight: 1
description: Learn how to retrieve and work with supported file formats in GroupDocs.Conversion Cloud API in this step-by-step tutorial for developers.
url: /quick-start-guide/supported-formats/
---

# Tutorial: Learn About Supported File Formats in GroupDocs.Conversion Cloud

## Learning Objectives

In this tutorial, you'll learn:
- How to retrieve all supported file formats in GroupDocs.Conversion Cloud
- How to get supported conversion formats for a specific file type
- How to process and utilize the formats information in your applications

## Prerequisites

Before starting this tutorial, you should have:
- A GroupDocs.Conversion Cloud account (get a [free trial](https://dashboard.groupdocs.cloud/#/apps) if needed)
- Your Client ID and Client Secret credentials
- Basic understanding of REST APIs
- Familiarity with your preferred programming language (C#, Java, PHP, etc.)

## Why Learn About Supported Formats?

Before attempting any document conversion, it's essential to know which formats are supported by the API. This knowledge helps you:
- Design appropriate file upload interfaces for your users
- Implement proper validation for input files
- Provide users with accurate conversion options

## Tutorial Steps

### Step 1: Set Up Your Environment

First, let's set up our development environment and initialize the necessary components to work with the GroupDocs.Conversion Cloud API.

#### Try it yourself:

1. Create a new project in your preferred IDE
2. Install the appropriate GroupDocs.Conversion Cloud SDK for your language:
   - For .NET: `Install-Package GroupDocs.Conversion-Cloud`
   - For Java: Add the dependency to your pom.xml
   - For PHP: `composer require groupdocs-conversion-cloud/groupdocs-conversion-cloud-php`
   - For Node.js: `npm install groupdocs-conversion-cloud`
   - For Python: `pip install groupdocs-conversion-cloud`
   - For Ruby: `gem install groupdocs_conversion_cloud`
   - For Go: `go get github.com/groupdocs-conversion-cloud/groupdocs-conversion-cloud-go`

3. Set up your authentication credentials:

```csharp
// C# example
string MyClientSecret = ""; // Get from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new InfoApi(configuration);
```

### Step 2: Retrieve All Supported File Formats

Now, let's retrieve all the supported file formats from the API:

#### Using cURL

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=xxxx&client_secret=xxxx" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Get supported formats
curl -X GET "https://api.groupdocs.cloud/v2.0/conversion/formats" \
-H "accept: application/json" \
-H "authorization: Bearer [Access Token]"
```

#### Using C# SDK

```csharp
try
{
    // Get supported file formats
    var response = apiInstance.GetSupportedConversionTypes(new GetSupportedConversionTypesRequest());

    Console.WriteLine("Available conversion formats:");
    foreach (var entry in response)
    {
        Console.WriteLine($"Source format: {entry.SourceFormat}");
        Console.WriteLine($"Can be converted to: {string.Join(", ", entry.TargetFormats)}");
        Console.WriteLine();
    }
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling InfoApi: " + e.Message);
}
```

### Step 3: Get Supported Formats for a Specific File Type

Sometimes you may need to know which formats a specific file type can be converted to:

#### Using cURL

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/conversion/formats?format=docx" \
-H "accept: application/json" \
-H "authorization: Bearer [Access Token]"
```

#### Using C# SDK

```csharp
try
{
    // Get supported file formats for DOCX
    var response = apiInstance.GetSupportedConversionTypes(
        new GetSupportedConversionTypesRequest(null, null, "docx"));

    foreach (var entry in response)
    {
        Console.WriteLine($"DOCX can be converted to: {string.Join(", ", entry.TargetFormats)}");
    }
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling InfoApi: " + e.Message);
}
```

### Step 4: Get Supported Formats for a Specific Document

You can also check which formats are supported for a specific document that's already in your storage:

#### Using C# SDK

```csharp
try
{
    // Get supported file formats for a specific document in storage
    var response = apiInstance.GetSupportedConversionTypes(
        new GetSupportedConversionTypesRequest("conversions/sample.docx", "MyStorage"));

    foreach (var entry in response)
    {
        Console.WriteLine($"The document can be converted to: {string.Join(", ", entry.TargetFormats)}");
    }
}
catch (Exception e)
{
    Console.WriteLine("Exception while calling InfoApi: " + e.Message);
}
```

### Step 5: Process and Utilize the Format Information

Now that you have the format information, let's implement a practical example that builds a user interface to show available conversion options:

```csharp
// Example: Building a dropdown list of target formats for a given source format
public List<string> GetTargetFormatsForSourceFormat(string sourceFormat)
{
    try
    {
        var response = apiInstance.GetSupportedConversionTypes(
            new GetSupportedConversionTypesRequest(null, null, sourceFormat));

        if (response != null && response.Count > 0)
        {
            return response[0].TargetFormats.ToList();
        }
    }
    catch (Exception e)
    {
        Console.WriteLine("Error getting formats: " + e.Message);
    }
    
    return new List<string>();
}

// Usage example
var pdfTargetFormats = GetTargetFormatsForSourceFormat("pdf");
Console.WriteLine("PDF can be converted to these formats:");
foreach (var format in pdfTargetFormats)
{
    Console.WriteLine($" - {format}");
}
```

## Troubleshooting Tips

- Authentication Errors: Make sure your Client ID and Client Secret are correct and that you're generating a valid JWT token.
- Empty Results: If you get empty results for a specific format, check if the format name is spelled correctly (e.g., "xlsx" not "excel").
- API Connection Issues: Ensure your network allows outbound connections to the GroupDocs API endpoints.

## What You've Learned

In this tutorial, you've learned how to:
- Set up your environment to work with GroupDocs.Conversion Cloud API
- Retrieve all supported file formats
- Get conversion options for specific file types
- Get conversion options for documents already in storage
- Process and utilize the format information in your applications

## Further Practice

To reinforce your learning, try these exercises:
1. Create a simple web form that dynamically updates the target format dropdown based on the selected source format.
2. Implement a function that checks if a specific conversion path is supported (e.g., DOCX → PDF).
3. Build a utility that validates user uploads against supported formats.

## Next Tutorial

Now that you understand the supported formats, proceed to our next tutorial: [Tutorial: How to Convert Documents](/quick-start-guide/convert-document) to learn how to actually perform conversions.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

We'd love to hear your feedback on this tutorial! Please feel free to ask questions or suggest improvements on our [support forum](https://forum.groupdocs.cloud/c/conversion/11).
