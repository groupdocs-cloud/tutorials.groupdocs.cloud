---
title: Automatically Classify Word Documents into Categories
articleTitle: Document Classification
linktitle: Document Classification

url: /operations/classify-document/
description: Learn how to automatically categorize Word documents based on content using Aspose.Words Cloud API with various taxonomies.
weight: 10
---

# Automatically Classify Word Documents into Categories

In today's digital workspace, organizations often deal with thousands of documents across departments. Manually sorting these documents into appropriate categories is time-consuming and prone to errors. Automatic document classification solves this challenge by analyzing document content and assigning relevant categories, making document management systems more efficient.

## What is Document Taxonomy?

Document taxonomy is the process of classifying and organizing documents according to their content into categories and subcategories. This systematic organization makes it easier to navigate large document collections and quickly find relevant information.

Think of document taxonomy like a library's cataloging system. Just as books are organized by subject matter, document taxonomies group similar documents together based on their content, improving searchability and retrieval.

## Use Cases for Document Classification

Here are some common scenarios where automatic document classification proves invaluable:

- Enterprise Content Management: Automatically categorize incoming documents in SharePoint or other document management systems
- Email Processing: Sort email attachments into appropriate folders based on content
- Customer Support: Classify support tickets to route them to appropriate departments
- Research Organizations: Organize research papers by subject area 
- Legal Firms: Categorize legal documents by case type, jurisdiction, or legal area

## Supported Taxonomies in Aspose.Words Cloud

Aspose.Words Cloud API supports several taxonomies for classification:

1. IAB-2 Taxonomy: A standard taxonomy for advertising categorization from the Interactive Advertising Bureau
2. Documents Taxonomy: Categories specific to document types, including:
   - ADVE - Advertisements, brochures
   - Email
   - Form
   - Letter
   - Memo
   - News - Articles, news content
   - Invoice
   - Report
   - Resume
   - Scientific - Academic papers
   - Other
3. Sentiment Taxonomies:
   - 2-Class: Negative/Positive
   - 3-Class: Negative/Neutral/Positive

## How Document Classification Works

When you send a document to the API for classification:

1. The API extracts text content from the document
2. This content is analyzed using natural language processing algorithms
3. The system identifies key topics, terminology, and patterns
4. Based on this analysis, the document is matched against the taxonomy categories
5. The API returns the best matching categories along with confidence scores

## Using the Document Classification API

### REST API Endpoint

| Server | Method | Endpoint |
|--------|--------|----------|
| `https://api.aspose.cloud/v4.0` | PUT | `/words/online/get/classify` |

### Request Parameters

| Parameter Name | Data Type | Required/Optional | Description |
|----------------|-----------|-------------------|-------------|
| `loadEncoding` | string | Optional | Encoding to use when loading HTML/TXT documents |
| `password` | string | Optional | Password for protected Word documents |
| `encryptedPassword` | string | Optional | Encrypted password for direct API calls |
| `bestClassesCount` | string | Optional | Number of best matching classes to return |
| `taxonomy` | string | Optional | The taxonomy to use (default is IAB-2) |

### Multipart Form Data

| Property Name | Data Type | Required/Optional | Description |
|---------------|-----------|-------------------|-------------|
| `document` | string(binary) | Required | The document to classify |

## Step-By-Step Implementation

Let's walk through classifying a document using different programming languages:

### Using cURL

Making a direct API call with cURL is straightforward:

```bash
# First, get the JWT token
curl -v "https://api.aspose.cloud/connect/token" \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now use the token for classification
curl -v "https://api.aspose.cloud/v4.0/words/online/get/classify" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-F "document=@sample.docx" \
-F "taxonomy=documents" \
-F "bestClassesCount=3" \
--output classification_result.json
```

### Using Python SDK

The Python SDK simplifies the process significantly:

```python
# Import necessary modules
import os
from aspose.words.cloud import WordsApi, Configuration
from aspose.words.cloud.models import ClassificationRequestParameters

# Set Aspose.Words Cloud credentials
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
words_api = WordsApi(configuration)

# Path to your document
input_file = "sample.docx"

# Prepare classification parameters
request_params = ClassificationRequestParameters(
    taxonomy="documents",
    best_classes_count=3
)

# Open document and execute classification
with open(input_file, 'rb') as f:
    result = words_api.classify_document_online(f, request_params)

# Process classification results
for class_info in result.best_classes:
    print(f"Class: {class_info.class_name}, Score: {class_info.score}")
```

### Using Java SDK

Java developers can use the Aspose.Words Cloud SDK for Java:

```java
import com.aspose.words.cloud.ApiClient;
import com.aspose.words.cloud.ApiException;
import com.aspose.words.cloud.Configuration;
import com.aspose.words.cloud.WordsApi;
import com.aspose.words.cloud.model.*;

import java.io.File;
import java.nio.file.Files;
import java.nio.file.Paths;

public class ClassifyDocument {
    public static void main(String[] args) throws Exception {
        // Configure API client
        ApiClient apiClient = Configuration.getDefaultApiClient();
        apiClient.setClientId("YOUR_CLIENT_ID");
        apiClient.setClientSecret("YOUR_CLIENT_SECRET");
        
        WordsApi wordsApi = new WordsApi(apiClient);
        
        // Prepare document for classification
        File file = new File("sample.docx");
        byte[] documentBytes = Files.readAllBytes(file.toPath());
        
        // Create classification parameters
        ClassificationRequestParameters parameters = new ClassificationRequestParameters();
        parameters.setTaxonomy("documents");
        parameters.setBestClassesCount(3);
        
        // Classify document
        ClassificationResponse result = wordsApi.classifyDocumentOnline(documentBytes, parameters);
        
        // Process results
        for (ClassificationResult classInfo : result.getBestClasses()) {
            System.out.println("Class: " + classInfo.getClassName() + ", Score: " + classInfo.getScore());
        }
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

namespace DocumentClassificationExample
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
            
            // Prepare file for classification
            var filePath = "sample.docx";
            
            using (var fileStream = File.OpenRead(filePath))
            {
                // Set up classification parameters
                var requestParams = new ClassificationRequestParameters
                {
                    Taxonomy = "documents",
                    BestClassesCount = 3
                };
                
                // Classify document
                var request = new ClassifyDocumentOnlineRequest(fileStream, requestParams);
                var result = wordsApi.ClassifyDocumentOnline(request);
                
                // Process results
                foreach (var classInfo in result.BestClasses)
                {
                    Console.WriteLine($"Class: {classInfo.ClassName}, Score: {classInfo.Score}");
                }
            }
        }
    }
}
```

## Interpreting Classification Results

The API returns classification results as a JSON object containing:

1. Best Classes: A list of the top matching categories based on your `bestClassesCount` parameter
2. Class Name: The name of the identified category
3. Score: A confidence score between 0 and 1 indicating how well the document matches the category

For example:

```json
{
  "bestClasses": [
    {
      "className": "Report",
      "score": 0.91
    },
    {
      "className": "Memo",
      "score": 0.45
    },
    {
      "className": "Form",
      "score": 0.12
    }
  ]
}
```

In this example, the document is primarily classified as a Report with high confidence (0.91), with secondary classifications as Memo and Form with lower confidence.

## Tips for Better Classification Results

- Provide sufficient content: Classification accuracy improves with more textual content
- Use appropriate taxonomy: Choose the taxonomy that best fits your document types
- Consider multi-classification: Some documents may logically belong to multiple categories
- Pre-process documents: Clean up formatting issues that might affect text extraction
- Evaluate confidence scores: Higher scores indicate stronger classification matches

## Going Further

Here are some ways to extend document classification in your applications:

- Automated Workflows: Trigger specific actions based on document classification results
- Document Routing: Automatically move documents to appropriate folders or departments
- Metadata Enrichment: Add classification results as metadata to improve searchability
- Hybrid Approaches: Combine automatic classification with human verification for sensitive documents
- Custom Taxonomies: Consider developing custom taxonomies for industry-specific needs

## See Also

- [Classify Text](/classification/text/) - For classifying text snippets without documents
- [GitHub Repository](https://github.com/aspose-words-cloud) - Explore SDK code examples
- [Product Page](https://products.aspose.cloud/words/)
- [Documentation](https://docs.aspose.cloud/words/)
- [Live Demo](https://products.aspose.app/words/family)
- [API Reference](https://reference.aspose.cloud/words/)
- [Blog](https://blog.aspose.cloud/category/words/)
- [Free Support](https://forum.aspose.cloud/c/words/17)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
