---
title: Generate Documents and Build Reports
articleTitle: Document Generation and Reporting
linktitle: Report Generation

url: /operations/report/
description: Create dynamic Word documents from templates and data sources with Aspose.Words Cloud API for automated document generation and reporting.
weight: 90
---

# Document Generation and Reporting with Aspose.Words Cloud

One of the most powerful features of word processing is the ability to generate documents dynamically using templates and data sources. Think about all the times you've needed to create multiple personalized letters, contracts with variable terms, or reports populated with the latest data. Manually creating each document would be time-consuming and error-prone. This is where document generation and reporting solutions become invaluable.

Aspose.Words Cloud provides robust APIs for document generation that allow you to:

1. Create template documents with special markup for dynamic content
2. Populate these templates with data from external sources (XML, JSON, CSV)
3. Generate final documents with all placeholders replaced by actual data

## The Document Generation Process

At its core, document generation follows these steps:

1. Template Creation: Design a Word document with placeholders for dynamic content
2. Data Preparation: Organize your data in a structured format (XML, JSON, or CSV)
3. Report Building: Use the Aspose.Words Cloud API to merge the template with your data
4. Output Generation: Receive the final document with all dynamic content populated

This process can be automated and integrated into your applications, allowing for on-demand document generation based on the latest data.

## Report Building with Aspose.Words Cloud

Let's explore how to use the Aspose.Words Cloud API to build reports from templates and data sources.

### REST API Endpoint

| Server | Method | Endpoint |
|--------|--------|----------|
| `https://api.aspose.cloud/v4.0` | PUT | `/words/buildReport` |

### Request Parameters

| Parameter Name | Data Type | Required/Optional | Description |
|----------------|-----------|-------------------|-------------|
| `documentFileName` | string | Optional | The filename of the output document that will be used when the resulting document has a dynamic field {filename}. If not set, "template" will be used. |

Your request should use `multipart/form-data` format with these properties:

| Property Name | Data Type | Required/Optional | Description |
|---------------|-----------|-------------------|-------------|
| `template` | string(binary) | Required | The template document file |
| `data` | string | Required | Data to populate the template (XML, JSON, or CSV format) |
| `reportEngineSettings` | ReportEngineSettings | Required | Settings for the report engine |

### Understanding Template Structure

A template document is a regular Word document that includes special markup to indicate where and how dynamic content should be inserted. Aspose.Words Cloud uses a powerful template engine that supports:

- Simple field replacements
- Conditional sections
- Repeating blocks for lists and tables
- Complex expressions and calculations
- Image insertion
- Table and chart data binding

### Data Source Formats

You can provide data to your templates in several formats:

1. XML: Hierarchical data with elements and attributes
2. JSON: Lightweight data interchange format
3. CSV: Tabular data in comma-separated format

Each format has its strengths. XML and JSON are ideal for complex, hierarchical data structures, while CSV works well for simple tabular data.

## Practical Examples

Let's see how to implement report generation in different programming environments.

### Using cURL

Here's how to build a report using cURL:

```bash
# Request with cURL
curl -X PUT "https://api.aspose.cloud/v4.0/words/buildReport" \
  -H "accept: application/json" \
  -H "Authorization: Bearer <JWT Token>" \
  -H "Content-Type: multipart/form-data" \
  -F "template=@template.docx" \
  -F "data=@data.json;type=application/json" \
  -F "reportEngineSettings={\"dataSourceType\":\"Json\",\"dataSourceName\":\"persons\"}"
```

### Programming Language Examples

#### Python Example

```python
# Python example of building a report
import requests
import json

# API endpoint
API_ENDPOINT = "https://api.aspose.cloud/v4.0/words/buildReport"

# Your JWT token (get from https://dashboard.aspose.cloud/)
TOKEN = "your_jwt_token"

# Request headers
headers = {
    "Authorization": f"Bearer {TOKEN}",
    "Accept": "application/json"
}

# Report engine settings
report_settings = {
    "dataSourceType": "Json",
    "dataSourceName": "persons"
}

# Open the files
with open("template.docx", "rb") as template_file, open("data.json", "r") as data_file:
    data = data_file.read()
    
    # Prepare multipart form data
    files = {
        "template": ("template.docx", template_file, "application/octet-stream"),
        "data": ("data.json", data, "application/json"),
        "reportEngineSettings": (None, json.dumps(report_settings), "application/json")
    }
    
    # Make the request
    response = requests.put(
        API_ENDPOINT,
        headers=headers,
        files=files
    )
    
    # Process the response
    if response.status_code == 200:
        # Save the generated document
        with open("result.docx", "wb") as f:
            f.write(response.content)
        print("Report generated successfully and saved as 'result.docx'")
    else:
        print(f"Error: {response.status_code} - {response.text}")
```

#### Java Example

```java
// Java example of building a report
import java.io.File;
import java.io.FileOutputStream;
import java.nio.file.Files;

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPut;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;

public class BuildReport {
    public static void main(String[] args) {
        try {
            // API endpoint
            String apiEndpoint = "https://api.aspose.cloud/v4.0/words/buildReport";
            
            // Your JWT token (get from https://dashboard.aspose.cloud/)
            String token = "your_jwt_token";
            
            // Create HTTP client
            CloseableHttpClient httpClient = HttpClients.createDefault();
            
            // Create PUT request
            HttpPut request = new HttpPut(apiEndpoint);
            request.setHeader("Authorization", "Bearer " + token);
            request.setHeader("Accept", "application/json");
            
            // Prepare the files
            File templateFile = new File("template.docx");
            byte[] templateContent = Files.readAllBytes(templateFile.toPath());
            
            File dataFile = new File("data.json");
            String dataContent = new String(Files.readAllBytes(dataFile.toPath()));
            
            String reportSettings = "{\"dataSourceType\":\"Json\",\"dataSourceName\":\"persons\"}";
            
            // Build multipart request
            HttpEntity multipart = MultipartEntityBuilder.create()
                .addBinaryBody("template", templateContent, ContentType.APPLICATION_OCTET_STREAM, templateFile.getName())
                .addTextBody("data", dataContent, ContentType.APPLICATION_JSON)
                .addTextBody("reportEngineSettings", reportSettings, ContentType.APPLICATION_JSON)
                .build();
            
            request.setEntity(multipart);
            
            // Execute the request
            CloseableHttpResponse response = httpClient.execute(request);
            
            // Process the response
            int statusCode = response.getStatusLine().getStatusCode();
            
            if (statusCode == 200) {
                // Save the generated document
                HttpEntity entity = response.getEntity();
                if (entity != null) {
                    try (FileOutputStream outstream = new FileOutputStream("result.docx")) {
                        entity.writeTo(outstream);
                    }
                    System.out.println("Report generated successfully and saved as 'result.docx'");
                }
            } else {
                System.out.println("Error: " + statusCode);
            }
            
            // Close resources
            response.close();
            httpClient.close();
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### C# Example

```csharp
// C# example of building a report
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

class BuildReport
{
    static async Task Main()
    {
        // API endpoint
        string apiEndpoint = "https://api.aspose.cloud/v4.0/words/buildReport";
        
        // Your JWT token (get from https://dashboard.aspose.cloud/)
        string token = "your_jwt_token";
        
        // Create HTTP client
        using var httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
        
        // Prepare the files
        var templateBytes = File.ReadAllBytes("template.docx");
        var dataJson = File.ReadAllText("data.json");
        var reportSettings = "{\"dataSourceType\":\"Json\",\"dataSourceName\":\"persons\"}";
        
        // Create multipart form data
        using var formData = new MultipartFormDataContent();
        
        // Add template file
        var templateContent = new ByteArrayContent(templateBytes);
        templateContent.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
        formData.Add(templateContent, "template", "template.docx");
        
        // Add data file
        var dataContent = new StringContent(dataJson, Encoding.UTF8, "application/json");
        formData.Add(dataContent, "data");
        
        // Add report settings
        var settingsContent = new StringContent(reportSettings, Encoding.UTF8, "application/json");
        formData.Add(settingsContent, "reportEngineSettings");
        
        // Make the request
        var response = await httpClient.PutAsync(apiEndpoint, formData);
        
        // Process the response
        if (response.IsSuccessStatusCode)
        {
            // Save the generated document
            var responseBytes = await response.Content.ReadAsByteArrayAsync();
            File.WriteAllBytes("result.docx", responseBytes);
            Console.WriteLine("Report generated successfully and saved as 'result.docx'");
        }
        else
        {
            var responseContent = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"Error: {response.StatusCode} - {responseContent}");
        }
    }
}
```

## Template Design Patterns

Creating effective templates is an art. Here are some best practices:

### 1. Simple Field Replacements

For basic data insertion, use field codes like:

```
<<[FirstName]>>
```

This will be replaced with the value of "FirstName" from your data source.

### 2. Conditional Sections

To include content only when certain conditions are met:

```
<<if [Age] >= 18>>
  Adult content here
<<else>>
  Minor content here
<<endif>>
```

### 3. Repeating Blocks

For generating tables or lists from collections:

```
<<foreach [item in Order.Items]>>
  Product: <<[item.Name]>>
  Quantity: <<[item.Quantity]>>
  Price: <<[item.Price]>>
<<endfor>>
```

### 4. Calculations and Formatting

You can perform calculations within templates:

```
Total: <<[[Order.Items.Sum(item => item.Price * item.Quantity)].ToString("C")>>
```

## Real-World Use Cases

### Automated Contract Generation

Law firms and legal departments can:
- Create template contracts with variable terms
- Populate contracts with client-specific information
- Generate hundreds of personalized contracts quickly
- Ensure consistency across all documents

### Financial Reporting

Financial institutions can:
- Create template statements and reports
- Populate them with the latest financial data
- Generate periodic reports automatically
- Ensure compliance with reporting standards

### Personalized Marketing Materials

Marketing teams can:
- Design template brochures and proposals
- Customize them with client-specific information
- Generate personalized marketing materials at scale
- Track and measure campaign effectiveness

## Going Further

Once you've implemented basic report generation, consider these extensions:

1. Template Management: Create a library of reusable templates for different document types
2. Data Validation: Validate data before report generation to ensure output quality
3. Report Scheduling: Automate report generation on a schedule (daily, weekly, monthly)
4. PDF Conversion: Automatically convert generated reports to PDF for distribution

## See Also

- [Product Page](https://products.aspose.cloud/words/)
- [Documentation](https://docs.aspose.cloud/words/)
- [Live Demo](https://products.aspose.app/words/family)
- [API Reference](https://reference.aspose.cloud/words/)
- [Blog](https://blog.aspose.cloud/category/words/)
- [Free Support](https://forum.aspose.cloud/c/words/17)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [GitHub Repository](https://github.com/aspose-words-cloud)

## Related Articles

- [Document Statistics](/operations/statistics/) - Get word count and other metrics from your documents
- [Document Comparison](/operations/compare/) - Compare two documents and identify differences
- [Document Protection](/operations/protect/) - Secure your generated reports with password protection
