---
title: Learn to Update Calendar Exceptions Tutorial
description: This step-by-step tutorial teaches developers how to modify existing calendar exceptions in project files using the Aspose.Tasks Cloud REST API with multiple programming languages.
url: /working-with-calendar-exceptions/update-calendar-exception/
weight: 30
---

## Introduction

In this tutorial, you'll learn how to update existing calendar exceptions in project files using the Aspose.Tasks Cloud REST API. Being able to modify calendar exceptions is essential for maintaining accurate project schedules when circumstances change, such as rescheduling holidays or adjusting working hours.

### Learning Objectives

By the end of this tutorial, you will be able to:
- Identify existing calendar exceptions for modification
- Construct API requests to update calendar exceptions
- Modify various properties of calendar exceptions
- Implement this functionality in multiple programming languages

### Prerequisites

Before you begin, ensure you have:
- An Aspose Cloud account with an active subscription or free trial
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A project file (MPP, MPT, or XML format) with at least one calendar containing exceptions
- Knowledge of the calendar UID and exception index you want to update
- Basic understanding of REST API concepts
- Familiarity with your chosen programming language

## Understanding Calendar Exception Updates

When updating a calendar exception, you need to know:

1. The project file name where the calendar resides
2. The calendar UID that contains the exception
3. The index of the exception you want to update
4. The properties you want to change

Unlike adding exceptions, updates require you to specify the exact exception using its index.

## Tutorial Steps

### Step 1: Find the Exception to Update

Before updating, you need to identify which exception to modify. You can use the [Get Calendar Exceptions](/working-with-calendar-exceptions/get-calendar-exceptions/) API to retrieve all exceptions and find the one you need.

#### Try It Yourself:

1. Retrieve all calendar exceptions for your calendar
2. Note the index of the exception you want to update
3. Identify which properties you want to modify

### Step 2: Prepare Your API Request

To update a calendar exception, you'll use the PUT method on the following endpoint:

```
PUT https://api.aspose.cloud/v3.0/tasks/{name}/calendars/{calendarUid}/calendarExceptions/{index}
```

Where:
- `{name}` is your project file name (with extension)
- `{calendarUid}` is the unique identifier of the calendar
- `{index}` is the position of the exception in the collection

### Step 3: Create the Request Payload

For updating a calendar exception, you need to provide a JSON payload with the updated exception details. You must include all properties, even those you're not changing:

```json
{
  "Index": 1,
  "EnteredByOccurrences": true,
  "FromDate": "2023-05-29T00:00:00.000Z",
  "ToDate": "2023-05-30T23:59:59.000Z",  // Extended to two days
  "Occurrences": 0,
  "Name": "Memorial Day Weekend",  // Updated name
  "Type": "Daily",
  "Period": 1,
  "DaysOfWeek": [],
  "MonthItem": "Undefined",
  "MonthPosition": "Undefined",
  "Month": "Undefined",
  "MonthDay": 0,
  "DayWorking": false
}
```

In this example, we're updating an existing exception by:
1. Changing its name from "Memorial Day" to "Memorial Day Weekend"
2. Extending its duration to include an additional day

### Step 4: Make the API Request

Let's see how to make this request using cURL:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/calendars/1/calendarExceptions/1" \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d '{ "Index":1, "EnteredByOccurrences":true, "FromDate":"2023-05-29T00:00:00.000Z", "ToDate":"2023-05-30T23:59:59.000Z", "Occurrences":0, "Name":"Memorial Day Weekend", "Type":"Daily", "Period":1, "DaysOfWeek":[], "MonthItem":"Undefined", "MonthPosition":"Undefined", "Month":"Undefined", "MonthDay":0, "DayWorking":false }'
```

Replace `YOUR_ACCESS_TOKEN` with the token obtained through authentication.

### Step 5: Process the Response

When successful, the API returns a JSON response with a success status:

```json
{
  "Code": 200,
  "Status": "OK"
}
```

### Step 6: Implement in Your Application

Now let's implement this functionality using different programming languages.

#### C# Implementation

```csharp
// Tutorial Code Example - Updating Calendar Exceptions in C#
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json;

namespace AsposeTasksCloudTutorial
{
    class Program
    {
        // Get your clientId and clientSecret from https://dashboard.aspose.cloud/
        static string clientId = "YOUR_CLIENT_ID";
        static string clientSecret = "YOUR_CLIENT_SECRET";
        static string baseUrl = "https://api.aspose.cloud/v3.0/tasks/";
        
        static async Task Main(string[] args)
        {
            try
            {
                // Step 1: Authenticate and get access token
                string accessToken = await GetAccessToken();
                
                // Step 2: Define project file name, calendar UID, and exception index
                string fileName = "Home move plan.mpp";
                int calendarUid = 1;
                int exceptionIndex = 1;  // The index of the exception to update
                
                // Step 3: Create updated exception payload
                var exceptionPayload = new
                {
                    Index = exceptionIndex,
                    EnteredByOccurrences = true,
                    FromDate = new DateTime(2023, 5, 29),
                    ToDate = new DateTime(2023, 5, 30, 23, 59, 59),
                    Occurrences = 0,
                    Name = "Memorial Day Weekend",
                    Type = "Daily",
                    Period = 1,
                    DaysOfWeek = new int[] { },
                    MonthItem = "Undefined",
                    MonthPosition = "Undefined",
                    Month = "Undefined",
                    MonthDay = 0,
                    DayWorking = false
                };
                
                // Step 4: Update calendar exception
                await UpdateCalendarException(accessToken, fileName, calendarUid, exceptionIndex, exceptionPayload);
                
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }
            catch (Exception ex)
            {
                Console.WriteLine($"An error occurred: {ex.Message}");
                Console.ReadKey();
            }
        }
        
        static async Task<string> GetAccessToken()
        {
            using (var client = new HttpClient())
            {
                // Preparing request
                var content = new FormUrlEncodedContent(new[]
                {
                    new KeyValuePair<string, string>("grant_type", "client_credentials"),
                    new KeyValuePair<string, string>("client_id", clientId),
                    new KeyValuePair<string, string>("client_secret", clientSecret)
                });
                
                // Making the request
                var response = await client.PostAsync("https://api.aspose.cloud/connect/token", content);
                var jsonString = await response.Content.ReadAsStringAsync();
                var tokenResponse = JsonConvert.DeserializeObject<TokenResponse>(jsonString);
                
                Console.WriteLine("Access token obtained successfully!");
                return tokenResponse.AccessToken;
            }
        }
        
        static async Task UpdateCalendarException(string accessToken, string fileName, int calendarUid, int exceptionIndex, object exceptionPayload)
        {
            using (var client = new HttpClient())
            {
                // Set authorization header
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
                
                // Prepare the request URL
                string requestUrl = $"{baseUrl}{Uri.EscapeDataString(fileName)}/calendars/{calendarUid}/calendarExceptions/{exceptionIndex}";
                
                Console.WriteLine($"Updating calendar exception at: {requestUrl}");
                
                // Serialize the payload
                var jsonPayload = JsonConvert.SerializeObject(exceptionPayload);
                var content = new StringContent(jsonPayload, Encoding.UTF8, "application/json");
                
                // Make the request
                var response = await client.PutAsync(requestUrl, content);
                
                if (response.IsSuccessStatusCode)
                {
                    var jsonString = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("Calendar exception updated successfully!");
                    Console.WriteLine(jsonString);
                }
                else
                {
                    Console.WriteLine($"Error: {response.StatusCode}");
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }
        }
    }
    
    class TokenResponse
    {
        [JsonProperty("access_token")]
        public string AccessToken { get; set; }
    }
}
```

#### Python Implementation

```python
# Tutorial Code Example - Updating Calendar Exceptions in Python
import requests
import json
from datetime import datetime

# Get your clientId and clientSecret from https://dashboard.aspose.cloud/
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"
base_url = "https://api.aspose.cloud/v3.0/tasks/"

def get_access_token():
    """
    Authenticate with Aspose Cloud and get access token
    """
    auth_url = "https://api.aspose.cloud/connect/token"
    
    # Prepare the form data
    auth_data = {
        "grant_type": "client_credentials",
        "client_id": client_id,
        "client_secret": client_secret
    }
    
    # Make the request
    response = requests.post(auth_url, data=auth_data)
    
    if response.status_code == 200:
        token_data = response.json()
        return token_data["access_token"]
    else:
        raise Exception(f"Authentication failed: {response.text}")

def get_calendar_exceptions(access_token, file_name, calendar_uid):
    """
    Get all calendar exceptions to find the one to update
    """
    # Prepare the request URL
    request_url = f"{base_url}{file_name}/calendars/{calendar_uid}/calendarExceptions"
    
    # Prepare headers
    headers = {
        "Authorization": f"Bearer {access_token}",
        "Accept": "application/json"
    }
    
    # Make the request
    response = requests.get(request_url, headers=headers)
    
    if response.status_code == 200:
        return response.json()
    else:
        print(f"Error: {response.status_code}")
        print(response.text)
        return None

def update_calendar_exception(access_token, file_name, calendar_uid, exception_index, exception_data):
    """
    Update a calendar exception
    """
    # Prepare the request URL
    request_url = f"{base_url}{file_name}/calendars/{calendar_uid}/calendarExceptions/{exception_index}"
    
    # Prepare headers
    headers = {
        "Authorization": f"Bearer {access_token}",
        "Content-Type": "application/json",
        "Accept": "application/json"
    }
    
    print(f"Updating calendar exception at: {request_url}")
    
    # Make the request
    response = requests.put(request_url, headers=headers, json=exception_data)
    
    if response.status_code == 200:
        print("Calendar exception updated successfully!")
        return response.json()
    else:
        print(f"Error: {response.status_code}")
        print(response.text)
        return None

def main():
    try:
        # Step 1: Get access token
        print("Authenticating...")
        access_token = get_access_token()
        print("Authentication successful!")
        
        # Step 2: Define file name and calendar UID
        file_name = "Home move plan.mpp"
        calendar_uid = 1
        exception_index = 1  # The index of the exception to update
        
        # Optional: Get current exceptions to see what we're updating
        exceptions_data = get_calendar_exceptions(access_token, file_name, calendar_uid)
        if exceptions_data and "calendarExceptions" in exceptions_data:
            exceptions = exceptions_data["calendarExceptions"]
            if len(exceptions) > exception_index:
                print(f"\nCurrent exception {exception_index}:")
                print(json.dumps(exceptions[exception_index], indent=2))
        
        # Step 3: Create updated exception data
        exception_data = {
            "Index": exception_index,
            "EnteredByOccurrences": True,
            "FromDate": "2023-05-29T00:00:00.000Z",
            "ToDate": "2023-05-30T23:59:59.000Z",
            "Occurrences": 0,
            "Name": "Memorial Day Weekend",
            "Type": "Daily",
            "Period": 1,
            "DaysOfWeek": [],
            "MonthItem": "Undefined",
            "MonthPosition": "Undefined",
            "Month": "Undefined",
            "MonthDay": 0,
            "DayWorking": False
        }
        
        # Step 4: Update the calendar exception
        result = update_calendar_exception(
            access_token, file_name, calendar_uid, exception_index, exception_data
        )
        
        if result:
            print("\nAPI Response:")
            print(json.dumps(result, indent=2))
            
    except Exception as e:
        print(f"An error occurred: {str(e)}")

if __name__ == "__main__":
    main()
```

### Common Update Scenarios

Here are some common scenarios for updating calendar exceptions:

#### 1. Changing Working Hours for an Exception

```json
{
  "Index": 1,
  "EnteredByOccurrences": true,
  "FromDate": "2023-06-03T00:00:00.000Z",
  "ToDate": "2023-06-03T23:59:59.000Z",
  "Occurrences": 0,
  "Name": "Special Saturday",
  "Type": "Daily",
  "Period": 0,
  "DaysOfWeek": [],
  "MonthDay": 0,
  "DayWorking": true,
  "WorkingTimes": [
    {
      "FromTime": "2023-06-03T08:00:00.000Z",  // Changed from 9:00 to 8:00
      "ToTime": "2023-06-03T14:00:00.000Z"     // Changed from 13:00 to 14:00
    }
  ]
}
```

#### 2. Changing a Working Day to a Non-Working Day

```json
{
  "Index": 1,
  "EnteredByOccurrences": true,
  "FromDate": "2023-06-03T00:00:00.000Z",
  "ToDate": "2023-06-03T23:59:59.000Z",
  "Occurrences": 0,
  "Name": "Canceled Saturday Work",  // Updated name
  "Type": "Daily",
  "Period": 0,
  "DaysOfWeek": [],
  "MonthDay": 0,
  "DayWorking": false,  // Changed from true to false
  "WorkingTimes": []    // Removed working times
}
```

#### 3. Adjusting Recurrence Pattern

```json
{
  "Index": 1,
  "EnteredByOccurrences": true,
  "FromDate": "2023-06-02T00:00:00.000Z",
  "ToDate": "2023-06-02T23:59:59.000Z",
  "Occurrences": 10,
  "Name": "Bi-Weekly Team Meeting",  // Updated name
  "Type": "Weekly",
  "Period": 2,  // Changed from weekly to bi-weekly
  "DaysOfWeek": [5],
  "MonthDay": 0,
  "DayWorking": true,
  "WorkingTimes": [
    {
      "FromTime": "2023-06-02T14:00:00.000Z",
      "ToTime": "2023-06-02T15:30:00.000Z"
    }
  ]
}
```

### Common Issues and Troubleshooting

1. Exception Not Found:
   - Verify that the exception index is correct
   - Confirm that the exception still exists in the calendar

2. Missing Properties:
   - When updating, include all required properties of the calendar exception
   - Some properties are interdependent (e.g., if DayWorking is false, WorkingTimes should be empty)

3. Date Validation Issues:
   - Ensure FromDate is not after ToDate
   - For recurrence patterns, make sure dates and properties are consistent

4. Inconsistent Type and Properties:
   - Different exception types require different properties to be set
   - For example, Weekly type should have DaysOfWeek defined

## What You've Learned

In this tutorial, you've learned:
- How to identify existing calendar exceptions for modification
- How to construct a request to update calendar exceptions
- How to modify various properties including dates, working times, and recurrence patterns
- Implementation approaches in different programming languages

## Next Steps

Now that you know how to update calendar exceptions, you might want to learn how to remove them. Continue your learning journey with our next tutorial:

→ [Tutorial: How to Delete Calendar Exceptions](/working-with-calendar-exceptions/delete-calendar-exception/)

## Further Practice

To reinforce your learning:
1. Create a script that fetches exceptions, modifies them, and updates them in one operation
2. Build a function that changes all non-working days to working days for emergency project scenarios
3. Implement batch updates for multiple exceptions 

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [forum](https://forum.aspose.cloud/c/tasks/16/) for assistance.
