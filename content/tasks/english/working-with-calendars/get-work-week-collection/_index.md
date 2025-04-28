---
title: How to Access Work Week Collections in a Calendar Tutorial
description: Learn how to retrieve and work with calendar work week collections using Aspose.Tasks Cloud API in this comprehensive developer tutorial with code examples.
url: /working-with-calendars/get-work-week-collection/
weight: 30
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Retrieve work week collections from a specific calendar
- Understand the structure of work week definitions
- Access and interpret weekday configurations and working times
- Implement this functionality in various programming languages

## Prerequisites

Before starting this tutorial:
- Complete the [previous tutorials on calendar operations](/working-with-calendars/)
- Have your Aspose Cloud API credentials ready
- Know the UID of the calendar whose work weeks you want to retrieve

## What Are Work Weeks?

Work weeks are a fundamental component of project calendars that define the standard working patterns for specific date ranges. They allow project managers to configure:

- Custom working days for specific time periods
- Specialized shifts or working hours for selected date ranges
- Recurring exceptions or special schedules that differ from the default calendar

Understanding how to access work week collections is essential for correctly interpreting and manipulating project schedules.

## Step-by-Step Tutorial

### 1. API Overview

For this tutorial, we'll use the following API endpoint:

| API | Type | Description | Resource Link |
| :- | :- | :- | :- |
| /tasks/{name}/calendars/{calendarUid}/workWeeks | GET | Read work week information for a given Calendar | [Get Calendar WorkWeeks](https://apireference.aspose.cloud/tasks/#/TasksCalendar/GetCalendarWorkWeeks) |


### 2. Making the Request with cURL

Let's retrieve the work weeks of a calendar using cURL:

```bash
curl -X GET "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/calendars/1/workWeeks" \
     -H "accept: application/json" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

> Note: Replace `YOUR_ACCESS_TOKEN` with an actual access token obtained from Aspose.Cloud authentication API.

### 3. Understanding the Response

The API returns a collection of work weeks defined in the calendar. Each work week includes:
- Name (optional)
- Date range (fromDate and toDate)
- List of weekdays with their configurations

Here's a sample response:

```json
{
  "code": 0,
  "status": "OK",
  "calendarWorkWeeks": [
    {
      "name": "Holiday Season",
      "fromDate": "2020-12-20T00:00:00",
      "toDate": "2021-01-03T00:00:00",
      "weekDays": [
        {
          "dayType": "Monday",
          "dayWorking": false,
          "fromDate": null,
          "toDate": null,
          "workingTimes": []
        },
        {
          "dayType": "Tuesday",
          "dayWorking": false,
          "fromDate": null,
          "toDate": null,
          "workingTimes": []
        },
        {
          "dayType": "Wednesday",
          "dayWorking": true,
          "fromDate": null,
          "toDate": null,
          "workingTimes": [
            {
              "fromTime": "2020-12-23T09:00:00",
              "toTime": "2020-12-23T13:00:00"
            }
          ]
        }
      ]
    }
  ]
}
```

### 4. Implementation in Different Languages

Let's implement this functionality in various programming languages.

#### C# Implementation

```csharp
// Tutorial Code Example: Get Calendar Work Weeks with C#
using System;
using Aspose.Tasks.Cloud.Sdk.Api;
using Aspose.Tasks.Cloud.Sdk.Model;

namespace AsposeTasksCloudTutorials
{
    class GetCalendarWorkWeeksExample
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
                // Make API call to get calendar work weeks
                var response = tasksApi.GetCalendarWorkWeeks(fileName, calendarUid);
                
                // Process results
                Console.WriteLine("Calendar work weeks retrieved successfully!");
                
                if (response.CalendarWorkWeeks != null && response.CalendarWorkWeeks.Count > 0)
                {
                    Console.WriteLine($"Total work weeks: {response.CalendarWorkWeeks.Count}");
                    
                    // Display work week information
                    foreach (var workWeek in response.CalendarWorkWeeks)
                    {
                        Console.WriteLine($"\nWork Week: {workWeek.Name ?? "Unnamed"}");
                        Console.WriteLine($"From Date: {workWeek.FromDate.ToString("yyyy-MM-dd")}");
                        Console.WriteLine($"To Date: {workWeek.ToDate.ToString("yyyy-MM-dd")}");
                        
                        Console.WriteLine("Weekdays:");
                        foreach (var weekDay in workWeek.WeekDays)
                        {
                            Console.WriteLine($"- {weekDay.DayType}: {(weekDay.DayWorking ? "Working" : "Non-working")}");
                            
                            if (weekDay.DayWorking && weekDay.WorkingTimes != null && weekDay.WorkingTimes.Count > 0)
                            {
                                Console.WriteLine("  Working Times:");
                                foreach (var workTime in weekDay.WorkingTimes)
                                {
                                    Console.WriteLine($"    {workTime.FromTime.ToString("HH:mm")} - {workTime.ToTime.ToString("HH:mm")}");
                                }
                            }
                        }
                    }
                }
                else
                {
                    Console.WriteLine("No work weeks defined for this calendar.");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error retrieving calendar work weeks: {ex.Message}");
            }
        }
    }
}
```

#### Python Implementation

```python
# Tutorial Code Example: Get Calendar Work Weeks with Python
import os
from datetime import datetime
from asposetaskscloud import ApiClient, TasksApi, Configuration

def get_calendar_work_weeks():
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
        # Make API call to get calendar work weeks
        response = tasks_api.get_calendar_work_weeks(file_name, calendar_uid)
        
        # Process and display results
        print("Calendar work weeks retrieved successfully!")
        
        if response.calendar_work_weeks and len(response.calendar_work_weeks) > 0:
            print(f"Total work weeks: {len(response.calendar_work_weeks)}")
            
            # Display work week information
            for work_week in response.calendar_work_weeks:
                print(f"\nWork Week: {work_week.name if work_week.name else 'Unnamed'}")
                print(f"From Date: {work_week.from_date.strftime('%Y-%m-%d')}")
                print(f"To Date: {work_week.to_date.strftime('%Y-%m-%d')}")
                
                print("Weekdays:")
                for week_day in work_week.week_days:
                    print(f"- {week_day.day_type}: {'Working' if week_day.day_working else 'Non-working'}")
                    
                    if week_day.day_working and week_day.working_times and len(week_day.working_times) > 0:
                        print("  Working Times:")
                        for work_time in week_day.working_times:
                            from_time = work_time.from_time.strftime("%H:%M")
                            to_time = work_time.to_time.strftime("%H:%M")
                            print(f"    {from_time} - {to_time}")
        else:
            print("No work weeks defined for this calendar.")
            
        return response.calendar_work_weeks
    except Exception as e:
        print(f"Error retrieving calendar work weeks: {str(e)}")
        return None

# Run the example
if __name__ == "__main__":
    get_calendar_work_weeks()
```

#### Java Implementation

```java
// Tutorial Code Example: Get Calendar Work Weeks with Java
import com.aspose.tasks.cloud.ApiClient;
import com.aspose.tasks.cloud.ApiException;
import com.aspose.tasks.cloud.Configuration;
import com.aspose.tasks.cloud.auth.OAuth;
import com.aspose.tasks.cloud.model.CalendarWorkWeek;
import com.aspose.tasks.cloud.model.CalendarWorkWeeksResponse;
import com.aspose.tasks.cloud.model.WeekDay;
import com.aspose.tasks.cloud.model.WorkingTime;
import com.aspose.tasks.cloud.api.TasksApi;

import java.time.format.DateTimeFormatter;
import java.util.List;

public class GetCalendarWorkWeeksExample {
    
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
            // Make API call to get calendar work weeks
            CalendarWorkWeeksResponse response = tasksApi.getCalendarWorkWeeks(fileName, calendarUid);
            
            // Process results
            System.out.println("Calendar work weeks retrieved successfully!");
            
            List<CalendarWorkWeek> workWeeks = response.getCalendarWorkWeeks();
            
            if (workWeeks != null && !workWeeks.isEmpty()) {
                System.out.println("Total work weeks: " + workWeeks.size());
                
                DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
                DateTimeFormatter timeFormatter = DateTimeFormatter.ofPattern("HH:mm");
                
                // Display work week information
                for (CalendarWorkWeek workWeek : workWeeks) {
                    System.out.println("\nWork Week: " + (workWeek.getName() != null ? workWeek.getName() : "Unnamed"));
                    System.out.println("From Date: " + workWeek.getFromDate().format(dateFormatter));
                    System.out.println("To Date: " + workWeek.getToDate().format(dateFormatter));
                    
                    System.out.println("Weekdays:");
                    List<WeekDay> weekDays = workWeek.getWeekDays();
                    
                    for (WeekDay weekDay : weekDays) {
                        System.out.println("- " + weekDay.getDayType() + ": " + 
                                          (weekDay.getDayWorking() ? "Working" : "Non-working"));
                        
                        if (weekDay.getDayWorking() && weekDay.getWorkingTimes() != null && !weekDay.getWorkingTimes().isEmpty()) {
                            System.out.println("  Working Times:");
                            
                            for (WorkingTime workTime : weekDay.getWorkingTimes()) {
                                String fromTime = workTime.getFromTime().format(timeFormatter);
                                String toTime = workTime.getToTime().format(timeFormatter);
                                System.out.println("    " + fromTime + " - " + toTime);
                            }
                        }
                    }
                }
            } else {
                System.out.println("No work weeks defined for this calendar.");
            }
        } catch (ApiException e) {
            System.err.println("Error retrieving calendar work weeks: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 5. Try It Yourself

Now that you understand how to access work week collections, try it yourself:

1. Set up the Aspose.Tasks Cloud SDK for your preferred programming language
2. Upload a project file to your Aspose.Cloud storage
3. Get a calendar UID from the calendar list (from the first tutorial)
4. Use the code examples above to retrieve work week collections
5. Examine the different work week properties and their meanings

> Troubleshooting Tip: If you receive an empty list of work weeks, it's likely that your calendar doesn't have any custom work weeks defined. Try to add a custom work week to your project file using Microsoft Project before uploading it to Aspose.Cloud storage.

### 6. Understanding Work Week Properties

Let's examine the key properties of work weeks and their significance:

- name: Optional name given to the work week (e.g., "Holiday Season", "Summer Hours")
- fromDate and toDate: The date range for which this work week configuration applies
- weekDays: Collection of day configurations for this work week
  - dayType: The type of day (Monday, Tuesday, etc.)
  - dayWorking: Boolean indicating if this is a working day
  - workingTimes: Time slots defined as working hours for this day

Work weeks override the standard calendar settings for specific date ranges, allowing project managers to define special working periods.

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve work week collections from a specific calendar using the Aspose.Tasks Cloud API
- Access and interpret work week properties such as date ranges and day configurations
- Understand the structure and purpose of work weeks in project calendars
- Implement this functionality using different programming languages

This knowledge is valuable for understanding project schedules and making accurate time calculations, especially when projects have periods with special working arrangements.

## Further Practice

To enhance your understanding:
1. Create a program that compares standard calendar days with work week definitions to identify differences
2. Develop a visualization of working hours across different work weeks
3. Calculate the total working hours for a specific date range, taking into account both standard calendar and work week definitions

## Next Tutorial

Ready to learn more? Proceed to the next tutorial: [Tutorial: Adding New Calendars to a Project](/working-with-calendars/add-calendar/) to learn how to create and add custom calendars to your projects.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).


