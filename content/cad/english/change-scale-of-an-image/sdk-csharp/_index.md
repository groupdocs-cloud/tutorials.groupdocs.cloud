---
title: SDK Implementation in C# Tutorial
url: /change-scale-of-an-image/sdk-csharp/
weight: 60
description: Learn to implement CAD image scaling operations in C# applications using Aspose.CAD Cloud SDK in this intermediate tutorial
---

# Tutorial: SDK Implementation in C#

## Learning Objectives

In this tutorial, you'll learn:
- How to set up a C# project with Aspose.CAD Cloud SDK
- How to authenticate and make API calls from C# applications
- How to implement complete scaling workflows in C#
- How to handle errors and edge cases in your C# code
- How to create a reusable scaling utility class

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [Proportional vs Non-Proportional Scaling](/change-scale-of-an-image/proportional-scaling/) tutorial
- An Aspose Cloud account with an active subscription or trial
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- Visual Studio 2019+ or another C# development environment
- Basic knowledge of C# programming
- .NET Core 3.1 or .NET 5+ installed

## Introduction

While the previous tutorials showed code snippets for various operations, this tutorial focuses on building a complete, robust C# implementation. We'll create a well-structured application that demonstrates best practices for working with the Aspose.CAD Cloud SDK.

## Step 1: Set Up Your C# Project

Let's start by setting up a new C# console application:

1. Open Visual Studio
2. Create a new Console Application (.NET Core)
3. Name the project "AsposeCadScaling"

## Step 2: Install the Aspose.CAD Cloud SDK

Add the Aspose.CAD Cloud SDK to your project using NuGet Package Manager:

1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Aspose.CAD-Cloud"
4. Install the latest version of the package

Alternatively, you can install via the Package Manager Console:

```
Install-Package Aspose.CAD-Cloud
```

## Step 3: Create Configuration and Authentication

First, let's create a class to handle configuration and authentication:

```csharp
// Tutorial Code Example - Authentication and Configuration in C#
using System;
using Aspose.CAD.Cloud.Sdk.Api;
using Aspose.CAD.Cloud.Sdk.Client;

namespace AsposeCadScaling
{
    public class ApiConnector
    {
        private readonly string _clientId;
        private readonly string _clientSecret;
        private CadApi _cadApi;

        public ApiConnector(string clientId, string clientSecret)
        {
            _clientId = clientId ?? throw new ArgumentNullException(nameof(clientId));
            _clientSecret = clientSecret ?? throw new ArgumentNullException(nameof(clientSecret));
        }

        public CadApi GetCadApi()
        {
            if (_cadApi == null)
            {
                // Initialize API with your credentials
                _cadApi = new CadApi(_clientId, _clientSecret);
            }

            return _cadApi;
        }
    }
}
```

## Step 4: Create a Scaling Utility Class

Now, let's create a utility class to handle different types of scaling operations:

```csharp
// Tutorial Code Example - CAD Scaling Utility in C#
using System;
using System.IO;
using Aspose.CAD.Cloud.Sdk.Api;
using Aspose.CAD.Cloud.Sdk.Model.Requests;

namespace AsposeCadScaling
{
    public class ScalingUtility
    {
        private readonly CadApi _cadApi;

        public ScalingUtility(CadApi cadApi)
        {
            _cadApi = cadApi ?? throw new ArgumentNullException(nameof(cadApi));
        }

        /// <summary>
        /// Resizes a drawing that is already in cloud storage
        /// </summary>
        public void ResizeFromStorage(string fileName, string outputPath, string outputFormat, 
                                      int newWidth, int newHeight)
        {
            try
            {
                Console.WriteLine($"Resizing {fileName} to {newWidth}x{newHeight} as {outputFormat}...");

                // Create request
                var request = new GetImageResizeRequest(fileName, outputFormat, newWidth, newHeight);

                // Make API call
                using (var response = _cadApi.GetImageResize(request))
                {
                    // Save the result to a file
                    using (var output = File.Create(outputPath))
                    {
                        response.CopyTo(output);
                    }
                    
                    Console.WriteLine($"Successfully resized the drawing. Output saved to {outputPath}");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error resizing drawing: {ex.Message}");
                throw;
            }
        }

        /// <summary>
        /// Uploads and resizes a drawing in a single operation
        /// </summary>
        public void ResizeFromLocal(string localFilePath, string outputPath, string outputFormat, 
                                   int newWidth, int newHeight)
        {
            try
            {
                Console.WriteLine($"Uploading and resizing {localFilePath} to {newWidth}x{newHeight} as {outputFormat}...");

                // Read the file data
                byte[] drawingData = File.ReadAllBytes(localFilePath);

                // Create request
                var request = new PostImageResizeRequest(outputFormat, newWidth, newHeight, drawingData);

                // Make API call
                using (var response = _cadApi.PostImageResize(request))
                {
                    // Save the result to a file
                    using (var output = File.Create(outputPath))
                    {
                        response.CopyTo(output);
                    }
                    
                    Console.WriteLine($"Successfully uploaded and resized the drawing. Output saved to {outputPath}");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error uploading and resizing drawing: {ex.Message}");
                throw;
            }
        }

        /// <summary>
        /// Resizes a drawing proportionally based on a target width
        /// </summary>
        public void ResizeProportionally(string fileName, string outputPath, string outputFormat, int targetWidth)
        {
            try
            {
                Console.WriteLine($"Getting properties of {fileName}...");

                // Get drawing properties
                var propertiesRequest = new GetDrawingPropertiesRequest(fileName);
                var properties = _cadApi.GetDrawingProperties(propertiesRequest);

                // Parse the properties
                if (!string.IsNullOrEmpty(properties.Width) && !string.IsNullOrEmpty(properties.Height))
                {
                    float originalWidth = float.Parse(properties.Width);
                    float originalHeight = float.Parse(properties.Height);
                    
                    // Calculate aspect ratio
                    float aspectRatio = originalWidth / originalHeight;
                    
                    // Calculate height to maintain proportions
                    int targetHeight = (int)(targetWidth / aspectRatio);
                    
                    Console.WriteLine($"Original dimensions: {originalWidth}x{originalHeight}");
                    Console.WriteLine($"New dimensions: {targetWidth}x{targetHeight}");
                    
                    // Create request
                    var request = new GetImageResizeRequest(fileName, outputFormat, targetWidth, targetHeight);
                    
                    // Make API call
                    using (var response = _cadApi.GetImageResize(request))
                    {
                        // Save the result to a file
                        using (var output = File.Create(outputPath))
                        {
                            response.CopyTo(output);
                        }
                        
                        Console.WriteLine($"Successfully resized the drawing proportionally. Output saved to {outputPath}");
                    }
                }
                else
                {
                    Console.WriteLine("Error: Could not retrieve drawing dimensions");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error resizing drawing proportionally: {ex.Message}");
                throw;
            }
        }

        /// <summary>
        /// Resizes a drawing to fit within maximum dimensions while maintaining aspect ratio
        /// </summary>
        public void ResizeToFitMaxDimensions(string fileName, string outputPath, string outputFormat, 
                                           int maxWidth, int maxHeight)
        {
            try
            {
                Console.WriteLine($"Getting properties of {fileName}...");

                // Get drawing properties
                var propertiesRequest = new GetDrawingPropertiesRequest(fileName);
                var properties = _cadApi.GetDrawingProperties(propertiesRequest);

                // Parse the properties
                if (!string.IsNullOrEmpty(properties.Width) && !string.IsNullOrEmpty(properties.Height))
                {
                    float originalWidth = float.Parse(properties.Width);
                    float originalHeight = float.Parse(properties.Height);
                    
                    // Calculate scaling factors for both dimensions
                    float widthScale = maxWidth / originalWidth;
                    float heightScale = maxHeight / originalHeight;
                    
                    // Use the smaller scaling factor to ensure the image fits within both constraints
                    float scale = Math.Min(widthScale, heightScale);
                    
                    // Calculate new dimensions
                    int targetWidth = (int)(originalWidth * scale);
                    int targetHeight = (int)(originalHeight * scale);
                    
                    Console.WriteLine($"Original dimensions: {originalWidth}x{originalHeight}");
                    Console.WriteLine($"New dimensions: {targetWidth}x{targetHeight}");
                    
                    // Create request
                    var request = new GetImageResizeRequest(fileName, outputFormat, targetWidth, targetHeight);
                    
                    // Make API call
                    using (var response = _cadApi.GetImageResize(request))
                    {
                        // Save the result to a file
                        using (var output = File.Create(outputPath))
                        {
                            response.CopyTo(output);
                        }
                        
                        Console.WriteLine($"Successfully resized the drawing to fit within {maxWidth}x{maxHeight}. Output saved to {outputPath}");
                    }
                }
                else
                {
                    Console.WriteLine("Error: Could not retrieve drawing dimensions");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error resizing drawing to fit max dimensions: {ex.Message}");
                throw;
            }
        }
    }
}
```

## Step 5: Create the Main Program

Now, let's create the main program to demonstrate the use of our utility classes:

```csharp
// Tutorial Code Example - Main Program in C#
using System;

namespace AsposeCadScaling
{
    class Program
    {
        // Replace these with your own credentials
        private const string ClientId = "YOUR_CLIENT_ID";
        private const string ClientSecret = "YOUR_CLIENT_SECRET";

        static void Main(string[] args)
        {
            try
            {
                Console.WriteLine("Aspose.CAD Cloud Scaling Demo");
                Console.WriteLine("============================");

                // Initialize the API connector
                var connector = new ApiConnector(ClientId, ClientSecret);
                var cadApi = connector.GetCadApi();

                // Create the scaling utility
                var scalingUtility = new ScalingUtility(cadApi);

                // Demo file in cloud storage
                string fileName = "sample.dxf"; // Make sure this file exists in your cloud storage
                
                // Local file for testing upload and resize
                string localFilePath = @"C:\path\to\your\drawing.dxf"; // Update this path

                // Perform various scaling operations
                Console.WriteLine("\n1. Fixed Dimensions Scaling");
                scalingUtility.ResizeFromStorage(fileName, "fixed_dimensions.pdf", "pdf", 800, 600);

                Console.WriteLine("\n2. Proportional Scaling");
                scalingUtility.ResizeProportionally(fileName, "proportional.png", "png", 1024);

                Console.WriteLine("\n3. Max Dimensions Scaling");
                scalingUtility.ResizeToFitMaxDimensions(fileName, "max_dimensions.jpg", "jpg", 1200, 800);

                Console.WriteLine("\n4. Upload and Resize");
                scalingUtility.ResizeFromLocal(localFilePath, "uploaded_and_resized.pdf", "pdf", 800, 600);

                Console.WriteLine("\nAll operations completed successfully.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"An error occurred: {ex.Message}");
            }

            Console.WriteLine("\nPress any key to exit...");
            Console.ReadKey();
        }
    }
}
```

## Step 6: Testing Your Implementation

Before running the application:

1. Replace `"YOUR_CLIENT_ID"` and `"YOUR_CLIENT_SECRET"` with your actual credentials
2. Make sure the `sample.dxf` file exists in your cloud storage
3. Update the `localFilePath` to point to a valid CAD file on your local machine

Then run the application and observe the results. You should see output for each of the scaling operations, and the resulting files should be created in your project directory.

## Advanced Implementation: Error Handling and Retries

In a production environment, you'll want to implement more robust error handling and retry logic. Let's enhance our ApiConnector class:

```csharp
// Tutorial Code Example - Advanced API Connector with Error Handling and Retries
using System;
using System.Net;
using System.Threading;
using System.Threading.Tasks;
using Aspose.CAD.Cloud.Sdk.Api;
using Aspose.CAD.Cloud.Sdk.Client;

namespace AsposeCadScaling
{
    public class AdvancedApiConnector
    {
        private readonly string _clientId;
        private readonly string _clientSecret;
        private CadApi _cadApi;
        private readonly int _maxRetries;
        private readonly int _retryDelayMs;

        public AdvancedApiConnector(string clientId, string clientSecret, int maxRetries = 3, int retryDelayMs = 1000)
        {
            _clientId = clientId ?? throw new ArgumentNullException(nameof(clientId));
            _clientSecret = clientSecret ?? throw new ArgumentNullException(nameof(clientSecret));
            _maxRetries = maxRetries;
            _retryDelayMs = retryDelayMs;
        }

        public CadApi GetCadApi()
        {
            if (_cadApi == null)
            {
                // Initialize API with your credentials
                _cadApi = new CadApi(_clientId, _clientSecret);
                
                // Configure any additional settings
                _cadApi.Configuration.Timeout = 60000; // 60 seconds timeout
            }

            return _cadApi;
        }

        /// <summary>
        /// Executes an API action with retry logic
        /// </summary>
        public async Task<T> ExecuteWithRetryAsync<T>(Func<Task<T>> apiAction)
        {
            int retryCount = 0;
            
            while (true)
            {
                try
                {
                    return await apiAction();
                }
                catch (ApiException ex) when (
                    (ex.ErrorCode == (int)HttpStatusCode.TooManyRequests || // 429
                     ex.ErrorCode == (int)HttpStatusCode.ServiceUnavailable || // 503
                     ex.ErrorCode == (int)HttpStatusCode.GatewayTimeout) && // 504
                    retryCount < _maxRetries)
                {
                    // These errors are potentially transient, so we'll retry
                    retryCount++;
                    int delay = _retryDelayMs * retryCount; // Exponential backoff
                    
                    Console.WriteLine($"API request failed with status {ex.ErrorCode}. Retrying in {delay}ms... (Attempt {retryCount} of {_maxRetries})");
                    
                    await Task.Delay(delay);
                }
                catch (ApiException ex)
                {
                    // Handle specific API errors
                    switch (ex.ErrorCode)
                    {
                        case (int)HttpStatusCode.Unauthorized: // 401
                            throw new UnauthorizedAccessException("Authentication failed. Please check your credentials.", ex);
                            
                        case (int)HttpStatusCode.Forbidden: // 403
                            throw new UnauthorizedAccessException("You don't have permission to access this resource.", ex);
                            
                        case (int)HttpStatusCode.NotFound: // 404
                            throw new FileNotFoundException("The requested resource was not found.", ex.Message, ex);
                            
                        case (int)HttpStatusCode.BadRequest: // 400
                            throw new ArgumentException("The request was invalid. Please check your parameters.", ex);
                            
                        default:
                            throw new Exception($"API error occurred. Status code: {ex.ErrorCode}. Message: {ex.Message}", ex);
                    }
                }
            }
        }
    }
}
```

## Example: Asynchronous Implementation

For better performance, especially when dealing with multiple files, let's create an asynchronous version of our scaling utility:

```csharp
// Tutorial Code Example - Asynchronous CAD Scaling Utility in C#
using System;
using System.IO;
using System.Threading.Tasks;
using Aspose.CAD.Cloud.Sdk.Api;
using Aspose.CAD.Cloud.Sdk.Model.Requests;

namespace AsposeCadScaling
{
    public class AsyncScalingUtility
    {
        private readonly CadApi _cadApi;
        private readonly AdvancedApiConnector _connector;

        public AsyncScalingUtility(CadApi cadApi, AdvancedApiConnector connector)
        {
            _cadApi = cadApi ?? throw new ArgumentNullException(nameof(cadApi));
            _connector = connector ?? throw new ArgumentNullException(nameof(connector));
        }

        /// <summary>
        /// Resizes a drawing that is already in cloud storage asynchronously
        /// </summary>
        public async Task ResizeFromStorageAsync(string fileName, string outputPath, string outputFormat, 
                                               int newWidth, int newHeight)
        {
            Console.WriteLine($"Resizing {fileName} to {newWidth}x{newHeight} as {outputFormat}...");

            // Create request
            var request = new GetImageResizeRequest(fileName, outputFormat, newWidth, newHeight);

            // Make API call with retry logic
            await _connector.ExecuteWithRetryAsync(async () =>
            {
                using (var response = await Task.FromResult(_cadApi.GetImageResize(request)))
                {
                    // Save the result to a file
                    using (var output = File.Create(outputPath))
                    {
                        await response.CopyToAsync(output);
                    }
                    
                    Console.WriteLine($"Successfully resized the drawing. Output saved to {outputPath}");
                    return true;
                }
            });
        }

        /// <summary>
        /// Gets properties of a drawing asynchronously
        /// </summary>
        public async Task<Aspose.CAD.Cloud.Sdk.Model.CadResponse> GetDrawingPropertiesAsync(string fileName)
        {
            var propertiesRequest = new GetDrawingPropertiesRequest(fileName);
            
            return await _connector.ExecuteWithRetryAsync(async () =>
            {
                return await Task.FromResult(_cadApi.GetDrawingProperties(propertiesRequest));
            });
        }

        /// <summary>
        /// Resizes a drawing proportionally based on a target width asynchronously
        /// </summary>
        public async Task ResizeProportionallyAsync(string fileName, string outputPath, string outputFormat, int targetWidth)
        {
            Console.WriteLine($"Getting properties of {fileName}...");

            // Get drawing properties
            var properties = await GetDrawingPropertiesAsync(fileName);

            // Parse the properties
            if (!string.IsNullOrEmpty(properties.Width) && !string.IsNullOrEmpty(properties.Height))
            {
                float originalWidth = float.Parse(properties.Width);
                float originalHeight = float.Parse(properties.Height);
                
                // Calculate aspect ratio
                float aspectRatio = originalWidth / originalHeight;
                
                // Calculate height to maintain proportions
                int targetHeight = (int)(targetWidth / aspectRatio);
                
                Console.WriteLine($"Original dimensions: {originalWidth}x{originalHeight}");
                Console.WriteLine($"New dimensions: {targetWidth}x{targetHeight}");
                
                // Resize with calculated dimensions
                await ResizeFromStorageAsync(fileName, outputPath, outputFormat, targetWidth, targetHeight);
            }
            else
            {
                Console.WriteLine("Error: Could not retrieve drawing dimensions");
            }
        }

        /// <summary>
        /// Processes multiple drawings in parallel
        /// </summary>
        public async Task ProcessMultipleDrawingsAsync(string[] fileNames, string outputFormat, int width, int height)
        {
            Console.WriteLine($"Processing {fileNames.Length} drawings in parallel...");
            
            // Create tasks for each file
            var tasks = new Task[fileNames.Length];
            
            for (int i = 0; i < fileNames.Length; i++)
            {
                string fileName = fileNames[i];
                string outputPath = $"output_{i}.{outputFormat}";
                
                // Start each task
                tasks[i] = ResizeFromStorageAsync(fileName, outputPath, outputFormat, width, height);
            }
            
            // Wait for all tasks to complete
            await Task.WhenAll(tasks);
            
            Console.WriteLine("All drawings processed successfully.");
        }
    }
}
```

## Working with File Upload and Storage

Sometimes you might need to upload files to cloud storage first, or check if files already exist. Let's create a utility class to help with these operations:

```csharp
// Tutorial Code Example - File Storage Utility in C#
using System;
using System.IO;
using System.Threading.Tasks;
using Aspose.CAD.Cloud.Sdk.Api;
using Aspose.CAD.Cloud.Sdk.Model.Requests;

namespace AsposeCadScaling
{
    public class StorageUtility
    {
        private readonly CadApi _cadApi;

        public StorageUtility(CadApi cadApi)
        {
            _cadApi = cadApi ?? throw new ArgumentNullException(nameof(cadApi));
        }

        /// <summary>
        /// Checks if a file exists in cloud storage
        /// </summary>
        public bool FileExists(string fileName, string folder = null)
        {
            var request = new ObjectExistsRequest(fileName, folder);
            var response = _cadApi.ObjectExists(request);
            
            return response.Exists && response.IsFolder == false;
        }

        /// <summary>
        /// Uploads a file to cloud storage
        /// </summary>
        public void UploadFile(string localFilePath, string remoteName, string folder = null)
        {
            if (!File.Exists(localFilePath))
            {
                throw new FileNotFoundException("Local file not found", localFilePath);
            }

            Console.WriteLine($"Uploading {localFilePath} to {remoteName}...");
            
            // Read the file data
            var fileStream = File.OpenRead(localFilePath);
            
            // Create upload request
            var request = new UploadFileRequest(remoteName, fileStream, folder);
            
            // Upload the file
            var response = _cadApi.UploadFile(request);
            
            if (response.Uploaded.Contains(remoteName))
            {
                Console.WriteLine($"File {remoteName} uploaded successfully");
            }
            else
            {
                throw new Exception($"Failed to upload file {remoteName}");
            }
        }

        /// <summary>
        /// Lists all CAD files in a specific folder
        /// </summary>
        public string[] ListCadFiles(string folder = null)
        {
            var request = new GetFilesListRequest(folder);
            var response = _cadApi.GetFilesList(request);
            
            var cadExtensions = new[] { ".dwg", ".dxf", ".dgn", ".dwf", ".dwfx", ".ifc", ".stl", ".igs", ".plt", ".obj" };
            var cadFiles = new System.Collections.Generic.List<string>();
            
            foreach (var file in response.Value)
            {
                string extension = Path.GetExtension(file.Name).ToLowerInvariant();
                if (Array.IndexOf(cadExtensions, extension) >= 0)
                {
                    cadFiles.Add(file.Name);
                }
            }
            
            return cadFiles.ToArray();
        }

        /// <summary>
        /// Deletes a file from cloud storage
        /// </summary>
        public void DeleteFile(string fileName, string folder = null)
        {
            var request = new DeleteFileRequest(fileName, folder);
            _cadApi.DeleteFile(request);
            
            Console.WriteLine($"File {fileName} deleted successfully");
        }
    }
}
```

## Creating a Complete Application

Let's now create a more sophisticated console application that uses all of these utility classes:

```csharp
// Tutorial Code Example - Complete CAD Scaling Application in C#
using System;
using System.Threading.Tasks;

namespace AsposeCadScaling
{
    class AdvancedProgram
    {
        // Replace these with your own credentials
        private const string ClientId = "YOUR_CLIENT_ID";
        private const string ClientSecret = "YOUR_CLIENT_SECRET";

        static async Task Main(string[] args)
        {
            try
            {
                Console.WriteLine("Aspose.CAD Cloud Advanced Scaling Demo");
                Console.WriteLine("=====================================");

                // Initialize the advanced API connector
                var connector = new AdvancedApiConnector(ClientId, ClientSecret);
                var cadApi = connector.GetCadApi();

                // Create utility classes
                var storageUtility = new StorageUtility(cadApi);
                var scalingUtility = new ScalingUtility(cadApi);
                var asyncScalingUtility = new AsyncScalingUtility(cadApi, connector);

                // Show menu and get user choice
                while (true)
                {
                    Console.WriteLine("\nPlease select an option:");
                    Console.WriteLine("1. Upload CAD file to cloud storage");
                    Console.WriteLine("2. List CAD files in cloud storage");
                    Console.WriteLine("3. Resize CAD file with fixed dimensions");
                    Console.WriteLine("4. Resize CAD file proportionally");
                    Console.WriteLine("5. Resize CAD file to fit maximum dimensions");
                    Console.WriteLine("6. Process multiple CAD files in parallel");
                    Console.WriteLine("7. Exit");
                    Console.Write("\nEnter your choice (1-7): ");

                    var choice = Console.ReadLine();

                    switch (choice)
                    {
                        case "1":
                            Console.Write("Enter local file path: ");
                            var localPath = Console.ReadLine();
                            Console.Write("Enter remote file name: ");
                            var remoteName = Console.ReadLine();
                            storageUtility.UploadFile(localPath, remoteName);
                            break;

                        case "2":
                            Console.WriteLine("\nCAD files in cloud storage:");
                            var files = storageUtility.ListCadFiles();
                            foreach (var file in files)
                            {
                                Console.WriteLine($"- {file}");
                            }
                            break;

                        case "3":
                            Console.Write("Enter file name in cloud storage: ");
                            var fileName = Console.ReadLine();
                            Console.Write("Enter output format (pdf, png, jpg, etc.): ");
                            var outputFormat = Console.ReadLine();
                            Console.Write("Enter new width: ");
                            var widthStr = Console.ReadLine();
                            Console.Write("Enter new height: ");
                            var heightStr = Console.ReadLine();

                            if (int.TryParse(widthStr, out int width) && int.TryParse(heightStr, out int height))
                            {
                                await asyncScalingUtility.ResizeFromStorageAsync(
                                    fileName, 
                                    $"output_fixed.{outputFormat}", 
                                    outputFormat, 
                                    width, 
                                    height);
                            }
                            else
                            {
                                Console.WriteLine("Invalid width or height. Please enter numeric values.");
                            }
                            break;

                        case "4":
                            Console.Write("Enter file name in cloud storage: ");
                            fileName = Console.ReadLine();
                            Console.Write("Enter output format (pdf, png, jpg, etc.): ");
                            outputFormat = Console.ReadLine();
                            Console.Write("Enter target width: ");
                            widthStr = Console.ReadLine();

                            if (int.TryParse(widthStr, out width))
                            {
                                await asyncScalingUtility.ResizeProportionallyAsync(
                                    fileName, 
                                    $"output_proportional.{outputFormat}", 
                                    outputFormat, 
                                    width);
                            }
                            else
                            {
                                Console.WriteLine("Invalid width. Please enter a numeric value.");
                            }
                            break;

                        case "5":
                            Console.Write("Enter file name in cloud storage: ");
                            fileName = Console.ReadLine();
                            Console.Write("Enter output format (pdf, png, jpg, etc.): ");
                            outputFormat = Console.ReadLine();
                            Console.Write("Enter maximum width: ");
                            var maxWidthStr = Console.ReadLine();
                            Console.Write("Enter maximum height: ");
                            var maxHeightStr = Console.ReadLine();

                            if (int.TryParse(maxWidthStr, out int maxWidth) && int.TryParse(maxHeightStr, out int maxHeight))
                            {
                                scalingUtility.ResizeToFitMaxDimensions(
                                    fileName, 
                                    $"output_maxdim.{outputFormat}", 
                                    outputFormat, 
                                    maxWidth, 
                                    maxHeight);
                            }
                            else
                            {
                                Console.WriteLine("Invalid width or height. Please enter numeric values.");
                            }
                            break;

                        case "6":
                            Console.WriteLine("\nCAD files in cloud storage:");
                            files = storageUtility.ListCadFiles();
                            for (int i = 0; i < files.Length; i++)
                            {
                                Console.WriteLine($"{i+1}. {files[i]}");
                            }

                            Console.Write("\nEnter output format (pdf, png, jpg, etc.): ");
                            outputFormat = Console.ReadLine();
                            Console.Write("Enter width for all files: ");
                            widthStr = Console.ReadLine();
                            Console.Write("Enter height for all files: ");
                            heightStr = Console.ReadLine();

                            if (int.TryParse(widthStr, out width) && int.TryParse(heightStr, out height))
                            {
                                await asyncScalingUtility.ProcessMultipleDrawingsAsync(
                                    files, 
                                    outputFormat, 
                                    width, 
                                    height);
                            }
                            else
                            {
                                Console.WriteLine("Invalid width or height. Please enter numeric values.");
                            }
                            break;

                        case "7":
                            return;

                        default:
                            Console.WriteLine("Invalid choice. Please try again.");
                            break;
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"An error occurred: {ex.Message}");
                Console.WriteLine(ex.StackTrace);
            }

            Console.WriteLine("\nPress any key to exit...");
            Console.ReadKey();
        }
    }
}
```

## Best Practices for Production Applications

When implementing Aspose.CAD Cloud in production applications, consider these best practices:

1. Configuration Management:
   - Store your API credentials securely (e.g., using user secrets or environment variables)
   - Use configuration files for settings like output paths and default dimensions

2. Error Handling:
   - Implement comprehensive error handling with specific exception types
   - Consider using a logging framework like Serilog or NLog for error tracking
   - Add retry logic for transient errors

3. Performance Optimization:
   - Use asynchronous methods for I/O operations
   - Implement batch processing for multiple files
   - Consider file caching strategies to avoid redundant API calls

4. User Experience:
   - Provide progress feedback for long-running operations
   - Implement cancellation support for time-consuming tasks
   - Add validation for input parameters

5. Security:
   - Validate and sanitize file paths and names
   - Implement role-based access control if multiple users are involved
   - Use HTTPS for all communications

## Troubleshooting Common SDK Issues

Here are solutions to common issues you might encounter:

1. Authentication Failures:
   - Double-check your Client ID and Client Secret
   - Ensure your subscription is active
   - Check if you're reaching API request limits

2. File Not Found Errors:
   - Verify the file exists in cloud storage
   - Check for typos in file names
   - Ensure you're specifying the correct folder path

3. Conversion Failures:
   - Some output formats might not be compatible with certain CAD types
   - Very large files might cause timeouts or memory issues
   - Check if the file is password-protected

4. Performance Issues:
   - Large files can take longer to process
   - Consider increasing timeout settings for API calls
   - Use asynchronous methods for better responsiveness

## Learning Checkpoint

Test your understanding of the concepts covered:

1. What is the benefit of using the async/await pattern when working with the Aspose.CAD Cloud SDK?
2. How would you implement retry logic for transient API errors?
3. What utility would you use to check if a file already exists in cloud storage?
4. How can you process multiple CAD files in parallel for better performance?

## What You've Learned

In this tutorial, you've learned:
- How to set up a C# project with Aspose.CAD Cloud SDK
- How to create reusable utility classes for CAD operations
- How to implement robust error handling and retry logic
- How to build an interactive console application
- How to process multiple CAD files in parallel

## Next Steps

Now that you know how to implement the Aspose.CAD Cloud SDK in C# applications, you're ready to learn more advanced techniques.

Continue to the next tutorial: [Batch Processing Multiple CAD Files](/change-scale-of-an-image/batch-processing/) to learn how to scale multiple CAD drawings efficiently in batch operations.

## Further Practice

To reinforce your understanding:
- Create a Windows Forms or WPF application with a user interface for CAD scaling
- Implement a REST API service that provides CAD conversion functionality
- Build a file syncing utility that monitors a local folder and uploads/processes CAD files automatically
- Create a reporting system that tracks usage of your CAD processing operations

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cad/)
- [Documentation](https://docs.aspose.cloud/cad/)
- [Live Demo](https://products.aspose.app/cad/family)
- [API Reference](https://reference.aspose.cloud/cad/)
- [Blog](https://blog.aspose.cloud/category/cad/)
- [Free Support](https://forum.aspose.cloud/c/cad/28/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Post them on the [Aspose.CAD Cloud Forum](https://forum.aspose.cloud/c/cad/28/) for quick assistance!