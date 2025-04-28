---
title: Get Document Statistics
second_title: Aspose.Words Cloud Document Operations

url: /operations/document-statistics/
description: Learn how to retrieve word counts, page counts, and other statistics from Word documents using Aspose.Words Cloud API.
weight: 40
---

# Getting Document Statistics with Aspose.Words Cloud

Understanding the composition of your documents is often crucial for content management, publication workflows, and data analysis. Whether you need to count words for billing purposes, track document length, or analyze content complexity, document statistics provide valuable insights.

## Why Document Statistics Matter

Consider these real-world scenarios where document statistics are essential:

- Content Writers: Freelance writers who need to verify word counts for billing
- Publishers: Editorial teams who need to ensure articles meet length requirements
- Educational Institutions: Teachers checking if student submissions meet requirements
- Legal Firms: Attorneys who need to analyze document complexity and length
- Content Management Systems: Applications that categorize or price content by length

Aspose.Words Cloud provides a powerful API to retrieve comprehensive statistics from Word documents without requiring Microsoft Word installation.

## Understanding Document Statistics

Document statistics typically include metrics such as:

- Word count: The total number of words in the document
- Page count: The number of pages in the document
- Paragraph count: The number of paragraphs
- Character count: The number of characters with and without spaces
- Line count: The number of lines in the document

These metrics help you understand the size and complexity of a document at a glance.

## Retrieving Document Statistics via API

Aspose.Words Cloud offers a straightforward API for retrieving document statistics. Let's explore how to use this functionality.

### Request Parameters

| Parameter Name | Data Type | Required/Optional | Description |
|----------------|-----------|-------------------|-------------|
| `loadEncoding` | string | Optional | Encoding to use when loading HTML or TXT documents |
| `password` | string | Optional | Password for protected documents (for SDK use) |
| `encryptedPassword` | string | Optional | Encrypted password for direct API calls |
| `includeComments` | boolean | Optional | Whether to include comments in the word count (default: false) |
| `includeFootnotes` | boolean | Optional | Whether to include footnotes in the word count (default: false) |
| `includeTextInShapes` | boolean | Optional | Whether to include text in shapes in the word count (default: false) |

For the multipart/form-data request:

| Property Name | Data Type | Required/Optional | Description |
|---------------|-----------|-------------------|-------------|
| `document` | binary | Required | The document to analyze |

### Step 1: Authentication

Before making any API calls, you need to authenticate with the Aspose.Words Cloud API. Authentication requires obtaining a JWT token using your client credentials.

> Note: If you don't have credentials yet, sign up for a free account at [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/).

### Step 2: Make the API Request

#### Using cURL

Here's how to get statistics for a document using cURL:

```bash
curl -X PUT "https://api.aspose.cloud/v4.0/words/online/get/statistics?includeComments=true&includeFootnotes=true&includeTextInShapes=true" \
     -H "accept: application/json" \
     -H "Authorization: Bearer <JWT_TOKEN>" \
     -H "Content-Type: multipart/form-data" \
     -F "document=@sample.docx;type=application/octet-stream"
```

#### Using Programming Languages

Let's look at how to retrieve document statistics in different programming languages:

##### Python Example

```python
# For complete examples and data files, please go to https://github.com/aspose-words-cloud/aspose-words-cloud-python
import os
import asposewordscloud
from asposewordscloud import WordsApi, GetDocumentStatisticsOnlineRequest

# Configure OAuth2 access token
configuration = asposewordscloud.Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API client
api = WordsApi(configuration)

# Load the document
with open("sample.docx", 'rb') as document_file:
    document = document_file.read()

# Set additional options
include_comments = True
include_footnotes = True
include_text_in_shapes = True

# Get document statistics
result = api.get_document_statistics_online(GetDocumentStatisticsOnlineRequest(
    document=document,
    include_comments=include_comments,
    include_footnotes=include_footnotes,
    include_text_in_shapes=include_text_in_shapes
))

# Display statistics
print(f"Word count: {result.word_count}")
print(f"Page count: {result.page_count}")
print(f"Paragraph count: {result.paragraph_count}")
print(f"Character count (with spaces): {result.character_count_including_spaces}")
print(f"Character count (without spaces): {result.character_count}")
print(f"Line count: {result.line_count}")
```

##### Java Example

```java
// For complete examples and data files, please go to https://github.com/aspose-words-cloud/aspose-words-cloud-java
import com.aspose.words.cloud.ApiClient;
import com.aspose.words.cloud.ApiException;
import com.aspose.words.cloud.api.WordsApi;
import com.aspose.words.cloud.model.StatDataResponse;
import com.aspose.words.cloud.model.requests.GetDocumentStatisticsOnlineRequest;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;

public class GetDocumentStatistics {
    public static void main(String[] args) throws ApiException, IOException {
        // Configure OAuth2 access token
        ApiClient apiClient = new ApiClient("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        // Create API instance
        WordsApi api = new WordsApi(apiClient);
        
        // Load the document
        byte[] document = Files.readAllBytes(new File("sample.docx").toPath());
        
        // Set additional options
        Boolean includeComments = true;
        Boolean includeFootnotes = true;
        Boolean includeTextInShapes = true;
        
        // Get document statistics
        StatDataResponse result = api.getDocumentStatisticsOnline(new GetDocumentStatisticsOnlineRequest(
            document,
            null,
            null,
            null,
            includeComments,
            includeFootnotes,
            includeTextInShapes
        ));
        
        // Display statistics
        System.out.println("Word count: " + result.getStatData().getWordCount());
        System.out.println("Page count: " + result.getStatData().getPageCount());
        System.out.println("Paragraph count: " + result.getStatData().getParagraphCount());
        System.out.println("Character count (with spaces): " + result.getStatData().getCharacterCountIncludingSpaces());
        System.out.println("Character count (without spaces): " + result.getStatData().getCharacterCount());
        System.out.println("Line count: " + result.getStatData().getLineCount());
    }
}
```

### Understanding the Response

When you request document statistics, the API returns a JSON response with detailed information about the document. Here's an example of what the response might look like:

```json
{
  "StatData": {
    "WordCount": 1542,
    "ParagraphCount": 42,
    "PageCount": 5,
    "LineCount": 152,
    "CharacterCount": 9568,
    "CharacterCountIncludingSpaces": 11110
  }
}
```

This response provides a comprehensive breakdown of the document's composition, giving you valuable insights about its content and structure.

##### C# Example

```csharp
// For complete examples and data files, please go to https://github.com/aspose-words-cloud/aspose-words-cloud-dotnet
using System;
using System.IO;
using Aspose.Words.Cloud.Sdk;
using Aspose.Words.Cloud.Sdk.Model.Requests;

class Program
{
    static void Main(string[] args)
    {
        // Configure OAuth2 access token
        var config = new Configuration
        {
            ClientId = "YOUR_CLIENT_ID",
            ClientSecret = "YOUR_CLIENT_SECRET"
        };

        // Create API instance
        var api = new WordsApi(config);

        // Load the document
        var document = File.ReadAllBytes("sample.docx");

        // Set additional options
        var includeComments = true;
        var includeFootnotes = true;
        var includeTextInShapes = true;

        // Get document statistics
        var result = api.GetDocumentStatisticsOnline(new GetDocumentStatisticsOnlineRequest(
            document,
            includeComments: includeComments,
            includeFootnotes: includeFootnotes,
            includeTextInShapes: includeTextInShapes
        ));

        // Display statistics
        Console.WriteLine($"Word count: {result.StatData.WordCount}");
        Console.WriteLine($"Page count: {result.StatData.PageCount}");
        Console.WriteLine($"Paragraph count: {result.StatData.ParagraphCount}");
        Console.WriteLine($"Character count (with spaces): {result.StatData.CharacterCountIncludingSpaces}");
        Console.WriteLine($"Character count (without spaces): {result.StatData.CharacterCount}");
        Console.WriteLine($"Line count: {result.StatData.LineCount}");
    }
}
```

## Customizing Statistics Calculations

Aspose.Words Cloud provides several options to customize how statistics are calculated:

### Including Comments

By default, text in comments is not included in the word count. However, in some scenarios (like academic or legal editing), comments may contain substantial content that should be counted. Setting the `includeComments` parameter to `true` will include this text in your statistics.

### Including Footnotes and Endnotes

Footnotes and endnotes are often excluded from word counts, but they can contain significant content in academic or scholarly works. The `includeFootnotes` parameter allows you to include this text in your statistics when set to `true`.

### Including Text in Shapes

Word documents can contain text boxes, SmartArt, and other shapes with text. By default, this text is not included in the statistics. Setting `includeTextInShapes` to `true` will include text from these elements in your counts.

## Practical Applications

Document statistics can be used in various practical applications:

### 1. Content Management Systems

Integrate document statistics into your CMS to:
- Categorize documents by length
- Display word counts alongside document listings
- Enforce content length policies

### 2. Editorial Workflows

Use document statistics in publishing and editorial systems to:
- Verify if submissions meet length requirements
- Track content production metrics
- Allocate payment based on word count

### 3. Academic Tools

Integrate statistics into academic software to:
- Check if student submissions meet length requirements
- Analyze content complexity through metrics like average sentence length
- Track writing progress over time

### 4. Legal Document Analysis

Use document statistics in legal applications to:
- Bill clients based on document complexity
- Compare versions of legal documents
- Identify sections that may need simplification

## Going Further

Document statistics open up many possibilities for document analysis and management. Here are some ways to extend your use of document statistics:

1. Trend Analysis: Track how document metrics change over time to identify patterns
2. Comparative Analysis: Compare statistics across multiple documents to identify outliers
3. Readability Scoring: Combine word and sentence counts to calculate readability metrics
4. Content Pricing: Implement automated pricing systems based on document length and complexity
5. Productivity Metrics: Track content creation speed by measuring word count over time

By incorporating Aspose.Words Cloud's document statistics capabilities into your applications, you can build powerful document analysis and management systems without requiring Microsoft Word installation.

## See Also

- [Product Page](https://products.aspose.cloud/words/)
- [Documentation](https://docs.aspose.cloud/words/)
- [Live Demo](https://products.aspose.app/words/family)
- [API Reference](https://reference.aspose.cloud/words/)
- [Blog](https://blog.aspose.cloud/category/words/)
- [Free Support](https://forum.aspose.cloud/c/words/17)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
