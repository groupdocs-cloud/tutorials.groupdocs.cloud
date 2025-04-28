---
title: Compare Two Word Documents to Track Changes
articleTitle: Document Comparison
linktitle: Document Comparison

url: /operations/compare/
description: Learn how to compare two Word documents to identify text additions, deletions, and formatting changes using Aspose.Words Cloud API.
weight: 30
---

# Compare Two Word Documents to Track Changes

Document comparison is essential in many professional workflows, especially in collaborative environments where multiple people work on the same document. Whether you're a legal professional reviewing contract changes, an editor tracking revisions, or a project manager monitoring document updates, automated document comparison can save hours of manual work.

## Why Compare Documents Programmatically?

Manual document comparison is tedious and error-prone, especially with lengthy documents. Here are some reasons why automated document comparison is valuable:

- Accuracy: Catch every change, no matter how small
- Efficiency: Compare documents in seconds, not hours
- Comprehensive: Identify additions, deletions, and formatting changes
- Visual Clarity: Generate documents with highlighted changes
- Integration: Build comparison into existing workflows

## How Document Comparison Works

When you compare documents with Aspose.Words Cloud API:

1. The API analyzes both the original document and the revised version
2. It identifies all differences between them, including:
   - Added text
   - Deleted text
   - Modified text
   - Formatting changes
3. The API generates a resulting document with tracked changes
4. These changes are marked using standard Word revision marking

This allows you to see exactly what changed between versions, just as if you had used Word's built-in "Track Changes" feature while making the edits.

## Supported File Formats

Aspose.Words Cloud supports comparison between documents of different formats, including:

- DOCX, DOC (Microsoft Word documents)
- DOCM (Word macro-enabled documents)
- PDF (Portable Document Format)
- RTF (Rich Text Format)
- DOTX, DOTM, DOT (Word templates)
- ODT, OTT (OpenDocument formats)
- TXT (Plain text)
- HTML, MHTML, XHTML (Web formats)

This flexibility allows you to compare documents even when they're in different formats.

## Using the Document Comparison API

### REST API Endpoint

| Server | Method | Endpoint |
|--------|--------|----------|
| `https://api.aspose.cloud/v4.0` | PUT | `/words/online/put/compareDocument` |

### Request Parameters

| Parameter Name | Data Type | Required/Optional | Description |
|----------------|-----------|-------------------|-------------|
| `loadEncoding` | string | Optional | Encoding to use when loading HTML/TXT documents |
| `password` | string | Optional | Password for protected Word document (original document) |
| `encryptedPassword` | string | Optional | Encrypted password for direct API calls (original document) |
| `destFileName` | string | Optional | Result path after comparison (if omitted, saves to source document) |
| `encryptedPassword2` | string | Optional | Encrypted password for the second document |

### Multipart Form Data

| Property Name | Data Type | Required/Optional | Description |
|---------------|-----------|-------------------|-------------|
| `document` | string(binary) | Required | The original document |
| `compareData` | CompareData | Required | Comparison data including author information |
| `comparingDocument` | string(binary) | Optional | The document to compare with original |

## Step-By-Step Implementation

Let's walk through implementing document comparison with different programming approaches:

### Using cURL

Direct API calls with cURL:

```bash
# First, get the JWT token
curl -v "https://api.aspose.cloud/connect/token" \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now compare documents
curl -v "https://api.aspose.cloud/v4.0/words/online/put/compareDocument?destFileName=result.docx" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/octet-stream" \
-F "document=@original.docx" \
-F "comparingDocument=@revised.docx" \
-F "compareData={\"Author\": \"John Doe\", \"DateTime\": \"2023-04-15T12:00:00Z\"}" \
--output comparison_result.docx
```

### Using Python SDK

The Python SDK makes document comparison straightforward:

```python
# Import necessary modules
import os
from aspose.words.cloud import WordsApi, Configuration
from aspose.words.cloud.models import CompareData

# Set Aspose.Words Cloud credentials
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
words_api = WordsApi(configuration)

# Paths to documents
original_doc_path = "original.docx"
revised_doc_path = "revised.docx"
output_path = "comparison_result.docx"

# Prepare comparison data
compare_data = CompareData(
    author="John Doe",
    date_time="2023-04-15T12:00:00Z"
)

# Load document files
with open(original_doc_path, 'rb') as original_file, open(revised_doc_path, 'rb') as revised_file:
    # Execute comparison
    result = words_api.compare_document_online(
        original_file,
        revised_file,
        compare_data,
        dest_file_name=output_path
    )

# Save result
with open(output_path, 'wb') as f:
    f.write(result)

print(f"Comparison complete. Result saved to {output_path}")
```

### Using Java SDK

Java developers can use the Aspose.Words Cloud SDK for Java:

```java
import com.aspose.words.cloud.ApiClient;
import com.aspose.words.cloud.ApiException;
import com.aspose.words.cloud.Configuration;
import com.aspose.words.cloud.WordsApi;
import com.aspose.words.cloud.model.*;
import com.aspose.words.cloud.model.requests.*;

import java.io.File;
import java.io.FileOutputStream;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.time.OffsetDateTime;

public class CompareDocuments {
    public static void main(String[] args) throws Exception {
        // Configure API client
        ApiClient apiClient = Configuration.getDefaultApiClient();
        apiClient.setClientId("YOUR_CLIENT_ID");
        apiClient.setClientSecret("YOUR_CLIENT_SECRET");
        
        WordsApi wordsApi = new WordsApi(apiClient);
        
        // Paths to documents
        String originalDocPath = "original.docx";
        String revisedDocPath = "revised.docx";
        String outputPath = "comparison_result.docx";
        
        // Prepare files for comparison
        File originalFile = new File(originalDocPath);
        File revisedFile = new File(revisedDocPath);
        
        byte[] originalBytes = Files.readAllBytes(originalFile.toPath());
        byte[] revisedBytes = Files.readAllBytes(revisedFile.toPath());
        
        // Create comparison data
        CompareData compareData = new CompareData();
        compareData.setAuthor("John Doe");
        compareData.setDateTime(OffsetDateTime.now());
        
        // Execute comparison
        CompareDocumentOnlineRequest request = new CompareDocumentOnlineRequest(
            originalBytes, 
            revisedBytes, 
            compareData, 
            null, null, null, outputPath, null
        );
        
        byte[] resultBytes = wordsApi.compareDocumentOnline(request);
        
        // Save the result
        try (FileOutputStream outputStream = new FileOutputStream(outputPath)) {
            outputStream.write(resultBytes);
        }
        
        System.out.println("Comparison complete. Result saved to " + outputPath);
    }
}
```

### Using C# SDK

C# developers can use the Aspose.Words Cloud SDK for .NET:

```csharp
using System;
using System.IO;
using Aspose.Words.Cloud.Sdk;
using Aspose.Words.Cloud.Sdk.Model;
using Aspose.Words.Cloud.Sdk.Model.Requests;

namespace DocumentComparisonExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API client
            var config = new Configuration
            {
                ClientId = "YOUR_CLIENT_ID",
                ClientSecret = "YOUR_CLIENT_SECRET"
            };
            
            var wordsApi = new WordsApi(config);
            
            // Paths to documents
            var originalDocPath = "original.docx";
            var revisedDocPath = "revised.docx";
            var outputPath = "comparison_result.docx";
            
            // Read document files
            byte[] originalBytes = File.ReadAllBytes(originalDocPath);
            byte[] revisedBytes = File.ReadAllBytes(revisedDocPath);
            
            // Create comparison data
            var compareData = new CompareData
            {
                Author = "John Doe",
                DateTime = DateTime.UtcNow
            };
            
            try
            {
                // Execute comparison
                var request = new CompareDocumentOnlineRequest(
                    originalBytes,
                    revisedBytes,
                    compareData,
                    null, null, null, outputPath, null
                );
                
                var resultBytes = wordsApi.CompareDocumentOnline(request);
                
                // Save the result
                File.WriteAllBytes(outputPath, resultBytes);
                
                Console.WriteLine($"Comparison complete. Result saved to {outputPath}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error: {ex.Message}");
            }
        }
    }
}
```

## Understanding the Comparison Result

The comparison result is a document that contains all the differences between the original and revised documents, marked as revisions:

1. Added text: Typically appears in a different color (often blue or green)
2. Deleted text: Usually shown with strikethrough formatting and in a different color (often red)
3. Formatting changes: Highlighted to show the difference between original and revised formatting
4. Revision information: Each change includes the author name and timestamp

You can open the resulting document in Microsoft Word or any compatible word processor to review the changes. The tracked changes can be accepted or rejected using the standard revision tools in Word.

## Customizing the Comparison Process

You can customize the comparison process by setting parameters in the `CompareData` object:

- Author: Name to associate with the revision marks
- DateTime: Custom timestamp for revisions (useful for historical comparisons)
- IgnoreCase: Whether to ignore case differences
- IgnoreComments: Whether to ignore comments during comparison
- IgnoreFields: Whether to ignore field differences
- IgnoreFootnotes: Whether to ignore footnotes/endnotes
- IgnoreFormatting: Whether to ignore formatting differences
- IgnoreHeadersAndFooters: Whether to ignore headers and footers
- IgnoreTables: Whether to ignore table structure differences
- IgnoreTextboxes: Whether to ignore textbox content
- Target: The target document to accept changes (original or comparing)

These options allow you to focus on the specific types of changes that matter to your use case.

## Practical Application Examples

### Legal Contract Review

Law firms often need to compare contract revisions:

```python
# Import modules
from aspose.words.cloud import WordsApi, Configuration
from aspose.words.cloud.models import CompareData

# Configure API
config = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
words_api = WordsApi(config)

# Contract files
original_contract = "contract_v1.docx"
revised_contract = "contract_v2.docx"
comparison_result = "contract_comparison.docx"

# Configure comparison - focusing on text changes, ignoring formatting
compare_data = CompareData(
    author="Jane Smith, Legal Counsel",
    date_time="2023-04-15T14:30:00Z",
    ignore_formatting=True,
    ignore_headers_and_footers=True,
    ignore_case=False  # Case is important in legal documents
)

# Execute comparison
with open(original_contract, 'rb') as original, open(revised_contract, 'rb') as revised:
    result = words_api.compare_document_online(
        original,
        revised,
        compare_data,
        dest_file_name=comparison_result
    )

# Save result
with open(comparison_result, 'wb') as f:
    f.write(result)

print(f"Contract comparison complete. Review {comparison_result} to see all changes.")
```

### Collaborative Document Editing

In a team environment where multiple editors work on the same document:

```csharp
// Configure API client
var config = new Configuration
{
    ClientId = "YOUR_CLIENT_ID",
    ClientSecret = "YOUR_CLIENT_SECRET"
};

var wordsApi = new WordsApi(config);

// Document paths
var masterDocument = "team_document_master.docx";
var editorVersions = new[]
{
    "team_document_editor1.docx",
    "team_document_editor2.docx",
    "team_document_editor3.docx"
};

// Process each editor's version
foreach (var editorVersion in editorVersions)
{
    // Get editor name from filename
    var editorName = Path.GetFileNameWithoutExtension(editorVersion).Replace("team_document_", "");
    var outputFile = $"comparison_{editorName}.docx";
    
    // Compare with master
    var compareData = new CompareData
    {
        Author = editorName,
        DateTime = DateTime.UtcNow
    };
    
    // Read files
    byte[] masterBytes = File.ReadAllBytes(masterDocument);
    byte[] editorBytes = File.ReadAllBytes(editorVersion);
    
    // Execute comparison
    var request = new CompareDocumentOnlineRequest(
        masterBytes,
        editorBytes,
        compareData,
        null, null, null, outputFile, null
    );
    
    var resultBytes = wordsApi.CompareDocumentOnline(request);
    
    // Save result
    File.WriteAllBytes(outputFile, resultBytes);
    
    Console.WriteLine($"Processed changes from {editorName}");
}

Console.WriteLine("All editor versions compared with master document.");
```

## Going Further

Here are some ways to extend document comparison in your applications:

- Batch Processing: Compare multiple document pairs in a single operation
- Change Summaries: Generate reports that summarize the types and extent of changes
- Approval Workflows: Build document approval processes with comparison results
- Version History: Maintain a history of document versions with comparison links
- Selective Merging: Accept or reject specific changes programmatically
- Pre-processing: Clean up or normalize documents before comparison
- Integration with Document Management: Add comparison features to existing systems

## Performance Considerations

When working with large documents, consider these performance tips:

- Optimize File Size: Large images can slow down the comparison process
- Focus Comparison: Use the ignore parameters to focus only on relevant changes
- Asynchronous Processing: For user interfaces, process comparisons asynchronously
- Caching: Cache comparison results for frequently accessed document pairs
- Chunking: For very large documents, consider comparing section by section

## See Also

- [Classify Documents](/classification/document/)
- [Document Compression](/documents/compress/)
- [GitHub Repository](https://github.com/aspose-words-cloud)
- [Product Page](https://products.aspose.cloud/words/)
- [Documentation](https://docs.aspose.cloud/words/)
- [Live Demo](https://products.aspose.app/words/family)
- [API Reference](https://reference.aspose.cloud/words/)
- [Blog](https://blog.aspose.cloud/category/words/)
- [Free Support](https://forum.aspose.cloud/c/words/17)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
