---
title: How to Get a Specific Project Calendar Tutorial
description: Learn how to retrieve detailed information about a specific calendar in a project file using Aspose.Tasks Cloud API in this step-by-step developer tutorial.
url: /working-with-calendars/get-specific-calendar/
weight: 20
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve detailed information about a specific calendar by its UID
- Access calendar days, working times, and other properties
- Implement this functionality in multiple programming languages

## Prerequisites

Before starting this tutorial:
- Complete the [previous tutorial on retrieving calendar items](/working-with-calendars/get-calendars/)
- Have your Aspose Cloud API credentials ready
- Know the UID of the calendar you want to retrieve (obtained from the previous tutorial)

## Why Retrieve Specific Calendar Details?

While the previous tutorial showed you how to get a list of all calendars, this tutorial focuses on retrieving comprehensive details about a specific calendar. This information is crucial for understanding working days, exceptions, and time configurations that affect project scheduling.

## Step-by-Step Tutorial

### 1. API Overview

For this tutorial, we'll use the following API endpoint:

| API | Type | Description | Resource Link |
| :- | :- | :- | :- |
| /tasks/{name}/calendars/{calendarUid} | GET | Read information for a specific calendar by UID | [Get Calendar](https://apireference.aspose.cloud/tasks/#/TasksCalendar/GetCalendar) |

### 2. Basic Request Structure

The basic structure of our request will be:

```
GET https://api.aspose.cloud/v3.0/tasks/{projectFileName}/calendars/{calendarUid}
```

Where:
- `{projectFileName}` is the name of your project file stored in the Aspose.Cloud storage
- `{calendarUid}` is the unique identifier of the calendar you want to retrieve

### 3. Making the Request with cURL

Let's try retrieving a specific calendar using cURL:

```bash
curl -X GET "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/calendars/1" \
     -H "accept: application/json" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> Note: Replace `YOUR_ACCESS_TOKEN` with an actual access token obtained from Aspose.Cloud authentication API.

### 4. Understanding the Response

The API returns detailed information about the calendar, including:
- Calendar name and UID
- List of working days and exceptions
- Working times for each day
- Base calendar information

Here's a sample response:

```json
{
  "code": 0,
  "status": "OK",
  "calendar": {
    "name": "Standard",
    "uid": 1,
    "days": [
      {
        "dayType": "Monday",
        "dayWorking": true,
        "fromDate": "2020-09-18T00:00:00",
        "toDate": "2020-09-18T23:59:59",
        "workingTimes": [
          {
            "fromTime": "2020-09-18T08:00:00",
            "toTime": "2020-09-18T12:00:00"
          },
          {
            "fromTime": "2020-09-18T13:00:00",
            "toTime": "2020-09-18T17:00:00"
          }
        ]
      },
      {
        "dayType": "Exception",
        "dayWorking": false,
        "fromDate": "2020-12-25T00:00:00",
        "toDate": "2020-12-25T23:59:59",
        "workingTimes": []
      }
    ],
    "isBaseCalendar": true,
    "isBaselineCalendar": false
  }
}
```

### 5. Implementation in Different Languages

Let's implement this functionality in various programming languages.

#### C# Implementation

```csharp
// Tutorial Code Example: Get Specific Calendar with C#
using System;
using Aspose.Tasks.Cloud.Sdk.Api;
using Aspose.Tasks.Cloud.Sdk.Model;

namespace AsposeTasksCloudTutorials
{
    class GetSpecificCalendarExample
    {
        static void Main(string[] args)
        {
            // Configure API client
            var config = new Configuration
            {
                ClientId = "YOUR_CLIENT_ID",
                ClientSecret = "YOUR_CLIENT_SECRET"
            };
            
            // Initialize TasksApi instance
            var tasksApi = new TasksApi(config);
            
            // Specify project file name and calendar UID
            var fileName = "Home move plan.mpp";
            var calendarUid = 1;
            
            try
            {
                // Make API call to get specific calendar
                var response = tasksApi.GetCalendar(fileName, calendarUid);
                
                // Process results
                Console.WriteLine("Calendar retrieved successfully!");
                Console.WriteLine($"Calendar Name: {response.Calendar.Name}");
                Console.WriteLine($"Calendar UID: {response.Calendar.Uid}");
                Console.WriteLine($"Is Base Calendar: {response.Calendar.IsBaseCalendar}");
                
                // Display calendar days
                Console.WriteLine("\nCalendar Days:");
                foreach (var day in response.Calendar.Days)
                {
                    Console.WriteLine($"- Day Type: {day.DayType}");
                    Console.WriteLine($"  Working Day: {day.DayWorking}");
                    
                    if (day.DayWorking && day.WorkingTimes != null && day.WorkingTimes.Count > 0)
                    {
                        Console.WriteLine("  Working Times:");
                        foreach (var workTime in day.WorkingTimes)
                        {
                            Console.WriteLine($"    {workTime.FromTime.ToString("HH:mm")} - {workTime.ToTime.ToString("HH:mm")}");
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error retrieving calendar: {ex.Message}");
            }
        }
    }
}
```

#### Python Implementation

```python
# Tutorial Code Example: Get Specific Calendar with Python
import os
from datetime import datetime
from asposetaskscloud import ApiClient, TasksApi, Configuration

def get_specific_calendar():
    # Configure API client
    configuration = Configuration(
        client_id="YOUR_CLIENT_ID",
        client_secret="YOUR_CLIENT_SECRET"
    )
    
    # Initialize API client
    api_client = ApiClient(configuration)
    tasks_api = TasksApi(api_client)
    
    # Specify project file name and calendar UID
    file_name = "Home move plan.mpp"
    calendar_uid = 1
    
    try:
        # Make API call to get specific calendar
        response = tasks_api.get_calendar(file_name, calendar_uid)
        
        # Process and display results
        calendar = response.calendar
        print("Calendar retrieved successfully!")
        print(f"Calendar Name: {calendar.name}")
        print(f"Calendar UID: {calendar.uid}")
        print(f"Is Base Calendar: {calendar.is_base_calendar}")
        
        # Display calendar days
        print("\nCalendar Days:")
        for day in calendar.days:
            print(f"- Day Type: {day.day_type}")
            print(f"  Working Day: {day.day_working}")
            
            if day.day_working and day.working_times and len(day.working_times) > 0:
                print("  Working Times:")
                for work_time in day.working_times:
                    from_time = work_time.from_time.strftime("%H:%M")
                    to_time = work_time.to_time.strftime("%H:%M")
                    print(f"    {from_time} - {to_time}")
                    
        return calendar
    except Exception as e:
        print(f"Error retrieving calendar: {str(e)}")
        return None

# Run the example
if __name__ == "__main__":
    get_specific_calendar()
```

#### Java Implementation

```java
// Tutorial Code Example: Get Specific Calendar with Java
import com.aspose.tasks.cloud.ApiClient;
import com.aspose.tasks.cloud.ApiException;
import com.aspose.tasks.cloud.Configuration;
import com.aspose.tasks.cloud.auth.OAuth;
import com.aspose.tasks.cloud.model.CalendarResponse;
import com.aspose.tasks.cloud.model.WeekDay;
import com.aspose.tasks.cloud.model.WorkingTime;
import com.aspose.tasks.cloud.api.TasksApi;

import java.time.format.DateTimeFormatter;
import java.util.List;

public class GetSpecificCalendarExample {
    
    public static void main(String[] args) {
        // Configure API client
        ApiClient apiClient = new ApiClient();
        apiClient.setBasePath("https://api.aspose.cloud/v3.0");
        
        // Configure OAuth2 access token
        OAuth oauth = (OAuth) apiClient.getAuthentication("JWT");
        oauth.setClientId("YOUR_CLIENT_ID");
        oauth.setClientSecret("YOUR_CLIENT_SECRET");
        
        // Initialize TasksApi instance
        TasksApi tasksApi = new TasksApi(apiClient);
        
        // Specify project file name and calendar UID
        String fileName = "Home move plan.mpp";
        Integer calendarUid = 1;
        
        try {
            // Make API call to get specific calendar
            CalendarResponse response = tasksApi.getCalendar(fileName, calendarUid);
            
            // Process results
            System.out.println("Calendar retrieved successfully!");
            System.out.println("Calendar Name: " + response.getCalendar().getName());
            System.out.println("Calendar UID: " + response.getCalendar().getUid());
            System.out.println("Is Base Calendar: " + response.getCalendar().getIsBaseCalendar());
            
            // Display calendar days
            System.out.println("\nCalendar Days:");
            List<WeekDay> days = response.getCalendar().getDays();
            
            DateTimeFormatter timeFormatter = DateTimeFormatter.ofPattern("HH:mm");
            
            for (WeekDay day : days) {
                System.out.println("- Day Type: " + day.getDayType());
                System.out.println("  Working Day: " + day.getDayWorking());
                
                if (day.getDayWorking() && day.getWorkingTimes() != null && !day.getWorkingTimes().isEmpty()) {
                    System.out.println("  Working Times:");
                    
                    for (WorkingTime workTime : day.getWorkingTimes()) {
                        String fromTime = workTime.getFromTime().format(timeFormatter);
                        String toTime = workTime.getToTime().format(timeFormatter);
                        System.out.println("    " + fromTime + " - " + toTime);
                    }
                }
            }
        } catch (ApiException e) {
            System.err.println("Error retrieving calendar: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 6. Try It Yourself

Now that you understand how to retrieve detailed calendar information, it's time to try it yourself:

1. Choose a programming language and set up the Aspose.Tasks Cloud SDK
2. Upload a project file to your Aspose.Cloud storage
3. Get the calendar UID from the calendar list (from the previous tutorial)
4. Use the code examples above to retrieve detailed calendar information
5. Examine the different calendar properties and their meanings

> Troubleshooting Tip: If you receive a "Calendar not found" error, ensure you're using a valid calendar UID that exists in your project. Calendar UIDs are usually integers starting from 1.

### 7. Understanding Calendar Properties

Let's examine the key calendar properties returned by the API:

- name: The display name of the calendar
- uid: Unique identifier for the calendar
- days: Collection of calendar days with their configurations
  - dayType: Can be a day of the week (Monday, Tuesday, etc.) or "Exception"
  - dayWorking: Boolean indicating if this is a working day
  - workingTimes: Time slots defined as working hours
- isBaseCalendar: Indicates if this is a base calendar in the project
- isBaselineCalendar: Indicates if this calendar is used for baseline calculations

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve detailed information about a specific calendar using its UID
- Access and interpret calendar properties like working days and working times
- Implement this functionality using different programming languages
- Understand the structure of calendar data in Microsoft Project

This knowledge is essential for working with project schedules as calendar definitions directly impact task scheduling and resource availability.

## Further Practice

To solidify your understanding:
1. Retrieve and compare multiple calendars from the same project
2. Create a visual representation of the working times for a calendar
3. Extract all exception days (holidays) from a calendar

## Next Tutorial

Ready to continue learning? Proceed to the next tutorial: [How to Access Work Week Collections in a Calendar](/working-with-calendars/get-work-week-collection/) to learn about accessing work week definitions within calendars.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).
