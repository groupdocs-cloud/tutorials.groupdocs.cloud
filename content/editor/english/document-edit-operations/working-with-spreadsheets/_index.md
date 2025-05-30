---
title: How to Edit Spreadsheet Documents with GroupDocs.Editor Cloud Tutorial
url: /document-edit-operations/working-with-spreadsheets/
weight: 2
description: This tutorial guides you through editing Excel and other spreadsheet formats in the cloud with step-by-step instructions using GroupDocs.Editor API.
---

# Tutorial: Working with Spreadsheet Documents

## Learning Objectives

In this tutorial, you'll learn how to:
- Load spreadsheet documents (XLS, XLSX, XLSM, etc.) into editable HTML format
- Configure worksheet-specific options during loading
- Edit spreadsheet content in HTML format
- Save the edited content back to spreadsheet format
- Implement the complete edit workflow using different programming languages

## Prerequisites

Before starting this tutorial, you should have:
- A [GroupDocs.Editor Cloud dashboard](https://dashboard.groupdocs.cloud/) account
- Your Client ID and Client Secret credentials
- Basic understanding of REST API concepts
- Familiarity with spreadsheet structure and editing
- Basic knowledge of your preferred programming language

## The Spreadsheet Editing Process

GroupDocs.Editor Cloud supports various spreadsheet formats including XLS, XLSX, XLSM, XLSB, ODS, SXC, and others. The editing process follows a structured workflow:

### Step 1: Upload Your Spreadsheet to Cloud Storage

First, upload your spreadsheet document to GroupDocs cloud storage. For details on storage operations, refer to the [Storage Operations Tutorial](/storage-operations).

### Step 2: Load the Spreadsheet for Editing

To load a spreadsheet document for editing, use the following API endpoint:

```
HTTP POST ~/load
```

When loading spreadsheet documents, you can configure these specific options:

| Option | Description |
|--------|-------------|
| FileInfo.FilePath | The file path in storage (required) |
| FileInfo.StorageName | Storage name (optional for default storage) |
| FileInfo.Password | Password for protected documents |
| OutputPath | Directory where editable files will be stored |
| WorksheetIndex | 0-based index of the worksheet to edit (required) |
| ExcludeHiddenWorksheets | Whether to exclude hidden worksheets (default: false) |

#### Try It Yourself: Loading a Spreadsheet

Let's see how to load an Excel file for editing:

#### cURL Example

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now load the spreadsheet
curl -v "https://api.groupdocs.cloud/v1.0/editor/load" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'Spreadsheet/sample.xlsx' },
    'OutputPath': 'output',
    'WorksheetIndex': 0
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
file_info.file_path = "Spreadsheet/sample.xlsx"

load_options = groupdocs_editor_cloud.LoadOptions()
load_options.file_info = file_info
load_options.output_path = "output"
load_options.worksheet_index = 0  # Specify which worksheet to edit

# Execute request
result = api.load(groupdocs_editor_cloud.LoadRequest(load_options))

# Print the HTML path
print("HTML file path: " + result.html_path)
print("Resources path: " + result.resources_path)
```

### Step 3: Understanding the Output Structure

When a spreadsheet is loaded for editing, GroupDocs.Editor Cloud generates:

1. An HTML file containing the worksheet content
2. A resources folder containing styles and other dependencies

The HTML representation maintains the structure of the worksheet, including:
- Cell values and formatting
- Row and column structures
- Basic styles and colors

#### Learning Checkpoint: HTML Structure

Take a moment to examine the generated HTML file. Notice how:
- Table elements are used to represent the grid structure
- Cell styles preserve the original formatting
- Cell content maintains data types (text, numbers, dates)

### Step 4: Edit the HTML Content

After downloading the HTML file, you can edit it according to your requirements:
1. Modify cell values
2. Change formatting
3. Add or remove rows/columns
4. Update formulas

Remember that editing is performed on the client side, using your preferred HTML editor or programmatically.

### Step 5: Save the Edited Spreadsheet

After editing, upload the HTML file back to storage and use the save operation to convert it back to spreadsheet format:

```
HTTP POST ~/save
```

Key options for saving spreadsheets include:

| Option | Description |
|--------|-------------|
| FileInfo.FilePath | The output file path in storage |
| FileInfo.StorageName | Storage name (optional for default storage) |
| FileInfo.Password | Password for protected documents |
| HtmlPath | The path to the edited HTML file |
| ResourcesPath | The path to the resources folder |
| Options | Format-specific save options |

#### Try It Yourself: Saving an Edited Spreadsheet

#### cURL Example

```bash
# Save the edited spreadsheet
curl -v "https://api.groupdocs.cloud/v1.0/editor/save" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    'FileInfo': { 'FilePath': 'Spreadsheet/edited-sample.xlsx' },
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

public class EditSpreadsheet {
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
            fileInfo.setFilePath("Spreadsheet/edited-sample.xlsx");
            
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

## Practical Example: Editing a Financial Report

Let's work through a complete example of editing a quarterly financial report in Excel format:

1. Upload the document: Upload "FinancialReport.xlsx" to cloud storage
2. Load for editing: Convert the first worksheet (index 0) to HTML
3. Edit content: Update the Q3 revenue figures and fix formula calculations
4. Save the changes: Convert back to XLSX format with a new filename

This workflow demonstrates a practical application of the GroupDocs.Editor Cloud API for real-world spreadsheet editing.

## Troubleshooting Tips

- Wrong Worksheet Index: If you receive an error, verify that the worksheet index exists in your document
- Formula Issues: When editing, be careful not to break Excel formulas; the HTML representation preserves them
- Hidden Worksheets: If you need to access hidden worksheets, set `ExcludeHiddenWorksheets` to false
- Large Files: For very large spreadsheets, consider editing one worksheet at a time

## What You've Learned

In this tutorial, you've learned how to:
- Load spreadsheet documents into editable HTML format
- Work with specific worksheets in multi-sheet documents
- Edit spreadsheet content in HTML format
- Save edited content back to its original format
- Use both REST API calls and SDK methods to implement the editing workflow

## Further Practice

To reinforce your learning, try these exercises:
1. Edit a spreadsheet with multiple worksheets, loading each worksheet separately
2. Create a simple web application that allows users to edit cell values and formulas
3. Implement a batch processing system to update multiple spreadsheets

## Next Steps

Ready to learn more? Continue your journey with these related tutorials:
- [Tutorial: Working with DSV Documents](/document-edit-operations/working-with-dsv-documents/)
- [Tutorial: Working with Text Documents](/document-edit-operations/working-with-text-documents/)

## Resources

- [Product Page](https://products.groupdocs.cloud/editor/)
- [Documentation](https://docs.groupdocs.cloud/editor/)
- [Live Demo](https://products.groupdocs.app/editor/family)
- [API Reference](https://reference.groupdocs.cloud/editor/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.editor-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/editor/20/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
