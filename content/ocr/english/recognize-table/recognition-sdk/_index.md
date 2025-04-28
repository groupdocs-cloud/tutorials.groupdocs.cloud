---
title: Implementing Table Recognition with Aspose.OCR Cloud SDKs Tutorial
weight: 30
url: /recognize-table/recognition-sdk/
description: Learn how to extract text from tables using Aspose.OCR Cloud SDKs in various programming languages with this intermediate-level tutorial.
---

# Tutorial: Implementing Table Recognition with SDKs

## Learning Objectives

In this tutorial, you'll learn how to:
- Set up and initialize Aspose.OCR Cloud SDKs in your projects
- Use language-specific SDKs to recognize tables
- Submit tables for recognition and fetch results in one workflow
- Handle exceptions and errors in SDK implementations
- Process and utilize the recognized table data

## Prerequisites

Before starting this tutorial, you should have:
- Completed the previous tutorials in this series (or have a basic understanding of the Aspose.OCR Cloud API)
- Client credentials (Client ID and Client Secret) from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
- Development environment set up for your chosen programming language:
  - For .NET: Visual Studio with .NET Core or .NET Framework
  - For Java: JDK and Maven/Gradle
  - For Android: Android Studio
  - For Node.js: Node.js and npm

## Introduction

While you can directly call the Aspose.OCR Cloud REST API to recognize tables, using language-specific SDKs can significantly simplify the process. In this tutorial, we'll explore how to implement table recognition using Aspose.OCR Cloud SDKs for various programming languages.

## Real-World Scenario

Consider a document management system that needs to extract data from various financial reports. By integrating table recognition capabilities using the Aspose.OCR Cloud SDK, the system can automatically process incoming scanned reports, extract structured data from tables, and populate databases without manual intervention.

## Step 1: Install the SDK for Your Language

First, you need to install the appropriate SDK for your programming language:

### .NET (using NuGet Package Manager)
```
Install-Package Aspose.OCR-Cloud
```

### Java (using Maven)
```xml
<dependency>
    <groupId>com.aspose</groupId>
    <artifactId>aspose-ocr-cloud</artifactId>
    <version>latest</version>
</dependency>
```

### Node.js (using npm)
```
npm install aspose_ocr_cloud_5_0_api --save
```

### Android (using Gradle)
```gradle
dependencies {
    implementation 'com.aspose:aspose-ocr-cloud-android:latest'
}
```

## Step 2: Initialize the SDK with Your Credentials

Before using the SDK, you need to initialize it with your Aspose Cloud credentials:

### .NET Example
```csharp
using Aspose.OCR.Cloud.SDK.Api;
using Aspose.OCR.Cloud.SDK.Model;

// Initialize the API with your client credentials
RecognizeTableApi recognizeTableApi = new RecognizeTableApi("<Client Id>", "<Client Secret>");
```

### Java Example
```java
import Aspose.OCR.Cloud.SDK.RecognizeTableApi;
import Aspose.OCR.Cloud.SDK.model.*;

// Initialize the API with your client credentials
RecognizeTableApi api = new RecognizeTableApi("<Client Id>", "<Client Secret>");
```

### Node.js Example
```javascript
const AsposeOcrCloud10040Api = require('aspose_ocr_cloud_5_0_api');
const request = require('aspose_ocr_cloud_5_0_api/node_modules/request');

// Authentication will be done in a later step for Node.js
```

### Android Example
```java
import Aspose.OCR.Cloud.SDK.RecognizeTableApi;
import Aspose.OCR.Cloud.SDK.model.*;

// Initialize the API with your client credentials
RecognizeTableApi api = new RecognizeTableApi("<Client Id>", "<Client Secret>");
```

## Step 3: Prepare Your Table Image

You'll need to read the table image into a byte array:

### .NET Example
```csharp
// Read table image to array of bytes
byte[] tableImage = File.ReadAllBytes("table.png");
```

### Java Example
```java
// Read table image to array of bytes
byte[] tableImage = Files.readAllBytes(Path.of("table.png"));
```

### Node.js Example
```javascript
const fs = require("fs");
const path = require("path");

// Read table image to buffer
var filePath = path.normalize("table.png");
var buffer = Buffer.alloc(1024 * 50);
var fileData = fs.readFileSync(filePath, buffer);
```

### Android Example
```java
// Read table image from assets
String tableFileName = "table.png";
InputStream inputStream = context.getAssets().open(tableFileName);
int size = inputStream.available();
byte[] tableImage = new byte[size];
inputStream.read(tableImage);
inputStream.close();
```

## Step 4: Configure Recognition Settings

Configure the recognition settings according to your needs:

### .NET Example
```csharp
// Specify recognition settings
OCRSettingsRecognizeTable recognitionSettings = new OCRSettingsRecognizeTable {
    Language = Language.English,
    ResultTypeTable = ResultTypeTable.Csv,
    MakeSpellCheck = true,
    MakeContrastCorrection = true
};
```

### Java Example
```java
// Specify recognition settings
OCRSettingsRecognizeTable settings = new OCRSettingsRecognizeTable();
settings.setLanguage(Language.ENGLISH);
settings.setResultTypeTable(ResultTypeTable.CSV);
settings.setMakeSpellCheck(true);
settings.setMakeContrastCorrection(true);
```

### Node.js Example
```javascript
// Specify recognition settings
let settings = new AsposeOcrCloud10040Api.OCRSettingsRecognizeTable();
settings.Language = "English";
settings.ResultTypeTable = "Csv";
settings.MakeSpellCheck = true;
settings.MakeContrastCorrection = true;
```

### Android Example
```java
// Specify recognition settings
OCRSettingsRecognizeTable settings = new OCRSettingsRecognizeTable();
settings.setLanguage(Language.ENGLISH);
settings.setResultTypeTable(ResultTypeTable.CSV);
settings.setMakeSpellCheck(true);
settings.setMakeContrastCorrection(true);
```

## Step 5: Submit the Table for Recognition and Fetch Results

Now, let's put it all together to submit the table for recognition and retrieve the results:

### .NET Example
```csharp
try {
    // Send table for recognition
    OCRRecognizeTableBody source = new OCRRecognizeTableBody(tableImage, recognitionSettings);
    string taskID = recognizeTableApi.PostRecognizeTable(source);
    
    Console.WriteLine($"Table submitted for recognition. Task ID: {taskID}");
    
    // Wait for a moment to ensure processing has started
    System.Threading.Thread.Sleep(2000);
    
    // Poll for results
    OCRResponse result;
    do {
        result = recognizeTableApi.GetRecognizeTable(taskID);
        Console.WriteLine($"Current status: {result.TaskStatus}");
        
        if (result.TaskStatus == "Pending" || result.TaskStatus == "Processing") {
            System.Threading.Thread.Sleep(1000);
        }
    } while (result.TaskStatus == "Pending" || result.TaskStatus == "Processing");
    
    // Check if recognition was successful
    if (result.TaskStatus == "Completed") {
        // Decode and process the result
        string csvData = Encoding.UTF8.GetString(result.Results[0].Data);
        Console.WriteLine("Recognition completed successfully!");
        Console.WriteLine("CSV Data:");
        Console.WriteLine(csvData);
        
        // Now you can process the CSV data as needed
        // For example, you could parse it into a data structure
    } else {
        Console.WriteLine($"Recognition failed with status: {result.TaskStatus}");
        if (result.Error != null && result.Error.Messages != null) {
            foreach (var message in result.Error.Messages) {
                Console.WriteLine($"Error: {message}");
            }
        }
    }
} catch (Exception ex) {
    Console.WriteLine($"An error occurred: {ex.Message}");
}
```

### Java Example
```java
try {
    // Send table for recognition
    OCRRecognizeTableBody requestBody = new OCRRecognizeTableBody();
    requestBody.setTable(tableImage);
    requestBody.setSettings(settings);
    String taskId = api.postRecognizeTable(requestBody);
    
    System.out.println("Table submitted for recognition. Task ID: " + taskId);
    
    // Wait for a moment to ensure processing has started
    Thread.sleep(2000);
    
    // Poll for results
    OCRResponse apiResponse;
    do {
        apiResponse = api.getRecognizeTable(taskId);
        System.out.println("Current status: " + apiResponse.getTaskStatus());
        
        if (apiResponse.getTaskStatus().equals("Pending") || 
            apiResponse.getTaskStatus().equals("Processing")) {
            Thread.sleep(1000);
        }
    } while (apiResponse.getTaskStatus().equals("Pending") || 
             apiResponse.getTaskStatus().equals("Processing"));
    
    // Check if recognition was successful
    if (apiResponse.getTaskStatus().equals("Completed")) {
        // Decode and process the result
        String csvData = new String(apiResponse.getResults().get(0).getData(), StandardCharsets.UTF_8);
        System.out.println("Recognition completed successfully!");
        System.out.println("CSV Data:");
        System.out.println(csvData);
        
        // Now you can process the CSV data as needed
    } else {
        System.out.println("Recognition failed with status: " + apiResponse.getTaskStatus());
        if (apiResponse.getError() != null && apiResponse.getError().getMessages() != null) {
            for (String message : apiResponse.getError().getMessages()) {
                System.out.println("Error: " + message);
            }
        }
    }
} catch (Exception ex) {
    System.out.println("An error occurred: " + ex.getMessage());
}
```

### Node.js Example
```javascript
// Getting access token
function connect() {
    return new Promise(function(resolve, reject) {
        request.post({
            headers: {"ContentType": "application/x-www-form-urlencoded", "Accept": "application/json;charset=UTF-8"},
            url: "https://api.aspose.cloud/connect/token",
            form: JSON.parse('{"client_id": "<Client Id>", "client_secret": "<Client Secret>", "grant_type": "client_credentials"}')
        }, (err, res, body) => {
            if (err) {
                reject(err);
            }
            resolve(body);
        });
    });
}

// Sending table image for recognition
function submitTableForRecognition(accessToken, imageData) {
    return new Promise(function (resolve, reject) {
        api = new AsposeOcrCloud10040Api.RecognizeTableApi();
        api.apiClient.basePath = "https://api.aspose.cloud/v5.0/ocr";
        var body_res = JSON.parse(accessToken);
        api.apiClient.authentications = {
            'JWT': {
                type: 'oauth2',
                accessToken: body_res['access_token']
            }
        }
        api.apiClient.defaultHeaders = {
            "User-Agent": "OpenAPI-Generator/1005.0/Javascript"
        }
        
        // Send table for recognition
        let requestData = new AsposeOcrCloud10040Api.OCRRecognizeTableBody(imageData.toString('base64'), settings);
        api.postRecognizeTable(requestData, (err, res, body) => {
            if (err) {
                reject(err);
            }
            resolve(res);
        });
    });
}

// Fetching recognition result
function fetchRecognitionResult(taskId) {
    return new Promise(function(resolve, reject) {
        api.getRecognizeTable(taskId, (err, res, body) => {
            if (err) {
                reject(err);
            }
            resolve(body);
        })
    })
}

// Function to implement polling
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

// Recognition flow with proper error handling
async function recognizeTable(imagePath) {
    try {
        console.log('Starting table recognition process...');
        
        // Get access token
        const accessToken = await connect();
        console.log('Authentication successful');
        
        // Submit table for recognition
        const taskId = await submitTableForRecognition(accessToken, fileData);
        console.log(`Table submitted for recognition. Task ID: ${taskId}`);
        
        // Wait for a moment before first check
        await sleep(2000);
        
        // Poll for results
        let result;
        let status = "";
        
        do {
            // Fetch current status
            result = await fetchRecognitionResult(taskId);
            const json_res = JSON.parse(result['text']);
            status = json_res['taskStatus'];
            console.log(`Current status: ${status}`);
            
            if (status === "Pending" || status === "Processing") {
                await sleep(1000);
            }
        } while (status === "Pending" || status === "Processing");
        
        // Process final result
        const json_res = JSON.parse(result['text']);
        
        if (status === "Completed") {
            console.log('Recognition completed successfully!');
            
            // Decode the base64 result
            for (const key in json_res['results']) {
                const decodedData = Buffer.from(json_res['results'][key]['data'], 'base64').toString('utf8');
                console.log('Recognized table data:');
                console.log(decodedData);
                
                // You can now process the CSV data as needed
            }
        } else {
            console.log(`Recognition failed with status: ${status}`);
            if (json_res['error'] && json_res['error']['messages']) {
                json_res['error']['messages'].forEach(message => {
                    console.log(`Error: ${message}`);
                });
            }
        }
    } catch (error) {
        console.error(`An error occurred: ${error.message}`);
    }
}

// Execute the recognition
recognizeTable("table.png");
```

### Android Example
```java
try {
    // Send table for recognition
    OCRRecognizeTableBody requestBody = new OCRRecognizeTableBody();
    requestBody.setTable(tableImage);
    requestBody.setSettings(settings);
    String taskId = api.postRecognizeTable(requestBody);
    
    Log.d("OCR", "Table submitted for recognition. Task ID: " + taskId);
    
    // Wait for a moment to ensure processing has started
    Thread.sleep(2000);
    
    // Poll for results
    OCRResponse apiResponse;
    do {
        apiResponse = api.getRecognizeTable(taskId);
        Log.d("OCR", "Current status: " + apiResponse.getTaskStatus());
        
        if (apiResponse.getTaskStatus().equals("Pending") || 
            apiResponse.getTaskStatus().equals("Processing")) {
            Thread.sleep(1000);
        }
    } while (apiResponse.getTaskStatus().equals("Pending") || 
             apiResponse.getTaskStatus().equals("Processing"));
    
    // Check if recognition was successful
    if (apiResponse.getTaskStatus().equals("Completed")) {
        // Decode and process the result
        String csvData = new String(apiResponse.getResults().get(0).getData(), StandardCharsets.UTF_8);
        Log.d("OCR", "Recognition completed successfully!");
        Log.d("OCR", "CSV Data: " + csvData);
        
        // Now you can process the CSV data as needed
        // For example, display it in a TextView
        runOnUiThread(() -> {
            TextView resultView = findViewById(R.id.resultTextView);
            resultView.setText(csvData);
        });
    } else {
        Log.e("OCR", "Recognition failed with status: " + apiResponse.getTaskStatus());
        if (apiResponse.getError() != null && apiResponse.getError().getMessages() != null) {
            for (String message : apiResponse.getError().getMessages()) {
                Log.e("OCR", "Error: " + message);
            }
        }
    }
} catch (Exception ex) {
    Log.e("OCR", "An error occurred: " + ex.getMessage());
}
```
## Learning Checkpoint

Before continuing, make sure you understand:
- How to initialize and configure the SDK for your language
- The process of submitting a table for recognition
- How to implement polling to check for completion
- The proper way to handle and decode the recognition results

## Troubleshooting Common Issues

### Authentication Failures
- Double-check your client credentials (Client ID and Client Secret)
- Ensure your account has an active subscription or free trial
- For token-based authentication (Node.js), check if the token has expired

### Recognition Errors
- Verify that the image format is supported (JPEG, PNG, BMP, TIFF)
- Ensure the table in the image is clearly visible with good contrast
- Try enabling image preprocessing options like contrast correction

### SDK-Specific Issues
- .NET: Make sure you have the correct NuGet package installed
- Java: Check Maven dependencies for version conflicts
- Node.js: Ensure all required modules are installed
- Android: Add internet permission in your AndroidManifest.xml

## What You've Learned

In this tutorial, you've learned how to:
- Set up and configure Aspose.OCR Cloud SDKs in various programming languages
- Submit tables for recognition using language-specific method calls
- Implement a polling mechanism to check for task completion
- Process and utilize the recognition results in your applications

## Further Practice

To reinforce what you've learned:
1. Implement table recognition in your preferred programming language
2. Create a simple application that allows users to upload and recognize tables
3. Experiment with different recognition settings to optimize for your use case
4. Try processing multiple tables in a batch

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/ocr/12/).
    