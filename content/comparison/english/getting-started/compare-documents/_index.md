---
title: How to Compare Documents with GroupDocs.Comparison Cloud Tutorial
weight: 3
description: Learn how to compare documents and identify changes between versions using GroupDocs.Comparison Cloud API in this comprehensive tutorial
url: /getting-started/compare-documents/
---

# Tutorial: How to Compare Documents with GroupDocs.Comparison Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Compare two documents and identify differences between them
- Understand the change detection capabilities of GroupDocs.Comparison Cloud
- Implement document comparison in different programming languages
- Interpret and work with comparison results

## Prerequisites

Before starting this tutorial, you'll need:
- A GroupDocs.Comparison Cloud account (get a [free trial here](https://dashboard.groupdocs.cloud/#/apps))
- Your Client ID and Client Secret credentials
- Source and target documents uploaded to your GroupDocs Cloud Storage
- Basic familiarity with RESTful APIs and your chosen programming language

## Understanding Document Comparison

GroupDocs.Comparison Cloud API provides powerful document comparison capabilities that can detect changes in different document parts including:

- Text blocks (paragraphs, words, and characters)
- Tables
- Images
- Shapes and other elements

Changes are highlighted with different colors in the resulting document:
- Added content - blue
- Modified content - green
- Style changes - green
- Deleted content - red

## Step 1: Upload Documents (If Needed)

Before comparing documents, make sure you have uploaded both source and target documents to your cloud storage. For detailed instructions on file uploads, refer to the [File API documentation](https://docs.groupdocs.cloud/comparison/developer-guide/working-with-file-api/).

## Step 2: Understanding the API Resource

To compare documents, we'll use the following GroupDocs.Comparison Cloud REST API resource:

`POST https://api.groupdocs.cloud/v2.0/comparison/comparisons`

This endpoint requires a JSON body with the source and target file paths, as well as the output path for the comparison result.

## Step 3: Implementing with cURL

Let's start with a cURL example to understand the request and response format:

```bash
# First, get the JWT token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Use the token to compare documents
curl -v "https://api.groupdocs.cloud/v2.0/comparison/comparisons" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
  'SourceFile': {
    'FilePath': 'source_files/word/source.docx'
  },
  'TargetFiles': [
    {
      'FilePath': 'target_files/word/target.docx'
    }
  ],
  'OutputPath': 'output/result.docx'
}"
```

Replace `YOUR_CLIENT_ID`, `YOUR_CLIENT_SECRET`, and `YOUR_JWT_TOKEN` with your actual credentials.

The response will provide information about the comparison result file:

```json
{
  "href": "https://api.groupdocs.cloud/v2.0/comparison/storage/file/output/result.docx",
  "rel": "output/result.docx",
  "type": "file",
  "title": "result.docx"
}
```

## Step 4: Implementing with SDKs

Now let's implement the same functionality using various SDKs.

### C# Example

```csharp
using System;
using System.Collections.Generic;
using GroupDocs.Comparison.Cloud.Sdk.Api;
using GroupDocs.Comparison.Cloud.Sdk.Client;
using GroupDocs.Comparison.Cloud.Sdk.Model;
using GroupDocs.Comparison.Cloud.Sdk.Model.Requests;

namespace CompareDocumentsExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API credentials
            var configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
            
            // Create CompareApi instance
            var compareApi = new CompareApi(configuration);
            
            // Prepare comparison options
            var options = new ComparisonOptions
            {
                SourceFile = new FileInfo { FilePath = "source_files/word/source.docx" },
                TargetFiles = new List<FileInfo> { new FileInfo { FilePath = "target_files/word/target.docx" } },
                OutputPath = "output/result.docx"
            };
            
            // Create comparison request
            var request = new ComparisonsRequest(options);
            
            try
            {
                // Execute comparison
                var response = compareApi.Comparisons(request);
                
                // Display result information
                Console.WriteLine($"Comparison completed successfully!");
                Console.WriteLine($"Result file URL: {response.Href}");
            }
            catch (Exception e)
            {
                Console.WriteLine($"Error: {e.Message}");
            }
        }
    }
}
```

### Python Example

```python
import groupdocs_comparison_cloud
from groupdocs_comparison_cloud import ComparisonOptions, FileInfo

# Configure API credentials
configuration = groupdocs_comparison_cloud.Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create CompareApi instance
compare_api = groupdocs_comparison_cloud.CompareApi.from_keys(
    configuration.client_id, configuration.client_secret
)

# Prepare comparison options
source_file = FileInfo()
source_file.file_path = "source_files/word/source.docx"

target_file = FileInfo()
target_file.file_path = "target_files/word/target.docx"

options = ComparisonOptions()
options.source_file = source_file
options.target_files = [target_file]
options.output_path = "output/result.docx"

# Create comparison request
request = groupdocs_comparison_cloud.ComparisonsRequest(options)

try:
    # Execute comparison
    response = compare_api.comparisons(request)
    
    # Display result information
    print("Comparison completed successfully!")
    print(f"Result file URL: {response.href}")
except groupdocs_comparison_cloud.ApiException as e:
    print(f"Error: {e}")
```

### Java Example

```java
import com.groupdocs.cloud.comparison.client.*;
import com.groupdocs.cloud.comparison.model.*;
import com.groupdocs.cloud.comparison.api.CompareApi;
import com.groupdocs.cloud.comparison.model.requests.ComparisonsRequest;
import java.util.Collections;

public class CompareDocumentsExample {
    public static void main(String[] args) {
        // Configure API credentials
        Configuration configuration = new Configuration("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
        
        // Create CompareApi instance
        CompareApi compareApi = new CompareApi(configuration);
        
        // Prepare comparison options
        FileInfo sourceFileInfo = new FileInfo();
        sourceFileInfo.setFilePath("source_files/word/source.docx");
        
        FileInfo targetFileInfo = new FileInfo();
        targetFileInfo.setFilePath("target_files/word/target.docx");
        
        ComparisonOptions options = new ComparisonOptions();
        options.setSourceFile(sourceFileInfo);
        options.setTargetFiles(Collections.singletonList(targetFileInfo));
        options.setOutputPath("output/result.docx");
        
        // Create comparison request
        ComparisonsRequest request = new ComparisonsRequest(options);
        
        try {
            // Execute comparison
            Link response = compareApi.comparisons(request);
            
            // Display result information
            System.out.println("Comparison completed successfully!");
            System.out.println("Result file URL: " + response.getHref());
        } catch (ApiException e) {
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Node.js Example

```javascript
// Load the GroupDocs.Comparison Cloud SDK
const comparison = require("groupdocs-comparison-cloud");

// Configure API credentials
const clientId = "YOUR_CLIENT_ID";
const clientSecret = "YOUR_CLIENT_SECRET";

// Create CompareApi instance
const compareApi = comparison.CompareApi.fromKeys(clientId, clientSecret);

// Prepare comparison options
const sourceFile = new comparison.FileInfo();
sourceFile.filePath = "source_files/word/source.docx";

const targetFile = new comparison.FileInfo();
targetFile.filePath = "target_files/word/target.docx";

const options = new comparison.ComparisonOptions();
options.sourceFile = sourceFile;
options.targetFiles = [targetFile];
options.outputPath = "output/result.docx";

// Create comparison request
const request = new comparison.ComparisonsRequest(options);

// Execute comparison
compareApi.comparisons(request)
    .then(response => {
        console.log("Comparison completed successfully!");
        console.log(`Result file URL: ${response.href}`);
    })
    .catch(error => {
        console.log("Error:", error.message);
    });
```

## Step 5: Customizing Comparison Options

GroupDocs.Comparison Cloud allows customizing the comparison process with additional options:

```csharp
// Additional comparison options
options.CompareOptions = new CompareOptions
{
    // Show deleted content
    ShowDeletedContent = true,
    // Show inserted content
    ShowInsertedContent = true,
    // Show styled content
    ShowStyleChangedContent = true,
    // Set sensitivity level (higher means more changes detected)
    SensitivityOfComparison = 75,
    // Set comparison style colors
    DeletedItemsStyle = new StyleSettings { HighlightColor = "Red" },
    InsertedItemsStyle = new StyleSettings { HighlightColor = "Blue" },
    ChangedItemsStyle = new StyleSettings { HighlightColor = "Green" },
    // Process headers and footers
    HeaderFootersComparison = true
};
```

## Step 6: Understanding the Comparison Results

After successful comparison, the API returns a link to the result document. This document contains visual highlights of all changes detected between the source and target documents.

For a detailed analysis, you can:

1. Download the result document using the provided link
2. Open the document to view highlighted changes
3. Use the color coding to identify the types of changes:
   - Blue highlights indicate added content
   - Green highlights indicate modified content
   - Red highlights indicate deleted content

## Try It Yourself

Now it's your turn to practice:

1. Upload two slightly different versions of a document to your cloud storage
2. Use the code examples to compare these documents
3. Download and examine the comparison result
4. Experiment with different comparison options to see how they affect the results
5. Try comparing different document formats (PDF, Word, Excel, etc.)

## Practical Use Cases

Document comparison has many practical applications:

1. Contract Review: Identify changes between contract versions
2. Document Collaboration: Track changes made by different contributors
3. Quality Assurance: Verify document updates meet requirements
4. Compliance: Ensure document modifications comply with regulations
5. Version Control: Maintain a history of document changes

## Troubleshooting Tips

- File Not Found: Ensure both source and target files exist at the specified paths
- Unsupported Format: Verify both documents are in supported formats
- Empty Result: Check that the documents actually contain differences
- Authorization Issues: Confirm your Client ID and Client Secret are correct
- Result File Issues: Ensure the output path is valid and you have write permissions

## What You've Learned

In this tutorial, you've learned:
- How to compare two documents and identify differences between them
- How to customize the comparison process for specific needs
- How to implement document comparison in different programming languages
- How to interpret and work with comparison results
- Practical applications for document comparison

## Next Steps

Now that you can perform basic document comparisons, you can explore more specific comparison scenarios:
- [Tutorial: How to Compare PDF Files](/getting-started/compare-pdf-files/)

You can also learn how to generate previews of your documents:
- [Tutorial: How to Create Document Previews](/getting-started/create-document-preview/)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Free Support Forum](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
