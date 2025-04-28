---
title: How to Add a New Extended Attribute Definition to a Project in Aspose.Tasks Cloud Tutorial
description: Learn how to create and add custom extended attribute definitions to MS Project files using Aspose.Tasks Cloud API in this comprehensive tutorial for developers
url: /working-with-outline-codes-and-extended-attributes/add-extended-attribute/
weight: 80
---

## Learning to Add a New Extended Attribute Definition to a Project

In this tutorial, you'll learn how to add a new extended attribute definition to a MS Project file using the Aspose.Tasks Cloud API. Creating custom fields allows you to extend MS Project's capabilities to track specific information relevant to your organization's needs, which is essential for developing comprehensive project management solutions.

### Learning Objectives

By the end of this tutorial, you will be able to:
- Define a new extended attribute with custom properties
- Create value lists for lookup fields
- Add the extended attribute to an existing project
- Configure calculation types and formulas
- Implement the functionality in your preferred programming language
- Verify successful attribute creation

## Prerequisites

Before starting this tutorial, make sure you have:

1. Completed the previous tutorials on working with extended attributes
2. An [Aspose Cloud account](https://dashboard.aspose.cloud/)
3. Valid Aspose.Tasks Cloud API credentials (Client ID and Client Secret)
4. A MS Project file stored in your Aspose Cloud Storage
5. Good understanding of MS Project's custom field concepts

## Understanding Extended Attribute Creation

Adding a new extended attribute to a project involves several key considerations:

- Choosing the appropriate field type (Text, Number, Date, etc.)
- Deciding whether to restrict values to a predefined list
- Setting up calculation behavior (if needed)
- Defining default values
- Creating meaningful aliases for better usability

Let's explore how to implement all these aspects using the Aspose.Tasks Cloud API.

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

### Step 2: Define the Extended Attribute Properties

Before making the API call, we need to define the properties of our new extended attribute. Let's create a Text field with a predefined list of values:

#### Extended Attribute JSON Structure

```json
{
  "fieldId": "string",
  "fieldName": "Text3",
  "cfType": "Text",
  "guid": "string",
  "elementType": "Task",
  "maxMultiValues": 0,
  "userDef": true,
  "alias": "New Field",
  "secondaryPid": "string",
  "autoRollDown": true,
  "defaultGuid": "string",
  "lookupUid": "string",
  "phoneticsAlias": "string",
  "rollupType": "Null",
  "calculationType": "Lookup",
  "formula": "string",
  "restrictValues": true,
  "valuelistSortOrder": 0,
  "appendNewValues": true,
  "default": "string",
  "valueList": [
    {
      "id": 111,
      "val": "Internal",
      "dateTimeValue": "2020-09-19T10:38:41.713Z",
      "durationValue": 0,
      "description": "descr1",
      "phonetic": "string"
    },
    {
      "id": 112,
      "val": "External",
      "dateTimeValue": "2020-09-19T10:38:41.713Z",
      "durationValue": 0,
      "description": "descr2",
      "phonetic": "string"
    }
  ],
  "secondaryGuid": "string"
}
```

### Step 3: Add the Extended Attribute to the Project

Now let's add the new extended attribute to a project file:

#### Using cURL

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/tasks/NewProductDev.mpp/extendedAttributes" \
     -H "accept: application/json" \
     -H "Content-Type: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -d "{ \"fieldId\": \"string\", \"fieldName\": \"Text3\", \"cfType\": \"Text\", \"guid\": \"string\", \"elementType\": \"Task\", \"maxMultiValues\": 0, \"userDef\": true, \"alias\": \"New Field\", \"secondaryPid\": \"string\", \"autoRollDown\": true, \"defaultGuid\": \"string\", \"lookupUid\": \"string\", \"phoneticsAlias\": \"string\", \"rollupType\": \"Null\", \"calculationType\": \"Lookup\", \"formula\": \"string\", \"restrictValues\": true, \"valuelistSortOrder\": 0, \"appendNewValues\": true, \"default\": \"string\", \"valueList\": [ { \"id\": 111, \"val\": \"Internal\", \"dateTimeValue\": \"2020-09-19T10:38:41.713Z\", \"durationValue\": 0, \"description\": \"descr1\", \"phonetic\": \"string\" }, { \"id\": 112, \"val\": \"External\", \"dateTimeValue\": \"2020-09-19T10:38:41.713Z\", \"durationValue\": 0, \"description\": \"descr2\", \"phonetic\": \"string\" } ], \"secondaryGuid\": \"string\"}"
```

Replace `YOUR_ACCESS_TOKEN` with the token received in Step 1.

### Step 4: Implement in Python

Let's see how to implement this in Python:

```python
# Tutorial Code Example: Adding a New Extended Attribute in Python
import requests
import json
import base64

def add_extended_attribute():
    # Step 1: Authentication
    client_id = 'YOUR_CLIENT_ID'
    client_secret = 'YOUR_CLIENT_SECRET'
    
    # Get JWT token
    auth_url = "https://api.aspose.cloud/connect/token"
    auth_data = {
        "grant_type": "client_credentials",
        "client_id": client_id,
        "client_secret": client_secret
    }
    auth_headers = {
        "Content-Type": "application/x-www-form-urlencoded",
        "Accept": "application/json"
    }
    
    auth_response = requests.post(auth_url, data=auth_data, headers=auth_headers)
    if auth_response.status_code != 200:
        print(f"Authentication failed: {auth_response.text}")
        return
    
    token = auth_response.json().get("access_token")
    
    # Step 2: Define the new extended attribute
    ext_attribute = {
        "fieldId": "",  # Auto-generated by the server
        "fieldName": "Text3",  # Standard MS Project field name
        "cfType": "Text",  # Field type
        "guid": "",  # Auto-generated by the server
        "elementType": "Task",  # Apply to tasks
        "maxMultiValues": 0,
        "userDef": True,  # User-defined field
        "alias": "Project Phase",  # User-friendly name
        "autoRollDown": True,  # Auto-copy to subtasks
        "calculationType": "Lookup",  # Use a lookup table
        "restrictValues": True,  # Only allow values from the list
        "valuelistSortOrder": 0,  # Don't sort the values
        "appendNewValues": True,  # Allow adding new values
        "valueList": [
            {
                "id": 1,
                "val": "Planning",
                "description": "Initial planning phase"
            },
            {
                "id": 2,
                "val": "Design",
                "description": "Design and architecture phase"
            },
            {
                "id": 3,
                "val": "Development",
                "description": "Implementation phase"
            },
            {
                "id": 4,
                "val": "Testing",
                "description": "QA and validation phase"
            },
            {
                "id": 5,
                "val": "Deployment",
                "description": "Release and deployment phase"
            }
        ]
    }
    
    # Step 3: Add the extended attribute to the project
    file_name = "NewProductDev.mpp"
    url = f"https://api.aspose.cloud/v3.0/tasks/{file_name}/extendedAttributes"
    
    headers = {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Authorization": f"Bearer {token}"
    }
    
    response = requests.put(url, data=json.dumps(ext_attribute), headers=headers)
    
    if response.status_code == 200:
        result = response.json()
        print("Extended attribute added successfully!")
        print(f"Status: {result.get('status')}")
        
        # Print the details of the added attribute
        attribute = result.get('extendedAttribute', {})
        print(f"Index: {attribute.get('index')}")
        print(f"Field Name: {attribute.get('fieldName')}")
        print(f"Alias: {attribute.get('alias')}")
        print(f"Field ID: {attribute.get('fieldId')}")
    else:
        print(f"Failed to add extended attribute: {response.text}")

# Run the function
if __name__ == "__main__":
    add_extended_attribute()
```

### Step 5: Understanding the Response

After adding the extended attribute, the API returns a response with information about the newly added attribute:

```json
{
  "code": 0,
  "status": "OK",
  "extendedAttribute": {
    "link": {
      "href": "https://api.aspose.cloud/v3.0/tasks/NewProductDev.mpp/extendedAttributes/1",
      "rel": "self",
      "type": "text/json",
      "title": "Extended attribute 1"
    },
    "index": 1,
    "fieldName": "Text3",
    "alias": "Project Phase",
    "fieldId": "188743733"
  }
}
```

### Step 6: Creating Different Types of Extended Attributes

Let's look at examples of how to create different types of extended attributes:

#### 1. Number Field with Formula

```python
number_attribute = {
    "fieldName": "Number1",
    "cfType": "Number",
    "elementType": "Task",
    "userDef": True,
    "alias": "Cost Variance %",
    "calculationType": "Formula",
    "formula": "[Cost Variance] / [Cost]"
}
```

#### 2. Date Field with Default Value

```python
date_attribute = {
    "fieldName": "Date1",
    "cfType": "Date",
    "elementType": "Task",
    "userDef": True,
    "alias": "Quality Review Date",
    "calculationType": "None",
    "default": "2023-12-31T00:00:00Z"
}
```

#### 3. Flag (Yes/No) Field

```python
flag_attribute = {
    "fieldName": "Flag1",
    "cfType": "Flag",
    "elementType": "Task",
    "userDef": True,
    "alias": "Requires Approval",
    "calculationType": "None"
}
```

### Try It Yourself

Now that you understand how to add new extended attributes, try implementing this feature in your own application:

1. Set up authentication with your Aspose Cloud credentials
2. Define the properties for your custom extended attribute
3. Make the API call to add the extended attribute to your project
4. Verify the addition by retrieving the updated list of extended attributes
5. Create a simple user interface to configure and add custom fields

## Troubleshooting Tips

- Invalid Field Name: Make sure to use standard MS Project field names (Text1-30, Number1-20, etc.)
- Field Already In Use: Check if the field is already customized in the project
- Value List Issues: Ensure IDs in the value list are unique
- Calculation Errors: Test formulas to make sure they reference valid fields
- File Access Issues: Make sure the file isn't locked or in use

## What You've Learned

In this tutorial, you've learned:
- How to define and add a new extended attribute to a project
- How to create value lists for lookup fields
- How to set up different types of extended attributes
- How to implement extended attribute creation in Python
- Best practices for configuring custom fields

## Further Practice

To strengthen your understanding, try these exercises:
1. Create a set of related custom fields for a specific tracking need
2. Implement a wizard-like interface for creating custom fields
3. Build validation to ensure formulas are correctly formatted
4. Create a function that copies custom field definitions between projects
5. Develop a feature to assign values to the new custom field for tasks

## Next Steps

Congratulations! You've completed all the tutorials in the Aspose.Tasks Cloud API series on working with Outline Codes and Extended Attributes. You now have the knowledge and skills to:

1. Retrieve outline codes and extended attributes information
2. Work with individual outline codes and attributes
3. Delete unnecessary outline codes and attributes
4. Generate reports in PDF format
5. Create new custom fields to extend project functionality

Consider exploring other Aspose.Tasks Cloud API features or developing a complete project management solution using the skills you've learned.

## Useful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).
