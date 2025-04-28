---
title: Using Aspose.OCR Cloud SDK for Label Recognition Tutorial
weight: 40
url: /recognize-label/sdk-implementation/
description: Learn how to simplify label recognition development with Aspose.OCR Cloud SDK instead of direct REST API calls in this hands-on developer tutorial.
---

# Tutorial: Using Aspose.OCR Cloud SDK for Label Recognition

## Learning Objectives

In this tutorial, you'll learn:
- How to set up and configure the Aspose.OCR Cloud SDK
- How to use the SDK to submit labels for recognition
- How to retrieve and process recognition results via the SDK
- How the SDK simplifies error handling and authentication
- How to integrate label recognition into your application workflow

## Prerequisites

Before starting this tutorial, you should have:
- Completed the previous tutorials on [sending labels](/recognize-label/send-for-recognition/) and [fetching results](/recognize-label/fetch-recognition-result/)
- Your Aspose Cloud Client ID and Client Secret
- Basic knowledge of your preferred programming language (.NET in this tutorial)
- Development environment set up for your chosen platform

## Practical Scenario

You're developing a retail inventory management application that needs to quickly scan and process product labels. Using the SDK will significantly reduce your development time and simplify maintenance compared to direct API calls.

## Tutorial Steps

### 1. Understanding the Benefits of Using the SDK

While you can directly call the Aspose.OCR Cloud REST API as we learned in previous tutorials, using the SDK offers several advantages:

- Simplified Authentication: The SDK handles authentication token management automatically
- Type Safety: Language-specific data structures instead of working with raw JSON
- Error Handling: Better exception handling and error messages
- Reduced Boilerplate: Common operations like encoding/decoding are handled for you

### 2. Setting Up the SDK

Let's start by setting up the Aspose.OCR Cloud SDK for .NET:


#### Create a Basic Project Structure

Create a new console application and add the following namespace imports:

```csharp
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;
using System;
using System.IO;
using System.Text;
```

### 3. Initializing the SDK Client

Now, let's initialize the SDK client with your credentials:

```csharp
// Initialize the Aspose.OCR Cloud SDK client
RecognizeLabelApi recognizeLabelApi = new RecognizeLabelApi("<Your Client ID>", "<Your Client Secret>");
```

Security Tip: In a production environment, don't hardcode your credentials. Use secure configuration management like environment variables, Azure Key Vault, or AWS Secrets Manager to store and retrieve sensitive information.

### 4. Configuring Recognition Settings

Next, we'll configure the recognition settings for our label:

```csharp
// Configure recognition settings
OCRSettingsRecognizeLabel recognitionSettings = new OCRSettingsRecognizeLabel 
{
    Language = Language.English,         // Currently only English is supported
    MakeSkewCorrect = true,              // Automatically correct image tilt
    MakeUpsampling = false,              // Set to true for small text
    ResultType = ResultType.Text         // Get plain text result
};
```

### 5. Sending a Label for Recognition

Now, let's read an image file and send it for recognition:

```csharp
try
{
    // Read image file to byte array
    byte[] imageData = File.ReadAllBytes("path_to_your_label_image.jpg");
    
    // Create recognition request body
    OCRRecognizeLabelBody requestBody = new OCRRecognizeLabelBody(imageData, recognitionSettings);
    
    // Send image for recognition and get task ID
    string taskId = recognizeLabelApi.PostRecognizeLabel(requestBody);
    
    Console.WriteLine($"Label submitted successfully. Task ID: {taskId}");
    
    // Continue to the next step to fetch results
}
catch (Exception ex)
{
    Console.WriteLine($"Error sending label for recognition: {ex.Message}");
}
```

### 6. Fetching and Processing Recognition Results

After submitting the image, we can fetch the recognition results:

```csharp
try
{
    // Wait a moment for the recognition to complete
    Console.WriteLine("Waiting for recognition to complete...");
    System.Threading.Thread.Sleep(2000);  // Simple delay for demonstration
    
    // Fetch recognition result using task ID
    OCRResponse result = recognizeLabelApi.GetRecognizeLabel(taskId);
    
    // Check task status
    if (result.TaskStatus == TaskStatus.Completed)
    {
        // Process completed result
        if (result.Results != null && result.Results.Count > 0)
        {
            // Decode the result - SDK handles Base64 decoding automatically
            string recognizedText = Encoding.UTF8.GetString(result.Results[0].Data);
            Console.WriteLine("Recognition completed successfully!");
            Console.WriteLine("Recognized text:");
            Console.WriteLine(recognizedText);
        }
        else
        {
            Console.WriteLine("Recognition completed but no text was found in the image.");
        }
    }
    else if (result.TaskStatus == TaskStatus.Pending || result.TaskStatus == TaskStatus.Processing)
    {
        Console.WriteLine("Recognition is still in progress. Implement polling to check again.");
    }
    else
    {
        // Handle error or other statuses
        Console.WriteLine($"Recognition failed with status: {result.TaskStatus}");
        if (result.Error != null && result.Error.Messages != null)
        {
            foreach (var message in result.Error.Messages)
            {
                Console.WriteLine($"Error: {message}");
            }
        }
    }
}
catch (Exception ex)
{
    Console.WriteLine($"Error fetching recognition result: {ex.Message}");
}
```

### 7. Implementing a Complete Solution with Polling

For a production-ready implementation, we need to poll the API until the results are available or an error occurs. This is a more robust approach than using a simple delay:

```csharp
public static string RecognizeLabelWithPolling(string imagePath, int maxAttempts = 10, int delayMs = 1500)
{
    try
    {
        // Initialize API client
        RecognizeLabelApi recognizeLabelApi = new RecognizeLabelApi("<Your Client ID>", "<Your Client Secret>");
        
        // Configure recognition settings
        OCRSettingsRecognizeLabel recognitionSettings = new OCRSettingsRecognizeLabel 
        {
            Language = Language.English,
            MakeSkewCorrect = true,
            MakeUpsampling = true,  // Enable upsampling for better accuracy
            ResultType = ResultType.Text
        };
        
        // Read image file
        Console.WriteLine($"Reading image from: {imagePath}");
        byte[] imageData = File.ReadAllBytes(imagePath);
        
        // Create recognition request
        OCRRecognizeLabelBody requestBody = new OCRRecognizeLabelBody(imageData, recognitionSettings);
        
        // Send image for recognition
        Console.WriteLine("Sending label for recognition...");
        string taskId = recognizeLabelApi.PostRecognizeLabel(requestBody);
        Console.WriteLine($"Label submitted. Task ID: {taskId}");
        
        // Poll for results
        Console.WriteLine("Polling for results...");
        int attempts = 0;
        
        while (attempts < maxAttempts)
        {
            attempts++;
            Console.WriteLine($"Attempt {attempts} of {maxAttempts}...");
            
            // Get current status
            OCRResponse result = recognizeLabelApi.GetRecognizeLabel(taskId);
            
            switch (result.TaskStatus)
            {
                case TaskStatus.Completed:
                    // Success! Process the results
                    if (result.Results != null && result.Results.Count > 0)
                    {
                        string recognizedText = Encoding.UTF8.GetString(result.Results[0].Data);
                        Console.WriteLine("Recognition completed successfully!");
                        return recognizedText;
                    }
                    else
                    {
                        return "Recognition completed but no text was found in the image.";
                    }
                
                case TaskStatus.Error:
                    // Handle error
                    string errorMsg = "Recognition failed with error: ";
                    if (result.Error != null && result.Error.Messages != null)
                    {
                        errorMsg += string.Join(", ", result.Error.Messages);
                    }
                    else
                    {
                        errorMsg += "Unknown error";
                    }
                    return errorMsg;
                
                case TaskStatus.NotExist:
                    return "Task ID not found or results expired.";
                
                default:
                    // Still processing, wait and try again
                    Console.WriteLine($"Status: {result.TaskStatus} - Waiting for processing to complete...");
                    System.Threading.Thread.Sleep(delayMs);
                    break;
            }
        }
        
        // If we get here, polling timed out
        return "Recognition timed out after maximum polling attempts.";
    }
    catch (Exception ex)
    {
        return $"Error in recognition process: {ex.Message}";
    }
}
```

### 8. Creating a Complete Application

Let's put everything together in a complete console application:

```csharp
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;
using System;
using System.IO;
using System.Text;

namespace LabelRecognitionApp
{
    class Program
    {
        // Credentials - in real app, store these securely
        private const string ClientId = "<Your Client ID>";
        private const string ClientSecret = "<Your Client Secret>";
        
        static void Main(string[] args)
        {
            Console.WriteLine("Aspose.OCR Cloud Label Recognition Demo");
            Console.WriteLine("======================================");
            
            try
            {
                // Prompt for image path
                Console.Write("Enter path to label image: ");
                string imagePath = Console.ReadLine();
                
                if (!File.Exists(imagePath))
                {
                    Console.WriteLine("Error: File not found!");
                    return;
                }
                
                // Process the image
                string recognizedText = RecognizeLabelWithPolling(imagePath);
                
                // Display results
                Console.WriteLine("\n=== Recognition Result ===");
                Console.WriteLine(recognizedText);
                Console.WriteLine("=========================");
                
                // Example of how you might use the result
                Console.WriteLine("\nWould you like to save the result to a file? (y/n)");
                string response = Console.ReadLine().ToLower();
                
                if (response == "y" || response == "yes")
                {
                    File.WriteAllText("recognition_result.txt", recognizedText);
                    Console.WriteLine("Result saved to recognition_result.txt");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"An error occurred: {ex.Message}");
            }
            
            Console.WriteLine("\nPress any key to exit...");
            Console.ReadKey();
        }
        
        public static string RecognizeLabelWithPolling(string imagePath, int maxAttempts = 10, int delayMs = 1500)
        {
            // Implementation from previous section
            // ...
        }
    }
}
```

## Try It Yourself

Now it's your turn to implement label recognition using the SDK:

1. Create a new project in your preferred development environment
2. Install the Aspose.OCR Cloud SDK package
3. Set up your credentials (get them from the Aspose Cloud Dashboard)
4. Implement the complete recognition flow with polling
5. Test it with different types of label images

## Troubleshooting Common Issues

| Issue | Possible Solution |
|-------|-------------------|
| SDK installation failures | Check your package manager settings and internet connection |
| Authentication errors | Verify your Client ID and Client Secret are correct and active |
| File not found exceptions | Double-check your file paths and ensure the application has read permissions |
| Memory issues with large images | Resize large images before processing or increase your application's memory allocation |
| Timeout during polling | Increase the maximum polling attempts or delay between attempts |

## What You've Learned

In this tutorial, you've learned:
- How to set up and configure the Aspose.OCR Cloud SDK
- How to structure your code to handle the complete label recognition workflow
- How to implement proper polling and error handling
- How to process and utilize recognition results
- How to create a complete console application for label recognition

## Next Steps

Now that you've mastered the basics of using the SDK for label recognition, you might want to:

1. Explore how to optimize recognition accuracy: [Tutorial: Optimizing Label Recognition Accuracy](/recognize-label/optimization-techniques/)

## Further Practice

To reinforce your learning:
1. Convert the console application to a web API that can be called from a mobile app
2. Add support for batch processing multiple images
3. Implement more advanced error handling and logging
4. Create a simple user interface for uploading images and displaying results

## Example Implementation in Other Languages

While this tutorial focused on .NET, the Aspose.OCR Cloud SDK is available for other languages as well. Here's a brief example of how you might implement similar functionality in Python:

```python
# This is a conceptual example - check Aspose documentation for the actual Python SDK API
import aspose_ocr_cloud
import time
import base64

def recognize_label_with_polling(image_path, max_attempts=10, delay_seconds=1.5):
    # Initialize client
    client = aspose_ocr_cloud.RecognizeLabelApi(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
    
    # Read image file
    with open(image_path, 'rb') as image_file:
        image_data = image_file.read()
    
    # Configure settings
    settings = aspose_ocr_cloud.OCRSettingsRecognizeLabel(
        language="English",
        make_skew_correct=True,
        make_upsampling=True,
        result_type="Text"
    )
    
    # Send image for recognition
    request_body = aspose_ocr_cloud.OCRRecognizeLabelBody(image=image_data, settings=settings)
    task_id = client.post_recognize_label(request_body)
    
    # Poll for results
    attempts = 0
    while attempts < max_attempts:
        attempts += 1
        print(f"Checking status: attempt {attempts} of {max_attempts}")
        
        # Get current status
        result = client.get_recognize_label(task_id)
        
        if result.task_status == "Completed":
            # Process completed result
            if result.results and len(result.results) > 0:
                # Decode result
                recognized_text = result.results[0].data.decode('utf-8')
                return recognized_text
            else:
                return "No text found in image"
        elif result.task_status in ["Error", "NotExist"]:
            # Handle error states
            error_msg = "Error occurred: "
            if result.error and result.error.messages:
                error_msg += ", ".join(result.error.messages)
            return error_msg
        else:
            # Still processing
            time.sleep(delay_seconds)
    
    return "Recognition timed out"
```

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance!
