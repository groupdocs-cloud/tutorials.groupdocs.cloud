---
title: How to Work with Files in GroupDocs.Viewer Cloud Tutorial
url: /developer-guide/working-with-files/
description: Learn how to manage files in GroupDocs.Viewer Cloud with this step-by-step tutorial covering downloading, uploading, copying, moving, and deleting files.
---

# Tutorial: How to Work with Files in GroupDocs.Viewer Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Download files from GroupDocs Cloud Storage
- Upload files to GroupDocs Cloud Storage
- Delete files from GroupDocs Cloud Storage
- Copy files within GroupDocs Cloud Storage
- Move files within GroupDocs Cloud Storage

## Prerequisites

Before you begin this tutorial, you need:
- A GroupDocs Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
- Client ID and Client Secret credentials
- Basic understanding of REST APIs and your preferred programming language
- Development environment with the respective GroupDocs.Viewer Cloud SDK installed

## Introduction

Managing files is a fundamental requirement when working with document viewing solutions. GroupDocs.Viewer Cloud provides comprehensive APIs to handle all aspects of file management. In this tutorial, we'll explore how to perform essential file operations.

Let's start with a real-world scenario: You're developing a document management system that needs to fetch documents from the cloud, display them to users, and allow for file operations like uploading new versions or organizing documents.

## 1. Download Files from Cloud Storage

The first operation we'll learn is downloading files. This is essential when you need to retrieve documents from your cloud storage.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the file path of the document you want to download
3. Optionally specify the storage name (if you're not using the default storage)
4. Execute the download request
5. Process the downloaded file

### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/viewer/storage/file/one-page.docx?storageName=MyStorage" \
     -H "accept: multipart/form-data" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's see how to implement this using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class DownloadFileExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FileApi(configuration);

            try
            {
                // Create request with file path and storage name
                var request = new DownloadFileRequest("viewerdocs/one-page.docx", Common.MyStorage);

                // Execute the request
                var response = apiInstance.DownloadFile(request);
                Console.WriteLine("File downloaded successfully, size: " + response.Length);
                
                // Now you can process the downloaded file, e.g., save it locally
                // System.IO.File.WriteAllBytes("local-path/one-page.docx", response);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error downloading file: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.requests.*;
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;

public class DownloadFileExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FileApi apiInstance = new FileApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create request with file path and storage name
            DownloadFileRequest request = new DownloadFileRequest(
                "viewerdocs/one-page.docx", // File path
                Common.MyStorage,           // Storage name
                null                        // Version ID (optional)
            );
            
            // Execute the request
            File response = apiInstance.downloadFile(request);
            System.out.println("File downloaded successfully, size: " + response.length());
            
            // Optional: Save to local path
            // try (OutputStream os = new FileOutputStream("local-path/one-page.docx")) {
            //     // Process the file as needed
            // }
        } catch (ApiException e) {
            System.err.println("Error downloading file: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import DownloadFileRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FileApi(configuration)

try:
    # Create request with file path and storage name
    request = DownloadFileRequest("viewerdocs/one-page.docx", Common.my_storage)
    
    # Execute the request
    response = api_instance.download_file(request)
    
    print(f"File downloaded successfully, size: {len(response)}")
    
    # Optional: Save to local path
    # with open("local-path/one-page.docx", "wb") as f:
    #     f.write(response)
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error downloading file: {e.message}")
```

### Try It Yourself

1. Replace `YOUR_APP_SID` and `YOUR_APP_KEY` with your actual application credentials
2. Replace the file path with a document in your storage
3. Run the code and verify that the file downloads successfully
4. Check the response size to make sure it matches your file size

## 2. Upload Files to Cloud Storage

Now let's learn how to upload files to your cloud storage, which is necessary when adding new documents to your system.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Prepare the file you want to upload
3. Specify the destination path in cloud storage
4. Optionally specify the storage name
5. Execute the upload request
6. Verify the upload was successful

### cURL Example

```bash
curl -X POST "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewerdocs/sample-document.docx?storageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: multipart/form-data" \
     -F "fileContent=@/local-path/sample-document.docx"
```

### SDK Examples

Let's see how to implement file uploads using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;
using System.IO;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class UploadFileExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FileApi(configuration);

            try
            {
                // Prepare the file stream for upload
                var fileStream = File.Open("local-path/sample-document.docx", FileMode.Open);

                // Create upload request
                var request = new UploadFileRequest(
                    "viewerdocs/sample-document.docx", // Destination file path
                    fileStream,                        // File content
                    Common.MyStorage                   // Storage name
                );

                // Execute the request
                var response = apiInstance.UploadFile(request);
                Console.WriteLine("File uploaded successfully. Files uploaded: " + response.Uploaded.Count);
            }
            catch (Exception e)
            {
                Console.WriteLine("Error uploading file: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.*;
import com.groupdocs.cloud.viewer.model.requests.*;
import java.io.File;
import java.nio.file.Paths;

public class UploadFileExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FileApi apiInstance = new FileApi(Common.AppSID, Common.AppKey);
        
        try {
            // Prepare the file for upload
            File fileToUpload = new File(Paths.get("local-path/sample-document.docx").toString());
            
            // Create upload request
            UploadFileRequest request = new UploadFileRequest(
                "viewerdocs/sample-document.docx", // Destination path
                fileToUpload,                      // File content
                Common.MyStorage                   // Storage name
            );
            
            // Execute the request
            FilesUploadResult response = apiInstance.uploadFile(request);
            System.out.println("File uploaded successfully. Files uploaded: " + response.getUploaded().size());
        } catch (ApiException e) {
            System.err.println("Error uploading file: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import UploadFileRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FileApi(configuration)

try:
    # Prepare file path for upload
    local_file_path = "local-path/sample-document.docx"
    
    # Create upload request
    request = groupdocs_viewer_cloud.UploadFileRequest(
        "viewerdocs/sample-document.docx",  # Destination path
        local_file_path,                    # Local file path
        Common.my_storage                   # Storage name
    )
    
    # Execute the request
    response = api_instance.upload_file(request)
    
    print(f"File uploaded successfully. Files uploaded: {len(response.uploaded)}")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error uploading file: {e.message}")
```

### Try It Yourself

1. Replace the file path with your local document path
2. Replace the destination path with your preferred path in cloud storage
3. Run the code and verify that the upload was successful
4. Check your cloud storage dashboard to confirm the file appears

## 3. Delete Files from Cloud Storage

Next, let's learn how to delete files from your cloud storage, which is necessary for managing document lifecycles.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the path of the file you want to delete
3. Optionally specify the storage name
4. Execute the delete request
5. Verify the file was successfully deleted

### cURL Example

```bash
curl -X DELETE "https://api.groupdocs.cloud/v2.0/viewer/storage/file/viewerdocs/sample-document.docx?storageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's implement file deletion using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class DeleteFileExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FileApi(configuration);

            try
            {
                // Create delete request
                var request = new DeleteFileRequest(
                    "viewerdocs/sample-document.docx", // File path
                    Common.MyStorage                   // Storage name
                );

                // Execute the request
                apiInstance.DeleteFile(request);
                Console.WriteLine("File deleted successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error deleting file: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.requests.*;

public class DeleteFileExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FileApi apiInstance = new FileApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create delete request
            DeleteFileRequest request = new DeleteFileRequest(
                "viewerdocs/sample-document.docx", // File path
                Common.MyStorage,                  // Storage name
                null                               // Version ID (optional)
            );
            
            // Execute the request
            apiInstance.deleteFile(request);
            System.out.println("File deleted successfully.");
        } catch (ApiException e) {
            System.err.println("Error deleting file: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import DeleteFileRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FileApi(configuration)

try:
    # Create delete request
    request = DeleteFileRequest(
        "viewerdocs/sample-document.docx",  # File path
        Common.my_storage                   # Storage name
    )
    
    # Execute the request
    api_instance.delete_file(request)
    
    print("File deleted successfully.")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error deleting file: {e.message}")
```

### Try It Yourself

1. Replace the file path with a document in your storage that you wish to delete
2. Run the code and verify that the deletion was successful
3. Check your cloud storage dashboard to confirm the file no longer appears

## 4. Copy Files within Cloud Storage

Let's explore how to copy files within your cloud storage, which is useful when you need to duplicate documents.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the source file path
3. Specify the destination file path
4. Optionally specify the source and destination storage names
5. Execute the copy request
6. Verify the file was successfully copied

### cURL Example

```bash
curl -X PUT "https://api.groupdocs.cloud/v2.0/viewer/storage/file/copy/viewerdocs/source-document.docx?destPath=viewerdocs/destination-document.docx&srcStorageName=MyStorage&destStorageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's implement file copying using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class CopyFileExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FileApi(configuration);

            try
            {
                // Create copy request
                var request = new CopyFileRequest(
                    "viewerdocs/source-document.docx",      // Source file path
                    "viewerdocs/destination-document.docx", // Destination file path
                    Common.MyStorage,                       // Source storage name
                    Common.MyStorage                        // Destination storage name
                );

                // Execute the request
                apiInstance.CopyFile(request);
                Console.WriteLine("File copied successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error copying file: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.requests.*;

public class CopyFileExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FileApi apiInstance = new FileApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create copy request
            CopyFileRequest request = new CopyFileRequest(
                "viewerdocs/source-document.docx",      // Source file path
                "viewerdocs/destination-document.docx", // Destination file path
                Common.MyStorage,                       // Source storage name
                Common.MyStorage,                       // Destination storage name
                null                                    // Version ID (optional)
            );
            
            // Execute the request
            apiInstance.copyFile(request);
            System.out.println("File copied successfully.");
        } catch (ApiException e) {
            System.err.println("Error copying file: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import CopyFileRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FileApi(configuration)

try:
    # Create copy request
    request = CopyFileRequest(
        "viewerdocs/source-document.docx",      # Source file path
        "viewerdocs/destination-document.docx", # Destination file path
        Common.my_storage,                      # Source storage name
        Common.my_storage                       # Destination storage name
    )
    
    # Execute the request
    api_instance.copy_file(request)
    
    print("File copied successfully.")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error copying file: {e.message}")
```

### Try It Yourself

1. Replace the source file path with a document in your storage
2. Replace the destination file path with your preferred path
3. Run the code and verify that the copy was successful
4. Check your cloud storage dashboard to confirm both files appear

## 5. Move Files within Cloud Storage

Finally, let's learn how to move files within your cloud storage, which is useful for organizing your documents.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the source file path
3. Specify the destination file path
4. Optionally specify the source and destination storage names
5. Execute the move request
6. Verify the file was successfully moved

### cURL Example

```bash
curl -X PUT "https://api.groupdocs.cloud/v2.0/viewer/storage/file/move/viewerdocs/source-document.docx?destPath=viewerdocs/moved-document.docx&srcStorageName=MyStorage&destStorageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's implement file moving using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class MoveFileExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new FileApi(configuration);

            try
            {
                // Create move request
                var request = new MoveFileRequest(
                    "viewerdocs/source-document.docx",  // Source file path
                    "viewerdocs/moved-document.docx",   // Destination file path
                    Common.MyStorage,                   // Source storage name
                    Common.MyStorage                    // Destination storage name
                );

                // Execute the request
                apiInstance.MoveFile(request);
                Console.WriteLine("File moved successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error moving file: " + e.Message);
            }
        }
    }
}
```

#### Java Example

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.model.requests.*;

public class MoveFileExample {

    public static void main(String[] args) {
        // Authenticate with the API
        FileApi apiInstance = new FileApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create move request
            MoveFileRequest request = new MoveFileRequest(
                "viewerdocs/source-document.docx",  // Source file path
                "viewerdocs/moved-document.docx",   // Destination file path
                Common.MyStorage,                   // Source storage name
                Common.MyStorage,                   // Destination storage name
                null                                // Version ID (optional)
            );
            
            // Execute the request
            apiInstance.moveFile(request);
            System.out.println("File moved successfully.");
        } catch (ApiException e) {
            System.err.println("Error moving file: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import MoveFileRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.FileApi(configuration)

try:
    # Create move request
    request = MoveFileRequest(
        "viewerdocs/source-document.docx",  # Source file path
        "viewerdocs/moved-document.docx",   # Destination file path
        Common.my_storage,                  # Source storage name
        Common.my_storage                   # Destination storage name
    )
    
    # Execute the request
    api_instance.move_file(request)
    
    print("File moved successfully.")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error moving file: {e.message}")
```

### Try It Yourself

1. Replace the source file path with a document in your storage
2. Replace the destination file path with your preferred path
3. Run the code and verify that the move was successful
4. Check your cloud storage dashboard to confirm the file appears in its new location and no longer exists at the original location

## Common Issues and Troubleshooting

When working with file operations, you might encounter these common issues:

1. Authentication Errors
   - Problem: API returns 401 Unauthorized
   - Solution: Check your Client ID and Client Secret; make sure your token hasn't expired

2. File Not Found
   - Problem: API returns 404 Not Found when trying to download, delete, copy, or move a file
   - Solution: Double-check the file path and storage name; verify the file exists

3. Permission Issues
   - Problem: API returns 403 Forbidden
   - Solution: Verify your account has the necessary permissions for the requested operation

4. Storage Quota Exceeded
   - Problem: Upload fails with a storage quota error
   - Solution: Free up space in your storage or upgrade your plan

## What You've Learned

In this tutorial, you've learned how to:
- Download files from your GroupDocs Cloud Storage
- Upload files to your GroupDocs Cloud Storage
- Delete files from your GroupDocs Cloud Storage
- Copy files within your GroupDocs Cloud Storage
- Move files within your GroupDocs Cloud Storage

You now have the essential skills to manage files for your document viewing applications.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a small application that uploads a file and then downloads it
2. Write a script that moves files from one folder to another based on file type
3. Implement a file management utility that handles bulk operations

## Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).
