---
title: How to Convert CSV Documents with Load Options using GroupDocs.Conversion Cloud API
description: Learn to convert CSV files with precise control over delimiters, encoding, and data formatting using GroupDocs.Conversion Cloud API
url: /advanced-features/csv-conversion-options/
weight: 120
---

# Tutorial: Converting CSV Documents with Load Options

In this tutorial, you'll learn how to convert Comma-Separated Values (CSV) files to various formats using GroupDocs.Conversion Cloud API. You'll master CSV parsing and conversion with full control over delimiters, encoding, and data formatting options.

## Learning Objectives

By the end of this tutorial, you will be able to:
- Convert CSV files to Excel, PDF, and other formats
- Customize CSV parsing with delimiter and encoding options
- Control how CSV data is formatted in the output document
- Implement both storage-based and stream-based CSV conversions
- Troubleshoot common CSV conversion challenges

## Prerequisites

Before starting this tutorial, you need:
1. A [GroupDocs.Conversion Cloud account](https://dashboard.groupdocs.cloud/#/apps)
2. Your Client ID and Client Secret credentials
3. Basic understanding of REST API concepts
4. Development environment with your preferred programming language set up
5. Sample CSV files to test conversion

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

### Step 2: Basic CSV to Excel Conversion

Let's start with a simple conversion from CSV to Excel format:

#### Try it yourself

Using cURL:

```bash
curl -X POST "https://api.groupdocs.cloud/v2.0/conversion" \
-H "accept: application/json" \
-H "authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: application/json" \
-d "{  
      'FilePath': 'data/sample.csv',  
      'Format': 'xlsx',  
      'OutputPath': 'converted'
    }"
```

Replace `YOUR_JWT_TOKEN` with the actual token received in Step 1.

### Step 3: Converting CSV with Custom Delimiter

CSV files can use various delimiters (commas, semicolons, tabs). Here's how to specify the delimiter:

```csharp
// C# SDK Example
using System;
using System.Collections.Generic;
using GroupDocs.Conversion.Cloud.Sdk.Api;
using GroupDocs.Conversion.Cloud.Sdk.Client;
using GroupDocs.Conversion.Cloud.Sdk.Model;
using GroupDocs.Conversion.Cloud.Sdk.Model.Requests;

namespace CsvConversionTutorial
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
                // Set up conversion from CSV to Excel with custom delimiter
                var settings = new ConvertSettings
                {
                    StorageName = "MyStorage",
                    FilePath = "data/semicolon_separated.csv",
                    Format = "xlsx",
                    // CSV-specific load options
                    LoadOptions = new CsvLoadOptions()
                    {
                        Separator = ";",  // Semicolon delimiter
                        ConvertDateTimeData = true,  // Convert date strings to Excel dates
                        ConvertNumericData = true    // Convert numeric strings to numbers
                    },
                    // Excel-specific convert options (if needed)
                    ConvertOptions = new XlsxConvertOptions(),
                    OutputPath = "converted"
                };

                // Execute conversion
                List<StoredConvertedResult> response = apiInstance.ConvertDocument(
                    new ConvertDocumentRequest(settings));
                
                Console.WriteLine("CSV document converted successfully to Excel: " + response[0].Url);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```

### Step 4: Converting CSV to PDF with Encoding Options

When converting to PDF, you might need to specify encoding for international character support:

```java
// Java SDK Example
import com.groupdocs.cloud.conversion.api.*;
import com.groupdocs.cloud.conversion.client.ApiException;
import com.groupdocs.cloud.conversion.model.*;
import com.groupdocs.cloud.conversion.model.requests.*;
import java.util.List;

public class CsvToPdfExample {
    public static void main(String[] args) {
        // Configure API client
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        Configuration configuration = new Configuration(clientId, clientSecret);
        ConvertApi apiInstance = new ConvertApi(configuration);
        
        try {
            // Prepare convert settings for CSV to PDF
            ConvertSettings settings = new ConvertSettings();
            settings.setFilePath("data/international_data.csv");
            settings.setFormat("pdf");
            
            // Set CSV-specific load options
            CsvLoadOptions loadOptions = new CsvLoadOptions();
            loadOptions.setSeparator(",");       // Comma delimiter
            loadOptions.setEncoding("utf-8");    // UTF-8 encoding for international characters
            loadOptions.setConvertDateTimeData(true);
            loadOptions.setConvertNumericData(true);
            
            settings.setLoadOptions(loadOptions);
            
            // Configure PDF-specific convert options
            PdfConvertOptions convertOptions = new PdfConvertOptions();
            convertOptions.setDpi(300);  // High resolution
            
            settings.setConvertOptions(convertOptions);
            settings.setOutputPath("converted");
            
            // Execute conversion
            List<StoredConvertedResult> result = apiInstance.convertDocument(
                new ConvertDocumentRequest(settings));
            
            System.out.println("CSV document converted successfully to PDF: " + result.get(0).getUrl());
        } catch (ApiException e) {
            System.err.println("Exception when calling ConvertApi: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Step 5: Converting CSV with Header Row Options

Many CSV files have a header row. Here's how to handle it properly:

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
    settings.file_path = "data/with_headers.csv"
    settings.format = "xlsx"
    
    # Configure CSV-specific load options
    load_options = groupdocs_conversion_cloud.CsvLoadOptions()
    load_options.separator = ","
    load_options.has_headers = True       # Treat first row as column headers
    load_options.convert_numeric_data = True
    load_options.convert_date_time_data = True
    
    settings.load_options = load_options
    
    # Configure Excel-specific convert options
    convert_options = groupdocs_conversion_cloud.SpreadsheetConvertOptions()
    
    settings.convert_options = convert_options
    settings.output_path = "converted"
    
    # Execute conversion
    request = ConvertDocumentRequest(settings)
    result = api_instance.convert_document(request)
    
    print(f"CSV document converted successfully with header processing: {result[0].url}")
except groupdocs_conversion_cloud.ApiException as e:
    print(f"Exception when calling ConvertApi: {e}")
```

### Step 6: Stream-Based CSV Conversion

For applications that need to process the converted CSV directly:

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
    filePath: "data/sample.csv",
    format: "xlsx",
    loadOptions: {
        // CSV-specific load options
        separator: ",",
        convertDateTimeData: true,
        convertNumericData: true,
        hasHeaders: true
    },
    convertOptions: {
        // XLSX-specific convert options if needed
    },
    // Set outputPath to null for stream output
    outputPath: null
};

// Execute conversion
apiInstance.convertDocumentDownload({ convertSettings: settings })
    .then((result) => {
        // Save the stream to a file
        const fileName = "./converted-csv.xlsx";
        const writeStream = fs.createWriteStream(fileName);
        
        result.pipe(writeStream);
        
        writeStream.on("finish", () => {
            console.log(`CSV document converted and saved to ${fileName}`);
        });
    })
    .catch((error) => {
        console.log(`Error: ${error.message}`);
    });
```

## CSV-Specific Load Options

When converting CSV documents, you can leverage these specialized options:

| Option | Description | Default | Impact |
|--------|-------------|---------|--------|
| Separator | Character used as value delimiter | "," (comma) | Critical for correct field parsing |
| Encoding | Character encoding of the CSV file | Default encoding | Affects international character display |
| ConvertNumericData | Convert numeric strings to numbers | false | Improves data usability in Excel |
| ConvertDateTimeData | Convert date strings to dates | false | Enables date manipulation in Excel |
| HasHeaders | Whether first row contains column names | false | Controls column header display |

## Troubleshooting Common Issues

### 1. Delimiter Detection Problems

If your CSV isn't parsed correctly:
- Verify the correct delimiter character (comma, semicolon, tab, etc.)
- For tab-delimited files, use "\t" as the separator
- For custom delimiters, specify the exact character

### 2. Encoding Issues

For files with international characters:
- Specify the correct encoding (UTF-8 is recommended for most cases)
- For European languages with special characters, consider "ISO-8859-1" or "Windows-1252"
- For Asian languages, use appropriate encodings like "Shift-JIS" for Japanese

### 3. Data Type Conversion Problems

If numbers or dates appear as text in the output:
- Set `ConvertNumericData` and `ConvertDateTimeData` to true
- Ensure date formats in the CSV are recognizable
- Check if numeric values use a locale-specific decimal separator (comma vs. period)

### 4. Layout and Formatting Considerations

When converting to formats like PDF:
- Consider setting page size and orientation appropriate for the data width
- For wide CSV files, landscape orientation may work better
- Add custom styling or formatting if needed through additional conversion options

## What You've Learned

In this tutorial, you've learned:
- How to convert CSV files to Excel, PDF, and other formats
- Customizing CSV parsing with delimiter and encoding options
- Handling header rows and data type conversions
- Implementing both storage-based and stream-based CSV conversions
- Troubleshooting common CSV conversion challenges

## Further Practice

To reinforce your learning, try these exercises:
1. Create a CSV converter that automatically detects the delimiter
2. Implement a batch conversion utility that processes multiple CSV files with different settings
3. Build a web form that allows users to upload CSV files and customize conversion options
4. Create a preview feature that shows how CSV data will be interpreted before conversion

## Additional Resources

- [Product Page](https://products.groupdocs.cloud/conversion/)
- [Documentation](https://docs.groupdocs.cloud/conversion/)
- [Live Demo](https://products.groupdocs.app/conversion/family)
- [API Reference](https://reference.groupdocs.cloud/conversion/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.conversion-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/conversion/11)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [forum](https://forum.groupdocs.cloud/c/conversion/11) for support.
