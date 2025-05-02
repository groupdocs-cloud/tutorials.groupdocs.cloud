---
title: Converting Email Documents with Load Options in GroupDocs.Conversion Cloud API Tutorial
url: /advanced-features/email-conversion-options/
description: Learn how to convert email messages and storage files to various formats with customization options using GroupDocs.Conversion Cloud API
weight: 130
---

# Tutorial: Converting Email Documents with Load Options

In this tutorial, you'll learn how to convert email messages (MSG, EML) and storage files (PST, OST) to other formats using GroupDocs.Conversion Cloud API. You'll master specialized email conversion options including field formatting, address display, and attachment handling.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Convert email messages to PDF, HTML, and other formats
- Customize how email fields and metadata are displayed
- Control email address visibility and formatting
- Handle embedded images and attachments during conversion
- Implement both storage-based and stream-based email conversions
- Troubleshoot common email conversion challenges

## Prerequisites

Before starting this tutorial, you need:
1. A [GroupDocs.Conversion Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Development environment with your preferred programming language set up
5. Sample email files to test conversion (MSG, EML, PST formats)

## Implementation Steps

### Step 1: Authentication with GroupDocs.Conversion Cloud API

Before performing any operations, we need to authenticate with the API using your Client ID and Client Secret.

#### Try it yourself

First, let's obtain a JWT access token using cURL:

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Make sure to replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

### Step 2: Basic Email to PDF Conversion

Let's start with a simple conversion from an MSG file to PDF format:

#### Try it yourself

Using cURL:

```bash
curl -X POST "https://api.groupdocs.cloud/v2.0/conversion" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: application/json" \
-d "{  
      'FilePath': 'emails/sample.msg',  
      'Format': 'pdf',  
      'OutputPath': 'converted'
    }"
```

Replace `YOUR_JWT_TOKEN` with the actual token received in Step 1.

### Step 3: Converting Email to PDF with Field Display Options

Now let's implement a comprehensive example that converts an email to PDF with specific field display options:

```csharp
// C# SDK Example
using System;
using System.Collections.Generic;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace EmailConversionTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API client
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            var apiInstance = new ConvertApi(configuration);

            try
            {
                // Set up conversion from Email to PDF with field display options
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "emails/business_message.msg",
                    Format = "pdf",
                    // Email-specific load options
                    LoadOptions = new EmailLoadOptions()
                    {
                        // Header display options
                        DisplayHeader = true,
                        DisplayFrom = true,
                        DisplayTo = true,
                        DisplayCc = true,
                        DisplayBcc = false,  // Hide BCC recipients
                        DisplaySubject = true,
                        
                        // Email address display options
                        DisplayEmailAddress = true,  // Show email addresses
                        DisplayFromEmailAddress = true,
                        DisplayToEmailAddress = true,
                        DisplayCcEmailAddress = true,
                        DisplayBccEmailAddress = false,
                        
                        // Time zone and date format options
                        TimeZoneOffset = "-04:00",  // Eastern Time
                        ConvertAttachments = true,  // Convert attachments
                        
                        // Preserve embedded message format if there are embedded emails
                        PreserveEmbeddedMessageFormat = true,
                        
                        // Field labels customization
                        FieldLabels = new List<FieldLabel>()
                        {
                            new FieldLabel() { Field = "From", Label = "Sender" },
                            new FieldLabel() { Field = "To", Label = "Recipients" },
                            new FieldLabel() { Field = "Subject", Label = "Email Topic" }
                        }
                    },
                    // PDF-specific convert options
                    ConvertOptions = new PdfConvertOptions()
                    {
                        Dpi = 300,
                        Width = 1024,
                        Height = 800
                    },
                    OutputPath = "converted"
                };

                // Execute conversion
                List<StoredConvertedResult> response = apiInstance.ConvertDocument(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("Email converted successfully to PDF: " + response[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 4: Converting Email to HTML Format

Email to HTML conversion is useful for web applications:

```java
// Java SDK Example
import com.groupdocs.cloud.conversion.api.*;
import com.groupdocs.cloud.conversion.client.ApiException;
import com.groupdocs.cloud.conversion.model.*;
import com.groupdocs.cloud.conversion.model.requests.*;
import java.util.List;
import java.util.ArrayList;

public class EmailToHtmlExample {
    public static void main(String[] args) {
        // Configure API client
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        Configuration configuration = new Configuration(clientId, clientSecret);
        ConvertApi apiInstance = new ConvertApi(configuration);
        
        try {
            // Prepare convert settings for Email to HTML
            ConvertSettings settings = new ConvertSettings();
            settings.setFilePath("emails/newsletter.eml");
            settings.setFormat("html");
            
            // Set Email-specific load options
            EmailLoadOptions loadOptions = new EmailLoadOptions();
            loadOptions.setDisplayHeader(true);
            loadOptions.setDisplayFrom(true);
            loadOptions.setDisplayTo(true);
            loadOptions.setDisplayCc(true);
            loadOptions.setDisplaySubject(true);
            
            // Preserve formatting of HTML content in the email
            loadOptions.setPreserveOriginalHtml(true);
                        
            // Add custom field labels
            List<FieldLabel> fieldLabels = new ArrayList<FieldLabel>();
            FieldLabel fromLabel = new FieldLabel();
            fromLabel.setField("From");
            fromLabel.setLabel("From");
            fieldLabels.add(fromLabel);
            
            FieldLabel toLabel = new FieldLabel();
            toLabel.setField("To");
            toLabel.setLabel("To");
            fieldLabels.add(toLabel);
            
            loadOptions.setFieldLabels(fieldLabels);
            
            settings.setLoadOptions(loadOptions);
            
            // Configure HTML-specific convert options
            WebConvertOptions convertOptions = new WebConvertOptions();
            convertOptions.setFixedLayout(false);  // Fluid layout for responsive design
            convertOptions.setResourcesEmbedded(true);  // Embed images and resources
            
            settings.setConvertOptions(convertOptions);
            settings.setOutputPath("converted");
            
            // Execute conversion
            List<StoredConvertedResult> result = apiInstance.convertDocument(
                new ConvertDocumentRequest(settings));
            
            System.out.println("Email converted successfully to HTML: " + result.get(0).getUrl());
        } catch (ApiException e) {
            System.err.println("Exception when calling ConvertApi: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Step 5: Converting Email Archive Files (PST/OST)

For handling email archives like PST files, you need to access specific folders:

```python
# Python SDK Example
import groupdocs_conversion_cloud
from groupdocs_conversion_cloud.models.requests import ConvertDocumentRequest

# Configure API client
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"
api_instance = groupdocs_conversion_cloud.ConvertApi.from_keys(client_id, client_secret)

try:
    # Prepare conversion settings
    settings = groupdocs_conversion_cloud.ConvertSettings()
    settings.file_path = "emails/archive.pst"
    settings.format = "pdf"
    
    # Configure PersonalStorage-specific load options
    load_options = groupdocs_conversion_cloud.PersonalStorageLoadOptions()
    load_options.folder = "Inbox"  # Specify which folder to convert
    load_options.depth = 2         # Recursion depth for subfolders
    
    settings.load_options = load_options
    
    # Configure PDF-specific convert options
    convert_options = groupdocs_conversion_cloud.PdfConvertOptions()
    convert_options.dpi = 300
    
    settings.convert_options = convert_options
    settings.output_path = "converted"
    
    # Execute conversion
    request = ConvertDocumentRequest(settings)
    result = api_instance.convert_document(request)
    
    print(f"Email archive converted successfully to PDF: {len(result)} files created")
    for i, file in enumerate(result):
        print(f" - {i+1}: {file.name} ({file.size} bytes)")
except groupdocs_conversion_cloud.ApiException as e:
    print(f"Exception when calling ConvertApi: {e}")
```

### Step 6: Stream-Based Email Conversion

For applications that need to process the converted email directly:

```javascript
// Node.js SDK Example
const { ConvertApi, Configuration } = require("groupdocs-conversion-cloud");
const fs = require("fs");

// Configure API client
const clientId = "YOUR_CLIENT_ID";
const clientSecret = "YOUR_CLIENT_SECRET";
const config = new Configuration(clientId, clientSecret);
const apiInstance = new ConvertApi(config);

// Prepare conversion settings
const settings = {
    filePath: "emails/sample.msg",
    format: "pdf",
    loadOptions: {
        // Email-specific load options
        displayHeader: true,
        displayFrom: true,
        displayTo: true,
        displayCc: true,
        displaySubject: true,
        
        displayEmailAddress: true,
        preserveOriginalHtml: true,
        convertAttachments: false  // Don't convert attachments
    },
    convertOptions: {
        // PDF-specific convert options
        dpi: 300
    },
    // Set outputPath to null for stream output
    outputPath: null
};

// Execute conversion
apiInstance.convertDocumentDownload({ convertSettings: settings })
    .then((result) => {
        // Save the stream to a file
        const fileName = "./converted-email.pdf";
        const writeStream = fs.createWriteStream(fileName);
        
        result.pipe(writeStream);
        
        writeStream.on("finish", () => {
            console.log(`Email document converted and saved to ${fileName}`);
        });
    })
    .catch((error) => {
        console.log(`Error: ${error.message}`);
    });
```

## Email-Specific Load Options

When converting email documents, you can leverage these specialized options:

| Option | Description | Default | Impact |
|--------|-------------|---------|--------|
| DisplayHeader | Show email header | true | Controls header visibility |
| DisplayFrom | Show From field | true | Controls From field visibility |
| DisplayTo | Show To field | true | Controls To field visibility |
| DisplayCc | Show Cc field | true | Controls Cc field visibility |
| DisplayBcc | Show Bcc field | true | Controls Bcc field visibility |
| DisplaySubject | Show Subject field | true | Controls Subject field visibility |
| DisplayEmailAddress | Show email addresses | true | Controls whether addresses are shown |
| TimeZoneOffset | Time zone for dates | System default | Affects how dates are displayed |
| ConvertAttachments | Process attachments | false | Controls attachment handling |
| PreserveEmbeddedMessageFormat | Keep format for embedded emails | false | Affects nested email display |
| PreserveOriginalHtml | Keep original HTML formatting | false | Affects HTML content display |
| FieldLabels | Custom labels for fields | Standard labels | Changes field labels in output |

## Email Storage Load Options

For PST/OST files, these additional options are available:

| Option | Description | Default | Impact |
|--------|-------------|---------|--------|
| Folder | Folder to process | Root folder | Controls which emails are converted |
| Depth | Subfolder recursion depth | 0 (no recursion) | Controls subfolder handling |

## Troubleshooting Common Issues

### 1. Email Field Display Problems

If email fields aren't displaying correctly:
- Check the corresponding DisplayXxx options (DisplayFrom, DisplayTo, etc.)
- Verify that you're setting the correct options for your needs
- For custom field labels, ensure the Field property matches a valid email field

### 2. Address Format Issues

If email addresses aren't showing as expected:
- Adjust the DisplayEmailAddress and related options
- For privacy, you can hide email addresses by setting these options to false
- Consider customizing the field labels for clearer presentation

### 3. HTML Content Preservation

For emails with rich HTML content:
- Set PreserveOriginalHtml to true to maintain the original formatting
- When converting to HTML format, ensure ResourcesEmbedded is set to include images
- If HTML content looks broken, try different conversion formats (PDF often works best)

### 4. Attachment Handling

When dealing with email attachments:
- Set ConvertAttachments to true if you want attachments included in the output
- Be aware that large attachments can significantly increase processing time
- For PST files with many emails, consider processing folders separately

## What You've Learned

In this tutorial, you've learned:
- How to convert email messages and storage files to various formats
- Customizing how email fields and metadata are displayed
- Controlling email address visibility and formatting
- Handling embedded images, HTML content, and attachments
- Implementing both storage-based and stream-based email conversions
- Troubleshooting common email conversion challenges

## Further Practice

To reinforce your learning, try these exercises:
1. Create an email archiving system that converts messages to PDF for storage
2. Implement a batch conversion utility that processes multiple email files with consistent formatting
3. Build a web application that allows users to convert email messages while controlling privacy settings
4. Create a report generator that extracts specific information from email archives

## Additional Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [Live Demo](https://products.groupdocs.app/conversion/family)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.conversion-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [forum](https://forum.groupdocs.cloud/c/conversion/11) for support.
