---
title: How to Add Assignments to a Project Tutorial
description: Learn to create new task-resource assignments in your project files using Aspose.Tasks Cloud API in this comprehensive step-by-step tutorial.
url: /working-with-assignments/add-assignment/
weight: 40
---

## Learning Objectives

In this tutorial, you'll learn how to create new assignments connecting tasks to resources in a Microsoft Project file using Aspose.Tasks Cloud API. By the end, you'll be able to:

- Assign resources to tasks programmatically
- Create new assignments with specific allocation units
- Understand the assignment creation process
- Implement this functionality in various programming languages

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account with active credentials
2. Your Client ID and Client Secret from the Aspose Cloud dashboard
3. Basic understanding of REST APIs and project management concepts
4. A development environment set up for your preferred language
5. An MS Project file with existing tasks and resources uploaded to Aspose Cloud Storage
6. Knowledge of task UIDs and resource UIDs in your project file (you can use the GET resources and GET tasks endpoints to retrieve these)

## The Practical Scenario

Imagine you're developing a project management application that allows project managers to assign resources to tasks. When a manager selects a task and a resource, your application needs to create a new assignment linking them together. This tutorial will show you how to implement this core functionality using Aspose.Tasks Cloud API.

## Step 1: Understanding the API Endpoint

The Aspose.Tasks Cloud API provides a dedicated endpoint for creating new assignments:

| API | Type | Description | Resource Link |
| --- | --- | --- | --- |
| /tasks/{name}/assignments | POST | Add a new assignment to a MS Project File | [PostAssignment](https://apireference.aspose.cloud/tasks/#/TasksAssignments/PostAssignment) |

Where:
- `{name}` is the name of your project file stored in the cloud storage
- Query parameters include:
  - `taskUid`: The unique identifier of the task
  - `resourceUid`: The unique identifier of the resource
  - `units`: The allocation units (typically 1.0 for 100% allocation)

## Step 2: Authentication Setup

As with previous tutorials, you first need to authenticate with the Aspose Cloud API:

### cURL Example:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
     -X POST \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "Accept: application/json"
```

This will return a JSON response containing your access token.

## Step 3: Creating a New Assignment

Now that you have your access token, you can make a request to create a new assignment:

### cURL Example:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/assignments?taskUid=1&resourceUid=1&units=1.0" \
     -H "accept: application/json" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace:
- `Home%20move%20plan.mpp` with the name of your project file
- `1` (first occurrence) with the task UID
- `1` (second occurrence) with the resource UID
- `1.0` with the desired allocation units (1.0 means 100% allocation)
- `YOUR_ACCESS_TOKEN` with the token obtained in Step 2

## Step 4: Understanding the Response

If the assignment is created successfully, you will receive a response like this:

```json
{
  "Code": "200",
  "Status": "OK"
}
```

This indicates that the assignment was successfully created in the project file.

## Step 5: Implementing in Your Code

Let's implement this functionality in various programming languages:

### C# Example

```csharp
// Tutorial Code Example: Add Assignment to Project with Aspose.Tasks Cloud
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
                // Specify the project file, task, and resource
                var fileName = "Home move plan.mpp"; // Your project file in cloud storage
                var taskUid = 1;    // The task to assign
                var resourceUid = 1; // The resource to assign to the task
                var units = 1.0;     // 100% allocation
                
                // Create the assignment
                var response = await api.PostAssignmentAsync(fileName, taskUid, resourceUid, units);
                
                // Check the result
                if (response.Code.Equals("200"))
                {
                    Console.WriteLine("Assignment created successfully!");
                    Console.WriteLine($"Task {taskUid} has been assigned to Resource {resourceUid} with {units * 100}% allocation.");
                    
                    // Optional: Retrieve the newly created assignment to confirm
                    Console.WriteLine("\nRetrieving assignments to verify...");
                    var assignments = await api.GetAssignmentsAsync(fileName);
                    foreach (var item in assignments.Assignments.AssignmentItem)
                    {
                        if (item.TaskUid == taskUid && item.ResourceUid == resourceUid)
                        {
                            Console.WriteLine($"Verified: Assignment {item.Uid} found connecting Task {item.TaskUid} and Resource {item.ResourceUid}");
                        }
                    }
                }
                else
                {
                    Console.WriteLine($"Error creating assignment. Status: {response.Status}");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error creating assignment: {ex.Message}");
                Console.WriteLine($"Exception details: {ex}");
            }
        }
    }
}
```

### Python Example

```python
# Tutorial Code Example: Add Assignment to Project with Aspose.Tasks Cloud
import aspose_tasks_cloud
from aspose_tasks_cloud.apis.tasks_api import TasksApi
from aspose_tasks_cloud.api_client import ApiClient
from aspose_tasks_cloud.configuration import Configuration

# Setup API client with your Aspose Cloud credentials
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
api_client = ApiClient(configuration)
tasks_api = TasksApi(api_client)

try:
    # Specify the project file, task, and resource
    file_name = "Home move plan.mpp"  # Your project file in cloud storage
    task_uid = 1       # The task to assign
    resource_uid = 1   # The resource to assign to the task
    units = 1.0        # 100% allocation
    
    # Create the assignment
    response = tasks_api.post_assignment(file_name, task_uid=task_uid, resource_uid=resource_uid, units=units)
    
    # Check the result
    if response.code == "200":
        print("Assignment created successfully!")
        print(f"Task {task_uid} has been assigned to Resource {resource_uid} with {units * 100}% allocation.")
        
        # Optional: Retrieve the assignments to verify
        print("\nRetrieving assignments to verify...")
        assignments = tasks_api.get_assignments(file_name)
        found = False
        for item in assignments.assignments.assignment_item:
            if item.task_uid == task_uid and item.resource_uid == resource_uid:
                print(f"Verified: Assignment {item.uid} found connecting Task {item.task_uid} and Resource {item.resource_uid}")
                found = True
        
        if not found:
            print("Warning: Assignment was created but could not be verified in the assignment list.")
    else:
        print(f"Error creating assignment. Status: {response.status}")
        
except Exception as e:
    print(f"Error creating assignment: {str(e)}")
    print(f"Exception details: {e}")
```

## Try it Yourself

Now it's your turn to practice! Follow these steps:

1. First, use the appropriate endpoints to retrieve the list of tasks and resources in your project file
2. Identify a task and a resource that you want to connect with an assignment
3. Modify the code examples with your credentials, project filename, task UID, and resource UID
4. Run the code to create the assignment
5. Verify that the assignment was created by retrieving the list of assignments

## Troubleshooting Tips

- Invalid Task or Resource UID: Ensure you're using valid UIDs that exist in your project file
- Permission Errors: Check if your access token is still valid or if you need to regenerate it
- Resource Already Assigned: If the resource is already assigned to the task, you may receive an error
- File Not Found: Verify that your project file exists in the cloud storage and is correctly specified
- Invalid Units Value: The units value should typically be between 0 and 1 (e.g., 0.5 for 50% allocation)

## What You've Learned

Congratulations! In this tutorial, you've learned how to:

1. Create new assignments connecting tasks to resources
2. Specify allocation units for assignments
3. Verify that assignments were successfully created
4. Handle potential errors during the assignment creation process

Creating assignments is a fundamental feature of project management applications, allowing resources to be allocated to tasks and work to be distributed effectively.

## Further Practice

To reinforce your learning:

1. Create a function that allows you to assign multiple resources to a single task
2. Implement a batch assignment feature that creates multiple assignments at once
3. Add validation to check if a resource is available before creating an assignment
4. Create a user interface that displays tasks and resources and allows users to create assignments
5. Experiment with different allocation units (e.g., part-time allocation)

## Next Steps

Now that you know how to create basic assignments, learn how to [Add Assignments with Cost Information](/working-with-assignments/add-assignment-with-cost/) in the next tutorial.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [Live Demo](https://products.aspose.app/tasks/family)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Blog](https://blog.aspose.cloud/category/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).
