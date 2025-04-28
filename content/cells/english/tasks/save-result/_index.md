---
title: Guide to SaveResult Task Operations
second_title: Aspose.Cells Cloud API Tutorial

url: /tasks/save-result/
keywords: REST API, task, save result, cloud storage, excel, tutorial, Learn Excel, Office Cloud, REST API, Spreadsheet, Tutorial, SaveResult, Cloud Storage
description: Learn how to save processing results from Aspose.Cells Cloud API tasks to cloud storage or return them in your response with step-by-step examples.
weight: 40
---

# Guide to SaveResult Task Operations

## Introduction

In this tutorial, you'll learn how to use the SaveResult task in Aspose.Cells Cloud API to manage the output of your spreadsheet processing operations. The SaveResult task is a critical component that allows you to control where and how the results of your API operations are saved or returned to the client.

## Prerequisites

Before starting this tutorial, make sure you have:
- An Aspose Cloud account ([sign up for free here](https://dashboard.aspose.cloud/#/apps))
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- Basic understanding of REST API concepts
- Completed at least one of our other task tutorials (recommended)

## Understanding the SaveResult Task

The SaveResult task requires two key pieces of information:

1. ResultSource - Where to find the result file:
   - InMemoryFiles: Files that were created or modified in the current task sequence
   - Response: Direct response from a previous task

2. ResultDestination - Where to save or send the result:
   - CloudFileSystem: Save to your Aspose Cloud storage
   - OutputStream: Return in the API response
   - Local: Save to local storage (server-side, for internal use)

## Tutorial: Saving Results to Cloud Storage

Let's create a workflow that performs operations and then saves the results to your Aspose Cloud storage.

### Step 1: Structure Your Basic SaveResult Request

```xml
<TaskDescription>
  <TaskType>SaveResult</TaskType>
  <SaveResultTaskParameter>
    <ResultSource>InMemoryFiles</ResultSource>
    <ResultDestination>
      <DestinationType>CloudFileSystem</DestinationType>
      <InputFile>ProcessedFile.xlsx</InputFile>
      <OutputFile>Results/FinalOutput.xlsx</OutputFile>
    </ResultDestination>
  </SaveResultTaskParameter>
</TaskDescription>
```

### Step 2: Incorporate in a Complete Workflow

Let's see how the SaveResult task fits into a complete workflow, where we first import data and then save the result:

```xml
<TaskData>
  <Tasks>
    <!-- First task: Import data into a spreadsheet -->
    <TaskDescription>
      <TaskType>ImportData</TaskType>
      <ImportDataTaskParameter>
        <Workbook>
          <FileSourceType>CloudFileSystem</FileSourceType>
          <FilePath>Template.xlsx</FilePath>
        </Workbook>
        <ImportBatchDataOption>
          <DestinationWorksheet>Sheet1</DestinationWorksheet>
          <IsInsert>true</IsInsert>
          <BatchData>
            <!-- Sample data cells -->
            <CellValue>
              <rowIndex>0</rowIndex>
              <columnIndex>1</columnIndex>
              <type>String</type>
              <value>Value</value>
            </CellValue>
            <CellValue>
              <rowIndex>1</rowIndex>
              <columnIndex>0</columnIndex>
              <type>String</type>
              <value>Product A</value>
            </CellValue>
            <CellValue>
              <rowIndex>1</rowIndex>
              <columnIndex>1</columnIndex>
              <type>Double</type>
              <value>25.50</value>
            </CellValue>
          </BatchData>
        </ImportBatchDataOption>
      </ImportDataTaskParameter>
    </TaskDescription>
    
    <!-- Second task: Save the result to cloud storage -->
    <TaskDescription>
      <TaskType>SaveResult</TaskType>
      <SaveResultTaskParameter>
        <ResultSource>InMemoryFiles</ResultSource>
        <ResultDestination>
          <DestinationType>CloudFileSystem</DestinationType>
          <InputFile>Template.xlsx</InputFile>
          <OutputFile>Processed/DataImport_Result.xlsx</OutputFile>
        </ResultDestination>
      </SaveResultTaskParameter>
    </TaskDescription>
  </Tasks>
</TaskData>
```

### Step 3: Execute the API Request

#### Using cURL

```bash
curl -X POST "https://api.aspose.cloud/v3.0/cells/task/runtask" \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d "YOUR_XML_REQUEST"
```

### Understanding the Result

After executing the request:
1. The ImportData task loads your template and adds data to it
2. The SaveResult task stores the modified file in your Aspose Cloud storage under the "Processed" folder with the name "DataImport_Result.xlsx"
3. The API response confirms the operation's success

## Tutorial: Returning Results in the API Response

Sometimes you want to get the processed file directly in your API response rather than saving it to cloud storage.

### Step 1: Configure the SaveResult Task for Response Output

```xml
<TaskDescription>
  <TaskType>SaveResult</TaskType>
  <SaveResultTaskParameter>
    <ResultSource>InMemoryFiles</ResultSource>
    <ResultDestination>
      <DestinationType>OutputStream</DestinationType>
      <InputFile>ProcessedWorkbook.xlsx</InputFile>
      <OutputFile>Result.xlsx</OutputFile>
    </ResultDestination>
  </SaveResultTaskParameter>
</TaskDescription>
```

### Step 2: Handle the Binary Response

When you use `OutputStream` as the destination type, the API response will contain the binary data of your file. Your client code needs to handle this appropriately.

#### Using C# to Process the Response

```csharp
var xml = @"<TaskData>
  <!-- Your tasks XML here -->
</TaskData>";

ServiceHelper helper = new ServiceHelper(sid, key);
using (HttpWebResponse response = helper.CallPost("https://api.aspose.cloud/v3.0/cells/task/runtask", xml, "application/xml"))
{
    if (response.StatusCode == HttpStatusCode.OK)
    {
        System.Console.WriteLine("OK");
        Stream st = response.GetResponseStream();
        FileStream fs = new FileStream("Result.xlsx", FileMode.OpenOrCreate);
        st.CopyTo(fs);
    }
}
```

## Advanced: Multiple SaveResult Operations

You can save the results of your operations to multiple destinations. For example, you might want to:
1. Save a copy to your cloud storage for archiving
2. Return the file in the response for immediate use

```xml
<TaskData>
  <Tasks>
    <!-- Your processing tasks here -->
    
    <!-- Save to cloud storage -->
    <TaskDescription>
      <TaskType>SaveResult</TaskType>
      <SaveResultTaskParameter>
        <ResultSource>InMemoryFiles</ResultSource>
        <ResultDestination>
          <DestinationType>CloudFileSystem</DestinationType>
          <InputFile>ProcessedFile.xlsx</InputFile>
          <OutputFile>Archive/Processed_2023-09-15.xlsx</OutputFile>
        </ResultDestination>
      </SaveResultTaskParameter>
    </TaskDescription>
    
    <!-- Return in response -->
    <TaskDescription>
      <TaskType>SaveResult</TaskType>
      <SaveResultTaskParameter>
        <ResultSource>InMemoryFiles</ResultSource>
        <ResultDestination>
          <DestinationType>OutputStream</DestinationType>
          <InputFile>ProcessedFile.xlsx</InputFile>
          <OutputFile>Response.xlsx</OutputFile>
        </ResultDestination>
      </SaveResultTaskParameter>
    </TaskDescription>
  </Tasks>
</TaskData>
```

## Saving Different File Formats

The SaveResult task works with any file format produced by previous tasks, not just Excel files. You can save PDFs, images, CSV files, etc.

### Example: Saving a PDF Conversion Result

```xml
<TaskData>
  <Tasks>
    <!-- Convert Excel to PDF -->
    <TaskDescription>
      <TaskType>Convert</TaskType>
      <ConvertTaskParameter>
        <Workbook>
          <FileSourceType>CloudFileSystem</FileSourceType>
          <FilePath>Source.xlsx</FilePath>
        </Workbook>
        <DestinationFile>Temp.pdf</DestinationFile>
      </ConvertTaskParameter>
    </TaskDescription>
    
    <!-- Save the PDF result -->
    <TaskDescription>
      <TaskType>SaveResult</TaskType>
      <SaveResultTaskParameter>
        <ResultSource>InMemoryFiles</ResultSource>
        <ResultDestination>
          <DestinationType>OutputStream</DestinationType>
          <InputFile>Temp.pdf</InputFile>
          <OutputFile>Output.pdf</OutputFile>
        </ResultDestination>
      </SaveResultTaskParameter>
    </TaskDescription>
  </Tasks>
</TaskData>
```

## Common Issues and Troubleshooting

### Issue: File Not Found

If you get a "file not found" error:
- Check that the InputFile name exactly matches the file created by previous tasks
- Verify that the previous tasks successfully completed
- Ensure the file path structure is correct

### Issue: Permission Denied

If you get permission issues when saving to cloud storage:
- Verify your access token has the necessary permissions
- Check that the destination folder exists in your cloud storage
- Ensure you're not trying to overwrite a read-only file

## Best Practices for SaveResult

1. Consistent Naming Conventions: Use a consistent naming scheme for your output files
2. Folder Organization: Organize files in your cloud storage with descriptive folder names
3. Error Handling: Always check the API response for success or error information
4. Temporary Files: Use the InMemoryFiles source for intermediate results that don't need to be permanently stored
5. Unique Filenames: Use timestamps or GUIDs in filenames to avoid overwriting existing files

## Try It Yourself

1. Create a workflow that:
   - Imports data into a spreadsheet
   - Converts it to PDF
   - Saves both the Excel and PDF versions to cloud storage
   
2. Implement a workflow that:
   - Loads an Excel file
   - Performs multiple operations
   - Returns the result directly in the response

## What You've Learned

In this tutorial, you've learned:
- How to save task results to your Aspose Cloud storage
- Methods for returning processed files directly in API responses
- Techniques for managing different file types with SaveResult
- How to chain SaveResult with other tasks for complete workflows
- Best practices for file organization and naming

## Further Practice

1. Create a scheduled task that processes files and saves them with date-stamped names
2. Implement a workflow that processes a file and saves the results in multiple formats
3. Build an archiving system that organizes files by date and type in your cloud storage

## Next Steps

Now that you've mastered saving results, you might want to explore other Aspose.Cells Cloud API capabilities:
- [SmartMarker Task Tutorial](/tasks/smart-marker/) for template-based document generation
- [CellsObjectOperate Task Tutorial](/tasks/cells-object-operate/) for advanced Excel object operations

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cells/)
- [Documentation](https://docs.aspose.cloud/cells/)
- [Live Demo](https://products.aspose.app/cells/family)
- [API Reference](https://reference.aspose.cloud/cells/)
- [Blog](https://blog.aspose.cloud/category/cells/)
- [Free Support](https://forum.aspose.cloud/c/cells/7)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/cells/7)
