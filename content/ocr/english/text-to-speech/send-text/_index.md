---
title: How to Send Text for Speech Conversion with Aspose.OCR Cloud Tutorial
weight: 20
url: /text-to-speech/send-text/
description: Learn step-by-step how to submit text to the Aspose.OCR Cloud API for conversion to natural-sounding speech in this practical tutorial.
---

# Tutorial: How to Send Text for Speech Conversion

## Learning Objectives

In this tutorial, you'll learn:
- How to authenticate with the Aspose.OCR Cloud API
- How to prepare a proper text-to-speech request
- How to submit text for speech conversion
- How to understand and handle the API response
- Implementation approaches in multiple programming languages

## Prerequisites

Before starting this tutorial:
- Basic programming knowledge in at least one language (cURL, Python, Java, or C#)
- A free Aspose Cloud account ([sign up here](https://dashboard.aspose.cloud/#/apps))
- Client ID and Client Secret from your Aspose Cloud dashboard
- Completion of the [Text to Speech Basics tutorial](/text-to-speech/basics/) is recommended

## Practical Scenario

Imagine you're developing a news reading application that needs to convert articles into audio for users who prefer listening over reading. You'll need to send the article text to the Aspose.OCR Cloud API for conversion to speech.

## Step 1: Set Up Authentication

Before sending any text for conversion, you need to authenticate with the Aspose.OCR Cloud API by obtaining an access token.

### Using cURL

```bash
curl -X POST "https://api.aspose.cloud/connect/token" \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded"
```

The response will contain your access token:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 3600,
  "token_type": "bearer"
}
```

Try it yourself: Replace YOUR_CLIENT_ID and YOUR_CLIENT_SECRET with your actual credentials and run the command to get your access token.

### Using Programming Languages

#### Python Example

```python
import requests

# Authentication credentials
client_id = 'YOUR_CLIENT_ID'
client_secret = 'YOUR_CLIENT_SECRET'

# Get authorization token
auth_data = {
    'grant_type': 'client_credentials',
    'client_id': client_id,
    'client_secret': client_secret
}

auth_response = requests.post(
    'https://api.aspose.cloud/connect/token',
    data=auth_data,
    headers={'Content-Type': 'application/x-www-form-urlencoded'}
)

# Extract token from response
token = auth_response.json()['access_token']
print(f"Access token: {token[:10]}...") # Print first 10 chars for verification
```
#### Java Example

```java
import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;

public class AsposeTTSAuth {
    public static void main(String[] args) {
        try {
            // Authentication credentials
            String clientId = "YOUR_CLIENT_ID";
            String clientSecret = "YOUR_CLIENT_SECRET";
            
            // Prepare request data
            String authData = "grant_type=client_credentials&client_id=" + clientId + 
                              "&client_secret=" + clientSecret;
            byte[] postData = authData.getBytes(StandardCharsets.UTF_8);
            
            // Set up connection
            URL url = new URL("https://api.aspose.cloud/connect/token");
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("POST");
            conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
            conn.setRequestProperty("Content-Length", Integer.toString(postData.length));
            conn.setDoOutput(true);
            
            // Send request
            try (DataOutputStream wr = new DataOutputStream(conn.getOutputStream())) {
                wr.write(postData);
            }
            
            // Read response
            BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            StringBuilder response = new StringBuilder();
            String line;
            while ((line = reader.readLine()) != null) {
                response.append(line);
            }
            reader.close();
            
            System.out.println("Response: " + response.toString());
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
#### C# Example

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using System.Text.Json;

class AsposeTTSAuth
{
    static async Task Main()
    {
        // Authentication credentials
        string clientId = "YOUR_CLIENT_ID";
        string clientSecret = "YOUR_CLIENT_SECRET";
        
        // Create HTTP client
        using var client = new HttpClient();
        
        // Prepare request content
        var content = new FormUrlEncodedContent(new[]
        {
            new KeyValuePair<string, string>("grant_type", "client_credentials"),
            new KeyValuePair<string, string>("client_id", clientId),
            new KeyValuePair<string, string>("client_secret", clientSecret)
        });
        
        // Send request
        var response = await client.PostAsync("https://api.aspose.cloud/connect/token", content);
        
        // Process response
        string responseBody = await response.Content.ReadAsStringAsync();
        Console.WriteLine($"Response: {responseBody}");
        
        // Extract token (simple approach)
        var responseJson = JsonDocument.Parse(responseBody);
        string token = responseJson.RootElement.GetProperty("access_token").GetString();
        Console.WriteLine($"Token: {token.Substring(0, 10)}...");
    }
}
```


## Step 2: Prepare the Text-to-Speech Request

Now that you have your access token, you need to prepare your text-to-speech request in JSON format. The request must include:

1. The text to be converted to speech
2. Conversion settings (language and output format)

```json
{
    "text": "Read this text aloud",
    "settings": {
        "language": "English",
        "resultType": "Wav"
    }
}
```

Note: Currently, Aspose.OCR Cloud API only supports English language and WAV audio format.

## Step 3: Send the Text for Conversion

Next, send a POST request to the Aspose.OCR Cloud API with your text and settings.

### Using cURL

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/converttexttospeech' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
--data '{
    "text": "Read this text aloud",
    "settings": {
        "language": "English",
        "resultType": "Wav"
    }
}'
```

Try it yourself: Replace YOUR_ACCESS_TOKEN with the token you obtained in Step 1 and run the command to submit text for conversion.

### Using Programming Languages

#### Python Example

```python
import requests
import json

# Use the token from previous step
token = "YOUR_ACCESS_TOKEN"

# Text to convert
tts_data = {
    "text": "Read this text aloud",
    "settings": {
        "language": "English",
        "resultType": "Wav"
    }
}

# Send text for conversion
tts_response = requests.post(
    'https://api.aspose.cloud/v5.0/ocr/converttexttospeech',
    json=tts_data,
    headers={
        'Accept': 'text/plain',
        'Content-Type': 'application/json',
        'Authorization': f'Bearer {token}'
    }
)

# Get task ID
if tts_response.status_code == 200:
    task_id = tts_response.text
    print(f"Text submitted successfully! Task ID: {task_id}")
else:
    print(f"Error: {tts_response.status_code}, {tts_response.text}")
```

#### Java Example

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;

public class AsposeTTSSendText {
    public static void main(String[] args) {
        try {
            // Use token from previous step
            String token = "YOUR_ACCESS_TOKEN";
            
            // Create JSON request
            String jsonRequest = "{"
                + "\"text\": \"Read this text aloud\","
                + "\"settings\": {"
                + "    \"language\": \"English\","
                + "    \"resultType\": \"Wav\""
                + "  }"
                + "}";
            
            // Set up connection
            URL url = new URL("https://api.aspose.cloud/v5.0/ocr/converttexttospeech");
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("POST");
            conn.setRequestProperty("Accept", "text/plain");
            conn.setRequestProperty("Content-Type", "application/json");
            conn.setRequestProperty("Authorization", "Bearer " + token);
            conn.setDoOutput(true);
            
            // Send request
            try (OutputStream os = conn.getOutputStream()) {
                byte[] input = jsonRequest.getBytes(StandardCharsets.UTF_8);
                os.write(input, 0, input.length);
            }
            
            // Read response
            int responseCode = conn.getResponseCode();
            if (responseCode == HttpURLConnection.HTTP_OK) {
                BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                String taskId = in.readLine();
                in.close();
                System.out.println("Text submitted successfully! Task ID: " + taskId);
            } else {
                System.out.println("Error: " + responseCode);
                BufferedReader errorReader = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
                String errorLine;
                StringBuilder errorResponse = new StringBuilder();
                while ((errorLine = errorReader.readLine()) != null) {
                    errorResponse.append(errorLine);
                }
                errorReader.close();
                System.out.println("Error details: " + errorResponse.toString());
            }
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### C# Example

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using System.Text.Json;

class AsposeTTSSendText
{
    static async Task Main()
    {
        // Use token from previous step
        string token = "YOUR_ACCESS_TOKEN";
        
        // Prepare request content
        var requestData = new
        {
            text = "Read this text aloud",
            settings = new
            {
                language = "English",
                resultType = "Wav"
            }
        };
        
        string jsonRequest = JsonSerializer.Serialize(requestData);
        
        // Create HTTP client
        using var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Authorization", $"Bearer {token}");
        
        // Send request
        var content = new StringContent(jsonRequest, Encoding.UTF8, "application/json");
        var response = await client.PostAsync("https://api.aspose.cloud/v5.0/ocr/converttexttospeech", content);
        
        // Process response
        if (response.IsSuccessStatusCode)
        {
            string taskId = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"Text submitted successfully! Task ID: {taskId}");
        }
        else
        {
            string errorDetails = await response.Content.ReadAsStringAsync();
            Console.WriteLine($"Error: {(int)response.StatusCode}, {errorDetails}");
        }
    }
}
```

## Step 4: Understanding the Response

If your request is successful, the API returns a task ID (GUID) as a string:

```
TTS01232-9d40-4f2d-8eda-f1fc0b12bce6
```

This ID is crucial - you'll need it to retrieve the audio file later. The task ID is essentially your receipt for the text-to-speech conversion request.

## Step 5: Using Evaluation Mode (Optional)

If you're just trying out the API or don't have a subscription yet, you can use the evaluation mode:

```bash
curl --request POST --location 'https://api.aspose.cloud/v5.0/ocr/ConvertTextToSpeechTrial' \
--header 'Accept: text/plain' \
--header 'Content-Type: application/json' \
--data '{
    "text": "Read this text aloud",
    "settings": {
        "language": "English",
        "resultType": "Wav"
    }
}'
```

> Note: When using the trial endpoint, no authorization header is required.

## Troubleshooting Tips

1. Authentication errors: Double-check your Client ID and Client Secret
2. 400 Bad Request: Verify the format of your JSON payload
3. Empty response: Ensure your request headers are correct
4. Task ID not received: Check your internet connection and API endpoint URL

## What You've Learned

In this tutorial, you've learned:
- How to authenticate with the Aspose.OCR Cloud API
- How to structure a text-to-speech conversion request
- How to send text for speech conversion
- How to retrieve and use the task ID for future reference
- How to use the evaluation mode for testing

## Further Practice

To reinforce your learning:
1. Try submitting texts of different lengths to see how the API handles them
2. Experiment with the evaluation mode if you haven't activated a subscription
3. Create a simple script that submits multiple texts in sequence

## Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to [reach out on our forum](https://forum.aspose.cloud/c/ocr/12/) for assistance!
