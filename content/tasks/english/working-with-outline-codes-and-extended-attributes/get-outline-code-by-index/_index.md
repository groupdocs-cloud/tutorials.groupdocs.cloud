---
title: How to Get a Specific Outline Code by Index in Aspose.Tasks Cloud Tutorial
description: Learn how to access individual outline codes by index from MS Project files using Aspose.Tasks Cloud API in this step-by-step tutorial for developers
url: /working-with-outline-codes-and-extended-attributes/get-outline-code-by-index/
weight: 20
---

## Learning to Retrieve a Specific Outline Code by Index

Difficulty Level: Beginner  
Estimated Completion Time: 20 minutes

In this tutorial, you'll learn how to retrieve a specific outline code by its index from MS Project files using the Aspose.Tasks Cloud API. This skill allows you to access detailed information about individual outline codes, which is essential for applications that need to work with project categorization and organization structures.

### Learning Objectives

By the end of this tutorial, you will be able to:
- Make API calls to retrieve a specific outline code by index
- Understand the structure and properties of individual outline codes
- Implement the functionality in your preferred programming language
- Process and utilize the detailed outline code information

## Prerequisites

Before starting this tutorial, make sure you have:

1. Completed the previous tutorial on [Getting Outline Codes Information](/working-with-outline-codes-and-extended-attributes/get-outline-codes/)
2. An [Aspose Cloud account](https://dashboard.aspose.cloud/)
3. Valid Aspose.Tasks Cloud API credentials (Client ID and Client Secret)
4. Appropriate SDK installed for your programming language

## Understanding Outline Code Structure

An individual outline code contains detailed information such as:
- GUID
- Field ID and name
- Values and their hierarchical structure
- Masks defining the outline code format
- Various properties controlling behavior

Accessing this information is crucial for applications that need to interpret or modify project organization structures.

## Tutorial Steps

### Step 1: Set Up Authentication

First, let's set up authentication to access the Aspose.Tasks Cloud API:

#### Using cURL

```bash
# First, get the JWT token
curl -v "https://api.aspose.cloud/connect/token" -X POST -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json"

# Store the received access_token for subsequent requests
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual credentials.

### Step 2: Make the API Call to Get a Specific Outline Code

Now let's retrieve a specific outline code by its index:

#### Using cURL

```bash
curl -X GET "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/outlineCodes/1" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with the token received in Step 1.

### Step 3: Implement in Your Preferred Language

Let's see how to implement this in various programming languages:

#### Python Example

```python
# Tutorial Code Example: Getting Specific Outline Code in Python
import aspose_tasks_cloud
from aspose_tasks_cloud.apis.tasks_api import TasksApi
from aspose_tasks_cloud.api_client import ApiClient
from aspose_tasks_cloud.configuration import Configuration

def get_outline_code_by_index():
    # Configure API client
    configuration = Configuration(
        client_id='YOUR_CLIENT_ID',
        client_secret='YOUR_CLIENT_SECRET'
    )
    
    # Initialize API client
    api_client = ApiClient(configuration)
    
    # Create TasksApi instance
    tasks_api = TasksApi(api_client)
    
    try:
        # Specify the project file name
        file_name = "Home move plan.mpp"
        
        # Specify the outline code index to retrieve
        index = 1
        
        # Call the API to get the outline code
        response = tasks_api.get_outline_code_by_index(file_name, index)
        
        # Process and display the result
        print(f"Status: {response.status}")
        
        # Access outline code details
        outline_code = response.outline_code
        print(f"Field ID: {outline_code.field_id}")
        print(f"Field Name: {outline_code.field_name}")
        print(f"Alias: {outline_code.alias}")
        
        # Display values if available
        if outline_code.values and len(outline_code.values) > 0:
            print("\nValues:")
            for value in outline_code.values:
                print(f"  - Value ID: {value.value_id}")
                print(f"    Value: {value.value}")
                print(f"    Description: {value.description}")
                
        # Display masks if available
        if outline_code.masks and len(outline_code.masks) > 0:
            print("\nMasks:")
            for mask in outline_code.masks:
                print(f"  - Level: {mask.level}")
                print(f"    Type: {mask.type}")
                print(f"    Length: {mask.length}")
                print(f"    Separator: {mask.separator}")
        
    except Exception as e:
        print(f"Error: {str(e)}")

# Call the function to execute the example
get_outline_code_by_index()
```

### Step 4: Process and Use the Response

The API response provides detailed information about the specified outline code. Let's examine a sample response:

```json
{
  "code": 0,
  "status": "OK",
  "outlineCode": {
    "guid": "11111111-1111-1111-1111-111111111111",
    "fieldId": "188743737",
    "fieldName": "Location",
    "alias": "Location Codes",
    "phoneticAlias": "",
    "values": [
      {
        "valueId": 1,
        "fieldGuid": "11111111-1111-1111-1111-111111111111",
        "type": "Text",
        "parentValueId": 0,
        "value": "North",
        "description": "Northern Region",
        "isCollapsed": false
      },
      {
        "valueId": 2,
        "fieldGuid": "11111111-1111-1111-1111-111111111111",
        "type": "Text",
        "parentValueId": 0,
        "value": "South",
        "description": "Southern Region",
        "isCollapsed": false
      }
    ],
    "enterprise": false,
    "enterpriseOutlineCodeAlias": 0,
    "resourceSubstitutionEnabled": false,
    "leafOnly": false,
    "allLevelsRequired": false,
    "onlyTableValuesAllowed": true,
    "masks": [
      {
        "level": 1,
        "type": "Characters",
        "length": 1,
        "separator": "."
      }
    ],
    "showIndent": true
  }
}
```

### Step 5: Understanding Key Properties

Let's understand some of the key properties in the response:

1. guid: Unique identifier for the outline code
2. fieldId: The internal ID used by MS Project
3. fieldName: The display name in MS Project
4. alias: A user-friendly name for the outline code
5. values: Collection of possible values for this outline code
6. masks: Format specifications for each level of the outline code
7. onlyTableValuesAllowed: Whether users can only select from predefined values

### Try It Yourself

Now that you understand how to access a specific outline code, try implementing this feature in your own application:

1. Set up authentication with your Aspose Cloud credentials
2. Retrieve all outline codes using the previous tutorial's method
3. Select a specific outline code index
4. Make the API call to retrieve detailed information for that index
5. Process and display the detailed information

## Troubleshooting Tips

- 404 Not Found: Verify that the specified file exists in your storage
- Invalid Index: Ensure that the outline code index you're requesting actually exists in the project
- Access Issues: Double-check your authentication credentials and token

## What You've Learned

In this tutorial, you've learned:
- How to retrieve a specific outline code by its index
- The structure and properties of individual outline codes
- How to implement this functionality in Python
- How to process and utilize the detailed outline code information
- The significance of various properties in outline code definition

## Further Practice

To strengthen your understanding, try these exercises:
1. Create a function that retrieves all outline codes and then gets detailed information for each one
2. Build a simple tool that displays outline code hierarchy in a tree-like structure
3. Compare outline codes between different project files to identify patterns

## Next Tutorial

Ready to learn more? Continue to the next tutorial: [How to Delete a Project Outline Code](/working-with-outline-codes-and-extended-attributes/delete-outline-code/) to learn how to remove outline codes from your projects.

## Useful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).
