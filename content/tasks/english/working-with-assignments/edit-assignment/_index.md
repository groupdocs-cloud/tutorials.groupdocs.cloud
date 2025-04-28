---
title: How to Edit Resource Assignments Tutorial
description: Learn to modify existing assignments in project files using Aspose.Tasks Cloud API in this comprehensive step-by-step tutorial for developers.
url: /working-with-assignments/edit-assignment/
weight: 60
---

## Learning Objectives

In this tutorial, you'll learn how to modify existing assignments in a Microsoft Project file using Aspose.Tasks Cloud API. By the end, you'll be able to:

- Update assignment properties such as work, costs, and dates
- Modify resource allocation units for existing assignments
- Use different update modes to control recalculation behavior
- Implement assignment editing functionality in your applications

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account with active credentials
2. Your Client ID and Client Secret from the Aspose Cloud dashboard
3. Strong understanding of REST APIs and project management concepts
4. A development environment set up for your preferred language
5. An MS Project file with existing assignments uploaded to Aspose Cloud Storage
6. Knowledge of assignment UIDs in your project file (from previous tutorials)
7. Completed the earlier tutorials on retrieving and creating assignments

## The Practical Scenario

Imagine you're developing a project management application that allows managers to adjust resource assignments as the project progresses. A manager might need to:
- Change the allocation of a resource from 50% to 100%
- Update the work estimates for an assignment
- Adjust the cost of an assignment
- Change the start or finish dates

This tutorial will show you how to implement these common assignment editing scenarios using Aspose.Tasks Cloud API.

## Step 1: Understanding the API Endpoint

The Aspose.Tasks Cloud API provides a dedicated endpoint for updating existing assignments:

| API | Type | Description | Resource Link |
| --- | --- | --- | --- |
| /tasks/{name}/assignments/{assignmentUid} | PUT | Update a Resource Assignment in a MS Project file | [PutAssignment](https://apireference.aspose.cloud/tasks/#/TasksAssignments/PutAssignment) |

Where:
- `{name}` is the name of your project file stored in the cloud storage
- `{assignmentUid}` is the unique identifier of the assignment you want to modify
- Query parameters include:
  - `mode`: The calculation mode for the project (None=0, Automatic=1, Manual=2)
  - `recalculate`: Whether to recalculate the project (true/false)

## Step 2: Authentication Setup

As with previous tutorials, you first need to authenticate with the Aspose Cloud API:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
     -X POST \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "Accept: application/json"
```

This will return a JSON response containing your access token.

## Step 3: Understanding Assignment Properties

Before updating an assignment, it's important to understand the key properties that can be modified:

- `Units`: The percentage of a resource's time allocated to a task (e.g., 1.0 = 100%)
- `Work`: The amount of work scheduled for the assignment
- `Cost`: The cost associated with the assignment
- `Start` and `Finish`: The start and finish dates of the assignment
- `PercentWorkComplete`: The percentage of work completed
- Various actual values: `ActualStart`, `ActualFinish`, `ActualWork`, `ActualCost`, etc.

Not all properties need to be updated at once. You can selectively update only the properties you want to change.

## Step 4: Updating an Assignment

Let's update an existing assignment with new values. First, we need to create a JSON payload with the properties we want to update, then send it to the API:

### cURL Example:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/assignments/1?mode=1&recalculate=true" \
     -H "accept: application/json" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d "{ 
         \"Uid\": 1,
         \"TaskUid\": 1,
         \"ResourceUid\": 1,
         \"Units\": 0.5,
         \"Work\": \"PT16H0M0S\",
         \"Start\": \"2023-07-15T08:00:00\",
         \"Finish\": \"2023-07-16T17:00:00\",
         \"PercentWorkComplete\": 50
     }"
```

Replace:
- `Home%20move%20plan.mpp` with the name of your project file
- `1` (in the URL) with the assignment UID you want to update
- `YOUR_ACCESS_TOKEN` with the token obtained in Step 2
- Update the JSON payload with the specific properties you want to modify

In this example, we're:
- Setting resource allocation to 50% (`Units`: 0.5)
- Setting the work to 16 hours (`Work`: "PT16H0M0S")
- Updating the start and finish dates
- Setting the percentage of work completed to 50%

## Step 5: Understanding the Response

If the assignment is updated successfully, you will receive a detailed response containing the updated assignment information:

```json
{
  "code": 0,
  "status": "OK",
  "assignment": {
    "taskUid": 1,
    "resourceUid": 1,
    "uid": 1,
    "percentWorkComplete": 50,
    "actualCost": 0,
    "actualFinish": null,
    "actualStart": null,
    "actualWork": "PT0H0M0S",
    "cost": 800,
    "finish": "2023-07-16T17:00:00",
    "start": "2023-07-15T08:00:00",
    "work": "PT16H0M0S",
    "units": 0.5,
    // ...other properties...
  }
}
```

The response includes all properties of the assignment, not just the ones you updated.

## Step 6: Implementing in Your Code

Let's implement this functionality in various programming languages:

### C# Example

```csharp
// Tutorial Code Example: Edit Resource Assignment with Aspose.Tasks Cloud
using System;
using System.Threading.Tasks;
using Aspose.Tasks.Cloud.Sdk.Api;
using Aspose.Tasks.Cloud.Sdk.Model;

namespace AsposeTasksCloudTutorials
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // Setup API client with your Aspose Cloud credentials
            var config = new Configuration
            {
                ClientId = "YOUR_CLIENT_ID",
                ClientSecret = "YOUR_CLIENT_SECRET"
            };
            
            var api = new TasksApi(config);
            
            try
            {
                // Specify the project file and assignment to update
                var fileName = "Home move plan.mpp";   // Your project file in cloud storage
                var assignmentUid = 1;                // The assignment UID to update
                var calculationMode = CalculationMode.Automatic; // Recalculate automatically
                var recalculate = true;               // Trigger recalculation
                
                // Create assignment object with properties to update
                var assignment = new AssignmentDto
                {
                    Uid = assignmentUid,
                    TaskUid = 1,                      // Must match existing assignment
                    ResourceUid = 1,                  // Must match existing assignment
                    Units = 0.5,                      // 50% allocation
                    Work = "PT16H0M0S",               // 16 hours of work
                    Start = DateTime.Parse("2023-07-15T08:00:00"),
                    Finish = DateTime.Parse("2023-07-16T17:00:00"),
                    PercentWorkComplete = 50          // 50% complete
                };
                
                // Update the assignment
                var response = await api.PutAssignmentAsync(fileName, assignmentUid, assignment, calculationMode, recalculate);
                
                // Check the result
                Console.WriteLine("Assignment updated successfully!");
                var updatedAssignment = response.Assignment;
                
                // Display updated information
                Console.WriteLine($"Assignment UID: {updatedAssignment.Uid}");
                Console.WriteLine($"Task: {updatedAssignment.TaskUid}, Resource: {updatedAssignment.ResourceUid}");
                Console.WriteLine($"Units: {updatedAssignment.Units * 100}%");
                Console.WriteLine($"Work: {updatedAssignment.Work}");
                Console.WriteLine($"Start: {updatedAssignment.Start}");
                Console.WriteLine($"Finish: {updatedAssignment.Finish}");
                Console.WriteLine($"Percent Complete: {updatedAssignment.PercentWorkComplete}%");
                Console.WriteLine($"Cost: {updatedAssignment.Cost}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error updating assignment: {ex.Message}");
                Console.WriteLine($"Exception details: {ex}");
            }
        }
    }
}
```

### Python Example

```python
# Tutorial Code Example: Edit Resource Assignment with Aspose.Tasks Cloud
import aspose_tasks_cloud
from aspose_tasks_cloud.apis.tasks_api import TasksApi
from aspose_tasks_cloud.models.assignment_dto import AssignmentDto
from aspose_tasks_cloud.models.calculation_mode import CalculationMode
from aspose_tasks_cloud.api_client import ApiClient
from aspose_tasks_cloud.configuration import Configuration
from datetime import datetime

# Setup API client with your Aspose Cloud credentials
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
api_client = ApiClient(configuration)
tasks_api = TasksApi(api_client)

try:
    # Specify the project file and assignment to update
    file_name = "Home move plan.mpp"  # Your project file in cloud storage
    assignment_uid = 1                # The assignment UID to update
    calculation_mode = "Automatic"    # Recalculate automatically
    recalculate = True                # Trigger recalculation
    
    # Create assignment object with properties to update
    assignment = AssignmentDto(
        uid=assignment_uid,
        task_uid=1,                   # Must match existing assignment
        resource_uid=1,               # Must match existing assignment
        units=0.5,                    # 50% allocation
        work="PT16H0M0S",             # 16 hours of work
        start=datetime.fromisoformat("2023-07-15T08:00:00"),
        finish=datetime.fromisoformat("2023-07-16T17:00:00"),
        percent_work_complete=50      # 50% complete
    )
    
    # Update the assignment
    response = tasks_api.put_assignment(
        file_name, 
        assignment_uid, 
        assignment, 
        mode=calculation_mode, 
        recalculate=recalculate
    )
    
    # Check the result
    print("Assignment updated successfully!")
    updated_assignment = response.assignment
    
    # Display updated information
    print(f"Assignment UID: {updated_assignment.uid}")
    print(f"Task: {updated_assignment.task_uid}, Resource: {updated_assignment.resource_uid}")
    print(f"Units: {updated_assignment.units * 100}%")
    print(f"Work: {updated_assignment.work}")
    print(f"Start: {updated_assignment.start}")
    print(f"Finish: {updated_assignment.finish}")
    print(f"Percent Complete: {updated_assignment.percent_work_complete}%")
    print(f"Cost: {updated_assignment.cost}")
    
except Exception as e:
    print(f"Error updating assignment: {str(e)}")
    print(f"Exception details: {e}")
```

## Understanding Calculation Modes

When updating assignments, the `mode` parameter controls how project calculations are performed:

1. None (0): No calculations are performed. This is useful for batch updates where you want to update multiple items and then trigger a single recalculation at the end.

2. Automatic (1): Project is automatically recalculated after the update. This ensures that all dependent values (like project duration, costs, etc.) are immediately updated.

3. Manual (2): Project calculations are done manually. Use this when you want more control over when recalculations happen.

The `recalculate` parameter (true/false) determines whether the project should be recalculated regardless of the calculation mode. Setting it to `true` ensures that all project values are up-to-date after your update.

## Try it Yourself

Now it's your turn to practice! Follow these steps:

1. First, retrieve a list of assignments in your project to identify one you want to modify
2. Note the assignment UID, task UID, and resource UID
3. Determine which properties you want to update (units, work, dates, completion, etc.)
4. Modify the code examples with your credentials, project filename, assignment details, and desired property changes
5. Run the code to update the assignment
6. Retrieve the assignment again to verify that your changes were applied correctly

## Advanced Scenarios

Here are some common scenarios for assignment updates:

### Scenario 1: Updating Progress

To update assignment progress without changing other properties:

```python
assignment = AssignmentDto(
    uid=assignment_uid,
    task_uid=task_uid,
    resource_uid=resource_uid,
    percent_work_complete=75,  # 75% complete
    actual_work="PT12H0M0S"    # 12 hours of actual work completed
)
```

### Scenario 2: Changing Resource Allocation

To change how much a resource is allocated to a task:

```python
assignment = AssignmentDto(
    uid=assignment_uid,
    task_uid=task_uid,
    resource_uid=resource_uid,
    units=0.25  # 25% allocation (part-time)
)
```

### Scenario 3: Rescheduling an Assignment

To change the dates of an assignment:

```python
assignment = AssignmentDto(
    uid=assignment_uid,
    task_uid=task_uid,
    resource_uid=resource_uid,
    start=datetime.fromisoformat("2023-08-01T09:00:00"),
    finish=datetime.fromisoformat("2023-08-05T17:00:00")
)
```

## Troubleshooting Tips

- Assignment ID Not Found: Ensure the assignment UID exists in your project
- Inconsistent Task/Resource IDs: The task UID and resource UID must match the original assignment
- Date Constraint Violations: Assignment dates must respect any constraints set on the task
- Calculation Errors: If the update violates project constraints, you might get calculation errors
- Duration Format Issues: When specifying work or duration values, use ISO8601 format (e.g., "PT8H30M0S" for 8 hours and 30 minutes)
## What You've Learned

Congratulations! In this tutorial, you've learned how to:

1. Update existing assignments in a project file using Aspose.Tasks Cloud API
2. Modify key assignment properties such as units, work, dates, and completion percentage
3. Use different calculation modes to control project recalculation behavior
4. Implement assignment editing functionality in your applications
5. Handle various common assignment update scenarios

These skills enable you to build sophisticated project management applications that can adapt to changing project requirements and track actual progress against plans.

## Further Practice

To reinforce your learning:

1. Create a function that updates multiple assignments in a single operation
2. Build a progress tracking feature that updates actual work and completion percentage
3. Implement a resource reallocation tool that adjusts units across multiple assignments
4. Create a dashboard that shows before and after states of assignments being modified
5. Experiment with different calculation modes to understand their impact on performance and accuracy

## Next Steps

You've learned how to create and modify assignments. The final step in managing assignments is removing them when they're no longer needed. Continue to the next tutorial to learn [How to Delete Assignments from a Project](/working-with-assignments/delete-assignment/).

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).