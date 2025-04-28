---
title: Getting Started with Email Client in Aspose.Email Cloud API Tutorial
url: /email-cloud/quick-start-with-email-client/
description: Learn how to set up and use the built-in email client in Aspose.Email Cloud API for sending, receiving, and managing emails in this step-by-step tutorial.
weight: 30
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Set up and configure an email client using Aspose.Email Cloud API
- Retrieve and search emails from your account
- Read email content and mark messages as read
- Send emails through your email account
- Delete unwanted messages
- Append new messages to specific folders

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account (sign up at [dashboard.aspose.cloud](https://dashboard.aspose.cloud/))
2. Your Aspose Cloud credentials (Client ID and Client Secret)
3. The Aspose.Email Cloud SDK for your preferred programming language installed
4. Access to an email account (Gmail, Outlook, or other provider)
5. Email account credentials (username/email and password or OAuth tokens)

## Introduction to Aspose.Email Cloud Built-in Email Client

The Aspose.Email Cloud API provides a powerful built-in email client that supports multiple email protocols:

- SMTP (Simple Mail Transfer Protocol) - for sending emails
- POP3 (Post Office Protocol) - for retrieving emails
- IMAP (Internet Message Access Protocol) - for accessing and managing emails on a server
- EWS (Exchange Web Services) - for interacting with Microsoft Exchange Server
- WebDav - for accessing mailboxes via WebDav protocol
- Virtual multi-account - a feature that allows you to work with multiple accounts as a single one

This unified API makes it easy to work with different email providers through a consistent interface, abstracting away the complexities of individual protocols.

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

### Step 3: Configure an Email Account for Receiving Messages

First, we'll set up an IMAP account to receive and manage emails:

```python
# Python
from asposeemail.models import EmailClientAccountPasswordCredentials, EmailClientAccount, StorageFileLocation, ClientAccountSaveRequest

# Set up email account credentials
credentials = EmailClientAccountPasswordCredentials(
    login="example@gmail.com", 
    password="your_password"
)

# Create email client account configuration
receive_account = EmailClientAccount(
    host="imap.gmail.com", 
    port=993, 
    protocol_type="IMAP", 
    security_options="SSLAuto", 
    credentials=credentials
)

# Define storage location for the account file
storage_name = "First Storage"
account_folder = "email/accounts"
imap_account = "imap.account"
imap_location = StorageFileLocation(
    storage=storage_name, 
    folder_path=account_folder, 
    file_name=imap_account
)

# Save account configuration to cloud storage
api.client.account.save(ClientAccountSaveRequest(
    storage_file=imap_location, 
    value=receive_account
))
```

Let's see how to do this in other programming languages:

### C# Example

```csharp
using Aspose.Email.Cloud.Sdk.Api;
using Aspose.Email.Cloud.Sdk.Model;

// Initialize API client
var api = new EmailCloud("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Set up email account credentials
var credentials = new EmailClientAccountPasswordCredentials { 
    Login = "example@gmail.com", 
    Password = "your_password" 
};

// Create email client account configuration
var receiveAccount = new EmailClientAccount {
    Host = "imap.gmail.com", 
    Port = 993, 
    ProtocolType = "IMAP", 
    SecurityOptions = "SSLAuto", 
    Credentials = credentials
};

// Define storage location for the account file
var storageName = "First Storage";
var accountFolder = "email/accounts";
var imapAccount = "imap.account";
var imapLocation = new StorageFileLocation(storageName, accountFolder, imapAccount);

// Save account configuration to cloud storage
await api.Client.Account.SaveAsync(new ClientAccountSaveRequest(
    imapLocation, receiveAccount));
```

### Java Example

```java
import com.aspose.email.cloud.sdk.api.EmailCloud;
import com.aspose.email.cloud.sdk.model.*;

// Initialize API client
EmailCloud api = new EmailCloud("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Set up email account credentials
EmailClientAccountPasswordCredentials credentials =
    new EmailClientAccountPasswordCredentials(
        "example@gmail.com", "your_password");

// Create email client account configuration
EmailClientAccount receiveAccount = new EmailClientAccount(
    "imap.gmail.com", 993, "SSLAuto", "IMAP", credentials, null);

// Define storage location for the account file
String storageName = "First Storage";
String accountFolder = "email/accounts";
String imapAccount = "imap.account";
StorageFileLocation imapLocation = new StorageFileLocation(
    storageName, accountFolder, imapAccount);

// Save account configuration to cloud storage
api.client().account().save(new ClientAccountSaveRequest(
    imapLocation, receiveAccount));
```

> Note: When using Gmail, you might need to use an App Password instead of your regular password if you have 2-factor authentication enabled. Visit [Google Account Security](https://myaccount.google.com/security) to create an App Password.

### Step 4: Search for Emails

Now that we have configured our email account, let's search for emails in the INBOX folder:

```python
# Python
from asposeemail.models import ClientMessageListRequest

# Define the folder to search in
folder_name = "INBOX"

# Create a search request with a query string
# This example searches for emails with a sender containing ".com"
messages = api.client.message.list(ClientMessageListRequest(
    folder=folder_name,
    account=imap_account,
    query_string="('From' Contains '.com')",
    storage=storage_name,
    account_storage_folder=account_folder,
    recursive=True,  # Search in subfolders
    type="Dto"       # Return emails as EmailDto objects
))

# Print the number of found messages
print(f"Found {len(messages.value)} messages")

# Print basic info about each message
for msg in messages.value:
    email = msg.value  # For 'Dto' type, the actual email is in the 'value' property
    print(f"Subject: {email.subject}")
    print(f"From: {email.from_.address}")
    print(f"Date: {email.date}")
    print("-" * 30)
```

The `query_string` parameter uses a query language to filter emails. Some examples:

- `('From' Contains 'john')` - Emails from addresses containing "john"
- `('Subject' Contains 'important')` - Emails with "important" in the subject
- `('To' Contains 'me@example.com')` - Emails sent to a specific address
- `('Date' Le '2023-04-30')` - Emails older than April 30, 2023
- Combined queries: `('Subject' Contains 'report') And ('Unread' Equal 'true')`

### Step 5: Fetch a Specific Email by ID

Once you've found messages of interest, you can fetch the complete content of a specific email using its ID:

```python
# Python
from asposeemail.models import ClientMessageFetchRequest

# Assuming we have at least one message from the previous step
if len(messages.value) > 0:
    # Get the first message from the list
    found_message = messages.value[0]
    message_id = found_message.value.message_id
    
    # Fetch the complete message
    email = api.client.message.fetch(ClientMessageFetchRequest(
        message_id=message_id,
        account=imap_account,
        folder=folder_name,  # For IMAP, we need to specify the folder
        storage=storage_name,
        account_storage_folder=account_folder,
        type="Dto"
    ))
    
    # Access email content
    print(f"Subject: {email.value.subject}")
    print(f"From: {email.value.from_.address}")
    
    # Print email body
    if email.value.body:
        print("\nBody (Plain Text):")
        print(email.value.body)
    
    # Print HTML body if available
    if email.value.html_body:
        print("\nBody (HTML):")
        print(email.value.html_body)
    
    # List attachments if any
    if email.value.attachments and len(email.value.attachments) > 0:
        print("\nAttachments:")
        for attachment in email.value.attachments:
            print(f"- {attachment.name} ({attachment.size} bytes)")
```

### Step 6: Mark an Email as Read

After reading an email, you might want to mark it as read:

```python
# Python
from asposeemail.models import ClientMessageSetIsReadRequest

# Mark the message as read
api.client.message.set_is_read(ClientMessageSetIsReadRequest(
    account_location=imap_location,
    message_id=message_id,
    is_read=True
))

print(f"Message {message_id} marked as read")
```

### C# Example

```csharp
// Mark the message as read
await api.Client.Message.SetIsReadAsync(
    new ClientMessageSetIsReadRequest(
        imapLocation, messageId, true));

Console.WriteLine($"Message {messageId} marked as read");
```

### Step 7: Configure an Account for Sending Emails

Now, let's set up an SMTP account for sending emails:

```python
# Python
# Create SMTP account configuration
smtp_account = EmailClientAccount(
    host="smtp.gmail.com", 
    port=465, 
    protocol_type="SMTP", 
    security_options="SSLAuto", 
    credentials=credentials  # We can reuse the credentials from before
)

# Define storage location for the SMTP account file
smtp_account_filename = "smtp.account"
smtp_location = StorageFileLocation(
    storage=storage_name, 
    folder_path=account_folder, 
    file_name=smtp_account_filename
)

# Save SMTP account configuration to cloud storage
api.client.account.save(ClientAccountSaveRequest(
    storage_file=smtp_location, 
    value=smtp_account
))
```

### Step 8: Send an Email

With our SMTP account configured, we can now send an email:

```python
# Python
from asposeemail.models import EmailDto, MailAddress, MailMessageDto, ClientMessageSendRequest

# Create an email
email = EmailDto(
    _from=MailAddress(address="example@gmail.com"),
    to=[MailAddress(address="recipient@example.com")],
    subject="Hello from Aspose.Email Cloud",
    body="This is a test email sent using Aspose.Email Cloud API."
)

    # Send the email
api.client.message.send(ClientMessageSendRequest(
    account_location=smtp_location,
    message=MailMessageDto(value=email)
))

print("Email sent successfully!")
```

### C# Example

```csharp
// Create an email
var email = new EmailDto
{
    From = new MailAddress { Address = "example@gmail.com" },
    To = new List<MailAddress> { new MailAddress { Address = "recipient@example.com" } },
    Subject = "Hello from Aspose.Email Cloud",
    Body = "This is a test email sent using Aspose.Email Cloud API."
};

// Send the email
await api.Client.Message.SendAsync(
    new ClientMessageSendRequest(
        smtpLocation, new MailMessageDto(email)));

Console.WriteLine("Email sent successfully!");
```

### Java Example

```java
// Create an email
EmailDto email = new EmailDto()
    .from(new MailAddress().address("example@gmail.com"))
    .addToItem(new MailAddress().address("recipient@example.com"))
    .subject("Hello from Aspose.Email Cloud")
    .body("This is a test email sent using Aspose.Email Cloud API.");

// Send the email
api.client().message().send(
    new ClientMessageSendRequest(
        smtpLocation, new MailMessageDto(email)));

System.out.println("Email sent successfully!");
```

### Step 9: Delete an Email

To delete unwanted emails, use the `delete` method:

```python
# Python
from asposeemail.models import ClientMessageDeleteRequest

# Delete the message
api.client.message.delete(ClientMessageDeleteRequest(
    account_location=imap_location,
    message_id=message_id,
    folder=folder_name
))

print(f"Message {message_id} deleted successfully")
```

### C# Example

```csharp
// Delete the message
await api.Client.Message.DeleteAsync(
    new ClientMessageDeleteRequest(
        imapLocation, messageId, folderName));

Console.WriteLine($"Message {messageId} deleted successfully");
```

### Step 10: Append a New Email to a Specific Folder

You can also add a new email directly to a specific folder in your email account:

```python
# Python
from asposeemail.models import ClientMessageAppendRequest

# Create a new email to append
new_email = EmailDto(
    _from=MailAddress(address="example@gmail.com"),
    to=[MailAddress(address="contact@example.com")],
    subject="Important document",
    body="Please find the attached document."
)

# Append the email to the "INBOX" folder
appended_message_id = api.client.message.append(ClientMessageAppendRequest(
    account_location=imap_location,
    folder="INBOX",
    message=MailMessageDto(value=new_email),
    mark_as_sent=True  # Mark the message as sent
))

print(f"Message appended to INBOX with ID: {appended_message_id.value}")
```

## Working with Advanced Email Features

The built-in email client in Aspose.Email Cloud API offers many advanced features:

### Working with Message Threads

Email threads group related messages together. Learn more in our [Email Client Threads Tutorial](/email-cloud/email-client-threads/).

### Using Search Queries

You can create complex search queries to find specific emails:

```python
# Python - Some advanced search query examples
# Find unread messages
unread_messages = api.client.message.list(ClientMessageListRequest(
    folder=folder_name,
    account=imap_account,
    query_string="('Unread' Equal 'true')",
    storage=storage_name,
    account_storage_folder=account_folder,
    type="Dto"
))

# Find messages with attachments
messages_with_attachments = api.client.message.list(ClientMessageListRequest(
    folder=folder_name,
    account=imap_account,
    query_string="('HasAttachments' Equal 'true')",
    storage=storage_name,
    account_storage_folder=account_folder,
    type="Dto"
))

# Find messages received in the last 7 days
from datetime import datetime, timedelta
one_week_ago = (datetime.now() - timedelta(days=7)).strftime('%Y-%m-%d')
recent_messages = api.client.message.list(ClientMessageListRequest(
    folder=folder_name,
    account=imap_account,
    query_string=f"('Date' Ge '{one_week_ago}')",
    storage=storage_name,
    account_storage_folder=account_folder,
    type="Dto"
))
```

### Working with Message Attachments

To access and download email attachments:

```python
# Python - Download an attachment from an email
if email.value.attachments and len(email.value.attachments) > 0:
    attachment = email.value.attachments[0]
    
    # The attachment data is available in Base64 format
    import base64
    
    # Decode the attachment data
    attachment_data = base64.b64decode(attachment.base64_data)
    
    # Save the attachment to a file
    with open(attachment.name, 'wb') as f:
        f.write(attachment_data)
        
    print(f"Attachment '{attachment.name}' saved successfully")
```

## Troubleshooting Tips

If you encounter issues with the email client:

1. Authentication errors: 
   - Verify your credentials are correct
   - For Gmail, make sure you've enabled less secure apps or are using App Passwords
   - Check that your account doesn't have two-factor authentication blocking API access

2. Connection issues:
   - Verify port numbers and server addresses
   - Check that SSL/TLS settings match your email provider's requirements
   - Ensure your network allows connections to these ports

3. Email retrieval problems:
   - Check that the folder name is correct (case-sensitive)
   - Verify that your query syntax is correct
   - Try listing all emails without a query to check connectivity

4. Sending issues:
   - Verify SMTP server settings
   - Check if your email provider has sending limits
   - Ensure your email content doesn't trigger spam filters

## What You've Learned

In this tutorial, you've learned how to:
- Configure email accounts for both receiving and sending emails
- Search for and retrieve emails using query filters
- Read email content including body and attachments
- Mark emails as read or unread
- Send emails through your account
- Delete unwanted messages
- Append new messages to specific folders

## Further Practice

To reinforce your learning:
1. Create a script to regularly check for new emails and process them
2. Implement a function to move emails between folders
3. Build a simple email auto-responder for specific senders
4. Create a backup system that saves all attachments from important emails
5. Implement a unified inbox from multiple email accounts using the virtual multi-account feature

## Next Steps

Consider exploring these related tutorials:
- [Email Client Threads](/email-cloud/email-client-threads/) - Learn how to work with email conversation threads
- [Email Message Files](/email-cloud/email-message-files/) - Explore how to work with email file formats server settings

## Helpful Resources

- [Product Page](https://products.aspose.cloud/email/)
- [Documentation](https://docs.aspose.cloud/email/)
- [Live Demo](https://products.aspose.app/email/family)
- [API Reference](https://reference.aspose.cloud/email/)
- [Blog](https://blog.aspose.cloud/category/email/)
- [Free Support](https://forum.aspose.cloud/c/email/9/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out to us on the [Aspose.Email Cloud forum](https://forum.aspose.cloud/c/email/9/).