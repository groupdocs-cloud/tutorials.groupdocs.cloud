---
title: Working with Email Message Files in Aspose.Email Cloud API Tutorial
url: /email-cloud/email-message-files/
description: Learn how to create, save, edit, and convert email message files with Aspose.Email Cloud API in this step-by-step tutorial for developers.
weight: 20
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Create email message files using the EmailDto model
- Save email files in various formats (EML, MSG, MHTML, HTML)
- Download email files from cloud storage
- Convert emails between different formats
- Append emails to email account folders
- Work with email properties and attachments

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account (sign up at [dashboard.aspose.cloud](https://dashboard.aspose.cloud/))
2. Your Aspose Cloud credentials (Client ID and Client Secret)
3. The Aspose.Email Cloud SDK for your preferred programming language installed
4. Basic understanding of email formats and components

## Introduction to Email Message Files

Email message files are the standard way to store and exchange email messages between electronic devices and applications. Aspose.Email Cloud API supports various email file formats, including:

- EML - The standard format used by many email clients including Microsoft Outlook Express
- MSG - Microsoft Outlook's proprietary email format
- MHTML - MIME HTML format that combines HTML and its resources
- HTML - Web page format, convenient for displaying emails on websites

Aspose.Email Cloud provides two main approaches for working with email files:
1. Using EmailDto objects - The approach we'll focus on in this tutorial
2. Using MapiMessageDto objects - For more advanced scenarios involving MAPI properties

## Tutorial Steps

### Step 1: Set Up Your Development Environment

First, ensure you have the Aspose.Email Cloud SDK installed for your preferred programming language. If you haven't done this yet, refer to our [SDK Setup Tutorial](/email-cloud/sdk-setup/).

### Step 2: Initialize the API Client

Create and configure the API client with your credentials:

```python
# Python example
import os
from asposeemail.api_client import ApiClient
from asposeemail.api.email_cloud import EmailCloud
from asposeemail.configuration import Configuration

# Configure API key authorization
configuration = Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API client
api = EmailCloud(configuration)
```

### Step 3: Create an Email Object and Save as a File

Let's create an email message and save it as a file on cloud storage:

```python
# Python
from asposeemail.models import EmailDto, MailAddress, Attachment, StorageFileLocation, EmailSaveRequest

# Create an email message
email = EmailDto(
    _from=MailAddress("From Name", "from@aspose.com"),
    to=[MailAddress("To Name", "to@aspose.com")],
    subject="Sample email subject",
    body="This is a sample email body."
)

# Add an attachment to the email
attachment_content = "This is a sample attachment content."
attachment_bytes = attachment_content.encode('utf-8')
import base64
base64_content = base64.b64encode(attachment_bytes).decode('utf-8')

email.attachments = [
    Attachment(
        base64_data=base64_content,
        name="sample-attachment.txt"
    )
]

# Define storage information
storage = "First Storage"
folder = "email_files"
email_file = "sample_email.eml"
file_format = "Eml"  # Can be Eml, Msg, MsgUnicode, MHTML, or HTML

# Save the email as a file on storage
api.email.save(EmailSaveRequest(
    storage_file=StorageFileLocation(storage, folder, email_file),
    value=email,
    format=file_format
))

print(f"Email saved as {email_file} in {folder} folder on {storage} storage")
```

Let's look at this example in other programming languages:

### C# Example

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using Aspose.Email.Cloud.Sdk.Api;
using Aspose.Email.Cloud.Sdk.Model;

// Initialize API client
var api = new EmailCloud("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Create an email message
var email = new EmailDto
{
    From = new MailAddress("From Name", "from@aspose.com"),
    To = new List<MailAddress> {new MailAddress("To Name", "to@aspose.com")},
    Subject = "Sample email subject",
    Body = "This is a sample email body."
};

// Add an attachment to the email
string attachmentContent = "This is a sample attachment content.";
email.Attachments = new List<Attachment>
{
    new Attachment
    {
        Base64Data = Convert.ToBase64String(Encoding.UTF8.GetBytes(attachmentContent)),
        Name = "sample-attachment.txt"
    }
};

// Define storage information
var storage = "First Storage";
var folder = "email_files";
var emailFile = "sample_email.eml";
var format = "Eml";  // Can be Eml, Msg, MsgUnicode, MHTML, or HTML

// Save the email as a file on storage
await api.Email.SaveAsync(new EmailSaveRequest(
    new StorageFileLocation(storage, folder, emailFile), 
    email, format));

Console.WriteLine($"Email saved as {emailFile} in {folder} folder on {storage} storage");
```


### Java Example

```java
import java.util.Arrays;
import java.util.Base64;
import com.aspose.email.cloud.sdk.api.EmailCloud;
import com.aspose.email.cloud.sdk.model.*;

// Initialize API client
EmailCloud api = new EmailCloud("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Create an email message
EmailDto email = new EmailDto()
    .from(new MailAddress("From Name", "from@aspose.com"))
    .addToItem(new MailAddress("To Name", "to@aspose.com"))
    .subject("Sample email subject")
    .body("This is a sample email body.");

// Add an attachment to the email
String attachmentContent = "This is a sample attachment content.";
byte[] bytes = attachmentContent.getBytes();
String base64Content = Base64.getEncoder().encodeToString(bytes);

email.setAttachments(Arrays.asList(
    new Attachment()
        .base64Data(base64Content)
        .name("sample-attachment.txt")
));

// Define storage information
String storage = "First Storage";
String folder = "email_files";
String emailFile = "sample_email.eml";
String format = "Eml";  // Can be Eml, Msg, MsgUnicode, MHTML, or HTML

// Save the email as a file on storage
api.email().save(new EmailSaveRequest(
    new StorageFileLocation(storage, folder, emailFile),
    email, format));

System.out.println("Email saved as " + emailFile + " in " + folder + " folder on " + storage + " storage");
```


### Step 4: Download an Email File from Storage

After saving an email to storage, you can download it:

```python
# Python
from asposeemail.models import DownloadFileRequest

# Download the email file from storage
result_stream = api.cloud_storage.file.download_file(
    DownloadFileRequest(f"{folder}/{email_file}", storage))

# Read the file content
with open("downloaded_email.eml", "wb") as f:
    f.write(result_stream)

print(f"Email file downloaded to downloaded_email.eml")
```

### Step 5: Download Email in a Different Format

You can download an email in a format different from the one it was saved in:

```python
# Python
from asposeemail.models import EmailGetAsFileRequest

# Download the email in MSG format
msg_stream = api.email.get_as_file(EmailGetAsFileRequest(
    file_name=email_file,
    format="Msg",  # Target format
    storage=storage,
    folder=folder
))

# Save the MSG file
with open("converted_email.msg", "wb") as f:
    f.write(msg_stream)

print("Email downloaded in MSG format")
```

### Step 6: Get Email as EmailDto Object

You can also retrieve an email file as an EmailDto object for further processing:

```python
# Python
from asposeemail.models import EmailGetRequest

# Get the email as an EmailDto object
email_dto = api.email.get(EmailGetRequest(
    format="Eml",  # Source format
    file_name=email_file,
    folder=folder,
    storage=storage
))

# Now you can access and modify email properties
print(f"Retrieved email subject: {email_dto.subject}")
print(f"From: {email_dto._from.address}")
print(f"To: {email_dto.to[0].address if email_dto.to else 'No recipients'}")

# Modify the email
email_dto.subject = "Modified subject"
email_dto.body = "This is the modified email body."

# Save the modified email back to storage
api.email.save(EmailSaveRequest(
    storage_file=StorageFileLocation(storage, folder, "modified_email.eml"),
    value=email_dto,
    format="Eml"
))

print("Modified email saved to storage")
```

### Step 7: Append Email to an Email Account Folder

You can append the EmailDto object to a folder in an email account:

```python
# Python
from asposeemail.models import StorageFileLocation, ClientMessageAppendRequest, MailMessageDto

# First, make sure you have an email account set up
# For IMAP accounts, you need to create an account file (see the Email Client tutorial)
imap_account = "imap.account"
account_folder = "email/accounts"

# Append the email to the "INBOX" folder
message_id = api.client.message.append(
    ClientMessageAppendRequest(
        account_location=StorageFileLocation(storage, account_folder, imap_account),
        folder="INBOX",
        message=MailMessageDto(email_dto),
        mark_as_sent=True
    )
)

print(f"Email appended to INBOX with message ID: {message_id.value}")
```

### Step 8: Convert Email Between Formats

For a quick conversion between email formats without storage:

```python
# Python
from asposeemail.models import EmailConvertRequest
import tempfile

# Create a temporary EML file
with tempfile.NamedTemporaryFile(suffix=".eml", delete=False) as temp_file:
    temp_file_path = temp_file.name
    temp_file.write(b"From: from@example.com\r\nTo: to@example.com\r\nSubject: Test\r\n\r\nEmail body")

# Convert from EML to MSG
with open(temp_file_path, "rb") as eml_file:
    msg_data = api.email.convert(
        EmailConvertRequest(
            from_format="Eml",
            to_format="Msg",
            file=eml_file.read()
        )
    )

# Save the converted MSG file
with open("converted_directly.msg", "wb") as msg_file:
    msg_file.write(msg_data)

print("Email converted directly from EML to MSG")
```

## Creating Different Types of Emails

Let's explore how to create different types of emails for various scenarios:

### HTML Email with Embedded Image

```python
# Python
html_body = """
<html>
<body>
<h1>HTML Email Example</h1>
<p>This is a paragraph with <b>bold</b> and <i>italic</i> text.</p>
<p>Below is an embedded image:</p>
<img src="cid:image1" alt="Embedded Image" />
<p>Thanks,<br>The Aspose Team</p>
</body>
</html>
"""

# Create an HTML email
html_email = EmailDto(
    _from=MailAddress("Sender", "sender@example.com"),
    to=[MailAddress("Recipient", "recipient@example.com")],
    subject="HTML Email with Embedded Image",
    html_body=html_body,
    is_body_html=True,
    body_type="Html"
)

# Create sample image data (in a real application, you would read an actual image file)
# For this example, we'll use a small sample image in base64
sample_image_base64 = "iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg=="

# Add as a linked resource (embedded image)
html_email.linked_resources = [
    Attachment(
        base64_data=sample_image_base64,
        name="image1.png",
        content_id="image1"  # This matches the "cid:image1" in the HTML
    )
]

# Save the HTML email
api.email.save(EmailSaveRequest(
    storage_file=StorageFileLocation(storage, folder, "html_email.eml"),
    value=html_email,
    format="Eml"
))

print("HTML email with embedded image saved")
```

### Email with CC and BCC Recipients

```python
# Python
# Create an email with CC and BCC recipients
complete_email = EmailDto(
    _from=MailAddress("Sender", "sender@example.com"),
    to=[MailAddress("Primary Recipient", "primary@example.com")],
    cc=[MailAddress("Copy Recipient", "copy@example.com")],
    bcc=[MailAddress("Hidden Recipient", "hidden@example.com")],
    subject="Email with CC and BCC Recipients",
    body="This email demonstrates CC and BCC functionality."
)

# Save the email
api.email.save(EmailSaveRequest(
    storage_file=StorageFileLocation(storage, folder, "cc_bcc_email.eml"),
    value=complete_email,
    format="Eml"
))

print("Email with CC and BCC recipients saved")
```

## Working with Email Properties

EmailDto provides access to various email properties:

```python
# Python
# Create an email with various properties
properties_email = EmailDto(
    _from=MailAddress("Sender", "sender@example.com"),
    to=[MailAddress("Recipient", "recipient@example.com")],
    subject="Email with Various Properties",
    body="This is the email body.",
    
    # Additional properties
    delivery_notification_options=["OnSuccess", "OnFailure"],  # Request delivery notifications
    importance="High",  # Set importance (Low, Normal, High)
    is_draft=True,  # Mark as draft
    is_read=False,  # Mark as unread
    read_receipt_to=MailAddress("Receipt Recipient", "receipt@example.com")  # Request read receipt
)

# Save the email
api.email.save(EmailSaveRequest(
    storage_file=StorageFileLocation(storage, folder, "properties_email.eml"),
    value=properties_email,
    format="Eml"
))

print("Email with various properties saved")
```

## Troubleshooting Tips

If you encounter issues when working with email message files:

1. File format compatibility: 
   - Not all email clients support all formats equally
   - MSG files are best for Microsoft Outlook
   - EML files are more widely supported across different clients

2. HTML content:
   - Not all email clients render HTML the same way
   - Include a plain text body as a fallback
   - Test your HTML emails in multiple clients

3. Storage issues:
   - Verify that the storage, folder, and file paths are correct
   - Ensure you have sufficient permissions on the storage

4. Conversion problems:
   - Check that the source and target formats are correctly specified
   - Ensure the source file is a valid email file

5. Attachment handling:
   - Large attachments may require special handling
   - Ensure base64 encoding/decoding is done correctly

## What You've Learned

In this tutorial, you've learned how to:
- Create email message files using the EmailDto model
- Add attachments and embedded images to emails
- Save emails in various formats to cloud storage
- Download and convert emails between different formats
- Retrieve emails as EmailDto objects for processing
- Configure various email properties like importance and delivery notifications
- Append emails to email account folders

## Further Practice

To reinforce your learning:
1. Create emails with multiple types of attachments (text, images, documents)
2. Build an HTML email template system that generates personalized emails
3. Create a batch conversion utility to convert multiple emails between formats
4. Create an email archiving system that downloads, processes, and stores emails
5. Build an email composer with a web interface using the EmailDto model

## Next Steps

Consider exploring these related tutorials:
- [Quick Start With Email Client](/email-cloud/quick-start-with-email-client/) - Learn how to work with email clients
- [Introduction to MAPI Message Files API](/email-cloud/mapi-message-files/) - For more advanced email processing

## Helpful Resources

- [Product Page](https://products.aspose.cloud/email/)
- [Documentation](https://docs.aspose.cloud/email/)
- [Live Demo](https://products.aspose.app/email/family)
- [API Reference](https://reference.aspose.cloud/email/)
- [Blog](https://blog.aspose.cloud/category/email/)
- [Free Support](https://forum.aspose.cloud/c/email/9/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out to us on the [Aspose.Email Cloud forum](https://forum.aspose.cloud/c/email/9/).