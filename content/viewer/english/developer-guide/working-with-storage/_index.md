---
title: Working with Storage in GroupDocs.Viewer Cloud Tutorial
description: Learn how to manage cloud storage in GroupDocs.Viewer Cloud with this step-by-step tutorial covering storage operations, checking existence, and monitoring usage.
url: /developer-guide/working-with-storage/
weight: 40
---


# Tutorial: Working with Storage in GroupDocs.Viewer Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Check if a storage exists in GroupDocs Cloud
- Verify if a file or folder exists in cloud storage
- Monitor storage space usage
- Retrieve file versions information

## Prerequisites

Before you begin this tutorial, you need:
- A GroupDocs Cloud account (if you don't have one, [sign up for a free trial](https://dashboard.groupdocs.cloud/#/apps))
- Client ID and Client Secret credentials
- Basic understanding of REST APIs and your preferred programming language
- Development environment with the respective GroupDocs.Viewer Cloud SDK installed

## Introduction

Efficient storage management is crucial for any document viewing application. GroupDocs.Viewer Cloud provides comprehensive APIs to monitor and manage your cloud storage resources.

In this tutorial, we'll explore a practical scenario: You're developing an enterprise document management system and need to implement features to monitor storage usage, check file existence before operations, and manage document versions.

Let's learn how to implement these storage management operations using the GroupDocs.Viewer Cloud API.

## 1. Check If Storage Exists

First, let's learn how to check if a specific storage exists in your GroupDocs Cloud account, which is useful when working with multiple storage locations.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the storage name you want to check
3. Execute the storage existence request
4. Process the boolean result indicating whether the storage exists

### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/viewer/storage/MyStorage/exist" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's see how to check storage existence using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class CheckStorageExistExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new StorageApi(configuration);

            try
            {
                // Create storage existence request
                var request = new StorageExistsRequest(Common.MyStorage);

                // Execute the request
                var response = apiInstance.StorageExists(request);
                
                // Process the result
                Console.WriteLine($"Storage '{Common.MyStorage}' exists: {response.Exists}");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error checking storage existence: " + e.Message);
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

public class CheckStorageExistExample {

    public static void main(String[] args) {
        // Authenticate with the API
        StorageApi apiInstance = new StorageApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create storage existence request
            StorageExistsRequest request = new StorageExistsRequest(Common.MyStorage);
            
            // Execute the request
            StorageExist response = apiInstance.storageExists(request);
            
            // Process the result
            System.out.println("Storage '" + Common.MyStorage + "' exists: " + response.getExists());
        } catch (ApiException e) {
            System.err.println("Error checking storage existence: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import StorageExistsRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.StorageApi(configuration)

try:
    # Create storage existence request
    request = StorageExistsRequest(Common.my_storage)
    
    # Execute the request
    response = api_instance.storage_exists(request)
    
    # Process the result
    print(f"Storage '{Common.my_storage}' exists: {response.exists}")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error checking storage existence: {e.message}")
```

### Try It Yourself

1. Replace the storage name with your actual storage name
2. Run the code and verify that the check was successful
3. Try checking for a non-existent storage name to see how the API handles it
4. Use this method to verify storage availability before performing operations

## 2. Check If File or Folder Exists

Next, let's learn how to check if a specific file or folder exists in your cloud storage, which is essential before performing operations on them.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the path of the file or folder you want to check
3. Optionally specify the storage name
4. Execute the object existence request
5. Process the boolean result and additional information about the object

### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/viewer/storage/exist/viewerdocs/document.docx?storageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's see how to check if a file or folder exists using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class CheckObjectExistsExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new StorageApi(configuration);

            try
            {
                // Create object existence request
                var request = new ObjectExistsRequest(
                    "viewerdocs/document.docx", // Object path
                    Common.MyStorage           // Storage name
                );

                // Execute the request
                var response = apiInstance.ObjectExists(request);
                
                // Process the result
                Console.WriteLine($"Object exists: {response.Exists}");
                if (response.Exists)
                {
                    Console.WriteLine($"Is folder: {response.IsFolder}");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error checking object existence: " + e.Message);
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

public class CheckObjectExistsExample {

    public static void main(String[] args) {
        // Authenticate with the API
        StorageApi apiInstance = new StorageApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create object existence request
            ObjectExistsRequest request = new ObjectExistsRequest(
                "viewerdocs/document.docx", // Object path
                Common.MyStorage,           // Storage name
                null                        // Version ID (optional)
            );
            
            // Execute the request
            ObjectExist response = apiInstance.objectExists(request);
            
            // Process the result
            System.out.println("Object exists: " + response.getExists());
            if (response.getExists()) {
                System.out.println("Is folder: " + response.getIsFolder());
            }
        } catch (ApiException e) {
            System.err.println("Error checking object existence: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import ObjectExistsRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.StorageApi(configuration)

try:
    # Create object existence request
    request = ObjectExistsRequest(
        "viewerdocs/document.docx",  # Object path
        Common.my_storage            # Storage name
    )
    
    # Execute the request
    response = api_instance.object_exists(request)
    
    # Process the result
    print(f"Object exists: {response.exists}")
    if response.exists:
        print(f"Is folder: {response.is_folder}")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error checking object existence: {e.message}")
```

### Try It Yourself

1. Replace the object path with a file or folder in your storage
2. Run the code and verify the existence check was successful
3. Test with both files and folders to see the different responses
4. Try checking for non-existent objects to see how the API handles it

## 3. Monitor Storage Space Usage

Now, let's learn how to monitor storage space usage, which is essential for managing your storage resources efficiently.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Optionally specify the storage name for which to check usage
3. Execute the disc usage request
4. Process the response containing total and used storage space information

### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/viewer/storage/disc?storageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's see how to monitor storage space usage using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class GetDiscUsageExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new StorageApi(configuration);

            try
            {
                // Create disc usage request
                var request = new GetDiscUsageRequest(Common.MyStorage);

                // Execute the request
                var response = apiInstance.GetDiscUsage(request);
                
                // Process the result
                Console.WriteLine($"Total space: {FormatSize(response.TotalSize)} bytes");
                Console.WriteLine($"Used space: {FormatSize(response.UsedSize)} bytes");
                
                // Calculate percentage used
                var percentUsed = (double)response.UsedSize / response.TotalSize * 100;
                Console.WriteLine($"Usage: {percentUsed:F2}%");
            }
            catch (Exception e)
            {
                Console.WriteLine("Error getting disc usage: " + e.Message);
            }
        }
        
        // Helper method to format size for readability
        private static string FormatSize(long bytes)
        {
            string[] sizes = { "B", "KB", "MB", "GB", "TB" };
            double len = bytes;
            int order = 0;
            while (len >= 1024 && order < sizes.Length - 1) {
                order++;
                len = len / 1024;
            }
            return String.Format("{0:0.##} {1}", len, sizes[order]);
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
import java.text.DecimalFormat;

public class GetDiscUsageExample {

    public static void main(String[] args) {
        // Authenticate with the API
        StorageApi apiInstance = new StorageApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create disc usage request
            GetDiscUsageRequest request = new GetDiscUsageRequest(Common.MyStorage);
            
            // Execute the request
            DiscUsage response = apiInstance.getDiscUsage(request);
            
            // Process the result
            System.out.println("Total space: " + formatSize(response.getTotalSize()) + " bytes");
            System.out.println("Used space: " + formatSize(response.getUsedSize()) + " bytes");
            
            // Calculate percentage used
            double percentUsed = (double) response.getUsedSize() / response.getTotalSize() * 100;
            DecimalFormat df = new DecimalFormat("#.##");
            System.out.println("Usage: " + df.format(percentUsed) + "%");
        } catch (ApiException e) {
            System.err.println("Error getting disc usage: " + e.getMessage());
            e.printStackTrace();
        }
    }
    
    // Helper method to format size for readability
    private static String formatSize(long bytes) {
        String[] sizes = { "B", "KB", "MB", "GB", "TB" };
        double len = bytes;
        int order = 0;
        while (len >= 1024 && order < sizes.length - 1) {
            order++;
            len = len / 1024;
        }
        DecimalFormat df = new DecimalFormat("#.##");
        return df.format(len) + " " + sizes[order];
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import GetDiscUsageRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.StorageApi(configuration)

try:
    # Create disc usage request
    request = GetDiscUsageRequest(Common.my_storage)
    
    # Execute the request
    response = api_instance.get_disc_usage(request)
    
    # Process the result
    print(f"Total space: {format_size(response.total_size)}")
    print(f"Used space: {format_size(response.used_size)}")
    
    # Calculate percentage used
    percent_used = (response.used_size / response.total_size) * 100
    print(f"Usage: {percent_used:.2f}%")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error getting disc usage: {e.message}")

# Helper function to format size for readability
def format_size(bytes):
    sizes = ["B", "KB", "MB", "GB", "TB"]
    i = 0
    while bytes >= 1024 and i < len(sizes) - 1:
        bytes /= 1024
        i += 1
    return f"{bytes:.2f} {sizes[i]}"
```

### Try It Yourself

1. Run the code to check your storage usage
2. Compare the results with your account dashboard
3. Use the formatted size for better readability
4. Implement monitoring logic that alerts when storage usage exceeds a threshold (e.g., 80%)

## 4. Retrieve File Versions Information

Finally, let's learn how to retrieve information about file versions in your cloud storage, which is useful for version control in document management systems.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Specify the file path for which to retrieve versions
3. Optionally specify the storage name
4. Execute the file versions request
5. Process the response containing version information for the file

### cURL Example

```bash
curl -X GET "https://api.groupdocs.cloud/v2.0/viewer/storage/version/viewerdocs/document.docx?storageName=MyStorage" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

### SDK Examples

Let's see how to retrieve file versions using various SDKs:

#### C# Example

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using GroupDocs.Viewer.Cloud.Sdk.Model.Requests;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class GetFileVersionsExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            var apiInstance = new StorageApi(configuration);

            try
            {
                // Create file versions request
                var request = new GetFileVersionsRequest(
                    "viewerdocs/document.docx", // File path
                    Common.MyStorage           // Storage name
                );

                // Execute the request
                var response = apiInstance.GetFileVersions(request);
                
                // Process the result
                Console.WriteLine($"File versions count: {response.Value.Count}");
                
                // Print version details
                foreach (var version in response.Value)
                {
                    Console.WriteLine($"Version: {version.VersionId}");
                    Console.WriteLine($"  Is latest: {version.IsLatest}");
                    Console.WriteLine($"  Name: {version.Name}");
                    Console.WriteLine($"  Modified date: {version.ModifiedDate}");
                    Console.WriteLine($"  Size: {version.Size} bytes");
                    Console.WriteLine($"  Path: {version.Path}");
                    Console.WriteLine();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error getting file versions: " + e.Message);
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

public class GetFileVersionsExample {

    public static void main(String[] args) {
        // Authenticate with the API
        StorageApi apiInstance = new StorageApi(Common.AppSID, Common.AppKey);
        
        try {
            // Create file versions request
            GetFileVersionsRequest request = new GetFileVersionsRequest(
                "viewerdocs/document.docx", // File path
                Common.MyStorage            // Storage name
            );
            
            // Execute the request
            FileVersions response = apiInstance.getFileVersions(request);
            
            // Process the result
            System.out.println("File versions count: " + response.getValue().size());
            
            // Print version details
            for (FileVersion version : response.getValue()) {
                System.out.println("Version: " + version.getVersionId());
                System.out.println("  Is latest: " + version.getIsLatest());
                System.out.println("  Name: " + version.getName());
                System.out.println("  Modified date: " + version.getModifiedDate());
                System.out.println("  Size: " + version.getSize() + " bytes");
                System.out.println("  Path: " + version.getPath());
                System.out.println();
            }
        } catch (ApiException e) {
            System.err.println("Error getting file versions: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Example

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import GetFileVersionsRequest

# Authenticate with the API
configuration = groupdocs_viewer_cloud.Configuration(Common.app_sid, Common.app_key)
api_instance = groupdocs_viewer_cloud.StorageApi(configuration)

try:
    # Create file versions request
    request = GetFileVersionsRequest(
        "viewerdocs/document.docx",  # File path
        Common.my_storage            # Storage name
    )
    
    # Execute the request
    response = api_instance.get_file_versions(request)
    
    # Process the result
    print(f"File versions count: {len(response.value)}")
    
    # Print version details
    for version in response.value:
        print(f"Version: {version.version_id}")
        print(f"  Is latest: {version.is_latest}")
        print(f"  Name: {version.name}")
        print(f"  Modified date: {version.modified_date}")
        print(f"  Size: {version.size} bytes")
        print(f"  Path: {version.path}")
        print()
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error getting file versions: {e.message}")
```

### Try It Yourself

1. Replace the file path with a document in your storage
2. Run the code and verify that the version information is retrieved
3. Try with a file that has multiple versions to see the complete version history
4. Implement version comparison or reversion logic based on the retrieved information

## Common Issues and Troubleshooting

When working with storage operations, you might encounter these common issues:

1. Authentication Errors
   - Problem: API returns 401 Unauthorized
   - Solution: Check your Client ID and Client Secret; make sure your token hasn't expired

2. Storage Not Found
   - Problem: API returns an error when specifying a non-existent storage
   - Solution: Verify the storage name; only use storage names that exist in your account

3. Permission Issues
   - Problem: API returns 403 Forbidden when accessing storage information
   - Solution: Verify your account has the necessary permissions for the storage operations

4. Rate Limiting
   - Problem: Too frequent API calls result in rate limiting
   - Solution: Implement caching for storage information and throttle API requests

## What You've Learned

In this tutorial, you've learned how to:
- Check if a specific storage exists in your GroupDocs Cloud account
- Verify if a file or folder exists in your cloud storage
- Monitor storage space usage to manage resources efficiently
- Retrieve file versions information for version control

You now have the essential skills to implement robust storage management features in your document viewing applications.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a storage dashboard that displays usage statistics and alerts
2. Implement a version control system that tracks document changes
3. Build a storage explorer that checks object existence before operations
4. Write a utility that manages storage resources across multiple storage locations


## Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).