---
title: Generating OMR Forms with Aspose.OMR Cloud SDK Tutorial
weight: 40
url: /generate-form/using-sdk/
description: Learn how to use the Aspose.OMR Cloud SDK to easily integrate form generation functionality into your applications in this step-by-step tutorial.
---

# Tutorial: Generating OMR Forms with Aspose.OMR Cloud SDK

## Learning Objectives

In this tutorial, you'll learn how to:
- Set up and configure the Aspose.OMR Cloud SDK
- Generate machine-readable forms using the SDK
- Include images in your forms
- Configure form layout and appearance settings
- Save the generated form and recognition pattern

## Prerequisites

Before starting this tutorial, you should:
- Have an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps)
- Know your Client ID and Client Secret from the Aspose Cloud Dashboard
- Have a basic understanding of the programming language you'll be using (.NET for this tutorial)
- Have a development environment set up for your chosen language

## Introduction

While you can directly interact with the Aspose.OMR Cloud REST API, using the SDK (Software Development Kit) makes integration much easier. In this tutorial, we'll explore how to use the Aspose.OMR Cloud SDK to generate forms in your applications, allowing you to focus on business logic rather than technical implementation details.

## Practical Scenario

You're building a school management system that needs to generate customized test forms for different subjects and classes. Instead of manually calling the REST API endpoints, you want to streamline the process by integrating the Aspose.OMR Cloud SDK directly into your application.

## Step 1: Install the SDK

For .NET applications, you can install the Aspose.OMR Cloud SDK using NuGet Package Manager:

```bash
Install-Package Aspose.Omr.Cloud.Sdk
```


> Try it yourself: Install the SDK for your preferred programming language and verify that it's correctly added to your project.

## Step 2: Set Up Authentication

Before using the SDK, you need to configure it with your authentication credentials:

```csharp
using Aspose.Omr.Cloud.Sdk.Api;
using Aspose.Omr.Cloud.Sdk.Model;

// Authorize your requests to Aspose.OMR Cloud API
GenerateTemplateApi api = new GenerateTemplateApi();
api.Configuration.ClientID = "<Your Client Id>";
api.Configuration.ClientSecret = "<Your Client Secret>";
```

Replace `<Your Client Id>` and `<Your Client Secret>` with the actual values from your Aspose Cloud Dashboard.

> Tip: Keep your credentials secure. Consider storing them in environment variables or a secure configuration file instead of hardcoding them in your application.

## Step 3: Read the Form Source Code

Next, you need to read the form source code from a file or another source:

```csharp
// Read form's source code
byte[] source = File.ReadAllBytes("source.txt");
```

## Step 4: Include Images (If Needed)

If your form includes images, you'll need to load them and add them to a dictionary:

```csharp
// Load images used on the form
Dictionary<string, byte[]> images = new Dictionary<string, byte[]>();
byte[] logoData = File.ReadAllBytes("logo.png");
images.Add("logo.png", logoData);
```

The key in the dictionary should match the image filename referenced in your source code.

> Note: Make sure the image path in your source code matches exactly the key in the dictionary.

## Step 5: Configure Form Settings

Set up the page settings for your form's design and layout:

```csharp
// Configure form design and layout
PageSettings settings = new PageSettings {
    BubbleColor = Color.Indigo,
    PaperSize = PaperSize.Legal,
    Orientation = Orientation.Vertical,
    FontFamily = "Arial",
    FontSize = 12,
    FontStyle = FontStyle.Regular,
    PageMarginLeft = 50,
    BubbleSize = BubbleSize.Normal
};
```

These settings control the overall appearance of your form, including:
- Paper size (A4, Letter, Legal)
- Orientation (Horizontal/landscape or Vertical/portrait)
- Font family, size, and style
- Bubble color and size
- Page margins

> Learning checkpoint: What settings would you adjust to create forms optimized for younger students? Consider bubble size, font size, and other parameters.

## Step 6: Generate the Form

Now you can generate the printable form and recognition pattern:

```csharp
// Generate printable form and recognition pattern
OmrGenerateTask task = new OmrGenerateTask(source, settings, images);
string taskID = api.PostGenerateTemplate(task);
```

The `PostGenerateTemplate` method sends the form source code, settings, and images to the Aspose.OMR Cloud API and returns a task ID.

## Step 7: Retrieve and Save the Generated Form

Finally, use the task ID to retrieve the generated form and save it:

```csharp
// Save printable form and recognition pattern
OMRResponse result = api.GetGenerateTemplate(taskID);
foreach(var item in result.Results)
{
    string fileName = $"form.{item.Type.ToLower()}";
    File.WriteAllBytes(fileName, item.Data);
}
```

This code saves both the printable form (PNG) and the recognition pattern (OMR) to files.

## Complete Example (.NET)

Here's a complete example that puts all the steps together:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Aspose.Omr.Cloud.Sdk.Api;
using Aspose.Omr.Cloud.Sdk.Model;

namespace AsposeMathExamGenerator
{
    internal class Program
    {
        static void Main(string[] args)
        {
            try
            {
                // Step 1: Authorize your requests to Aspose.OMR Cloud API
                Console.WriteLine("Configuring API client...");
                GenerateTemplateApi api = new GenerateTemplateApi();
                api.Configuration.ClientID = "YOUR_CLIENT_ID";
                api.Configuration.ClientSecret = "YOUR_CLIENT_SECRET";
                
                // Step 2: Read form's source code
                Console.WriteLine("Loading source code...");
                byte[] source = File.ReadAllBytes("math_exam.txt");
                
                // Step 3: Load images used on the form
                Console.WriteLine("Loading images...");
                Dictionary<string, byte[]> images = new Dictionary<string, byte[]>();
                byte[] logoData = File.ReadAllBytes("school_logo.png");
                images.Add("school_logo.png", logoData);
                
                // Step 4: Configure form design and layout
                Console.WriteLine("Configuring form settings...");
                PageSettings settings = new PageSettings {
                    BubbleColor = Color.Blue,
                    PaperSize = PaperSize.Letter,
                    Orientation = Orientation.Vertical,
                    FontFamily = "Arial",
                    FontSize = 12,
                    BubbleSize = BubbleSize.Large
                };
                
                // Step 5: Generate printable form and recognition pattern
                Console.WriteLine("Generating form...");
                OmrGenerateTask task = new OmrGenerateTask(source, settings, images);
                string taskID = api.PostGenerateTemplate(task);
                Console.WriteLine($"Form generation task ID: {taskID}");
                
                // Step 6: Save printable form and recognition pattern
                Console.WriteLine("Retrieving and saving generated form...");
                OMRResponse result = api.GetGenerateTemplate(taskID);
                
                foreach(var item in result.Results)
                {
                    string extension = item.Type.ToLower();
                    string fileName = $"math_exam.{extension}";
                    File.WriteAllBytes(fileName, item.Data);
                    Console.WriteLine($"Saved: {fileName}");
                }
                
                Console.WriteLine("Form generation completed successfully!");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error generating form: {ex.Message}");
            }
        }
    }
}
```

## Troubleshooting Tips

Here are some common issues and their solutions:

1. Authentication Failures
   - Verify your Client ID and Client Secret are correct
   - Check that your account has an active subscription
   - Ensure your account has API access permissions

2. Image Loading Issues
   - Make sure image paths match exactly what's referenced in the source code
   - Verify that images are in supported formats (PNG, JPG)
   - Check that image files exist and are accessible

3. Form Generation Errors
   - Check the API response for specific error messages
   - Validate your source code syntax
   - Ensure all referenced images are included in the images dictionary

> Tip: Enable detailed logging in your application to help diagnose issues with API calls.

## What You've Learned

In this tutorial, you learned how to:
- Set up and configure the Aspose.OMR Cloud SDK
- Generate machine-readable forms using code instead of direct API calls
- Include images in your forms
- Configure form layout and appearance settings
- Retrieve and save the generated form and recognition pattern

## Further Practice

Try these exercises to reinforce your learning:

1. Modify the example to generate a multi-page form with different question types
2. Create a form with custom bubble colors and sizes for different question sections
3. Build a simple UI that allows users to select form options and generate forms on demand
4. Add error handling and retry logic for API calls

## Next Steps

Now that you know how to use the SDK for basic form generation, you might want to explore [creating complex multi-page forms](/generate-form/complex-forms/) with various elements.

## Additional Resources

- [Product Page](https://products.aspose.cloud/omr/)
- [Documentation](https://docs.aspose.cloud/omr/)
- [Live Demo](https://products.aspose.app/omr/family)
- [API Reference](https://reference.aspose.cloud/omr/)
- [Blog](https://blog.aspose.cloud/category/omr/)
- [Free Support](https://forum.aspose.cloud/c/omr/8/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [Aspose.OMR Cloud SDK GitHub Repository](https://github.com/aspose-omr-cloud/aspose-omr-cloud-dotnet)

Have questions about this tutorial? Feel free to reach out on our [support forum](https://forum.aspose.cloud/c/omr/8/).