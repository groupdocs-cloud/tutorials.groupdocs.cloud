---
title: How to Fetch Parsed Invoice Data from Aspose.OCR Cloud Tutorial
weight: 20
url: /recognize-parse-invoice/fetch-recognition-result/
description: Learn how to retrieve and process structured data extracted from your invoices using the Aspose.OCR Cloud API in this step-by-step tutorial.
---

# Tutorial: How to Fetch Parsed Invoice Data

## Learning Objectives

In this tutorial, you'll learn how to:
- Check the status of your invoice processing task
- Retrieve structured data from processed invoices
- Decode and parse the JSON response
- Work with all available invoice data fields
- Handle different task statuses and errors

## Prerequisites

Before starting this tutorial, you should have:
- Completed the previous tutorial on [sending invoices for recognition](/recognize-parse-invoice/send-for-recognition/)
- A task ID from a previously submitted invoice
- Basic understanding of JSON data format
- Your Aspose Cloud API credentials

## Understanding the Invoice Result Retrieval Process

After submitting an invoice for processing, Aspose.OCR Cloud queues the task to ensure stability even under high load. The processing typically takes a few seconds to complete, depending on the complexity of the invoice and the current system load.

In this tutorial, we'll focus on fetching and working with the processed invoice data.

## Step 1: Check If Processing Is Complete

Once you've submitted an invoice and received a task ID, you need to check if the processing has been completed before attempting to retrieve the results.

Use the following endpoint to fetch the current status:

```
GET https://api.aspose.cloud/v5.0/ocr/RecognizeAndParseInvoice
```

Here's a cURL example:

```bash
curl --request GET --location 'https://api.aspose.cloud/v5.0/ocr/RecognizeAndParseInvoice?id=YOUR_TASK_ID' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```

Replace `YOUR_TASK_ID` with the ID you received after submitting the invoice, and `YOUR_ACCESS_TOKEN` with your valid authentication token.

### Understanding Task Statuses

The response will include a `taskStatus` field with one of these values:

| Status | Description | Action |
|--------|-------------|--------|
| Pending | Task is queued but not yet processed | Wait and check again |
| Processing | Task is currently being processed | Wait and check again |
| Completed | Processing is finished successfully | Proceed to extract data |
| Error | Processing failed | Check error messages |
| NotExist | Invalid ID or result expired | Resubmit the invoice |

### Try it yourself:
Check the status of your invoice processing task. If it's not completed yet, wait a few seconds and try again.

## Step 2: Retrieve the Processed Invoice Data

Once the task status is "Completed", you can extract the processed data from the response. The result will be in JSON format with the following structure:

```json
{
  "id": "39b37b24-86e8-4e91-9a99-6c2574853eb5",
  "responseStatusCode": "Ok",
  "taskStatus": "Completed",
  "results": [
    {
      "type": "Text",
      "data": "eyJpc3N1ZV9kYXRlIjogIjIwMTctMTEtMjciL...sICJhY2NvdW50IjogIjJ4eHh4MG9veGsifQ=="
    }
  ],
  "error": null
}
```

The most important part is the `data` field inside the `results` array, which contains the Base64 encoded JSON of the parsed invoice.

## Step 3: Decode the Base64 Invoice Data

The extracted invoice data is provided as a Base64 encoded string. You'll need to decode it to get the actual JSON data:

Using cURL and command line:
```bash
echo "eyJpc3N1ZV9kYXRlIjogIjIwMTctMTEtMjciL...sICJhY2NvdW50IjogIjJ4eHh4MG9veGsifQ==" | base64 --decode
```

Using Python:
```python
import base64
import json

# Assume base64_data contains your Base64 encoded string
decoded_bytes = base64.b64decode(base64_data)
invoice_json = json.loads(decoded_bytes.decode('utf-8'))

# Now invoice_json contains the structured invoice data
print(json.dumps(invoice_json, indent=2))
```

### Try it yourself:
Extract the Base64 data from your API response and decode it to reveal the structured invoice data.

## Step 4: Understanding the Invoice Data Structure

Once decoded, you'll have access to all the extracted invoice fields in JSON format:

```json
{
  "issue_date": "2017-11-27",
  "due_date": "",
  "supplier_name": "abc exports",
  "supplier_address": "4300 longbeach blvd longbeach california 90807 united states",
  "supplier_email": "",
  "supplier_phone": "15627349957",
  "supplier_tax_id": "",
  "receiver_name": "abc imports",
  "receiver_address": "140 wecker road manstield brisbane queensland 4122 australia",
  "receiver_tax_id": "",
  "currency": "usd",
  "total_amount": 43550.0,
  "vat": -1,
  "net_amount": 43550.0,
  "bank_name": "bank of america",
  "bic": "",
  "account": "2xxxx0ooxk"
}
```

Here's a breakdown of all the available fields:

| Field | Data Type | Description | Notes |
|-------|-----------|-------------|-------|
| issue_date | string | Date when invoice was issued | Format: YYYY-MM-DD |
| due_date | string | Payment due date | Format: YYYY-MM-DD |
| supplier_name | string | Company that issued the invoice | |
| supplier_address | string | Full address of the supplier | Single string with all components |
| supplier_email | string | Contact email of the supplier | |
| supplier_phone | string | Contact phone number | As shown on invoice (not normalized) |
| supplier_tax_id | string | Tax identification number | |
| receiver_name | string | Company receiving the invoice | |
| receiver_address | string | Full address of the receiver | Single string with all components |
| receiver_tax_id | string | Tax identification number | |
| currency | string | Invoice currency code | Usually lowercase (e.g., "usd") |
| total_amount | number | Raw amount due | With decimal precision |
| vat | number | Value Added Tax percentage | -1 if not found |
| net_amount | number | Amount after tax calculation | With decimal precision |
| bank_name | string | Bank of the supplier | |
| bic | string | Bank identifier code | SWIFT or similar |
| account | string | Bank account number | May be partially masked |

### Learning Checkpoint:
What does it mean if the "vat" field has a value of -1?

<details>
<summary>Answer</summary>
A value of -1 for the "vat" field indicates that the VAT percentage was not found in the invoice. This could mean either the invoice doesn't include VAT, or the OCR engine couldn't confidently extract this information.
</details>

## Step 5: Implementing a Complete Solution

Let's look at a complete example that submits an invoice and polls for the result until it's ready:

### Python Example
```python
import requests
import base64
import json
import time

# Authentication
def get_token(client_id, client_secret):
    auth_url = "https://api.aspose.cloud/connect/token"
    auth_data = {
        "grant_type": "client_credentials",
        "client_id": client_id,
        "client_secret": client_secret
    }
    auth_headers = {
        "Content-Type": "application/x-www-form-urlencoded"
    }
    
    auth_response = requests.post(auth_url, data=auth_data, headers=auth_headers)
    return auth_response.json().get("access_token")

# Send invoice for processing
def submit_invoice(token, image_path):
    # Read and encode image
    with open(image_path, "rb") as image_file:
        encoded_string = base64.b64encode(image_file.read()).decode('utf-8')
    
    url = "https://api.aspose.cloud/v5.0/ocr/RecognizeAndParseInvoice"
    headers = {
        "Accept": "text/plain",
        "Content-Type": "application/json",
        "Authorization": f"Bearer {token}"
    }
    payload = {
        "image": encoded_string,
        "settings": {
            "language": "English",
            "makeSkewCorrect": True,
            "makeSpellCheck": True
        }
    }
    
    response = requests.post(url, headers=headers, data=json.dumps(payload))
    return response.text

# Check result status and get data when ready
def get_invoice_data(token, task_id):
    url = f"https://api.aspose.cloud/v5.0/ocr/RecognizeAndParseInvoice?id={task_id}"
    headers = {
        "Authorization": f"Bearer {token}"
    }
    
    max_attempts = 10
    attempt = 0
    
    while attempt < max_attempts:
        response = requests.get(url, headers=headers)
        result = response.json()
        
        # Check task status
        status = result.get("taskStatus")
        print(f"Attempt {attempt+1}: Status = {status}")
        
        if status == "Completed":
            # Extract and decode Base64 data
            base64_data = result["results"][0]["data"]
            decoded_bytes = base64.b64decode(base64_data)
            invoice_data = json.loads(decoded_bytes.decode('utf-8'))
            return invoice_data
        elif status == "Error":
            error_msg = result.get("error", {}).get("messages", ["Unknown error"])
            raise Exception(f"Processing failed: {error_msg}")
        elif status == "NotExist":
            raise Exception("Task ID not found or expired")
        
        # Wait before checking again
        attempt += 1
        time.sleep(2)  # Wait 2 seconds between checks
    
    raise Exception("Processing timeout - too many attempts")

# Main process
def process_invoice(client_id, client_secret, image_path):
    # Get authentication token
    token = get_token(client_id, client_secret)
    
    # Submit invoice
    print("Submitting invoice...")
    task_id = submit_invoice(token, image_path)
    print(f"Task ID: {task_id}")
    
    # Wait for processing and get results
    print("Waiting for processing to complete...")
    try:
        invoice_data = get_invoice_data(token, task_id)
        print("\nExtracted Invoice Data:")
        print(json.dumps(invoice_data, indent=2))
        return invoice_data
    except Exception as e:
        print(f"Error: {e}")
        return None

# Example usage
if __name__ == "__main__":
    client_id = "YOUR_CLIENT_ID"
    client_secret = "YOUR_CLIENT_SECRET"
    invoice_path = "invoice.png"
    
    result = process_invoice(client_id, client_secret, invoice_path)
```

### Try it yourself:
Adapt the above example to your preferred programming language and test it with a real invoice.

## Troubleshooting Common Issues

| Problem | Possible Solution |
|---------|------------------|
| Results not available | Check if you're using the correct task ID |
| Decoding errors | Ensure you're extracting only the Base64 string from "data" field |
| Missing invoice fields | Some fields may not be present if they weren't on the invoice |
| Error status | Check the error messages in the response for details |
| Task expires | Results are available for 24 hours after processing |

## Working with Extracted Invoice Data

Once you have the structured invoice data, you can use it for various purposes:

1. Database Storage: Import the data into your accounting or ERP system
2. Analysis: Group invoices by supplier or calculate total expenses
3. Verification: Cross-check amounts and supplier details
4. Automation: Trigger payment workflows based on due dates

### Learning Checkpoint:
Which field should you check to determine if VAT was applied to the invoice?

<details>
<summary>Answer</summary>
You should check both the "vat" field (for the percentage) and compare the "total_amount" with the "net_amount". If they differ and the "vat" value is not -1, then VAT was applied to the invoice.
</details>

## What You've Learned

In this tutorial, you've learned how to:

- Poll the Aspose.OCR Cloud API to check the status of processing tasks
- Retrieve and decode the Base64-encoded invoice data
- Work with the structured JSON representation of an invoice
- Handle various task statuses and potential errors
- Implement a complete solution for invoice processing

## Further Practice

To reinforce your learning:

1. Process multiple invoices and compare the extracted data
2. Create a function to validate the extracted data (e.g., check date formats)
3. Build a simple invoice dashboard with the extracted information
4. Handle edge cases like missing fields or partially extracted data

## Next Steps

Now that you can extract structured data from invoices, proceed to the next tutorial: [Complete Guide to Invoice Processing with Aspose.OCR Cloud SDK](/recognize-parse-invoice/recognition-sdk/) to learn how to simplify this process using the official SDKs.


## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post in our [community forum](https://forum.aspose.cloud/c/ocr/12/) for assistance.
