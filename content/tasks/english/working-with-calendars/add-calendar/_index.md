---
title: Adding New Calendars to a Project Tutorial
description: Learn how to create and add custom calendars to your project files using Aspose.Tasks Cloud API with this hands-on developer tutorial and code examples.
url: /working-with-calendars/add-calendar/
weight: 40
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Create a new calendar with custom settings
- Add the calendar to an existing project
- Configure working days and times for the calendar
- Implement this functionality in multiple programming languages

## Prerequisites

Before starting this tutorial:
- Complete the [previous tutorials on calendar operations](/working-with-calendars/)
- Have your Aspose Cloud API credentials ready
- Have a project file uploaded to your Aspose.Cloud storage

## Why Create Custom Calendars?

Custom calendars are essential for accurate project scheduling when:
- Different resources follow different work schedules
- Teams work in multiple shifts or time zones
- Certain project phases require special working hours
- You need to account for regional holidays or specific non-working periods

Creating and adding custom calendars allows for more precise scheduling and resource allocation in your projects.

## Step-by-Step Tutorial

### 1. API Overview

For this tutorial, we'll use the following API endpoint:

| API | Type | Description | Resource Link |
| :- | :- | :- | :- |
| /tasks/{name}/calendars | POST | Add a calendar to a project | [PostCalendar](https://apireference.aspose.cloud/tasks/#/TasksCalendar/PostCalendar) |

### 2. Basic Request Structure

The basic structure of our request will be:

```
POST https://api.aspose.cloud/v3.0/tasks/{projectFileName}/calendars
```

Where `{projectFileName}` is the name of your project file stored in the Aspose.Cloud storage.

The request body will contain a JSON representation of the calendar to be added, including:
- Calendar name
- UID (optional - will be generated if not provided)
- Working days configuration
- Working times for each day
- Base calendar properties

### 3. Making the Request with cURL

Let's create a new calendar using cURL:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/calendars" \
     -H "accept: application/json" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -d "{
           \"Name\": \"Evening Shift\",
           \"Uid\": 0,
           \"Days\": [
             {
               \"DayType\": \"Monday\",
               \"DayWorking\": true,
               \"FromDate\": \"2023-01-01T00:00:00\",
               \"ToDate\": \"2023-01-01T23:59:59\",
               \"WorkingTimes\": [
                 {
                   \"FromTime\": \"2023-01-01T16:00:00\",
                   \"ToTime\": \"2023-01-01T20:00:00\"
                 },
                 {
                   \"FromTime\": \"2023-01-01T21:00:00\",
                   \"ToTime\": \"2023-01-01T00:00:00\"
                 }
               ]
             },
             {
               \"DayType\": \"Tuesday\",
               \"DayWorking\": true,
               \"FromDate\": \"2023-01-02T00:00:00\",
               \"ToDate\": \"2023-01-02T23:59:59\",
               \"WorkingTimes\": [
                 {
                   \"FromTime\": \"2023-01-02T16:00:00\",
                   \"ToTime\": \"2023-01-02T00:00:00\"
                 }
               ]
             },
             {
               \"DayType\": \"Saturday\",
               \"DayWorking\": true,
               \"FromDate\": \"2023-01-06T00:00:00\",
               \"ToDate\": \"2023-01-06T23:59:59\",
               \"WorkingTimes\": [
                 {
                   \"FromTime\": \"2023-01-06T10:00:00\",
                   \"ToTime\": \"2023-01-06T18:00:00\"
                 }
               ]
             }
           ],
           \"IsBaseCalendar\": false,
           \"IsBaselineCalendar\": false
         }"
```

> Note: Replace `YOUR_ACCESS_TOKEN` with an actual access token obtained from Aspose.Cloud authentication API.

### 4. Understanding the Response

If successful, the API returns information about the newly created calendar:

```json
{
  "code": 0,
  "status": "OK",
  "calendarItem": {
    "link": {
      "href": "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/calendars/3",
      "rel": "self",
      "type": "calendar",
      "title": "Evening Shift"
    },
    "uid": 3,
    "name": "Evening Shift"
  }
}
```

The response contains:
- Status code and message
- The newly created calendar item with its assigned UID
- A link to the calendar resource

### 5. Implementation in Different Languages

Let's implement this functionality in various programming languages.

#### C# Implementation

```csharp
// Tutorial Code Example: Add Calendar to Project with C#
using System;
using System.Collections.Generic;
using Aspose.Tasks.Cloud.Sdk.Api;
using Aspose.Tasks.Cloud.Sdk.Model;

namespace AsposeTasksCloudTutorials
{
    class AddCalendarToProjectExample
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
            
            // Specify project file name
            var fileName = "Home move plan.mpp";
            
            try
            {
                // Create a calendar object
                var calendar = new Calendar
                {
                    Name = "Evening Shift",
                    Uid = 0, // Will be assigned by the server
                    Days = new List<WeekDay>(),
                    IsBaseCalendar = false,
                    IsBaselineCalendar = false
                };
                
                // Add Monday configuration
                var monday = new WeekDay
                {
                    DayType = WeekDay.DayTypeEnum.Monday,
                    DayWorking = true,
                    FromDate = new DateTime(2023, 1, 1),
                    ToDate = new DateTime(2023, 1, 1, 23, 59, 59),
                    WorkingTimes = new List<WorkingTime>
                    {
                        new WorkingTime
                        {
                            FromTime = new DateTime(2023, 1, 1, 16, 0, 0),
                            ToTime = new DateTime(2023, 1, 1, 20, 0, 0)
                        },
                        new WorkingTime
                        {
                            FromTime = new DateTime(2023, 1, 1, 21, 0, 0),
                            ToTime = new DateTime(2023, 1, 2, 0, 0, 0)
                        }
                    }
                };
                calendar.Days.Add(monday);
                
                // Add Tuesday configuration
                var tuesday = new WeekDay
                {
                    DayType = WeekDay.DayTypeEnum.Tuesday,
                    DayWorking = true,
                    FromDate = new DateTime(2023, 1, 2),
                    ToDate = new DateTime(2023, 1, 2, 23, 59, 59),
                    WorkingTimes = new List<WorkingTime>
                    {
                        new WorkingTime
                        {
                            FromTime = new DateTime(2023, 1, 2, 16, 0, 0),
                            ToTime = new DateTime(2023, 1, 3, 0, 0, 0)
                        }
                    }
                };
                calendar.Days.Add(tuesday);
                
                // Add Saturday configuration (working weekend)
                var saturday = new WeekDay
                {
                    DayType = WeekDay.DayTypeEnum.Saturday,
                    DayWorking = true,
                    FromDate = new DateTime(2023, 1, 6),
                    ToDate = new DateTime(2023, 1, 6, 23, 59, 59),
                    WorkingTimes = new List<WorkingTime>
                    {
                        new WorkingTime
                        {
                            FromTime = new DateTime(2023, 1, 6, 10, 0, 0),
                            ToTime = new DateTime(2023, 1, 6, 18, 0, 0)
                        }
                    }
                };
                calendar.Days.Add(saturday);
                
                // Make API call to add calendar
                var response = tasksApi.PostCalendar(fileName, calendar);
                
                // Process results
                Console.WriteLine("Calendar added successfully!");
                Console.WriteLine($"New Calendar UID: {response.CalendarItem.Uid}");
                Console.WriteLine($"New Calendar Name: {response.CalendarItem.Name}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error adding calendar: {ex.Message}");
            }
        }
    }
}
```

#### Python Implementation

```python
# Tutorial Code Example: Add Calendar to Project with Python
import os
from datetime import datetime
from asposetaskscloud import ApiClient, TasksApi, Configuration
from asposetaskscloud.models import Calendar, WeekDay, WorkingTime

def add_calendar_to_project():
    # Configure API client
    configuration = Configuration(
        client_id="YOUR_CLIENT_ID",
        client_secret="YOUR_CLIENT_SECRET"
    )
    
    # Initialize API client
    api_client = ApiClient(configuration)
    tasks_api = TasksApi(api_client)
    
    # Specify project file name
    file_name = "Home move plan.mpp"
    
    try:
        # Create calendar object
        calendar = Calendar(
            name="Evening Shift",
            uid=0,  # Will be assigned by the server
            days=[],
            is_base_calendar=False,
            is_baseline_calendar=False
        )
        
        # Add Monday configuration
        monday = WeekDay(
            day_type="Monday",
            day_working=True,
            from_date=datetime(2023, 1, 1),
            to_date=datetime(2023, 1, 1, 23, 59, 59),
            working_times=[
                WorkingTime(
                    from_time=datetime(2023, 1, 1, 16, 0, 0),
                    to_time=datetime(2023, 1, 1, 20, 0, 0)
                ),
                WorkingTime(
                    from_time=datetime(2023, 1, 1, 21, 0, 0),
                    to_time=datetime(2023, 1, 2, 0, 0, 0)
                )
            ]
        )
        calendar.days.append(monday)
        
        # Add Tuesday configuration
        tuesday = WeekDay(
            day_type="Tuesday",
            day_working=True,
            from_date=datetime(2023, 1, 2),
            to_date=datetime(2023, 1, 2, 23, 59, 59),
            working_times=[
                WorkingTime(
                    from_time=datetime(2023, 1, 2, 16, 0, 0),
                    to_time=datetime(2023, 1, 3, 0, 0, 0)
                )
            ]
        )
        calendar.days.append(tuesday)
        
        # Add Saturday configuration (working weekend)
        saturday = WeekDay(
            day_type="Saturday",
            day_working=True,
            from_date=datetime(2023, 1, 6),
            to_date=datetime(2023, 1, 6, 23, 59, 59),
            working_times=[
                WorkingTime(
                    from_time=datetime(2023, 1, 6, 10, 0, 0),
                    to_time=datetime(2023, 1, 6, 18, 0, 0)
                )
            ]
        )
        calendar.days.append(saturday)
        
        # Make API call to add calendar
        response = tasks_api.post_calendar(file_name, calendar)
        
        # Process and display results
        print("Calendar added successfully!")
        print(f"New Calendar UID: {response.calendar_item.uid}")
        print(f"New Calendar Name: {response.calendar_item.name}")
        
        return response.calendar_item
    except Exception as e:
        print(f"Error adding calendar: {str(e)}")
        return None

# Run the example
if __name__ == "__main__":
    add_calendar_to_project()
```

#### Java Implementation

```java
// Tutorial Code Example: Add Calendar to Project with Java
import com.aspose.tasks.cloud.ApiClient;
import com.aspose.tasks.cloud.ApiException;
import com.aspose.tasks.cloud.Configuration;
import com.aspose.tasks.cloud.auth.OAuth;
import com.aspose.tasks.cloud.model.*;
import com.aspose.tasks.cloud.api.TasksApi;

import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class AddCalendarToProjectExample {
    
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
        
        // Specify project file name
        String fileName = "Home move plan.mpp";
        
        try {
            // Create calendar object
            Calendar calendar = new Calendar();
            calendar.setName("Evening Shift");
            calendar.setUid(0); // Will be assigned by the server
            calendar.setDays(new ArrayList<>());
            calendar.setIsBaseCalendar(false);
            calendar.setIsBaselineCalendar(false);
            
            // Helper method to convert LocalDateTime to Date
            java.util.function.Function<LocalDateTime, Date> toDate = 
                ldt -> Date.from(ldt.atZone(ZoneId.systemDefault()).toInstant());
            
            // Add Monday configuration
            WeekDay monday = new WeekDay();
            monday.setDayType(WeekDay.DayTypeEnum.MONDAY);
            monday.setDayWorking(true);
            monday.setFromDate(toDate.apply(LocalDateTime.of(2023, 1, 1, 0, 0, 0)));
            monday.setToDate(toDate.apply(LocalDateTime.of(2023, 1, 1, 23, 59, 59)));
            
            List<WorkingTime> mondayWorkingTimes = new ArrayList<>();
            
            WorkingTime mondayFirstShift = new WorkingTime();
            mondayFirstShift.setFromTime(toDate.apply(LocalDateTime.of(2023, 1, 1, 16, 0, 0)));
            mondayFirstShift.setToTime(toDate.apply(LocalDateTime.of(2023, 1, 1, 20, 0, 0)));
            mondayWorkingTimes.add(mondayFirstShift);
            
            WorkingTime mondaySecondShift = new WorkingTime();
            mondaySecondShift.setFromTime(toDate.apply(LocalDateTime.of(2023, 1, 1, 21, 0, 0)));
            mondaySecondShift.setToTime(toDate.apply(LocalDateTime.of(2023, 1, 2, 0, 0, 0)));
            mondayWorkingTimes.add(mondaySecondShift);
            
            monday.setWorkingTimes(mondayWorkingTimes);
            calendar.getDays().add(monday);
            
            // Add Tuesday configuration
            WeekDay tuesday = new WeekDay();
            tuesday.setDayType(WeekDay.DayTypeEnum.TUESDAY);
            tuesday.setDayWorking(true);
            tuesday.setFromDate(toDate.apply(LocalDateTime.of(2023, 1, 2, 0, 0, 0)));
            tuesday.setToDate(toDate.apply(LocalDateTime.of(2023, 1, 2, 23, 59, 59)));
            
            List<WorkingTime> tuesdayWorkingTimes = new ArrayList<>();
            
            WorkingTime tuesdayShift = new WorkingTime();
            tuesdayShift.setFromTime(toDate.apply(LocalDateTime.of(2023, 1, 2, 16, 0, 0)));
            tuesdayShift.setToTime(toDate.apply(LocalDateTime.of(2023, 1, 3, 0, 0, 0)));
            tuesdayWorkingTimes.add(tuesdayShift);
            
            tuesday.setWorkingTimes(tuesdayWorkingTimes);
            calendar.getDays().add(tuesday);
            
            // Add Saturday configuration (working weekend)
            WeekDay saturday = new WeekDay();
            saturday.setDayType(WeekDay.DayTypeEnum.SATURDAY);
            saturday.setDayWorking(true);
            saturday.setFromDate(toDate.apply(LocalDateTime.of(2023, 1, 6, 0, 0, 0)));
            saturday.setToDate(toDate.apply(LocalDateTime.of(2023, 1, 6, 23, 59, 59)));
            
            List<WorkingTime> saturdayWorkingTimes = new ArrayList<>();
            
            WorkingTime saturdayShift = new WorkingTime();
            saturdayShift.setFromTime(toDate.apply(LocalDateTime.of(2023, 1, 6, 10, 0, 0)));
            saturdayShift.setToTime(toDate.apply(LocalDateTime.of(2023, 1, 6, 18, 0, 0)));
            saturdayWorkingTimes.add(saturdayShift);
            
            saturday.setWorkingTimes(saturdayWorkingTimes);
            calendar.getDays().add(saturday);
            
            // Make API call to add calendar
            CalendarItemResponse response = tasksApi.postCalendar(fileName, calendar);
            
            // Process results
            System.out.println("Calendar added successfully!");
            System.out.println("New Calendar UID: " + response.getCalendarItem().getUid());
            System.out.println("New Calendar Name: " + response.getCalendarItem().getName());
        } catch (ApiException e) {
            System.err.println("Error adding calendar: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 6. Try It Yourself

Now it's time to create your own custom calendar:

1. Set up your development environment with the Aspose.Tasks Cloud SDK
2. Upload a project file to your Aspose.Cloud storage
3. Modify the code examples to create a calendar with your desired working schedule
4. Run the code and verify that the calendar was added successfully
5. Use Microsoft Project or another viewer to open the modified project file and confirm the calendar appears

> Troubleshooting Tip: If you encounter date-related errors, make sure that your date formats are consistent and that the working times are properly defined. The calendar definition is quite sensitive to correct date and time formats.

### 7. Understanding Calendar Creation Parameters

When creating a calendar, it's important to understand these key parameters:

- Name: A descriptive name for the calendar
- Uid: Unique identifier (can be 0 to let the API assign one)
- Days: Collection of day definitions
  - DayType: Type of day (Monday, Tuesday, etc., or Exception)
  - DayWorking: Whether this is a working day
  - FromDate and ToDate: Date range for this day definition
  - WorkingTimes: Working time slots for this day
- IsBaseCalendar: Whether this is a base calendar
- IsBaselineCalendar: Whether this calendar is used for baseline calculations

For most custom calendars, you'll want to set both `IsBaseCalendar` and `IsBaselineCalendar` to `false`.

## What You've Learned

In this tutorial, you've learned how to:
- Create a new calendar with custom working days and times
- Add the calendar to an existing project
- Define multiple working time slots for specific days
- Configure weekend days as working days
- Implement this functionality using different programming languages

These skills allow you to create specialized calendars for different teams, shifts, or project phases, enabling more accurate scheduling in your project management applications.

## Further Practice

To reinforce your learning:
1. Create a calendar with a holiday exception (set a specific date as non-working)
2. Create a calendar with multiple shifts (morning, afternoon, night)
3. Create a calendar for a team working in a different time zone
4. Create a calendar based on an existing calendar but with modified working hours

## Next Tutorial

Ready to continue learning? Proceed to the next tutorial: [Learn to Edit and Update Project Calendars](/working-with-calendars/edit-calendar/) to learn how to modify existing calendars in your projects.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).