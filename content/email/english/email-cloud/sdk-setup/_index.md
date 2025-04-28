---
title: Getting Started with Aspose.Email Cloud SDK Tutorial
url: /email-cloud/sdk-setup/
description: Learn how to set up the Aspose.Email Cloud SDK, obtain your credentials, and make your first API call in this beginner-friendly tutorial for developers.
weight: 10
---

# Tutorial: Getting Started with Aspose.Email Cloud SDK

## Learning Objectives

In this tutorial, you'll learn how to:
- Install the Aspose.Email Cloud SDK for your preferred programming language
- Set up your client credentials for API authentication
- Create and configure an EmailCloud object
- Test your setup with a simple API call

## Prerequisites

Before starting this tutorial, you should have:
- A development environment for your chosen programming language (C#, Java, Python, Ruby, TypeScript/Node.js, or PHP)
- Basic knowledge of your chosen programming language
- An [Aspose Cloud account](https://dashboard.aspose.cloud/) (you can sign up for a free trial)

## What is Aspose.Email Cloud API?

Aspose.Email Cloud API is a comprehensive cloud-based email processing solution that allows you to work with email messages, calendars, contacts, and more from your applications. With this API, you can send, receive, append, flag, and convert cloud emails, as well as create folder structures for email archiving in the cloud.

## Step 1: Install the Aspose.Email Cloud SDK

First, let's install the SDK for your preferred programming language using your package manager:

### Try it yourself

Choose your programming language and follow the installation instructions:

{{< tabs tabTotal="6" tabID="1" tabName1="C#" tabName2="Java" tabName3="Python" tabName4="Ruby" tabName5="TypeScript/Node.js" tabName6="PHP" >}}

{{< tab tabNum="1" >}}

```csharp
// Using Package Manager
PM> Install-Package Aspose.Email-Cloud 

// Or using .NET CLI
dotnet add package Aspose.Email-Cloud

// Or by adding a Package reference in your .csproj file
<PackageReference Include="Aspose.Email-Cloud"/>
```

{{< /tab >}}

{{< tab tabNum="2" >}}

First, add a repository to your pom.xml:

```xml
<repository>
    <id>aspose-cloud</id>
    <name>Aspose.Cloud repository</name>
    <url>https://repository.aspose.cloud/repo</url>
</repository>
```

Then, add a dependency:

```xml
<dependency>
    <groupId>com.aspose</groupId>
    <artifactId>aspose-email-cloud</artifactId>
</dependency>
```

{{< /tab >}}

{{< tab tabNum="3" >}}

Use pip to install the SDK:

```python
pip install aspose-email-cloud
```

{{< /tab >}}

{{< tab tabNum="4" >}}

Use gem to install the SDK:

```ruby
gem install aspose_email_cloud
```

{{< /tab >}}

{{< tab tabNum="5" >}}

Use npm to install the SDK:

```bash
npm install @asposecloud/aspose-email-cloud --save
```

{{< /tab >}}

{{< tab tabNum="6" >}}

Use Composer to install the SDK:

```bash
composer require aspose/aspose-email-cloud
```

{{< /tab >}}

{{< /tabs >}}

## Step 2: Obtain Your Client ID and Client Secret

Your Client ID and Client Secret act as your username and password for authenticating with the Aspose.Email Cloud API. These credentials are used to obtain a temporary JWT token, which provides access to the API methods.

### Try it yourself

Follow these steps to get your credentials:

1. Open the [Aspose.Cloud Dashboard](https://dashboard.aspose.cloud/)
2. Sign in or sign up for an account
3. Navigate to the [Applications page](https://dashboard.aspose.cloud/applications)
4. Use the existing "First App" or create a new application
5. Note down your Client ID and Client Secret

Learning Checkpoint: Why do we need Client ID and Client Secret? 
- These credentials authenticate your application with the Aspose.Email Cloud API
- They are used to obtain a temporary JWT token for API access
- The SDK automatically handles token renewal when it expires

## Step 3: Set Up the EmailCloud Object

The EmailCloud class is the central point for accessing all API methods. To initialize this object, you need to provide your Client ID and Client Secret.

### Try it yourself

Create an instance of the EmailCloud class in your preferred programming language:

{{< tabs tabTotal="6" tabID="2" tabName1="C#" tabName2="Java" tabName3="Python" tabName4="Ruby" tabName5="TypeScript/Node.js" tabName6="PHP" >}}

{{< tab tabNum="1" >}}

```csharp
using Aspose.Email.Cloud.Sdk.Api; // EmailCloud class is here
using Aspose.Email.Cloud.Sdk.Model; // REST API models are here

// Replace these with your actual credentials
var clientSecret = "Your Client Secret";
var clientId = "Your Client ID";

// Create the EmailCloud instance
var api = new EmailCloud(clientSecret, clientId);
```

{{< /tab >}}

{{< tab tabNum="2" >}}

```java
import com.aspose.email.cloud.sdk.invoker.ApiException;
import com.aspose.email.cloud.sdk.model.*;
import com.aspose.email.cloud.sdk.api.*;

// Replace these with your actual credentials
String clientSecret = "Your Client Secret";
String clientId = "Your Client ID";

// Create the EmailCloud instance
EmailCloud api = new EmailCloud(clientSecret, clientId);
```

{{< /tab >}}

{{< tab tabNum="3" >}}

```python
from AsposeEmailCloudSdk import api, models

# Replace these with your actual credentials
client_secret = "Your Client Secret"
client_id = "Your Client ID"

# Create the EmailCloud instance
api = api.EmailCloud(client_id, client_secret)
```

{{< /tab >}}

{{< tab tabNum="4" >}}

```ruby
require 'aspose-email-cloud'
include AsposeEmailCloud

# Replace these with your actual credentials
client_secret = "Your Client Secret"
client_id = "Your Client ID"

# Create the EmailCloud instance
@api = EmailCloud.new(client_secret, client_id)
```

{{< /tab >}}

{{< tab tabNum="5" >}}

```javascript
import * as email from "@asposecloud/aspose-email-cloud";

// Replace these with your actual credentials
const clientSecret = "Your Client Secret";
const clientId = "Your Client ID";

// Create the EmailCloud instance
const api = new email.EmailCloud(clientId, clientSecret);
```

{{< /tab >}}

{{< tab tabNum="6" >}}

```php
use Aspose\Email\EmailCloud;
use Aspose\Email\Configuration;
use Aspose\Email\Model;

// Replace these with your actual credentials
$clientSecret = "Your Client Secret";
$clientId = "Your Client ID";

// Create and configure the EmailCloud instance
$configuration = new Configuration();
$configuration
    ->setClientSecret($clientSecret)
    ->setClientId($clientId);

$api = new EmailCloud($configuration);
```

{{< /tab >}}

{{< /tabs >}}

Note: The SDK automatically handles obtaining and refreshing your JWT token using the provided credentials. You don't need to manage tokens manually.

## Step 4: Test Your Setup

Let's test your setup by performing a simple operation - converting an EML file to MSG format.

### Try it yourself

First, ensure you have an EML file available (you can create a simple one in most email clients). Then, implement the following code:

{{< tabs tabTotal="6" tabID="3" tabName1="C#" tabName2="Java" tabName3="Python" tabName4="Ruby" tabName5="TypeScript/Node.js" tabName6="PHP" >}}

{{< tab tabNum="1" >}}

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Aspose.Email.Cloud.Sdk.Api;
using Aspose.Email.Cloud.Sdk.Model;

class Program
{
    static async Task Main(string[] args)
    {
        // Create the EmailCloud instance (as shown in Step 3)
        var clientSecret = "Your Client Secret";
        var clientId = "Your Client ID";
        var api = new EmailCloud(clientSecret, clientId);
        
        try
        {
            // Path to your EML file
            var emlFilePath = "path/to/your/email.eml";
            
            // Convert EML to MSG
            using(var emlStream = File.OpenRead(emlFilePath))
            {
                var result = await api.Email.ConvertAsync(new EmailConvertRequest(
                    "Eml", "Msg", emlStream));
                
                // Save the converted file
                using(var msgStream = File.Create("converted_email.msg"))
                {
                    result.CopyTo(msgStream);
                }
                
                Console.WriteLine("Conversion successful! The MSG file has been saved as 'converted_email.msg'");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
```

{{< /tab >}}

{{< tab tabNum="2" >}}

```java
import com.aspose.email.cloud.sdk.api.EmailCloud;
import com.aspose.email.cloud.sdk.model.EmailConvertRequest;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import org.apache.commons.io.IOUtils;

public class EmailCloudTest {
    public static void main(String[] args) {
        try {
            // Create the EmailCloud instance (as shown in Step 3)
            String clientSecret = "Your Client Secret";
            String clientId = "Your Client ID";
            EmailCloud api = new EmailCloud(clientSecret, clientId);
            
            // Path to your EML file
            String emlFilePath = "path/to/your/email.eml";
            
            // Convert EML to MSG
            byte[] fileToConvert = IOUtils.toByteArray(new FileInputStream(emlFilePath));
            byte[] result = api.email().convert(new EmailConvertRequest(
                "Eml", "Msg", fileToConvert));
                
            // Save the converted file
            FileOutputStream outputStream = new FileOutputStream("converted_email.msg");
            outputStream.write(result);
            outputStream.close();
            
            System.out.println("Conversion successful! The MSG file has been saved as 'converted_email.msg'");
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

{{< /tab >}}

{{< tab tabNum="3" >}}

```python
from AsposeEmailCloudSdk import api, models
import os

try:
    # Create the EmailCloud instance (as shown in Step 3)
    client_secret = "Your Client Secret"
    client_id = "Your Client ID"
    api_instance = api.EmailCloud(client_id, client_secret)
    
    # Path to your EML file
    eml_file_path = "path/to/your/email.eml"
    
    # Convert EML to MSG
    result = api_instance.email.convert(
        models.EmailConvertRequest('Eml', 'Msg', eml_file_path))
    
    # Save the converted file
    with open("converted_email.msg", "wb") as file:
        file.write(result)
    
    print("Conversion successful! The MSG file has been saved as 'converted_email.msg'")
except Exception as e:
    print(f"Error: {str(e)}")
```

{{< /tab >}}

{{< tab tabNum="4" >}}

```ruby
require 'aspose-email-cloud'
include AsposeEmailCloud

begin
  # Create the EmailCloud instance (as shown in Step 3)
  client_secret = "Your Client Secret"
  client_id = "Your Client ID"
  api = EmailCloud.new(client_secret, client_id)
  
  # Path to your EML file
  eml_file_path = "path/to/your/email.eml"
  
  # Convert EML to MSG
  result = api.email.convert(EmailConvertRequest.new(
    from_format: 'Eml', 
    to_format: 'Msg', 
    file: File.new(eml_file_path)))
  
  # Save the converted file
  File.open("converted_email.msg", "wb") do |file|
    file.write(result)
  end
  
  puts "Conversion successful! The MSG file has been saved as 'converted_email.msg'"
rescue => e
  puts "Error: #{e.message}"
end
```

{{< /tab >}}

{{< tab tabNum="5" >}}

```javascript
import * as email from "@asposecloud/aspose-email-cloud";
import * as fs from "fs";

// Create the EmailCloud instance (as shown in Step 3)
const clientSecret = "Your Client Secret";
const clientId = "Your Client ID";
const api = new email.EmailCloud(clientId, clientSecret);

// Path to your EML file
const emlFilePath = "path/to/your/email.eml";

// Convert EML to MSG
try {
  const emlFile = fs.readFileSync(emlFilePath);
  api.email.convert(new email.EmailConvertRequest(
    'Eml', 'Msg', emlFile))
    .then((result) => {
      // Save the converted file
      fs.writeFileSync("converted_email.msg", result);
      console.log("Conversion successful! The MSG file has been saved as 'converted_email.msg'");
    })
    .catch((error) => {
      console.log("Error:", error.message);
    });
} catch (error) {
  console.log("Error:", error.message);
}
```

{{< /tab >}}

{{< tab tabNum="6" >}}

```php
<?php
require_once "vendor/autoload.php";

use Aspose\Email\EmailCloud;
use Aspose\Email\Configuration;
use Aspose\Email\Model\EmailConvertRequest;

try {
    // Create the EmailCloud instance (as shown in Step 3)
    $clientSecret = "Your Client Secret";
    $clientId = "Your Client ID";
    
    $configuration = new Configuration();
    $configuration
        ->setClientSecret($clientSecret)
        ->setClientId($clientId);
    
    $api = new EmailCloud($configuration);
    
    // Path to your EML file
    $emlFilePath = "path/to/your/email.eml";
    
    // Convert EML to MSG
    $result = $api->email()->convert(new EmailConvertRequest(
        'Eml', 'Msg', $emlFilePath));
    
    // Save the converted file
    $fileContent = $result->fread($result->getSize());
    file_put_contents("converted_email.msg", $fileContent);
    
    echo "Conversion successful! The MSG file has been saved as 'converted_email.msg'";
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

{{< /tab >}}

{{< /tabs >}}

## Troubleshooting Tips

If you encounter issues during setup or when making API calls, here are some common problems and their solutions:

1. Authentication Failed
   - Double-check your Client ID and Client Secret for typos
   - Ensure your account subscription is active

2. FileNotFoundException
   - Verify the file path is correct
   - Check file permissions

3. Request Timeout
   - Check your internet connection
   - Consider increasing the client timeout settings

## What You've Learned

In this tutorial, you've learned how to:
- Install the Aspose.Email Cloud SDK for your preferred programming language
- Obtain and understand the purpose of your Client ID and Client Secret
- Create and configure an EmailCloud object
- Test your setup with a simple file conversion operation

## Further Practice

To reinforce your learning, try these exercises:
1. Use the SDK to get a list of files from your storage
2. Try converting an email file to a different format (e.g., HTML)
3. Create a simple email message object and save it to your storage

## Next Tutorial

Ready to continue your learning journey? Check out our next tutorial on [Working with VCard API](/email-cloud/vcard-api/) to learn how to manage contact information using Aspose.Email Cloud.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/email/)
- [Documentation](https://docs.aspose.cloud/email/)
- [Live Demo](https://products.aspose.app/email/family)
- [API Reference](https://reference.aspose.cloud/email/)
- [Blog](https://blog.aspose.cloud/category/email/)
- [Free Support](https://forum.aspose.cloud/c/email/9/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out to us on the [Aspose.Email Cloud forum](https://forum.aspose.cloud/c/email/9/).