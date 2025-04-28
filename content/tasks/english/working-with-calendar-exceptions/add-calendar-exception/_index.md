---
title: How to Add Calendar Exceptions Tutorial
description: Learn step-by-step how to add custom calendar exceptions to project files using Aspose.Tasks Cloud API in this hands-on developer tutorial.
url: /working-with-calendar-exceptions/add-calendar-exception/
weight: 20
---

## Introduction

In this tutorial, you'll learn how to add custom calendar exceptions to project files using the Aspose.Tasks Cloud REST API. Calendar exceptions are crucial for defining non-standard working days and times in your project calendars, such as holidays, special events, or modified working hours.

### Learning Objectives

By the end of this tutorial, you will be able to:
- Understand the structure and properties of calendar exceptions
- Create API requests to add new exceptions to project calendars
- Configure different types of calendar exceptions for various scenarios
- Implement this functionality in multiple programming languages

### Prerequisites

Before you begin, ensure you have:
- An Aspose Cloud account with an active subscription or free trial
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A project file (MPP, MPT, or XML format) with at least one calendar
- The calendar UID where you want to add exceptions
- Basic knowledge of REST API concepts
- Familiarity with your chosen programming language

## Understanding Calendar Exception Properties

Before adding a calendar exception, it's important to understand the key properties:

- Index: The position in the exceptions collection
- EnteredByOccurrences: Whether the exception is defined by occurrences or date range
- FromDate/ToDate: The date range for the exception
- Occurrences: Number of occurrences if applicable
- Name: A descriptive name for the exception
- Type: The recurrence type (Daily, Weekly, Monthly, etc.)
- Period: The recurrence interval
- DaysOfWeek: Specific days of the week for the exception
- DayWorking: Whether work is performed during the exception time
- WorkingTimes: The working hour ranges if DayWorking is true

## Tutorial Steps

### Step 1: Prepare Your API Request

To add a calendar exception, you'll use the POST method on the following endpoint:

```
POST https://api.aspose.cloud/v3.0/tasks/{name}/calendars/{calendarUid}/calendarExceptions
```

Where:
- `{name}` is your project file name (with extension)
- `{calendarUid}` is the unique identifier of the calendar

#### Try It Yourself:

1. Identify a project file you want to work with
2. Determine the calendar UID (you can get this by retrieving all calendars)
3. Decide what type of exception you want to add (holiday, special working day, etc.)
4. Prepare the JSON payload with the exception details

### Step 2: Create the Request Payload

For adding a calendar exception, you need to provide a JSON payload with the exception details. Here's an example:

```json
{
  "Index": 0,
  "EnteredByOccurrences": true,
  "FromDate": "2023-05-29T00:00:00.000Z",
  "ToDate": "2023-05-29T23:59:59.000Z",
  "Occurrences": 0,
  "Name": "Memorial Day",
  "Type": "Daily",
  "Period": 0,
  "DaysOfWeek": [],
  "MonthDay": 0,
  "DayWorking": false
}
```

This example creates a non-working day for Memorial Day. If you want to add a working exception with custom hours, include the `WorkingTimes` property:

```json
{
  "Index": 0,
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
      "FromTime": "2023-06-03T09:00:00.000Z",
      "ToTime": "2023-06-03T13:00:00.000Z"
    }
  ]
}
```

### Step 3: Make the API Request

Let's see how to make this request using cURL:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/calendars/1/calendarExceptions" \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d '{ "Index": 0, "EnteredByOccurrences": true, "FromDate": "2023-05-29T00:00:00.000Z", "ToDate": "2023-05-29T23:59:59.000Z", "Occurrences": 0, "Name": "Memorial Day", "Period": 0, "DaysOfWeek": [], "MonthDay": 0, "DayWorking": false }'
```

Replace `YOUR_ACCESS_TOKEN` with the token obtained through authentication.

### Step 4: Process the Response

When successful, the API returns a JSON response with a success status:

```json
{
  "Code": 200,
  "Status": "OK"
}
```

### Step 5: Implement in Your Application

Now let's implement this functionality using different programming languages.

#### C# Implementation

```csharp
// Tutorial Code Example - Adding Calendar Exceptions in C#
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
                
                // Step 2: Define project file name and calendar UID
                string fileName = "Home move plan.mpp";
                int calendarUid = 1;
                
                // Step 3: Create calendar exception payload
                var exceptionPayload = new
                {
                    Index = 0,
                    EnteredByOccurrences = true,
                    FromDate = DateTime.Now.Date.AddDays(5), // 5 days from now
                    ToDate = DateTime.Now.Date.AddDays(5).AddHours(23).AddMinutes(59).AddSeconds(59),
                    Occurrences = 0,
                    Name = "Project Review Day",
                    Type = "Daily",
                    Period = 0,
                    DaysOfWeek = new int[] { },
                    MonthDay = 0,
                    DayWorking = true,
                    WorkingTimes = new[]
                    {
                        new
                        {
                            FromTime = DateTime.Today.AddHours(10),
                            ToTime = DateTime.Today.AddHours(15)
                        }
                    }
                };
                
                // Step 4: Add calendar exception
                await AddCalendarException(accessToken, fileName, calendarUid, exceptionPayload);
                
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
        
        static async Task AddCalendarException(string accessToken, string fileName, int calendarUid, object exceptionPayload)
        {
            using (var client = new HttpClient())
            {
                // Set authorization header
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
                
                // Prepare the request URL
                string requestUrl = $"{baseUrl}{Uri.EscapeDataString(fileName)}/calendars/{calendarUid}/calendarExceptions";
                
                Console.WriteLine($"Adding calendar exception to: {requestUrl}");
                
                // Serialize the payload
                var jsonPayload = JsonConvert.SerializeObject(exceptionPayload);
                var content = new StringContent(jsonPayload, Encoding.UTF8, "application/json");
                
                // Make the request
                var response = await client.PostAsync(requestUrl, content);
                
                if (response.IsSuccessStatusCode)
                {
                    var jsonString = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("Calendar exception added successfully!");
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
# Tutorial Code Example - Adding Calendar Exceptions in Python
import requests
import json
from datetime import datetime, timedelta

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

def add_calendar_exception(access_token, file_name, calendar_uid, exception_data):
    """
    Add a calendar exception to the specified calendar
    """
    # Prepare the request URL
    request_url = f"{base_url}{file_name}/calendars/{calendar_uid}/calendarExceptions"
    
    # Prepare headers
    headers = {
        "Authorization": f"Bearer {access_token}",
        "Content-Type": "application/json",
        "Accept": "application/json"
    }
    
    print(f"Adding calendar exception to: {request_url}")
    
    # Make the request
    response = requests.post(request_url, headers=headers, json=exception_data)
    
    if response.status_code == 200:
        print("Calendar exception added successfully!")
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
        
        # Step 3: Create exception data
        # Create a holiday next Monday
        next_monday = datetime.now() + timedelta(days=(7 - datetime.now().weekday()) % 7)
        exception_data = {
            "Index": 0,
            "EnteredByOccurrences": True,
            "FromDate": next_monday.strftime("%Y-%m-%dT00:00:00.000Z"),
            "ToDate": next_monday.strftime("%Y-%m-%dT23:59:59.999Z"),
            "Occurrences": 0,
            "Name": "Team Building Day",
            "Type": "Daily",
            "Period": 0,
            "DaysOfWeek": [],
            "MonthDay": 0,
            "DayWorking": False  # Non-working day
        }
        
        # Step 4: Add the calendar exception
        result = add_calendar_exception(access_token, file_name, calendar_uid, exception_data)
        
        if result:
            print("\nAPI Response:")
            print(json.dumps(result, indent=2))
            
    except Exception as e:
        print(f"An error occurred: {str(e)}")

if __name__ == "__main__":
    main()
```

### Creating Different Types of Calendar Exceptions

#### Example 1: Single Holiday

```json
{
  "Index": 0,
  "EnteredByOccurrences": true,
  "FromDate": "2023-12-25T00:00:00.000Z",
  "ToDate": "2023-12-25T23:59:59.999Z",
  "Occurrences": 0,
  "Name": "Christmas Day",
  "Type": "Daily",
  "Period": 0,
  "DaysOfWeek": [],
  "MonthDay": 0,
  "DayWorking": false
}
```

#### Example 2: Weekly Recurring Meeting

```json
{
  "Index": 0,
  "EnteredByOccurrences": true,
  "FromDate": "2023-06-02T00:00:00.000Z",
  "ToDate": "2023-06-02T23:59:59.999Z",
  "Occurrences": 10,
  "Name": "Weekly Team Meeting",
  "Type": "Weekly",
  "Period": 1,
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

#### Example 3: Monthly Event (First Monday)

```json
{
  "Index": 0,
  "EnteredByOccurrences": true,
  "FromDate": "2023-06-05T00:00:00.000Z",
  "ToDate": "2023-06-05T23:59:59.999Z",
  "Occurrences": 6,
  "Name": "Monthly Review",
  "Type": "Monthly",
  "Period": 1,
  "MonthItem": "Day",
  "MonthPosition": "First",
  "DaysOfWeek": [2],
  "MonthDay": 0,
  "DayWorking": true,
  "WorkingTimes": [
    {
      "FromTime": "2023-06-05T10:00:00.000Z",
      "ToTime": "2023-06-05T12:00:00.000Z"
    }
  ]
}
```

### Common Issues and Troubleshooting

1. Authentication Errors:
   - Ensure your Client ID and Client Secret are correct
   - Check that your subscription is active

2. Invalid Calendar UID:
   - Verify that the calendar UID exists in the project file
   - Retrieve all calendars first to confirm the correct UID

3. Date Format Issues:
   - Always use ISO 8601 format for dates (YYYY-MM-DDThh:mm:ss.sssZ)
   - Ensure that ToDate is not before FromDate

4. Index Conflicts:
   - If you receive an error about index already existing, either specify a different index
   - Or retrieve existing exceptions first to find a vacant index

## What You've Learned

In this tutorial, you've learned:
- How to construct a request to add calendar exceptions
- The various properties of calendar exceptions and their purpose
- How to create different types of exceptions (holidays, meetings, recurring events)
- Implementation approaches in different programming languages

## Next Steps

Now that you know how to add calendar exceptions, you might want to learn how to update them. Continue your learning journey with our next tutorial:

→ [Learn to Update Calendar Exceptions](/working-with-calendar-exceptions/update-calendar-exception/)

## Further Practice

To reinforce your learning:
1. Create a holiday calendar by adding exceptions for standard holidays
2. Build a script that adds multiple exceptions in a single operation
3. Implement a user interface that allows users to visually add exceptions

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [forum](https://forum.aspose.cloud/c/tasks/16/) for assistance.