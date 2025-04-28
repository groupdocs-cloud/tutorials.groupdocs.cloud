---
title: How to Work with VCard API Tutorial
url: /email-cloud/vcard-api/
description: Learn how to create, edit, and process contact card files using Aspose.Email Cloud API. Master business card recognition and VCard management in this tutorial.
weight: 50
---

# Tutorial: How to Work with VCard API

## Learning Objectives

In this tutorial, you'll learn how to:
- Create and manage contact information using the VCard API
- Save VCard files to cloud storage and download them
- Convert ContactDto objects to VCard format and vice versa
- Extract contact information from business card images using AI recognition
- Retrieve lists of VCard files from cloud storage

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [SDK Setup tutorial](/email-cloud/sdk-setup/)
- Obtained your Client ID and Client Secret from the Aspose Cloud Dashboard
- Basic understanding of contact management concepts
- A development environment for your chosen programming language

## What is VCard API?

The VCard API, part of Aspose.Email Cloud, allows you to work with contact card files (.vcf) programmatically. You can create, read, edit, and save contact information, as well as extract contact details from business card images using AI recognition technology.

## Step 1: Create a Contact Object and Save it to Storage

Let's start by creating a simple contact (ContactDto object) and saving it to cloud storage as a VCard (.vcf) file.

### Try it yourself

{{< tabs tabTotal="6" tabID="1" tabName1="C#" tabName2="Java" tabName3="Python" tabName4="Ruby" tabName5="TypeScript/Node.js" tabName6="PHP" >}}

{{< tab tabNum="1" >}}

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Aspose.Email.Cloud.Sdk.Api;
using Aspose.Email.Cloud.Sdk.Model;

class Program
{
    static async Task Main(string[] args)
    {
        // Create the EmailCloud instance (replace with your credentials)
        var clientSecret = "Your Client Secret";
        var clientId = "Your Client ID";
        var api = new EmailCloud(clientSecret, clientId);

        try
        {
            // Create a ContactDto object with basic information
            var contact = new ContactDto
            {
                Gender = "Male", 
                Surname = "Thomas", 
                GivenName = "Alex",
                EmailAddresses = new List<EmailAddress>
                {
                    new EmailAddress
                    {
                        Category = new EnumWithCustomOfEmailAddressCategory("Work", null),
                        Address = "alex.thomas@work.com", 
                        Preferred = true,
                        DisplayName = "Alex Thomas"
                    }
                },
                PhoneNumbers = new List<PhoneNumber>
                {
                    new PhoneNumber
                    {
                        Category = new EnumWithCustomOfPhoneNumberCategory("Work", null),
                        Number = "+49211424721", 
                        Preferred = true
                    }
                }
            };

            // Define storage location
            var storage = "First Storage"; // Name of your storage
            var folder = "Contacts"; // Folder path in storage
            var contactFile = "alex_thomas.vcf"; // Filename for the VCard

            // Save the ContactDto as a VCard file to storage
            await api.Contact.SaveAsync(
                new ContactSaveRequest(
                    new StorageFileLocation(storage, folder, contactFile),
                    contact, "VCard"));

            Console.WriteLine($"Contact successfully saved as {contactFile} in {folder}");
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
import com.aspose.email.cloud.sdk.model.*;

public class VCardExample {
    public static void main(String[] args) {
        try {
            // Create the EmailCloud instance (replace with your credentials)
            String clientSecret = "Your Client Secret";
            String clientId = "Your Client ID";
            EmailCloud api = new EmailCloud(clientSecret, clientId);

            // Create a ContactDto object with basic information
            ContactDto contact = new ContactDto()
                .gender("Male")
                .surname("Thomas")
                .givenName("Alex")
                .addEmailAddressesItem(new EmailAddress(
                    new EnumWithCustomOfEmailAddressCategory("Work", null),
                    "Alex Thomas", true, null, "alex.thomas@work.com"))
                .addPhoneNumbersItem(new PhoneNumber(
                    new EnumWithCustomOfPhoneNumberCategory("Work", null),
                    "+49211424721", true));

            // Define storage location
            String storage = "First Storage"; // Name of your storage
            String folder = "Contacts"; // Folder path in storage
            String contactFile = "alex_thomas.vcf"; // Filename for the VCard

            // Save the ContactDto as a VCard file to storage
            api.contact().save(
                new ContactSaveRequest(
                    new StorageFileLocation(storage, folder, contactFile),
                    contact, "VCard"));

            System.out.println("Contact successfully saved as " + contactFile + " in " + folder);
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

try:
    # Create the EmailCloud instance (replace with your credentials)
    client_secret = "Your Client Secret"
    client_id = "Your Client ID"
    api_instance = api.EmailCloud(client_id, client_secret)
    
    # Create a ContactDto object with basic information
    contact = models.ContactDto(
        gender='Male',
        surname='Thomas',
        given_name='Alex',
        email_addresses=[
            models.EmailAddress(
                models.EnumWithCustomOfEmailAddressCategory('Work'),
                'Alex Thomas', True, address='alex.thomas@work.com')],
        phone_numbers=[
            models.PhoneNumber(
                models.EnumWithCustomOfPhoneNumberCategory('Work'),
                '+49211424721', True)])
    
    # Define storage location
    storage = 'First Storage'  # Name of your storage
    folder = 'Contacts'  # Folder path in storage
    contact_file = 'alex_thomas.vcf'  # Filename for the VCard
    
    # Save the ContactDto as a VCard file to storage
    api_instance.contact.save(
        models.ContactSaveRequest(
            models.StorageFileLocation(storage, folder, contact_file),
            contact, 'VCard'))
    
    print(f"Contact successfully saved as {contact_file} in {folder}")
except Exception as e:
    print(f"Error: {str(e)}")
```

{{< /tab >}}

{{< tab tabNum="4" >}}

```ruby
require 'aspose-email-cloud'
include AsposeEmailCloud

begin
  # Create the EmailCloud instance (replace with your credentials)
  client_secret = "Your Client Secret"
  client_id = "Your Client ID"
  api = EmailCloud.new(client_secret, client_id)
  
  # Create a ContactDto object with basic information
  contact = ContactDto.new(
    surname: 'Thomas',
    given_name: 'Alex',
    gender: 'Male',
    email_addresses: [
      EmailAddress.new(
        category: EnumWithCustomOfEmailAddressCategory.new(value: 'Work'),
        display_name: 'Alex Thomas',
        preferred: true,
        address: 'alex.thomas@work.com')
    ],
    phone_numbers: [
      PhoneNumber.new(
        category: EnumWithCustomOfPhoneNumberCategory.new(value: 'Work'),
        number: '+49211424721',
        preferred: true)
    ])
  
  # Define storage location
  storage = 'First Storage'  # Name of your storage
  folder = 'Contacts'  # Folder path in storage
  contact_file = 'alex_thomas.vcf'  # Filename for the VCard
  
  # Save the ContactDto as a VCard file to storage
  api.contact.save(
    ContactSaveRequest.new(
      storage_file: StorageFileLocation.new(
        storage: storage,
        folder_path: folder,
        file_name: contact_file),
      value: contact,
      format: 'VCard'))
  
  puts "Contact successfully saved as #{contact_file} in #{folder}"
rescue => e
  puts "Error: #{e.message}"
end
```

{{< /tab >}}

{{< tab tabNum="5" >}}

```typescript
import * as email from "@asposecloud/aspose-email-cloud";

// Create the EmailCloud instance (replace with your credentials)
const clientSecret = "Your Client Secret";
const clientId = "Your Client ID";
const api = new email.EmailCloud(clientId, clientSecret);

// Create a ContactDto object with basic information
const contact = new email.ContactDto();
contact.gender = 'Male';
contact.surname = 'Thomas';
contact.givenName = 'Alex';
contact.emailAddresses = [new email.EmailAddress(
    new email.EnumWithCustomOfEmailAddressCategory('Work'),
    'Alex Thomas', true, undefined, 'alex.thomas@work.com')];
contact.phoneNumbers = [new email.PhoneNumber(
    new email.EnumWithCustomOfPhoneNumberCategory('Work'),
    '+49211424721', true)];

// Define storage location
const storage = 'First Storage'; // Name of your storage
const folder = 'Contacts'; // Folder path in storage
const contactFile = 'alex_thomas.vcf'; // Filename for the VCard

// Save the ContactDto as a VCard file to storage
api.contact.save(
    new email.ContactSaveRequest(
        new email.StorageFileLocation(storage, folder, contactFile),
        contact, 'VCard'))
    .then(() => {
        console.log(`Contact successfully saved as ${contactFile} in ${folder}`);
    })
    .catch((error) => {
        console.log("Error:", error.message);
    });
```

{{< /tab >}}

{{< tab tabNum="6" >}}

```php
<?php
require_once "vendor/autoload.php";

use Aspose\Email\EmailCloud;
use Aspose\Email\Configuration;
use Aspose\Email\Model\ContactDto;
use Aspose\Email\Model\EmailAddress;
use Aspose\Email\Model\PhoneNumber;
use Aspose\Email\Model\EnumWithCustomOfEmailAddressCategory;
use Aspose\Email\Model\EnumWithCustomOfPhoneNumberCategory;
use Aspose\Email\Model\ContactSaveRequest;
use Aspose\Email\Model\StorageFileLocation;

try {
    // Create the EmailCloud instance (replace with your credentials)
    $clientSecret = "Your Client Secret";
    $clientId = "Your Client ID";
    
    $configuration = new Configuration();
    $configuration
        ->setClientSecret($clientSecret)
        ->setClientId($clientId);
    
    $api = new EmailCloud($configuration);
    
    // Create a ContactDto object with basic information
    $contact = (new ContactDto())
        ->setGender("Male")
        ->setSurname("Thomas")
        ->setGivenName("Alex")
        ->setEmailAddresses(array(new EmailAddress(
            new EnumWithCustomOfEmailAddressCategory("Work"),
            "Alex Thomas", true, null, "alex.thomas@work.com")))
        ->setPhoneNumbers(array(new PhoneNumber(
            new EnumWithCustomOfPhoneNumberCategory("Work"),
            "+49211424721", true)));
    
    // Define storage location
    $storage = "First Storage"; // Name of your storage
    $folder = "Contacts"; // Folder path in storage
    $contactFile = "alex_thomas.vcf"; // Filename for the VCard
    
    // Save the ContactDto as a VCard file to storage
    $api->contact()->save(new ContactSaveRequest(
        new StorageFileLocation($storage, $folder, $contactFile),
        $contact,
        "VCard"
    ));
    
    echo "Contact successfully saved as $contactFile in $folder";
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

{{< /tab >}}

{{< /tabs >}}

Learning Checkpoint: What information did we include in our basic ContactDto object?
- Gender, surname, and given name
- Email address with category, display name, and preference settings
- Phone number with category and preference settings

## Step 2: Download VCard File From Storage

Once you've saved a VCard file to cloud storage, you might want to download it for local use or processing. Let's retrieve that file from storage.

### Try it yourself

{{< tabs tabTotal="6" tabID="2" tabName1="C#" tabName2="Java" tabName3="Python" tabName4="Ruby" tabName5="TypeScript/Node.js" tabName6="PHP" >}}

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
        // Create the EmailCloud instance (replace with your credentials)
        var clientSecret = "Your Client Secret";
        var clientId = "Your Client ID";
        var api = new EmailCloud(clientSecret, clientId);
        
        try
        {
            // Define storage location
            var storage = "First Storage"; // Name of your storage
            var folder = "Contacts"; // Folder path in storage
            var contactFile = "alex_thomas.vcf"; // Filename for the VCard
            
            // Download the VCard file from storage
            var resultStream = await api.CloudStorage.File.DownloadFileAsync(
                new DownloadFileRequest($"{folder}/{contactFile}", storage));
            
            // Save the downloaded file locally
            using (var fileStream = File.Create("downloaded_" + contactFile))
            {
                resultStream.CopyTo(fileStream);
            }
            
            Console.WriteLine($"VCard file successfully downloaded as 'downloaded_{contactFile}'");
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
import com.aspose.email.cloud.sdk.model.DownloadFileRequest;
import java.io.FileOutputStream;

public class DownloadVCardExample {
    public static void main(String[] args) {
        try {
            // Create the EmailCloud instance (replace with your credentials)
            String clientSecret = "Your Client Secret";
            String clientId = "Your Client ID";
            EmailCloud api = new EmailCloud(clientSecret, clientId);
            
            // Define storage location
            String storage = "First Storage"; // Name of your storage
            String folder = "Contacts"; // Folder path in storage
            String contactFile = "alex_thomas.vcf"; // Filename for the VCard
            
            // Download the VCard file from storage
            byte[] result = api.cloudStorage().file().downloadFile(
                new DownloadFileRequest(folder + "/" + contactFile, storage, null));
            
            // Save the downloaded file locally
            FileOutputStream outputStream = new FileOutputStream("downloaded_" + contactFile);
            outputStream.write(result);
            outputStream.close();
            
            System.out.println("VCard file successfully downloaded as 'downloaded_" + contactFile + "'");
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
import shutil

try:
    # Create the EmailCloud instance (replace with your credentials)
    client_secret = "Your Client Secret"
    client_id = "Your Client ID"
    api_instance = api.EmailCloud(client_id, client_secret)
    
    # Define storage location
    storage = 'First Storage'  # Name of your storage
    folder = 'Contacts'  # Folder path in storage
    contact_file = 'alex_thomas.vcf'  # Filename for the VCard
    
    # Download the VCard file from storage
    downloaded_file = api_instance.cloud_storage.file.download_file(
        models.DownloadFileRequest(folder + '/' + contact_file, storage))
    
    # Save the downloaded file locally
    with open("downloaded_" + contact_file, "wb") as output_file:
        shutil.copyfileobj(downloaded_file, output_file)
    
    print(f"VCard file successfully downloaded as 'downloaded_{contact_file}'")
except Exception as e:
    print(f"Error: {str(e)}")
```

{{< /tab >}}

{{< tab tabNum="4" >}}

```ruby
require 'aspose-email-cloud'
include AsposeEmailCloud

begin
  # Create the EmailCloud instance (replace with your credentials)
  client_secret = "Your Client Secret"
  client_id = "Your Client ID"
  api = EmailCloud.new(client_secret, client_id)
  
  # Define storage location
  storage = 'First Storage'  # Name of your storage
  folder = 'Contacts'  # Folder path in storage
  contact_file = 'alex_thomas.vcf'  # Filename for the VCard
  
  # Download the VCard file from storage
  downloaded_file = api.cloud_storage.file.download_file(
    DownloadFileRequest.new(
      path: folder + '/' + contact_file, 
      storage_name: storage))
  
  # Save the downloaded file locally
  File.open("downloaded_" + contact_file, "wb") do |file|
    file.write(downloaded_file.read)
  end
  
  puts "VCard file successfully downloaded as 'downloaded_#{contact_file}'"
rescue => e
  puts "Error: #{e.message}"
end
```

{{< /tab >}}

{{< tab tabNum="5" >}}

```typescript
import * as email from "@asposecloud/aspose-email-cloud";
import * as fs from "fs";

// Create the EmailCloud instance (replace with your credentials)
const clientSecret = "Your Client Secret";
const clientId = "Your Client ID";
const api = new email.EmailCloud(clientId, clientSecret);

// Define storage location
const storage = 'First Storage'; // Name of your storage
const folder = 'Contacts'; // Folder path in storage
const contactFile = 'alex_thomas.vcf'; // Filename for the VCard

// Download the VCard file from storage
api.cloudStorage.file.downloadFile(
    new email.DownloadFileRequest(folder + '/' + contactFile, storage))
    .then((downloaded) => {
        // Save the downloaded file locally
        fs.writeFileSync("downloaded_" + contactFile, downloaded);
        console.log(`VCard file successfully downloaded as 'downloaded_${contactFile}'`);
    })
    .catch((error) => {
        console.log("Error:", error.message);
    });
```

{{< /tab >}}

{{< tab tabNum="6" >}}

```php
<?php
require_once "vendor/autoload.php";

use Aspose\Email\EmailCloud;
use Aspose\Email\Configuration;
use Aspose\Email\Model\DownloadFileRequest;

try {
    // Create the EmailCloud instance (replace with your credentials)
    $clientSecret = "Your Client Secret";
    $clientId = "Your Client ID";
    
    $configuration = new Configuration();
    $configuration
        ->setClientSecret($clientSecret)
        ->setClientId($clientId);
    
    $api = new EmailCloud($configuration);
    
    // Define storage location
    $storage = "First Storage"; // Name of your storage
    $folder = "Contacts"; // Folder path in storage
    $contactFile = "alex_thomas.vcf"; // Filename for the VCard
    
    // Download the VCard file from storage
    $downloaded = $api->cloudStorage()->file()->downloadFile(
        new DownloadFileRequest($folder.'/'.$contactFile, $storage));
    
    // Save the downloaded file locally
    $fileContent = $downloaded->fread($downloaded->getSize());
    file_put_contents("downloaded_" . $contactFile, $fileContent);
    
    echo "VCard file successfully downloaded as 'downloaded_" . $contactFile . "'";
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

{{< /tab >}}

{{< /tabs >}}

## Step 3: Get VCard File From Storage as ContactDto Object

Now, let's retrieve a VCard file from storage and convert it to a ContactDto object for manipulation within your application.

### Try it yourself

{{< tabs tabTotal="6" tabID="3" tabName1="C#" tabName2="Java" tabName3="Python" tabName4="Ruby" tabName5="TypeScript/Node.js" tabName6="PHP" >}}

{{< tab tabNum="1" >}}

```csharp
using System;
using System.Threading.Tasks;
using Aspose.Email.Cloud.Sdk.Api;
using Aspose.Email.Cloud.Sdk.Model;

class Program
{
    static async Task Main(string[] args)
    {
        // Create the EmailCloud instance (replace with your credentials)
        var clientSecret = "Your Client Secret";
        var clientId = "Your Client ID";
        var api = new EmailCloud(clientSecret, clientId);
        
        try
        {
            // Define storage location
            var storage = "First Storage"; // Name of your storage
            var folder = "Contacts"; // Folder path in storage
            var contactFile = "alex_thomas.vcf"; // Filename for the VCard
            
            // Get the VCard file as a ContactDto object
            var contactDto = await api.Contact.GetAsync(new ContactGetRequest(
                "VCard", contactFile, folder, storage));
            
            // Now you can work with the contact information
            Console.WriteLine($"Contact retrieved: {contactDto.GivenName} {contactDto.Surname}");
            Console.WriteLine($"Gender: {contactDto.Gender}");
            
            if (contactDto.EmailAddresses != null && contactDto.EmailAddresses.Count > 0)
            {
                Console.WriteLine($"Email: {contactDto.EmailAddresses[0].Address}");
            }
            
            if (contactDto.PhoneNumbers != null && contactDto.PhoneNumbers.Count > 0)
            {
                Console.WriteLine($"Phone: {contactDto.PhoneNumbers[0].Number}");
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
import com.aspose.email.cloud.sdk.model.*;

public class GetVCardExample {
    public static void main(String[] args) {
        try {
            // Create the EmailCloud instance (replace with your credentials)
            String clientSecret = "Your Client Secret";
            String clientId = "Your Client ID";
            EmailCloud api = new EmailCloud(clientSecret, clientId);
            
            // Define storage location
            String storage = "First Storage"; // Name of your storage
            String folder = "Contacts"; // Folder path in storage
            String contactFile = "alex_thomas.vcf"; // Filename for the VCard
            
            // Get the VCard file as a ContactDto object
            ContactDto contactDto = api.contact().get(new ContactGetRequest(
                "VCard", contactFile, folder, storage));
            
            // Now you can work with the contact information
            System.out.println("Contact retrieved: " + contactDto.getGivenName() + " " + contactDto.getSurname());
            System.out.println("Gender: " + contactDto.getGender());
            
            if (contactDto.getEmailAddresses() != null && !contactDto.getEmailAddresses().isEmpty()) {
                System.out.println("Email: " + contactDto.getEmailAddresses().get(0).getAddress());
            }
            
            if (contactDto.getPhoneNumbers() != null && !contactDto.getPhoneNumbers().isEmpty()) {
                System.out.println("Phone: " + contactDto.getPhoneNumbers().get(0).getNumber());
            }
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

try:
    # Create the EmailCloud instance (replace with your credentials)
    client_secret = "Your Client Secret"
    client_id = "Your Client ID"
    api_instance = api.EmailCloud(client_id, client_secret)
    
    # Define storage location
    storage = 'First Storage'  # Name of your storage
    folder = 'Contacts'  # Folder path in storage
    contact_file = 'alex_thomas.vcf'  # Filename for the VCard
    
    # Get the VCard file as a ContactDto object
    contact_dto = api_instance.contact.get(models.ContactGetRequest(
        'VCard', contact_file, folder, storage))
    
    # Now you can work with the contact information
    print(f"Contact retrieved: {contact_dto.given_name} {contact_dto.surname}")
    print(f"Gender: {contact_dto.gender}")
    
    if contact_dto.email_addresses and len(contact_dto.email_addresses) > 0:
        print(f"Email: {contact_dto.email_addresses[0].address}")
    
    if contact_dto.phone_numbers and len(contact_dto.phone_numbers) > 0:
        print(f"Phone: {contact_dto.phone_numbers[0].number}")
except Exception as e:
    print(f"Error: {str(e)}")
```

{{< /tab >}}

{{< tab tabNum="4" >}}

```ruby
require 'aspose-email-cloud'
include AsposeEmailCloud

begin
  # Create the EmailCloud instance (replace with your credentials)
  client_secret = "Your Client Secret"
  client_id = "Your Client ID"
  api = EmailCloud.new(client_secret, client_id)
  
  # Define storage location
  storage = 'First Storage'  # Name of your storage
  folder = 'Contacts'  # Folder path in storage
  contact_file = 'alex_thomas.vcf'  # Filename for the VCard
  
  # Get the VCard file as a ContactDto object
  contact_dto = api.contact.get(
    ContactGetRequest.new(
      format: 'VCard',
      file_name: contact_file,
      folder: folder,
      storage: storage))
  
  # Now you can work with the contact information
  puts "Contact retrieved: #{contact_dto.given_name} #{contact_dto.surname}"
  puts "Gender: #{contact_dto.gender}"
  
  if contact_dto.email_addresses && !contact_dto.email_addresses.empty?
    puts "Email: #{contact_dto.email_addresses[0].address}"
  end
  
  if contact_dto.phone_numbers && !contact_dto.phone_numbers.empty?
    puts "Phone: #{contact_dto.phone_numbers[0].number}"
  end
rescue => e
  puts "Error: #{e.message}"
end
```

{{< /tab >}}

{{< tab tabNum="5" >}}

```typescript
import * as email from "@asposecloud/aspose-email-cloud";

// Create the EmailCloud instance (replace with your credentials)
const clientSecret = "Your Client Secret";
const clientId = "Your Client ID";
const api = new email.EmailCloud(clientId, clientSecret);

// Define storage location
const storage = 'First Storage'; // Name of your storage
const folder = 'Contacts'; // Folder path in storage
const contactFile = 'alex_thomas.vcf'; // Filename for the VCard

// Get the VCard file as a ContactDto object
api.contact.get(new email.ContactGetRequest(
    'VCard', contactFile, folder, storage))
    .then((contactDto) => {
        // Now you can work with the contact information
        console.log(`Contact retrieved: ${contactDto.givenName} ${contactDto.surname}`);
        console.log(`Gender: ${contactDto.gender}`);
        
        if (contactDto.emailAddresses && contactDto.emailAddresses.length > 0) {
            console.log(`Email: ${contactDto.emailAddresses[0].address}`);
        }
        
        if (contactDto.phoneNumbers && contactDto.phoneNumbers.length > 0) {
            console.log(`Phone: ${contactDto.phoneNumbers[0].number}`);
        }
    })
    .catch((error) => {
        console.log("Error:", error.message);
    });
```

{{< /tab >}}

{{< tab tabNum="6" >}}

```php
<?php
require_once "vendor/autoload.php";

use Aspose\Email\EmailCloud;
use Aspose\Email\Configuration;
use Aspose\Email\Model\ContactGetRequest;

try {
    // Create the EmailCloud instance (replace with your credentials)
    $clientSecret = "Your Client Secret";
    $clientId = "Your Client ID";
    
    $configuration = new Configuration();
    $configuration
        ->setClientSecret($clientSecret)
        ->setClientId($clientId);
    
    $api = new EmailCloud($configuration);
    
    // Define storage location
    $storage = "First Storage"; // Name of your storage
    $folder = "Contacts"; // Folder path in storage
    $contactFile = "alex_thomas.vcf"; // Filename for the VCard
    
    // Get the VCard file as a ContactDto object
    $contactDto = $api->contact()->get(new ContactGetRequest(
        'VCard', $contactFile, $folder, $storage));
    
    // Now you can work with the contact information
    echo "Contact retrieved: " . $contactDto->getGivenName() . " " . $contactDto->getSurname() . "\n";
    echo "Gender: " . $contactDto->getGender() . "\n";
    
    $emailAddresses = $contactDto->getEmailAddresses();
    if ($emailAddresses && count($emailAddresses) > 0) {
        echo "Email: " . $emailAddresses[0]->getAddress() . "\n";
    }
    
    $phoneNumbers = $contactDto->getPhoneNumbers();
    if ($phoneNumbers && count($phoneNumbers) > 0) {
        echo "Phone: " . $phoneNumbers[0]->getNumber() . "\n";
    }
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

{{< /tab >}}

{{< /tabs >}}

Learning Checkpoint: What can you do after retrieving a VCard as a ContactDto object?
- Access and modify all contact information (name, email, phone, etc.)
- Save changes back to storage with the same ContactSaveRequest
- Use the contact information in your application logic

## Step 4: Use Business Card Recognition API

One of the most powerful features of the VCard API is the ability to extract contact information from business card images using AI-powered recognition. Let's see how to use this feature.

### Try it yourself

For this example, you'll need an image of a business card (PNG, JPG, etc.). The AI-based business card recognition will analyze the image and extract contact information.

{{< tabs tabTotal="6" tabID="4" tabName1="C#" tabName2="Java" tabName3="Python" tabName4="Ruby" tabName5="TypeScript/Node.js" tabName6="PHP" >}}

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
        // Create the EmailCloud instance (replace with your credentials)
        var clientSecret = "Your Client Secret";
        var clientId = "Your Client ID";
        var api = new EmailCloud(clientSecret, clientId);
        
        try
        {
            // Path to your business card image
            var imagePath = "path/to/your/businesscard.png";
            
            // Parse the business card image
            using (var file = File.OpenRead(imagePath))
            {
                ContactList result = await api.Ai.Bcr.ParseAsync(
                    new AiBcrParseRequest(file, isSingle: true));
                
                // Process the results
                Console.WriteLine($"Found {result.Value.Count} contacts in the image");
                
                foreach (var contact in result.Value)
                {
                    Console.WriteLine("\nContact Information:");
                    
                    if (!string.IsNullOrEmpty(contact.GivenName) || !string.IsNullOrEmpty(contact.Surname))
                    {
                        Console.WriteLine($"Name: {contact.GivenName} {contact.Surname}");
                    }
                    
                    if (contact.CompanyName != null)
                    {
                        Console.WriteLine($"Company: {contact.CompanyName.Name}");
                    }
                    
                    if (contact.PhoneNumbers != null && contact.PhoneNumbers.Count > 0)
                    {
                        Console.WriteLine("Phone Numbers:");
                        foreach (var phone in contact.PhoneNumbers)
                        {
                            Console.WriteLine($"  - {phone.Category?.Value}: {phone.Number}");
                        }
                    }
                    
                    if (contact.EmailAddresses != null && contact.EmailAddresses.Count > 0)
                    {
                        Console.WriteLine("Email Addresses:");
                        foreach (var email in contact.EmailAddresses)
                        {
                            Console.WriteLine($"  - {email.Category?.Value}: {email.Address}");
                        }
                    }
                    
                    // You can save the contact to storage
                    var storage = "First Storage"; // Name of your storage
                    var folder = "Contacts"; // Folder path in storage
                    var contactFile = $"recognized_contact_{Guid.NewGuid()}.vcf"; // Generate unique filename
                    
                    await api.Contact.SaveAsync(
                        new ContactSaveRequest(
                            new StorageFileLocation(storage, folder, contactFile),
                            contact, "VCard"));
                    
                    Console.WriteLine($"Contact saved to storage as {contactFile}");
                }
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
import com.aspose.email.cloud.sdk.model.*;
import java.io.FileInputStream;
import java.util.UUID;
import org.apache.commons.io.IOUtils;

public class BusinessCardRecognitionExample {
    public static void main(String[] args) {
        try {
            // Create the EmailCloud instance (replace with your credentials)
            String clientSecret = "Your Client Secret";
            String clientId = "Your Client ID";
            EmailCloud api = new EmailCloud(clientSecret, clientId);
            
            // Path to your business card image
            String imagePath = "path/to/your/businesscard.png";
            
            // Parse the business card image
            byte[] fileBytes = IOUtils.toByteArray(new FileInputStream(imagePath));
            ContactList result = api.ai().bcr().parse(
                new AiBcrParseRequest(fileBytes, null, null, true));
            
            // Process the results
            System.out.println("Found " + result.getValue().size() + " contacts in the image");
            
            for (ContactDto contact : result.getValue()) {
                System.out.println("\nContact Information:");
                
                if (contact.getGivenName() != null || contact.getSurname() != null) {
                    System.out.println("Name: " + contact.getGivenName() + " " + contact.getSurname());
                }
                
                if (contact.getCompanyName() != null) {
                    System.out.println("Company: " + contact.getCompanyName().getName());
                }
                
                if (contact.getPhoneNumbers() != null && !contact.getPhoneNumbers().isEmpty()) {
                    System.out.println("Phone Numbers:");
                    for (PhoneNumber phone : contact.getPhoneNumbers()) {
                        System.out.println("  - " + (phone.getCategory() != null ? phone.getCategory().getValue() : "Default") + 
                                           ": " + phone.getNumber());
                    }
                }
                
                if (contact.getEmailAddresses() != null && !contact.getEmailAddresses().isEmpty()) {
                    System.out.println("Email Addresses:");
                    for (EmailAddress email : contact.getEmailAddresses()) {
                        System.out.println("  - " + (email.getCategory() != null ? email.getCategory().getValue() : "Default") + 
                                           ": " + email.getAddress());
                    }
                }
                
                // You can save the contact to storage
                String storage = "First Storage"; // Name of your storage
                String folder = "Contacts"; // Folder path in storage
                String contactFile = "recognized_contact_" + UUID.randomUUID().toString() + ".vcf"; // Generate unique filename
                
                api.contact().save(
                    new ContactSaveRequest(
                        new StorageFileLocation(storage, folder, contactFile),
                        contact, "VCard"));
                
                System.out.println("Contact saved to storage as " + contactFile);
            }
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
import uuid

try:
    # Create the EmailCloud instance (replace with your credentials)
    client_secret = "Your Client Secret"
    client_id = "Your Client ID"
    api_instance = api.EmailCloud(client_id, client_secret)
    
    # Path to your business card image
    image_path = "path/to/your/businesscard.png"
    
    # Parse the business card image
    result = api_instance.ai.bcr.parse(
        models.AiBcrParseRequest(image_path, is_single=True))
    
    # Process the results
    print(f"Found {len(result.value)} contacts in the image")
    
    for contact in result.value:
        print("\nContact Information:")
        
        if contact.given_name or contact.surname:
            print(f"Name: {contact.given_name} {contact.surname}")
        
        if contact.company_name:
            print(f"Company: {contact.company_name.name}")
        
        if contact.phone_numbers:
            print("Phone Numbers:")
            for phone in contact.phone_numbers:
                category = phone.category.value if phone.category else "Default"
                print(f"  - {category}: {phone.number}")
        
        if contact.email_addresses:
            print("Email Addresses:")
            for email in contact.email_addresses:
                category = email.category.value if email.category else "Default"
                print(f"  - {category}: {email.address}")
        
        # You can save the contact to storage
        storage = "First Storage"  # Name of your storage
        folder = "Contacts"  # Folder path in storage
        contact_file = f"recognized_contact_{uuid.uuid4()}.vcf"  # Generate unique filename
        
        api_instance.contact.save(
            models.ContactSaveRequest(
                models.StorageFileLocation(storage, folder, contact_file),
                contact, "VCard"))
        
        print(f"Contact saved to storage as {contact_file}")
except Exception as e:
    print(f"Error: {str(e)}")
```

{{< /tab >}}

{{< tab tabNum="4" >}}

```ruby
require 'aspose-email-cloud'
include AsposeEmailCloud
require 'securerandom'

begin
  # Create the EmailCloud instance (replace with your credentials)
  client_secret = "Your Client Secret"
  client_id = "Your Client ID"
  api = EmailCloud.new(client_secret, client_id)
  
  # Path to your business card image
  image_path = "path/to/your/businesscard.png"
  
  # Parse the business card image
  file = File.new(image_path)
  result = api.ai.bcr.parse(AiBcrParseRequest.new(
    file: file, is_single: true))
  
  # Process the results
  puts "Found #{result.value.length} contacts in the image"
  
  result.value.each do |contact|
    puts "\nContact Information:"
    
    if contact.given_name || contact.surname
      puts "Name: #{contact.given_name} #{contact.surname}"
    end
    
    if contact.company_name
      puts "Company: #{contact.company_name.name}"
    end
    
    if contact.phone_numbers && !contact.phone_numbers.empty?
      puts "Phone Numbers:"
      contact.phone_numbers.each do |phone|
        category = phone.category ? phone.category.value : "Default"
        puts "  - #{category}: #{phone.number}"
      end
    end
    
    if contact.email_addresses && !contact.email_addresses.empty?
      puts "Email Addresses:"
      contact.email_addresses.each do |email|
        category = email.category ? email.category.value : "Default"
        puts "  - #{category}: #{email.address}"
      end
    end
    
    # You can save the contact to storage
    storage = 'First Storage'  # Name of your storage
    folder = 'Contacts'  # Folder path in storage
    contact_file = "recognized_contact_#{SecureRandom.uuid}.vcf"  # Generate unique filename
    
    api.contact.save(
      ContactSaveRequest.new(
        storage_file: StorageFileLocation.new(
          storage: storage,
          folder_path: folder,
          file_name: contact_file),
        value: contact,
        format: 'VCard'))
    
    puts "Contact saved to storage as #{contact_file}"
  end
rescue => e
  puts "Error: #{e.message}"
end
```

{{< /tab >}}

{{< tab tabNum="5" >}}

```typescript
import * as email from "@asposecloud/aspose-email-cloud";
import * as fs from "fs";
import { v4 as uuidv4 } from 'uuid';

// Create the EmailCloud instance (replace with your credentials)
const clientSecret = "Your Client Secret";
const clientId = "Your Client ID";
const api = new email.EmailCloud(clientId, clientSecret);

// Path to your business card image
const imagePath = "path/to/your/businesscard.png";

try {
    // Parse the business card image
    const imageData = fs.readFileSync(imagePath);
    api.ai.bcr.parse(
        new email.AiBcrParseRequest(imageData, undefined, undefined, true))
        .then((result) => {
            // Process the results
            console.log(`Found ${result.value.length} contacts in the image`);
            
            result.value.forEach(contact => {
                console.log("\nContact Information:");
                
                if (contact.givenName || contact.surname) {
                    console.log(`Name: ${contact.givenName} ${contact.surname}`);
                }
                
                if (contact.companyName) {
                    console.log(`Company: ${contact.companyName.name}`);
                }
                
                if (contact.phoneNumbers && contact.phoneNumbers.length > 0) {
                    console.log("Phone Numbers:");
                    contact.phoneNumbers.forEach(phone => {
                        const category = phone.category ? phone.category.value : "Default";
                        console.log(`  - ${category}: ${phone.number}`);
                    });
                }
                
                if (contact.emailAddresses && contact.emailAddresses.length > 0) {
                    console.log("Email Addresses:");
                    contact.emailAddresses.forEach(email => {
                        const category = email.category ? email.category.value : "Default";
                        console.log(`  - ${category}: ${email.address}`);
                    });
                }
                
                // You can save the contact to storage
                const storage = 'First Storage'; // Name of your storage
                const folder = 'Contacts'; // Folder path in storage
                const contactFile = `recognized_contact_${uuidv4()}.vcf`; // Generate unique filename
                
                api.contact.save(
                    new email.ContactSaveRequest(
                        new email.StorageFileLocation(storage, folder, contactFile),
                        contact, 'VCard'))
                    .then(() => {
                        console.log(`Contact saved to storage as ${contactFile}`);
                    })
                    .catch((error) => {
                        console.log("Error saving contact:", error.message);
                    });
            });
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
use Aspose\Email\Model\AiBcrParseRequest;
use Aspose\Email\Model\ContactSaveRequest;
use Aspose\Email\Model\StorageFileLocation;

try {
    // Create the EmailCloud instance (replace with your credentials)
    $clientSecret = "Your Client Secret";
    $clientId = "Your Client ID";
    
    $configuration = new Configuration();
    $configuration
        ->setClientSecret($clientSecret)
        ->setClientId($clientId);
    
    $api = new EmailCloud($configuration);
    
    // Path to your business card image
    $imagePath = "path/to/your/businesscard.png";
    
    // Parse the business card image
    $result = $api->ai()->bcr()->parse(
        new AiBcrParseRequest(new SplFileObject($imagePath), null, null, true));
    
    // Process the results
    $contacts = $result->getValue();
    echo "Found " . count($contacts) . " contacts in the image\n";
    
    foreach ($contacts as $contact) {
        echo "\nContact Information:\n";
        
        if ($contact->getGivenName() || $contact->getSurname()) {
            echo "Name: " . $contact->getGivenName() . " " . $contact->getSurname() . "\n";
        }
        
        if ($contact->getCompanyName()) {
            echo "Company: " . $contact->getCompanyName()->getName() . "\n";
        }
        
        $phoneNumbers = $contact->getPhoneNumbers();
        if ($phoneNumbers && count($phoneNumbers) > 0) {
            echo "Phone Numbers:\n";
            foreach ($phoneNumbers as $phone) {
                $category = $phone->getCategory() ? $phone->getCategory()->getValue() : "Default";
                echo "  - " . $category . ": " . $phone->getNumber() . "\n";
            }
        }
        
        $emailAddresses = $contact->getEmailAddresses();
        if ($emailAddresses && count($emailAddresses) > 0) {
            echo "Email Addresses:\n";
            foreach ($emailAddresses as $email) {
                $category = $email->getCategory() ? $email->getCategory()->getValue() : "Default";
                echo "  - " . $category . ": " . $email->getAddress() . "\n";
            }
        }
        
        // You can save the contact to storage
        $storage = "First Storage"; // Name of your storage
        $folder = "Contacts"; // Folder path in storage
        $contactFile = "recognized_contact_" . uniqid() . ".vcf"; // Generate unique filename
        
        $api->contact()->save(new ContactSaveRequest(
            new StorageFileLocation($storage, $folder, $contactFile),
            $contact,
            "VCard"
        ));
        
        echo "Contact saved to storage as " . $contactFile . "\n";
    }
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

{{< /tab >}}

{{< /tabs >}}

Learning Checkpoint: What information can be extracted from a business card image?
- Name (given name and surname)
- Company information
- Phone numbers with categories (work, mobile, etc.)
- Email addresses with categories
- Potentially more depending on what's visible in the image

## Step 5: Get a List of VCard Files From Storage

When working with multiple contacts, you may want to retrieve a list of all VCard files stored in a specific folder. The API provides a convenient method to do this with pagination support.

### Try it yourself

{{< tabs tabTotal="6" tabID="5" tabName1="C#" tabName2="Java" tabName3="Python" tabName4="Ruby" tabName5="TypeScript/Node.js" tabName6="PHP" >}}

{{< tab tabNum="1" >}}

```csharp
using System;
using System.Threading.Tasks;
using Aspose.Email.Cloud.Sdk.Api;
using Aspose.Email.Cloud.Sdk.Model;

class Program
{
    static async Task Main(string[] args)
    {
        // Create the EmailCloud instance (replace with your credentials)
        var clientSecret = "Your Client Secret";
        var clientId = "Your Client ID";
        var api = new EmailCloud(clientSecret, clientId);
        
        try
        {
            // Define storage location
            var storage = "First Storage"; // Name of your storage
            var folder = "Contacts"; // Folder path in storage
            
            // Get a list of VCard files (10 per page, starting from page 0)
            var contactsList = await api.Contact.GetListAsync(
                new ContactGetListRequest(
                    "VCard", folder, storage, 10, 0));
            
            // Display information about the returned list
            Console.WriteLine($"Total contacts: {contactsList.TotalCount}");
            Console.WriteLine($"Contacts on this page: {contactsList.Value.Count}");
            
            // Process each contact in the list
            foreach (var contactDto in contactsList.Value)
            {
                Console.WriteLine("\nContact Information:");
                Console.WriteLine($"Name: {contactDto.GivenName} {contactDto.Surname}");
                
                if (contactDto.EmailAddresses != null && contactDto.EmailAddresses.Count > 0)
                {
                    Console.WriteLine($"Email: {contactDto.EmailAddresses[0].Address}");
                }
                
                if (contactDto.PhoneNumbers != null && contactDto.PhoneNumbers.Count > 0)
                {
                    Console.WriteLine($"Phone: {contactDto.PhoneNumbers[0].Number}");
                }
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
import com.aspose.email.cloud.sdk.model.*;
import java.util.List;

public class GetContactsListExample {
    public static void main(String[] args) {
        try {
            // Create the EmailCloud instance (replace with your credentials)
            String clientSecret = "Your Client Secret";
            String clientId = "Your Client ID";
            EmailCloud api = new EmailCloud(clientSecret, clientId);
            
            // Define storage location
            String storage = "First Storage"; // Name of your storage
            String folder = "Contacts"; // Folder path in storage
            
            // Get a list of VCard files (10 per page, starting from page 0)
            ContactStorageList contactsList = api.contact().getList(
                new ContactGetListRequest(
                    "VCard", folder, storage, 10, 0));
            
            // Display information about the returned list
            System.out.println("Total contacts: " + contactsList.getTotalCount());
            System.out.println("Contacts on this page: " + contactsList.getValue().size());
            
            // Process each contact in the list
            for (ContactDto contactDto : contactsList.getValue()) {
                System.out.println("\nContact Information:");
                System.out.println("Name: " + contactDto.getGivenName() + " " + contactDto.getSurname());
                
                List<EmailAddress> emailAddresses = contactDto.getEmailAddresses();
                if (emailAddresses != null && !emailAddresses.isEmpty()) {
                    System.out.println("Email: " + emailAddresses.get(0).getAddress());
                }
                
                List<PhoneNumber> phoneNumbers = contactDto.getPhoneNumbers();
                if (phoneNumbers != null && !phoneNumbers.isEmpty()) {
                    System.out.println("Phone: " + phoneNumbers.get(0).getNumber());
                }
            }
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

try:
    # Create the EmailCloud instance (replace with your credentials)
    client_secret = "Your Client Secret"
    client_id = "Your Client ID"
    api_instance = api.EmailCloud(client_id, client_secret)
    
    # Define storage location
    storage = 'First Storage'  # Name of your storage
    folder = 'Contacts'  # Folder path in storage
    
    # Get a list of VCard files (10 per page, starting from page 0)
    contacts_list = api_instance.contact.get_list(models.ContactGetListRequest(
        'VCard', folder, storage, 10, 0))
    
    # Display information about the returned list
    print(f"Total contacts: {contacts_list.total_count}")
    print(f"Contacts on this page: {len(contacts_list.value)}")
    
    # Process each contact in the list
    for contact_dto in contacts_list.value:
        print("\nContact Information:")
        print(f"Name: {contact_dto.given_name} {contact_dto.surname}")
        
        if contact_dto.email_addresses and len(contact_dto.email_addresses) > 0:
            print(f"Email: {contact_dto.email_addresses[0].address}")
        
        if contact_dto.phone_numbers and len(contact_dto.phone_numbers) > 0:
            print(f"Phone: {contact_dto.phone_numbers[0].number}")
except Exception as e:
    print(f"Error: {str(e)}")
```

{{< /tab >}}

{{< tab tabNum="4" >}}

```ruby
require 'aspose-email-cloud'
include AsposeEmailCloud

begin
  # Create the EmailCloud instance (replace with your credentials)
  client_secret = "Your Client Secret"
  client_id = "Your Client ID"
  api = EmailCloud.new(client_secret, client_id)
  
  # Define storage location
  storage = 'First Storage'  # Name of your storage
  folder = 'Contacts'  # Folder path in storage
  
  # Get a list of VCard files (10 per page, starting from page 0)
  contacts_list = api.contact.get_list(ContactGetListRequest.new(
    format: 'VCard',
    folder: folder,
    storage: storage,
    items_per_page: 10,
    page_number: 0))
  
  # Display information about the returned list
  puts "Total contacts: #{contacts_list.total_count}"
  puts "Contacts on this page: #{contacts_list.value.length}"
  
  # Process each contact in the list
  contacts_list.value.each do |contact_dto|
    puts "\nContact Information:"
    puts "Name: #{contact_dto.given_name} #{contact_dto.surname}"
    
    if contact_dto.email_addresses && !contact_dto.email_addresses.empty?
      puts "Email: #{contact_dto.email_addresses[0].address}"
    end
    
    if contact_dto.phone_numbers && !contact_dto.phone_numbers.empty?
      puts "Phone: #{contact_dto.phone_numbers[0].number}"
    end
  end
rescue => e
  puts "Error: #{e.message}"
end
```

{{< /tab >}}

{{< tab tabNum="5" >}}

```typescript
import * as email from "@asposecloud/aspose-email-cloud";

// Create the EmailCloud instance (replace with your credentials)
const clientSecret = "Your Client Secret";
const clientId = "Your Client ID";
const api = new email.EmailCloud(clientId, clientSecret);

// Define storage location
const storage = 'First Storage'; // Name of your storage
const folder = 'Contacts'; // Folder path in storage

// Get a list of VCard files (10 per page, starting from page 0)
api.contact.getList(new email.ContactGetListRequest(
    'VCard', folder, storage, 10, 0))
    .then((contactsList) => {
        // Display information about the returned list
        console.log(`Total contacts: ${contactsList.totalCount}`);
        console.log(`Contacts on this page: ${contactsList.value.length}`);
        
        // Process each contact in the list
        contactsList.value.forEach(contactDto => {
            console.log("\nContact Information:");
            console.log(`Name: ${contactDto.givenName} ${contactDto.surname}`);
            
            if (contactDto.emailAddresses && contactDto.emailAddresses.length > 0) {
                console.log(`Email: ${contactDto.emailAddresses[0].address}`);
            }
            
            if (contactDto.phoneNumbers && contactDto.phoneNumbers.length > 0) {
                console.log(`Phone: ${contactDto.phoneNumbers[0].number}`);
            }
        });
    })
    .catch((error) => {
        console.log("Error:", error.message);
    });
```

{{< /tab >}}

{{< tab tabNum="6" >}}

```php
<?php
require_once "vendor/autoload.php";

use Aspose\Email\EmailCloud;
use Aspose\Email\Configuration;
use Aspose\Email\Model\ContactGetListRequest;

try {
    // Create the EmailCloud instance (replace with your credentials)
    $clientSecret = "Your Client Secret";
    $clientId = "Your Client ID";
    
    $configuration = new Configuration();
    $configuration
        ->setClientSecret($clientSecret)
        ->setClientId($clientId);
    
    $api = new EmailCloud($configuration);
    
    // Define storage location
    $storage = "First Storage"; // Name of your storage
    $folder = "Contacts"; // Folder path in storage
    
    // Get a list of VCard files (10 per page, starting from page 0)
    $contactsList = $api->contact()->getList(new ContactGetListRequest(
        'VCard', $folder, $storage, 10, 0));
    
    // Display information about the returned list
    echo "Total contacts: " . $contactsList->getTotalCount() . "\n";
    echo "Contacts on this page: " . count($contactsList->getValue()) . "\n";
    
    // Process each contact in the list
    foreach ($contactsList->getValue() as $contactDto) {
        echo "\nContact Information:\n";
        echo "Name: " . $contactDto->getGivenName() . " " . $contactDto->getSurname() . "\n";
        
        $emailAddresses = $contactDto->getEmailAddresses();
        if ($emailAddresses && count($emailAddresses) > 0) {
            echo "Email: " . $emailAddresses[0]->getAddress() . "\n";
        }
        
        $phoneNumbers = $contactDto->getPhoneNumbers();
        if ($phoneNumbers && count($phoneNumbers) > 0) {
            echo "Phone: " . $phoneNumbers[0]->getNumber() . "\n";
        }
    }
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}
?>
```

{{< /tab >}}

{{< /tabs >}}

Learning Checkpoint: What pagination parameters can be used when retrieving a list of VCard files?
- itemsPerPage: The number of items to return per page
- pageNumber: The page number to retrieve (starting from 0)

## Troubleshooting Tips

If you encounter issues when working with the VCard API, here are some common problems and their solutions:

1. File Not Found
   - Verify the storage name, folder path, and file name are correct
   - Check that you have the necessary permissions to access the file

2. Invalid VCard Format
   - Ensure the VCard file adheres to the standard format
   - Try using a different file or creating a new one using the ContactDto object

3. Business Card Recognition Issues
   - Ensure the image is clear and well-lit
   - Try using a higher resolution image
   - Make sure the business card is properly framed in the image

4. API Authentication Errors
   - Verify your Client ID and Client Secret are correct
   - Check that your account subscription is active

## What You've Learned

In this tutorial, you've learned how to:
- Create ContactDto objects to represent contact information
- Save VCard files to cloud storage and download them
- Convert VCard files to ContactDto objects for manipulation
- Extract contact information from business card images using AI recognition
- Retrieve and paginate through lists of VCard files in storage

These skills enable you to build sophisticated contact management applications that can handle a variety of business scenarios, from manual contact creation to automated contact extraction from business cards.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a ContactDto with additional fields like addresses, job titles, and websites
2. Modify an existing VCard file and save it back to storage
3. Implement pagination to navigate through a large collection of contacts
4. Create a simple contact management application that uses the VCard API

## Helpful Resources

- [Product Page](https://products.aspose.cloud/email/)
- [Documentation](https://docs.aspose.cloud/email/)
- [Live Demo](https://products.aspose.app/email/family)
- [API Reference](https://reference.aspose.cloud/email/)
- [Blog](https://blog.aspose.cloud/category/email/)
- [Free Support](https://forum.aspose.cloud/c/email/9/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out to us on the [Aspose.Email Cloud forum](https://forum.aspose.cloud/c/email/9/).