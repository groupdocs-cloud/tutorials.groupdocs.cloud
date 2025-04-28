---
title: Learn to Edit and Update Project Calendars Tutorial
description: "Learn how to modify existing calendars in your project files with this hands-on tutorial using Aspose.Tasks Cloud API, including code examples in multiple languages."
url: /working-with-calendars/edit-calendar/
weight: 50
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Modify an existing calendar in a project file
- Update calendar properties, working days, and working times
- Make specific days working or non-working
- Implement this functionality in multiple programming languages

## Prerequisites

Before starting this tutorial:
- Complete the [previous tutorials on calendar operations](/working-with-calendars/)
- Have your Aspose Cloud API credentials ready
- Know the UID of the calendar you want to edit
- Have a project file with at least one calendar uploaded to your Aspose.Cloud storage

## Why Edit Existing Calendars?

Editing existing calendars is a common task when:
- Project schedules change due to shifting requirements
- Working hours need adjustment for specific project phases
- Unexpected holidays or working weekends need to be accommodated
- Resource availability patterns change

Mastering calendar editing allows you to keep your project schedules accurate and up-to-date as conditions change.

## Step-by-Step Tutorial

### 1. API Overview

For this tutorial, we'll use the following API endpoint:

| API | Type | Description | Resource Link |
| :- | :- | :- | :- |
| /tasks/{name}/calendars | PUT | Update a Calendar in a Project | [PutCalendar](https://apireference.aspose.cloud/tasks/#/TasksCalendar/PutCalendar) |

### 2. Basic Request Structure

The basic structure of our request will be:

```
PUT https://api.aspose.cloud/v3.0/tasks/{projectFileName}/calendars?calendarUid={calendarUid}
```

Where:
- `{projectFileName}` is the name of your project file stored in the Aspose.Cloud storage
- `{calendarUid}` is the unique identifier of the calendar you want to update

The request body will contain a JSON representation of the updated calendar, similar to the one used when creating a calendar, but with the existing calendar's UID.

### 3. Making the Request with cURL

Let's update an existing calendar using cURL:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/calendars?calendarUid=3" \
     -H "accept: application/json" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -d "{
           \"Name\": \"Modified Evening Shift\",
           \"Uid\": 3,
           \"Days\": [
             {
               \"DayType\": \"Monday\",
               \"DayWorking\": true,
               \"FromDate\": \"2023-01-01T00:00:00\",
               \"ToDate\": \"2023-01-01T23:59:59\",
               \"WorkingTimes\": [
                 {
                   \"FromTime\": \"2023-01-01T14:00:00\",
                   \"ToTime\": \"2023-01-01T22:00:00\"
                 }
               ]
             },
             {
               \"DayType\": \"Friday\",
               \"DayWorking\": true,
               \"FromDate\": \"2023-01-05T00:00:00\",
               \"ToDate\": \"2023-01-05T23:59:59\",
               \"WorkingTimes\": [
                 {
                   \"FromTime\": \"2023-01-05T14:00:00\",
                   \"ToTime\": \"2023-01-05T23:00:00\"
                 }
               ]
             },
             {
               \"DayType\": \"Exception\",
               \"DayWorking\": false,
               \"FromDate\": \"2023-01-16T00:00:00\",
               \"ToDate\": \"2023-01-16T23:59:59\",
               \"WorkingTimes\": []
             }
           ],
           \"IsBaseCalendar\": false,
           \"IsBaselineCalendar\": false
         }"
```

> Note: Replace `YOUR_ACCESS_TOKEN` with an actual access token obtained from Aspose.Cloud authentication API.

In this example, we're:
- Updating the calendar name from "Evening Shift" to "Modified Evening Shift"
- Changing Monday's working hours from two shifts to a single shift (2pm to 10pm)
- Adding a Friday working day configuration
- Adding a non-working exception day on January 16, 2023

### 4. Understanding the Response

If successful, the API returns a simple status response:

```json
{
  "Code": 200,
  "Status": "OK"
}
```

This indicates that the calendar was successfully updated.

### 5. Implementation in Different Languages

Let's implement this functionality in various programming languages.

#### C# Implementation

```csharp
// Tutorial Code Example: Edit Project Calendar with C#
using System;
using System.Collections.Generic;
using Aspose.Tasks.Cloud.Sdk.Api;
using Aspose.Tasks.Cloud.Sdk.Model;

namespace AsposeTasksCloudTutorials
{
    class EditProjectCalendarExample
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
            var calendarUid = 3; // The UID of the calendar to update
            
            try
            {
                // Create updated calendar object
                var calendar = new Calendar
                {
                    Name = "Modified Evening Shift",
                    Uid = calendarUid,
                    Days = new List<WeekDay>(),
                    IsBaseCalendar = false,
                    IsBaselineCalendar = false
                };
                
                // Update Monday configuration (single shift instead of two)
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
                            FromTime = new DateTime(2023, 1, 1, 14, 0, 0),
                            ToTime = new DateTime(2023, 1, 1, 22, 0, 0)
                        }
                    }
                };
                calendar.Days.Add(monday);
                
                // Add Friday configuration
                var friday = new WeekDay
                {
                    DayType = WeekDay.DayTypeEnum.Friday,
                    DayWorking = true,
                    FromDate = new DateTime(2023, 1, 5),
                    ToDate = new DateTime(2023, 1, 5, 23, 59, 59),
                    WorkingTimes = new List<WorkingTime>
                    {
                        new WorkingTime
                        {
                            FromTime = new DateTime(2023, 1, 5, 14, 0, 0),
                            ToTime = new DateTime(2023, 1, 5, 23, 0, 0)
                        }
                    }
                };
                calendar.Days.Add(friday);
                
                // Add exception day (non-working day)
                var exceptionDay = new WeekDay
                {
                    DayType = WeekDay.DayTypeEnum.Exception,
                    DayWorking = false,
                    FromDate = new DateTime(2023, 1, 16),
                    ToDate = new DateTime(2023, 1, 16, 23, 59, 59),
                    WorkingTimes = new List<WorkingTime>()
                };
                calendar.Days.Add(exceptionDay);
                
                // Make API call to update calendar
                var response = tasksApi.PutCalendar(fileName, calendarUid, calendar);
                
                // Process results
                Console.WriteLine("Calendar updated successfully!");
                Console.WriteLine($"Response Code: {response.Code}");
                Console.WriteLine($"Response Status: {response.Status}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error updating calendar: {ex.Message}");
            }
        }
    }
}
```

#### Python Implementation

```python
# Tutorial Code Example: Edit Project Calendar with Python
import os
from datetime import datetime
from asposetaskscloud import ApiClient, TasksApi, Configuration
from asposetaskscloud.models import Calendar, WeekDay, WorkingTime

def edit_project_calendar():
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
    calendar_uid = 3  # The UID of the calendar to update
    
    try:
        # Create updated calendar object
        calendar = Calendar(
            name="Modified Evening Shift",
            uid=calendar_uid,
            days=[],
            is_base_calendar=False,
            is_baseline_calendar=False
        )
        
        # Update Monday configuration (single shift instead of two)
        monday = WeekDay(
            day_type="Monday",
            day_working=True,
            from_date=datetime(2023, 1, 1),
            to_date=datetime(2023, 1, 1, 23, 59, 59),
            working_times=[
                WorkingTime(
                    from_time=datetime(2023, 1, 1, 14, 0, 0),
                    to_time=datetime(2023, 1, 1, 22, 0, 0)
                )
            ]
        )
        calendar.days.append(monday)
        
        # Add Friday configuration
        friday = WeekDay(
            day_type="Friday",
            day_working=True,
            from_date=datetime(2023, 1, 5),
            to_date=datetime(2023, 1, 5, 23, 59, 59),
            working_times=[
                WorkingTime(
                    from_time=datetime(2023, 1, 5, 14, 0, 0),
                    to_time=datetime(2023, 1, 5, 23, 0, 0)
                )
            ]
        )
        calendar.days.append(friday)
        
        # Add exception day (non-working day)
        exception_day = WeekDay(
            day_type="Exception",
            day_working=False,
            from_date=datetime(2023, 1, 16),
            to_date=datetime(2023, 1, 16, 23, 59, 59),
            working_times=[]
        )
        calendar.days.append(exception_day)
        
        # Make API call to update calendar
        response = tasks_api.put_calendar(file_name, calendar_uid, calendar)
        
        # Process and display results
        print("Calendar updated successfully!")
        print(f"Response Code: {response.code}")
        print(f"Response Status: {response.status}")
        
        return response
    except Exception as e:
        print(f"Error updating calendar: {str(e)}")
        return None

# Run the example
if __name__ == "__main__":
    edit_project_calendar()
```

#### Java Implementation

```java
// Tutorial Code Example: Edit Project Calendar with Java
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

public class EditProjectCalendarExample {
    
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
        Integer calendarUid = 3; // The UID of the calendar to update
        
        try {
            // Create updated calendar object
            Calendar calendar = new Calendar();
            calendar.setName("Modified Evening Shift");
            calendar.setUid(calendarUid);
            calendar.setDays(new ArrayList<>());
            calendar.setIsBaseCalendar(false);
            calendar.setIsBaselineCalendar(false);
            
            // Helper method to convert LocalDateTime to Date
            java.util.function.Function<LocalDateTime, Date> toDate = 
                ldt -> Date.from(ldt.atZone(ZoneId.systemDefault()).toInstant());
            
            // Update Monday configuration (single shift instead of two)
            WeekDay monday = new WeekDay();
            monday.setDayType(WeekDay.DayTypeEnum.MONDAY);
            monday.setDayWorking(true);
            monday.setFromDate(toDate.apply(LocalDateTime.of(2023, 1, 1, 0, 0, 0)));
            monday.setToDate(toDate.apply(LocalDateTime.of(2023, 1, 1, 23, 59, 59)));
            
            List<WorkingTime> mondayWorkingTimes = new ArrayList<>();
            
            WorkingTime mondayShift = new WorkingTime();
            mondayShift.setFromTime(toDate.apply(LocalDateTime.of(2023, 1, 1, 14, 0, 0)));
            mondayShift.setToTime(toDate.apply(LocalDateTime.of(2023, 1, 1, 22, 0, 0)));
            mondayWorkingTimes.add(mondayShift);
            
            monday.setWorkingTimes(mondayWorkingTimes);
            calendar.getDays().add(monday);
            
            // Add Friday configuration
            WeekDay friday = new WeekDay();
            friday.setDayType(WeekDay.DayTypeEnum.FRIDAY);
            friday.setDayWorking(true);
            friday.setFromDate(toDate.apply(LocalDateTime.of(2023, 1, 5, 0, 0, 0)));
            friday.setToDate(toDate.apply(LocalDateTime.of(2023, 1, 5, 23, 59, 59)));
            
            List<WorkingTime> fridayWorkingTimes = new ArrayList<>();
            
            WorkingTime fridayShift = new WorkingTime();
            fridayShift.setFromTime(toDate.apply(LocalDateTime.of(2023, 1, 5, 14, 0, 0)));
            fridayShift.setToTime(toDate.apply(LocalDateTime.of(2023, 1, 5, 23, 0, 0)));
            fridayWorkingTimes.add(fridayShift);
            
            friday.setWorkingTimes(fridayWorkingTimes);
            calendar.getDays().add(friday);
            
            // Add exception day (non-working day)
            WeekDay exceptionDay = new WeekDay();
            exceptionDay.setDayType(WeekDay.DayTypeEnum.EXCEPTION);
            exceptionDay.setDayWorking(false);
            exceptionDay.setFromDate(toDate.apply(LocalDateTime.of(2023, 1, 16, 0, 0, 0)));
            exceptionDay.setToDate(toDate.apply(LocalDateTime.of(2023, 1, 16, 23, 59, 59)));
            exceptionDay.setWorkingTimes(new ArrayList<>());
            
            calendar.getDays().add(exceptionDay);
            
            // Make API call to update calendar
            AsposeResponse response = tasksApi.putCalendar(fileName, calendarUid, calendar);
            
            // Process results
            System.out.println("Calendar updated successfully!");
            System.out.println("Response Code: " + response.getCode());
            System.out.println("Response Status: " + response.getStatus());
        } catch (ApiException e) {
            System.err.println("Error updating calendar: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### 6. Try It Yourself

Now it's time to practice editing a calendar:

1. Set up your development environment with the Aspose.Tasks Cloud SDK
2. Upload a project file with an existing calendar to your Aspose.Cloud storage
3. Get the UID of the calendar you want to edit
4. Modify the code examples to update the calendar with your desired changes
5. Run the code and verify that the calendar was updated successfully

> Troubleshooting Tip: When updating a calendar, you must include the correct UID in both the URL parameter and in the calendar object itself. If these don't match, you might get an error or unexpected results.

### 7. Common Calendar Modifications

Here are some common calendar modifications you might want to implement:

1. Change working days: Make previously non-working days working, or vice versa
2. Add exception days: Add specific dates as non-working days (holidays) or special working days
3. Modify working hours: Change the start and end times for working hours
4. Add multiple working periods: Split working time into multiple periods (e.g., morning and afternoon shifts)
5. Rename the calendar: Update the calendar name to better reflect its purpose

Remember that when you update a calendar, you need to specify all the day configurations you want to keep. Any days not included in the update request will be removed from the calendar.

## What You've Learned

In this tutorial, you've learned how to:
- Update an existing calendar in a project file
- Modify working days and their configurations
- Add exception days for holidays or special events
- Change working times for specific days
- Implement this functionality in different programming languages

These skills are essential for maintaining accurate project schedules as project requirements and conditions change over time.

## Further Practice

To reinforce your learning:
1. Retrieve an existing calendar and then update it by changing only specific properties
2. Update a calendar to add a holiday period (multiple consecutive non-working days)
3. Change a standard 9-to-5 calendar to a night shift calendar
4. Update a calendar to include special working times for a project crunch period

## Next Tutorial

Ready to continue learning? Proceed to the final tutorial in this series: [Tutorial: Removing Calendars from a Project](/working-with-calendars/delete-calendar/) to learn how to delete calendars when they're no longer needed.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).