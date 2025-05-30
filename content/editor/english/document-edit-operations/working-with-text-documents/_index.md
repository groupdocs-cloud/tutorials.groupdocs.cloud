---
title: How to Edit Text Documents with GroupDocs.Editor Cloud Tutorial
url: /document-edit-operations/working-with-text-documents/
weight: 4
description: This step-by-step tutorial teaches you how to edit plain text files using GroupDocs.Editor Cloud API with options to convert to other formats.
---

# Tutorial: Working with Text Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Load plain text (TXT) documents for editing
- Configure special handling for leading and trailing spaces
- Edit text content in HTML format
- Convert edited text documents to WordProcessing formats
- Implement a complete text editing workflow

## Prerequisites

Before starting this tutorial, you should have:
- A [GroupDocs.Editor Cloud dashboard](https://dashboard.groupdocs.cloud/) account
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with text file structure
- Basic knowledge of your preferred programming language

## Understanding Text Documents

Plain Text (TXT) files are the simplest document format containing only unformatted text without embedded images, pages, or complex formatting. However, you can create simple formatting using:
- Lists with leading markers
- Indentation with whitespace
- Tables with pseudo-graphics
- Paragraphs with line breaks

GroupDocs.Editor Cloud can recognize some of these structures and also offers the unique ability to save edited text documents not only back to TXT but also to WordProcessing formats.

### Step 1: Load the Text Document for Editing

To load a text document for editing, use the following API endpoint:

```
HTTP POST ~/load
```

When loading text documents, you can configure these specific options:

| Option | Description |
|--------|-------------|
| FileInfo.FilePath | The file path in storage (required) |
| FileInfo.StorageName | Storage name (optional for default storage) |
| OutputPath | Directory where editable files will be stored |
| LeadingSpaces | How to handle leading spaces (default: convert to left indent) |
| TrailingSpaces | How to handle trailing spaces (default: truncate all trailing spaces) |
| EnablePagination | Enable/disable pagination in HTML (default: false) |

#### Try It Yourself: Loading a Text Document

Let's load a text file for editing:

#### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now load the text document
curl -v "https://api.groupdocs.cloud/v1.0/editor/load" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'Text/sample.txt' },
    'OutputPath': 'output',
    'EnablePagination': true
}"
```

Upon successful execution, you'll receive a response with paths to the generated HTML and resources:

```json
{
  "resourcesPath": "output/sample.files",
  "htmlPath": "output/sample.html"
}
```

#### SDK Example (Ruby)

```ruby
# For complete examples and data files, please go to https://github.com/groupdocs-editor-cloud/groupdocs-editor-cloud-ruby-samples
require 'groupdocs_editor_cloud'

# Get your client ID and client secret from https://dashboard.groupdocs.cloud
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# Create instance of the API
api = GroupDocsEditorCloud::EditApi.from_keys(client_id, client_secret)

# Prepare request
file_info = GroupDocsEditorCloud::FileInfo.new
file_info.file_path = "Text/sample.txt"

load_options = GroupDocsEditorCloud::LoadOptions.new
load_options.file_info = file_info
load_options.output_path = "output"
load_options.enable_pagination = true

# Execute request
result = api.load(GroupDocsEditorCloud::LoadRequest.new(load_options))

# Print the result
puts "HTML file path: #{result.html_path}"
puts "Resources path: #{result.resources_path}"
```

### Step 2: Understanding Text Document Conversion to HTML

When a text document is loaded for editing, GroupDocs.Editor Cloud:

1. Analyzes the text structure to identify potential formatting (lists, paragraphs, etc.)
2. Converts the content to HTML with appropriate structure
3. Applies formatting based on the identified structures
4. Handles spaces according to the specified options

#### Learning Checkpoint: HTML Structure

Let's examine a sample text document and its HTML representation:

Original Text:
```
Company Report
--------------
  * First quarter results
  * Second quarter forecast
    * Sales projection
    * Expense estimates
```

Converted HTML Structure:
```html
<h1>Company Report</h1>
<hr />
<ul>
  <li>First quarter results</li>
  <li>Second quarter forecast
    <ul>
      <li>Sales projection</li>
      <li>Expense estimates</li>
    </ul>
  </li>
</ul>
```

Notice how GroupDocs.Editor Cloud has recognized:
- The heading structure
- The horizontal rule (dashes)
- The nested bullet list structure
- Indentation levels

### Step 3: Edit the Text Content

After downloading the HTML file, you can edit it according to your requirements:
1. Update text content
2. Modify the recognized structure
3. Add new elements
4. Format content with HTML tags

The editing is done on the client side using your preferred HTML editor or programmatically.

### Step 4: Save the Edited Document

After editing, upload the HTML file back to storage and use the save operation to convert it back to text format or to a WordProcessing format:

```
HTTP POST ~/save
```

Key options for saving text documents include:

| Option | Description |
|--------|-------------|
| FileInfo.FilePath | The output file path in storage |
| FileInfo.StorageName | Storage name (optional for default storage) |
| HtmlPath | The path to the edited HTML file |
| ResourcesPath | The path to the resources folder |
| Format | Output format (e.g., "txt" or "docx") |

#### Try It Yourself: Saving an Edited Text Document

#### cURL Example (Save as TXT)

```bash
# Save the edited document as TXT
curl -v "https://api.groupdocs.cloud/v1.0/editor/save" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'Text/edited-sample.txt' },
    'HtmlPath': 'output/sample.html',
    'ResourcesPath': 'output/sample.files'
}"
```

#### cURL Example (Save as DOCX)

```bash
# Save the edited document as DOCX
curl -v "https://api.groupdocs.cloud/v1.0/editor/save" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'Text/edited-sample.docx' },
    'HtmlPath': 'output/sample.html',
    'ResourcesPath': 'output/sample.files',
    'Format': 'docx'
}"
```

#### SDK Example (C#)

```csharp
// For complete examples and data files, please go to https://github.com/groupdocs-editor-cloud/groupdocs-editor-cloud-dotnet-samples
string MyClientSecret = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud
string MyClientId = ""; // Get Client Id and Client Secret from https://dashboard.groupdocs.cloud

var configuration = new Configuration(MyClientId, MyClientSecret);
var apiInstance = new EditApi(configuration);

// Prepare save options (save as DOCX)
var saveOptions = new SaveOptions
{
    FileInfo = new FileInfo
    {
        FilePath = "Text/edited-sample.docx"
    },
    HtmlPath = "output/sample.html",
    ResourcesPath = "output/sample.files",
    Format = "docx"
};

// Execute save request
var response = apiInstance.Save(new SaveRequest(saveOptions));

// The edited document is now saved to the specified path
Console.WriteLine("Document saved successfully to: " + saveOptions.FileInfo.FilePath);
```

## Practical Example: Converting a Text-Based Report to Word

Let's work through a complete example of converting a text-based report to a formatted Word document:

### Scenario: Enhance a Legacy Text Report

Imagine you have a legacy system that generates monthly reports in plain text format. You need to convert these reports to Word format with proper formatting:

1. Upload the text report: Upload "MonthlyReport.txt" to cloud storage
2. Load for editing: Convert to HTML with pagination enabled
3. Enhance formatting:
   - Add proper headings and styles
   - Convert asterisk lists to proper bullet lists
   - Add tables for data sections
   - Add a company logo (through HTML)
4. Save as DOCX: Convert to Word format for better distribution

This workflow demonstrates how to enhance a plain text document by leveraging GroupDocs.Editor Cloud's capabilities.

## Troubleshooting Tips

- Space Handling: If your text relies on specific spacing, experiment with the LeadingSpaces and TrailingSpaces options
- Structure Recognition: For complex text structures, you might need to manually adjust the HTML representation
- Format Conversion: When converting to Word formats, verify that all structures were properly translated
- HTML Validation: Ensure your edited HTML is well-formed to avoid issues during saving

## What You've Learned

In this tutorial, you've learned how to:
- Load text documents into editable HTML format
- Configure special handling for spaces and pagination
- Edit text content and enhance formatting in HTML
- Save edited content back to text format or convert to Word format
- Use both REST API calls and SDK methods to implement the editing workflow

## Further Practice

To reinforce your learning, try these exercises:
1. Convert a text file with pseudo-tables (using characters like +, -, |) to a Word document with proper tables
2. Create a script that batch-processes multiple text files to format them as Word documents
3. Build a simple web interface that allows users to upload text files and enhance their formatting

## Next Steps

Ready to learn more? Continue your journey with these related tutorials:
- [Tutorial: Working with DSV Documents](/document-edit-operations/working-with-dsv-documents/)
- [Tutorial: Working with WordProcessing Documents](/document-edit-operations/working-with-wordprocessing-documents/)
- [Tutorial: Working with Spreadsheet Documents](/document-edit-operations/working-with-spreadsheets/)

## Resources

- [Product Page](https://products.groupdocs.cloud/editor/)
- [Documentation](https://docs.groupdocs.cloud/editor/)
- [Live Demo](https://products.groupdocs.app/editor/family)
- [API Reference](https://reference.groupdocs.cloud/editor/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.editor-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/editor/20/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
