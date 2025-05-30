---
title: How to Edit DSV Documents with GroupDocs.Editor Cloud Tutorial
url: /document-edit-operations/working-with-dsv-documents/
weight: 5
description: Learn to edit CSV, TSV and other delimiter-separated values files in the cloud with this step-by-step tutorial using GroupDocs.Editor Cloud API.
---

# Tutorial: Working with DSV Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Load Delimiter-Separated Values (DSV) documents for editing
- Configure delimiter and data handling options
- Edit DSV content using HTML representation
- Save the edited content back to DSV or other formats
- Implement a complete DSV editing workflow

## Prerequisites

Before starting this tutorial, you should have:
- A [GroupDocs.Editor Cloud dashboard](https://dashboard.groupdocs.cloud/) account
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with DSV file structure (CSV, TSV, etc.)
- Basic knowledge of your preferred programming language

## Understanding DSV Documents

Delimiter-Separated Values (DSV) documents are a specific form of text-based spreadsheets where values are separated by a delimiter character. Common formats include:

- CSV (Comma-Separated Values)
- TSV (Tab-Separated Values)
- Other custom formats with different delimiters

These files are widely used for data exchange between systems due to their simple structure. GroupDocs.Editor Cloud allows you to edit these files and provides options to handle different separator characters and data conversion settings.

### Step 1: Upload Your DSV Document to Cloud Storage

First, upload your DSV document to GroupDocs cloud storage. For details on storage operations, refer to the [Storage Operations Tutorial](/storage-handling-procedures/).

### Step 2: Load the DSV Document for Editing

To load a DSV document for editing, use the following API endpoint:

```
HTTP POST ~/load
```

When loading DSV documents, you can configure these specific options:

| Option | Description |
|--------|-------------|
| FileInfo.FilePath | The file path in storage (required) |
| FileInfo.StorageName | Storage name (optional for default storage) |
| OutputPath | Directory where editable files will be stored |
| Separator | String separator (delimiter) for the document |
| ConvertDateTimeData | Whether to convert strings to date data (default: false) |
| ConvertNumericData | Whether to convert strings to numeric data (default: false) |
| TreatConsecutiveDelimitersAsOne | Whether consecutive delimiters should be treated as one (default: false) |

#### Try It Yourself: Loading a DSV Document

Let's load a CSV file for editing:

#### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now load the DSV document
curl -v "https://api.groupdocs.cloud/v1.0/editor/load" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'cells/sample.csv' },
    'OutputPath': 'output',
    'Separator': ',',
    'ConvertNumericData': true
}"
```

Upon successful execution, you'll receive a response with paths to the generated HTML and resources:

```json
{
  "resourcesPath": "output/sample.files",
  "htmlPath": "output/sample.html"
}
```

#### SDK Example (Python)

```python
# For complete examples and data files, please go to https://github.com/groupdocs-editor-cloud/groupdocs-editor-cloud-python-samples
import groupdocs_editor_cloud

# Get your app_sid and app_key from https://dashboard.groupdocs.cloud
app_sid = "YOUR_CLIENT_ID"
app_key = "YOUR_CLIENT_SECRET"

# Create instance of the API
api = groupdocs_editor_cloud.EditApi.from_keys(app_sid, app_key)

# Prepare request
file_info = groupdocs_editor_cloud.FileInfo()
file_info.file_path = "cells/sample.csv"

load_options = groupdocs_editor_cloud.LoadOptions()
load_options.file_info = file_info
load_options.output_path = "output"
load_options.separator = ","  # Specify the delimiter
load_options.convert_numeric_data = True  # Convert strings to numbers where possible

# Execute request
result = api.load(groupdocs_editor_cloud.LoadRequest(load_options))

# Print the HTML path
print("HTML file path: " + result.html_path)
print("Resources path: " + result.resources_path)
```

### Step 3: Understanding DSV to HTML Conversion

When a DSV document is loaded for editing, GroupDocs.Editor Cloud converts it to an HTML table representation:

1. Each row in the DSV file becomes a row in the HTML table
2. Each field becomes a cell in the table
3. Data is converted according to settings (numeric/date conversion)
4. The first row is often treated as a header row

#### Learning Checkpoint: HTML Structure

Let's examine a sample CSV content and its HTML representation:

Original CSV:
```
Name,Age,Department,Salary
John Smith,35,Marketing,65000
Jane Doe,42,Engineering,85000
Mike Johnson,28,Sales,55000
```

Converted HTML Structure:
```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>Department</th>
      <th>Salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John Smith</td>
      <td>35</td>
      <td>Marketing</td>
      <td>65000</td>
    </tr>
    <!-- Other rows -->
  </tbody>
</table>
```

Notice how the CSV data is properly structured into an HTML table with headers and data cells.

### Step 4: Editing DSV Content

After downloading the HTML file, you can edit it using your preferred HTML editor or programmatically:

1. Modify cell values
2. Add or remove rows and columns
3. Apply formatting (which will be stripped when saving back to DSV)
4. Sort or reorganize data

When editing, remember that the table structure must be maintained for proper conversion back to DSV format.

### Step 5: Save the Edited Document

After editing, upload the HTML file back to storage and use the save operation to convert it back to DSV format:

```
HTTP POST ~/save
```

Key options for saving DSV documents include:

| Option | Description |
|--------|-------------|
| FileInfo.FilePath | The output file path in storage |
| FileInfo.StorageName | Storage name (optional for default storage) |
| HtmlPath | The path to the edited HTML file |
| ResourcesPath | The path to the resources folder |
| OutputFormat | Output format specification |

#### Try It Yourself: Saving an Edited DSV Document

#### cURL Example

```bash
# Save the edited document
curl -v "https://api.groupdocs.cloud/v1.0/editor/save" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'cells/edited-sample.csv' },
    'HtmlPath': 'output/sample.html',
    'ResourcesPath': 'output/sample.files'
}"
```

#### SDK Example (Java)

```java
// For complete examples and data files, please go to https://github.com/groupdocs-editor-cloud/groupdocs-editor-cloud-java-samples
import com.groupdocs.cloud.editor.client.*;
import com.groupdocs.cloud.editor.model.*;
import com.groupdocs.cloud.editor.api.EditApi;

public class EditDsvDocument {
    public static void main(String[] args) {
        // Get your client ID and client secret from https://dashboard.groupdocs.cloud
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Create instance of the API
        Configuration configuration = new Configuration(clientId, clientSecret);
        EditApi apiInstance = new EditApi(configuration);
        
        try {
            // Prepare save options
            FileInfo fileInfo = new FileInfo();
            fileInfo.setFilePath("cells/edited-sample.csv");
            
            SaveOptions saveOptions = new SaveOptions();
            saveOptions.setFileInfo(fileInfo);
            saveOptions.setHtmlPath("output/sample.html");
            saveOptions.setResourcesPath("output/sample.files");
            
            // Execute save request
            SaveResult response = apiInstance.save(new SaveRequest(saveOptions));
            
            // Print the result
            System.out.println("Document saved successfully to: " + fileInfo.getFilePath());
        } catch (ApiException e) {
            System.err.println("Exception when calling EditApi: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

## Practical Example: Working with Sales Data

Let's work through a complete example of updating a sales data CSV file:

### Scenario: Update Monthly Sales Report

Imagine you have a CSV file containing monthly sales data that needs updating:

1. Upload the file: Upload "MonthlySales.csv" to cloud storage
2. Load for editing:
   - Specify comma as the separator
   - Enable numeric data conversion
   - Enable date/time data conversion
3. Edit content:
   - Update sales figures for the current month
   - Add new product rows
   - Correct errors in existing data
4. Save the changes: Convert back to CSV format

This workflow demonstrates a practical application for updating tabular data stored in DSV format.

### Special Considerations for Different DSV Types

Different DSV formats may require specific handling:

- CSV (comma-separated): Most common format, typically uses `,` as separator
- TSV (tab-separated): Uses tab character `\t` as separator
- Custom formats: May use other characters like `;` or `|`

When working with these formats, be sure to specify the correct separator character in the load options.

## Troubleshooting Tips

- Delimiter Issues: If fields contain the delimiter character, ensure they're properly quoted in the original file
- Data Type Conversion: If numeric or date conversion produces unexpected results, try disabling these options
- Consecutive Delimiters: For data with empty fields, the `TreatConsecutiveDelimitersAsOne` option affects how they're processed
- Table Structure: When editing, maintain the HTML table structure to ensure proper conversion back to DSV

## What You've Learned

In this tutorial, you've learned how to:
- Load DSV documents into editable HTML format
- Configure delimiter and data conversion settings
- Edit tabular data in HTML format
- Save edited content back to DSV format
- Use both REST API calls and SDK methods to implement the editing workflow

## Further Practice

To reinforce your learning, try these exercises:
1. Create a script that converts between different DSV formats (e.g., CSV to TSV)
2. Build a web application that allows users to edit and visualize DSV data
3. Implement a data cleaning workflow for a CSV file with inconsistent formatting
4. Create a batch processor that updates multiple DSV files with new data

## Next Steps

Ready to learn more? Continue your journey with these related tutorials:
- [Tutorial: Working with Spreadsheet Documents](/document-edit-operations/working-with-spreadsheets/)

## Resources

- [Product Page](https://products.groupdocs.cloud/editor/)
- [Documentation](https://docs.groupdocs.cloud/editor/)
- [Live Demo](https://products.groupdocs.app/editor/family)
- [API Reference](https://reference.groupdocs.cloud/editor/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.editor-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/editor/20/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
