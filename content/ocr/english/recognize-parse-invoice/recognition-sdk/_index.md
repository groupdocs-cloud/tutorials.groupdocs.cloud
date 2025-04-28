---
title: Complete Guide to Invoice Processing with Aspose.OCR Cloud SDK
weight: 30
url: /recognize-parse-invoice/recognition-sdk/
description: Learn how to simplify invoice recognition and parsing using the Aspose.OCR Cloud SDK in this hands-on tutorial for developers.
---

# Tutorial: Invoice Processing with Aspose.OCR Cloud SDK

## Learning Objectives

In this tutorial, you'll learn how to:
- Set up and configure the Aspose.OCR Cloud SDK in your projects
- Implement complete invoice recognition with just a few lines of code
- Configure recognition settings for optimal results
- Process and utilize the structured invoice data in your applications
- Handle exceptions and implement error recovery

## Prerequisites

Before starting this tutorial, you should have:
- An [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps) with active subscription or free trial
- Basic knowledge of your preferred programming language (.NET, Java, Python, etc.)
- Development environment set up for your chosen language
- Sample invoice images for testing

## Understanding the SDK Advantage

While you can directly use the REST API to process invoices (as shown in previous tutorials), the Aspose.OCR Cloud SDK offers significant advantages:

- Simplicity: Complex API interactions are wrapped into simple method calls
- Type Safety: Language-specific types instead of raw JSON handling
- Error Handling: Built-in exception management for API errors
- Authentication: Automatic token management
- Response Parsing: Automatic deserialization of responses

In this tutorial, we'll focus on using the .NET SDK, but the concepts apply to all supported languages.

## Step 1: Setting Up the SDK

Let's begin by installing and configuring the Aspose.OCR Cloud SDK in your project.

### For .NET Projects

1. Install the NuGet Package:

   Using the Package Manager Console:
   ```
   Install-Package Aspose.OCR-Cloud
   ```

   Or using the .NET CLI:
   ```
   dotnet add package Aspose.OCR-Cloud
   ```

2. Add the Necessary Namespaces:

   ```csharp
   using Aspose.OCR.Cloud.SDK.Api;
   using Aspose.OCR.Cloud.SDK.Model;
   using System.Text;
   ```

3. Initialize the API Client:

   ```csharp
   // Replace with your actual credentials
   string clientId = "YOUR_CLIENT_ID";
   string clientSecret = "YOUR_CLIENT_SECRET";
   
   // Create the API client
   RecognizeAndParseInvoiceApi api = new RecognizeAndParseInvoiceApi(clientId, clientSecret);
   ```

### Try it yourself:
Set up a new project in your preferred development environment and install the Aspose.OCR Cloud SDK package. Then initialize the API client with your credentials.

## Step 2: Preparing Your Invoice Image

The SDK handles most of the complex operations, but you still need to prepare your invoice image:

```csharp
// Read invoice image to array of bytes
byte[] invoice = File.ReadAllBytes("invoice.png");

// Specify recognition language and settings
OCRSettingsRecognizeAndParseInvoice recognitionSettings = new OCRSettingsRecognizeAndParseInvoice {
    Language = Language.English,
    MakeSkewCorrect = true,
    MakeSpellCheck = true
};
```

The settings object allows you to customize various recognition parameters, similar to what we discussed in the previous tutorials:

| Setting | Type | Description |
|---------|------|-------------|
| Language | enum | Recognition language (English, French, German, etc.) |
| MakeSkewCorrect | bool | Automatically correct tilted images |
| Rotate | int | Manually rotate image (0, 90, 180, 270 degrees) |
| MakeBinarization | bool | Convert to black and white |
| MakeUpsampling | bool | Improve small text recognition |
| MakeSpellCheck | bool | Correct common misspellings |

## Step 3: Sending the Invoice for Processing

With the SDK, sending an invoice for processing is reduced to just a couple of lines of code:

```csharp
// Prepare the request body
OCRRecognizeAndParseInvoiceBody source = new OCRRecognizeAndParseInvoiceBody(invoice, recognitionSettings);

// Send the invoice for processing
string taskID = api.PostRecognizeReceipt(source);

Console.WriteLine($"Task ID: {taskID}");
```

The `PostRecognizeReceipt` method sends your invoice to the Aspose.OCR Cloud service and returns a task ID that you can use to fetch the results.

### Learning Checkpoint:
What's the advantage of using the SDK's `OCRRecognizeAndParseInvoiceBody` class instead of manually creating a JSON request?

<details>
<summary>Answer</summary>
The SDK class handles proper formatting, validation, and serialization of the request automatically. It also provides type safety and IntelliSense support in your IDE, reducing the chance of errors compared to manually constructing JSON.
</details>

## Step 4: Fetching and Processing Results

Once you've submitted the invoice for processing, you can fetch the results using the task ID:

```csharp
// Fetch recognition result
OCRResponse result = api.GetRecognizeAndParseInvoice(taskID);

// Check if processing is complete
if (result.TaskStatus == TaskStatus.Completed)
{
    // Decode the Base64 data
    string jsonData = Encoding.UTF8.GetString(result.Results[0].Data);
    
    // Display or process the JSON data
    Console.WriteLine(jsonData);
}
else if (result.TaskStatus == TaskStatus.Processing || result.TaskStatus == TaskStatus.Pending)
{
    Console.WriteLine("The task is still being processed. Try again later.");
}
else
{
    Console.WriteLine($"Error: {result.Error?.Messages?.FirstOrDefault() ?? "Unknown error"}");
}
```

Unlike the direct REST API approach, the SDK already handles the HTTP requests and response parsing, giving you a strongly typed `OCRResponse` object.

### Try it yourself:
Implement a function that polls for results until the task is complete, with a timeout mechanism.

## Step 5: Parsing the Invoice Data

After fetching and decoding the results, you'll need to parse the JSON data into a usable format. Let's create a class for the invoice data:

```csharp
using System.Text.Json;

// Define a class to represent the invoice data
public class InvoiceData
{
    public string issue_date { get; set; }
    public string due_date { get; set; }
    public string supplier_name { get; set; }
    public string supplier_address { get; set; }
    public string supplier_email { get; set; }
    public string supplier_phone { get; set; }
    public string supplier_tax_id { get; set; }
    public string receiver_name { get; set; }
    public string receiver_address { get; set; }
    public string receiver_tax_id { get; set; }
    public string currency { get; set; }
    public double total_amount { get; set; }
    public double vat { get; set; }
    public double net_amount { get; set; }
    public string bank_name { get; set; }
    public string bic { get; set; }
    public string account { get; set; }
}

// Parse the JSON data into an InvoiceData object
public InvoiceData ParseInvoiceData(string jsonData)
{
    return JsonSerializer.Deserialize<InvoiceData>(jsonData);
}
```

Now you can use this class to work with the extracted invoice data:

```csharp
// Parse the JSON data
InvoiceData invoice = ParseInvoiceData(jsonData);

// Use the structured data in your application
Console.WriteLine($"Supplier: {invoice.supplier_name}");
Console.WriteLine($"Issue Date: {invoice.issue_date}");
Console.WriteLine($"Total Amount: {invoice.total_amount} {invoice.currency}");
```

## Step 6: Implementing a Complete Solution

Let's put everything together into a complete solution that handles the entire invoice processing workflow:

```csharp
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;
using System;
using System.IO;
using System.Linq;
using System.Text;
using System.Text.Json;
using System.Threading;

class Program
{
    static void Main(string[] args)
    {
        try
        {
            // Initialize API client
            RecognizeAndParseInvoiceApi api = new RecognizeAndParseInvoiceApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Process invoice
            string invoicePath = "invoice.png";
            InvoiceData invoiceData = ProcessInvoice(api, invoicePath);
            
            // Display results
            if (invoiceData != null)
            {
                DisplayInvoiceData(invoiceData);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
    
    static InvoiceData ProcessInvoice(RecognizeAndParseInvoiceApi api, string invoicePath)
    {
        Console.WriteLine("Reading invoice file...");
        byte[] invoiceBytes = File.ReadAllBytes(invoicePath);
        
        Console.WriteLine("Setting up recognition parameters...");
        OCRSettingsRecognizeAndParseInvoice settings = new OCRSettingsRecognizeAndParseInvoice
        {
            Language = Language.English,
            MakeSkewCorrect = true,
            MakeSpellCheck = true
        };
        
        Console.WriteLine("Sending invoice for processing...");
        OCRRecognizeAndParseInvoiceBody requestBody = new OCRRecognizeAndParseInvoiceBody(invoiceBytes, settings);
        string taskId = api.PostRecognizeReceipt(requestBody);
        
        Console.WriteLine($"Invoice submitted. Task ID: {taskId}");
        
        // Poll for results with timeout
        return WaitForResults(api, taskId);
    }
    
    static InvoiceData WaitForResults(RecognizeAndParseInvoiceApi api, string taskId)
    {
        int maxAttempts = 30;  // Maximum number of polling attempts
        int delaySeconds = 2;   // Delay between polling attempts
        
        Console.WriteLine("Waiting for processing to complete...");
        
        for (int attempt = 1; attempt <= maxAttempts; attempt++)
        {
            Console.WriteLine($"Checking status (attempt {attempt}/{maxAttempts})...");
            
            OCRResponse response = api.GetRecognizeAndParseInvoice(taskId);
            
            switch (response.TaskStatus)
            {
                case TaskStatus.Completed:
                    Console.WriteLine("Processing completed successfully!");
                    string jsonData = Encoding.UTF8.GetString(response.Results[0].Data);
                    return JsonSerializer.Deserialize<InvoiceData>(jsonData);
                    
                case TaskStatus.Error:
                    string errorMessage = response.Error?.Messages?.FirstOrDefault() ?? "Unknown error";
                    Console.WriteLine($"Processing failed: {errorMessage}");
                    return null;
                    
                case TaskStatus.NotExist:
                    Console.WriteLine("Task not found or expired.");
                    return null;
                    
                default:
                    // Still processing or pending, wait and try again
                    Console.WriteLine($"Status: {response.TaskStatus}. Waiting {delaySeconds} seconds...");
                    Thread.Sleep(delaySeconds * 1000);
                    break;
            }
        }
        
        Console.WriteLine("Timeout reached. Process did not complete within the allowed time.");
        return null;
    }
    
    static void DisplayInvoiceData(InvoiceData invoice)
    {
        Console.WriteLine("\n--- Invoice Data ---");
        Console.WriteLine($"Supplier: {invoice.supplier_name}");
        Console.WriteLine($"Supplier Contact: {invoice.supplier_phone} | {invoice.supplier_email}");
        Console.WriteLine($"Receiver: {invoice.receiver_name}");
        Console.WriteLine($"Issue Date: {invoice.issue_date}");
        Console.WriteLine($"Due Date: {invoice.due_date}");
        Console.WriteLine($"Total Amount: {invoice.total_amount} {invoice.currency.ToUpper()}");
        
        if (invoice.vat >= 0)
        {
            Console.WriteLine($"VAT: {invoice.vat}%");
        }
        
        Console.WriteLine($"Net Amount: {invoice.net_amount} {invoice.currency.ToUpper()}");
        
        if (!string.IsNullOrEmpty(invoice.bank_name))
        {
            Console.WriteLine("\n--- Payment Details ---");
            Console.WriteLine($"Bank: {invoice.bank_name}");
            Console.WriteLine($"Account: {invoice.account}");
            if (!string.IsNullOrEmpty(invoice.bic))
            {
                Console.WriteLine($"BIC/SWIFT: {invoice.bic}");
            }
        }
    }
}

// Invoice data class definition
public class InvoiceData
{
    public string issue_date { get; set; }
    public string due_date { get; set; }
    public string supplier_name { get; set; }
    public string supplier_address { get; set; }
    public string supplier_email { get; set; }
    public string supplier_phone { get; set; }
    public string supplier_tax_id { get; set; }
    public string receiver_name { get; set; }
    public string receiver_address { get; set; }
    public string receiver_tax_id { get; set; }
    public string currency { get; set; }
    public double total_amount { get; set; }
    public double vat { get; set; }
    public double net_amount { get; set; }
    public string bank_name { get; set; }
    public string bic { get; set; }
    public string account { get; set; }
}
```

### Try it yourself:
Copy this code into your development environment, replace the placeholder credentials, and run it with a real invoice image.

## Step 7: Advanced Error Handling

In a production environment, you'll need robust error handling to deal with various scenarios. Let's enhance our solution:

```csharp
try
{
    // Initialize API client with error handling for authentication issues
    RecognizeAndParseInvoiceApi api;
    try
    {
        api = new RecognizeAndParseInvoiceApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
    }
    catch (Exception authEx)
    {
        Console.WriteLine($"Authentication failed: {authEx.Message}");
        Console.WriteLine("Please check your Client ID and Client Secret.");
        return;
    }
    
    // Process invoice with proper file validation
    string invoicePath = "invoice.png";
    if (!File.Exists(invoicePath))
    {
        Console.WriteLine($"Error: File not found at {invoicePath}");
        return;
    }
    
    // Check file size
    FileInfo fileInfo = new FileInfo(invoicePath);
    if (fileInfo.Length > 20 * 1024 * 1024) // 20MB limit
    {
        Console.WriteLine("Error: File is too large. Maximum size is 20MB.");
        return;
    }
    
    // Check file extension
    string extension = Path.GetExtension(invoicePath).ToLower();
    string[] supportedExtensions = { ".png", ".jpg", ".jpeg", ".tif", ".tiff", ".pdf" };
    if (!supportedExtensions.Contains(extension))
    {
        Console.WriteLine($"Error: Unsupported file format. Supported formats are: {string.Join(", ", supportedExtensions)}");
        return;
    }
    
    // Process with try-catch for API exceptions
    try
    {
        InvoiceData invoiceData = ProcessInvoice(api, invoicePath);
        if (invoiceData != null)
        {
            DisplayInvoiceData(invoiceData);
        }
    }
    catch (Exception processEx)
    {
        Console.WriteLine($"Processing error: {processEx.Message}");
        Console.WriteLine("Please try again or check your invoice image quality.");
    }
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
```

## Step 8: Working with Multiple Programming Languages

While we've focused on C# in this tutorial, the Aspose.OCR Cloud SDK is available for multiple programming languages. Here's how you can achieve the same functionality in other languages:

### Python Example

```python
# Import the required modules
import os
import time
import json
import base64
from aspose_ocr_cloud import Configuration, ApiClient, RecognizeAndParseInvoiceApi
from aspose_ocr_cloud.models import OCRSettingsRecognizeAndParseInvoice, OCRRecognizeAndParseInvoiceBody

def recognize_invoice(client_id, client_secret, invoice_path):
    # Configure API client
    configuration = Configuration(client_id=client_id, client_secret=client_secret)
    api_client = ApiClient(configuration)
    api = RecognizeAndParseInvoiceApi(api_client)
    
    # Read the invoice file
    with open(invoice_path, "rb") as file:
        invoice_data = file.read()
    
    # Configure recognition settings
    settings = OCRSettingsRecognizeAndParseInvoice(
        language="English",
        make_skew_correct=True,
        make_spell_check=True
    )
    
    # Create request body
    request_body = OCRRecognizeAndParseInvoiceBody(
        image=invoice_data,
        settings=settings
    )
    
    # Submit invoice for processing
    task_id = api.post_recognize_receipt(request_body)
    print(f"Invoice submitted. Task ID: {task_id}")
    
    # Poll for results
    max_attempts = 30
    delay_seconds = 2
    
    for attempt in range(max_attempts):
        print(f"Checking status (attempt {attempt+1}/{max_attempts})...")
        response = api.get_recognize_and_parse_invoice(task_id)
        
        if response.task_status == "Completed":
            print("Processing completed successfully!")
            # Decode the Base64 data
            json_data = base64.b64decode(response.results[0].data).decode('utf-8')
            return json.loads(json_data)
        elif response.task_status in ["Error", "NotExist"]:
            error_msg = "Unknown error"
            if response.error and response.error.messages:
                error_msg = response.error.messages[0]
            print(f"Processing failed: {error_msg}")
            return None
        else:
            print(f"Status: {response.task_status}. Waiting {delay_seconds} seconds...")
            time.sleep(delay_seconds)
    
    print("Timeout reached. Process did not complete within the allowed time.")
    return None

# Example usage
if __name__ == "__main__":
    client_id = "YOUR_CLIENT_ID"
    client_secret = "YOUR_CLIENT_SECRET"
    invoice_path = "invoice.png"
    
    invoice_data = recognize_invoice(client_id, client_secret, invoice_path)
    
    if invoice_data:
        print("\n--- Invoice Data ---")
        print(f"Supplier: {invoice_data.get('supplier_name', 'N/A')}")
        print(f"Issue Date: {invoice_data.get('issue_date', 'N/A')}")
        print(f"Total Amount: {invoice_data.get('total_amount', 'N/A')} "
              f"{invoice_data.get('currency', '').upper()}")
```

## Step 9: Integrating with Business Applications

Once you've extracted the invoice data, you can integrate it with various business applications:

1. Accounting Software Integration:
   ```csharp
   public void ExportToAccountingSoftware(InvoiceData invoice)
   {
       // Example: Format data for QuickBooks, Xero, or other accounting software
       var accountingEntry = new
       {
           InvoiceNumber = GenerateInvoiceNumber(invoice),
           VendorName = invoice.supplier_name,
           Date = DateTime.Parse(invoice.issue_date),
           DueDate = string.IsNullOrEmpty(invoice.due_date) ? 
                     DateTime.Parse(invoice.issue_date).AddDays(30) : 
                     DateTime.Parse(invoice.due_date),
           Amount = invoice.total_amount,
           TaxAmount = invoice.total_amount - invoice.net_amount,
           Currency = invoice.currency.ToUpper()
       };
       
       // Export to accounting system
       // YourAccountingSystemAPI.CreateInvoice(accountingEntry);
   }
   ```

2. Database Storage:
   ```csharp
   public void SaveToDatabase(InvoiceData invoice)
   {
       // Example using ADO.NET (you would replace with your actual DB implementation)
       using (var connection = new SqlConnection("YOUR_CONNECTION_STRING"))
       {
           connection.Open();
           using (var command = new SqlCommand())
           {
               command.Connection = connection;
               command.CommandText = @"
                   INSERT INTO Invoices (
                       SupplierName, SupplierEmail, SupplierPhone, 
                       ReceiverName, IssueDate, DueDate, 
                       TotalAmount, NetAmount, VAT, Currency
                   ) VALUES (
                       @SupplierName, @SupplierEmail, @SupplierPhone,
                       @ReceiverName, @IssueDate, @DueDate,
                       @TotalAmount, @NetAmount, @VAT, @Currency
                   )";
                   
               command.Parameters.AddWithValue("@SupplierName", invoice.supplier_name);
               command.Parameters.AddWithValue("@SupplierEmail", 
                   string.IsNullOrEmpty(invoice.supplier_email) ? DBNull.Value : (object)invoice.supplier_email);
               // Add remaining parameters
               
               command.ExecuteNonQuery();
           }
       }
   }
   ```

### Learning Checkpoint:
What additional validation might you want to add before importing invoice data into your accounting system?

<details>
<summary>Answer</summary>
Important validations include: checking for duplicate invoices (to prevent double payments), validating date formats, ensuring tax calculations are correct (net + tax = total), verifying supplier details against your vendor database, and checking for any unusual values that could indicate recognition errors.
</details>

## What You've Learned

In this tutorial, you've learned how to:

- Set up and configure the Aspose.OCR Cloud SDK in your projects
- Create a complete invoice processing solution with minimal code
- Handle errors and implement robust polling mechanisms
- Parse and work with structured invoice data
- Integrate invoice data with business applications

The SDK approach significantly simplifies the development process compared to direct API integration, allowing you to focus on your business logic rather than the technical details of OCR implementation.

## Further Practice

To reinforce your learning:

1. Create a simple invoice processing application with a user interface
2. Add support for batch processing multiple invoices
3. Implement validation rules for the extracted data
4. Create reports and visualizations based on processed invoices
5. Build a workflow that routes invoices for approval based on extracted data

## Conclusion

The Aspose.OCR Cloud SDK provides a powerful yet simple way to integrate invoice recognition and parsing into your applications. By wrapping the complexity of the REST API calls into simple method invocations, it allows you to quickly implement OCR functionality without worrying about the technical details of HTTP requests, authentication, and response parsing.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post in our [community forum](https://forum.aspose.cloud/c/ocr/12/) for assistance.