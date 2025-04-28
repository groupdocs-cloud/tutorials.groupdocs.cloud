---
title: How to Get Extended Attributes Information in Aspose.Tasks Cloud Tutorial
description: Learn how to retrieve extended attributes information from MS Project files using Aspose.Tasks Cloud API in this comprehensive tutorial for developers
url: /working-with-outline-codes-and-extended-attributes/get-extended-attributes/
weight: 50
---

## Learning to Retrieve Extended Attributes Information

In this tutorial, you'll learn how to retrieve information about extended attributes from MS Project files using the Aspose.Tasks Cloud API. Extended attributes are custom fields that enhance the standard set of fields in MS Project, allowing project managers to track additional information. Understanding how to access these custom fields programmatically is essential for building comprehensive project management applications.

### Learning Objectives

By the end of this tutorial, you will be able to:
- Understand what extended attributes are in MS Project
- Make API calls to retrieve extended attributes information
- Implement code examples in your preferred programming language
- Interpret and use the returned extended attributes data
- Work with both enterprise and local custom fields

## Prerequisites

Before starting this tutorial, make sure you have:

1. An [Aspose Cloud account](https://dashboard.aspose.cloud/)
2. Valid Aspose.Tasks Cloud API credentials (Client ID and Client Secret)
3. A MS Project file with extended attributes stored in your Aspose Cloud Storage
4. Basic understanding of RESTful APIs and your programming language of choice

## Understanding Extended Attributes

Extended attributes in MS Project are custom fields that can be added to tasks, resources, or the project itself. They allow project managers to track specific information that is not covered by the standard MS Project fields. For example:

- Custom text fields for tracking additional notes or categories
- Custom number fields for tracking specific metrics
- Custom date fields for tracking important milestones
- Custom flag fields for tracking yes/no conditions
- Custom formula fields for calculated values

These custom fields enable organizations to tailor MS Project to their specific needs and business processes.

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

### Step 2: Make the API Call to Get Extended Attributes

Now let's retrieve the extended attributes information from a specific MS Project file:

#### Using cURL

```bash
curl -X GET "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/extendedAttributes" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with the token received in Step 1.

### Step 3: Implement in Java

Let's see how to implement this in Java:

```java
// Tutorial Code Example: Getting Extended Attributes in Java
import com.aspose.tasks.cloud.ApiClient;
import com.aspose.tasks.cloud.ApiException;
import com.aspose.tasks.cloud.Configuration;
import com.aspose.tasks.cloud.api.TasksApi;
import com.aspose.tasks.cloud.model.ExtendedAttributeItemResponse;
import com.aspose.tasks.cloud.model.ExtendedAttributeItems;
import com.aspose.tasks.cloud.model.ExtendedAttributeItemsResponse;

public class GetExtendedAttributesExample {
    
    public static void main(String[] args) {
        // Configure API client
        ApiClient apiClient = Configuration.getDefaultApiClient();
        apiClient.setClientId("YOUR_CLIENT_ID");
        apiClient.setClientSecret("YOUR_CLIENT_SECRET");
        
        // Initialize TasksApi instance
        TasksApi tasksApi = new TasksApi(apiClient);
        
        try {
            // Specify the project file name
            String fileName = "Home move plan.mpp";
            
            // Call the API to get extended attributes
            ExtendedAttributeItemsResponse response = tasksApi.getExtendedAttributes(fileName, null, null, null);
            
            // Process and display results
            System.out.println("Status: " + response.getStatus());
            
            ExtendedAttributeItems attributes = response.getExtendedAttributes();
            System.out.println("Extended attributes count: " + attributes.getList().size());
            
            // Display each extended attribute
            for (ExtendedAttributeItemResponse attribute : attributes.getList()) {
                System.out.println("Index: " + attribute.getIndex());
                System.out.println("Field Name: " + attribute.getFieldName());
                System.out.println("Alias: " + attribute.getAlias());
                System.out.println("Field ID: " + attribute.getFieldId());
                System.out.println("Link: " + attribute.getLink().getHref());
                System.out.println("-------------------");
            }
            
        } catch (ApiException e) {
            System.err.println("Exception when calling TasksApi#getExtendedAttributes");
            System.err.println("Status code: " + e.getCode());
            System.err.println("Reason: " + e.getResponseBody());
            e.printStackTrace();
        }
    }
}
```

### Step 4: Process and Use the Response

The API response will include a collection of extended attributes with their respective indices, field names, aliases, and links. Let's look at a sample response:

```json
{
  "code": 0,
  "status": "OK",
  "extendedAttributes": {
    "link": {
      "href": "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/extendedAttributes",
      "rel": "self",
      "type": "text/json",
      "title": "Extended attributes"
    },
    "list": [
      {
        "link": {
          "href": "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/extendedAttributes/1",
          "rel": "self",
          "type": "text/json",
          "title": "Extended attribute 1"
        },
        "index": 1,
        "fieldName": "Text1",
        "alias": "Custom Category",
        "fieldId": "188743731"
      },
      {
        "link": {
          "href": "https://api.aspose.cloud/v3.0/tasks/Home%20move%20plan.mpp/extendedAttributes/2",
          "rel": "self",
          "type": "text/json",
          "title": "Extended attribute 2"
        },
        "index": 2,
        "fieldName": "Cost1",
        "alias": "Additional Expense",
        "fieldId": "188743736"
      }
    ]
  }
}
```

### Step 5: Understanding Extended Attribute Types

Extended attributes can have different types and properties:

1. Text fields: Store text values, often used for descriptions or categories
2. Number fields: Store numerical data for metrics or calculations
3. Cost fields: Special number fields formatted as currency
4. Date fields: Store date and time values
5. Duration fields: Store time periods
6. Flag fields: Store boolean (yes/no) values
7. Calculated fields: Store formulas that compute values based on other fields

To get detailed information about a specific extended attribute, you'll need to use the index in a separate API call (covered in the next tutorial).

### Try It Yourself

Now that you understand how to retrieve extended attributes information, try implementing this feature in your own application:

1. Create a new project in your preferred development environment
2. Install the appropriate Aspose.Tasks Cloud SDK
3. Set up authentication with your Aspose Cloud credentials
4. Make the API call to retrieve extended attributes from your own MS Project file
5. Process and display the results in a way that makes sense for your application

## Troubleshooting Tips

- Authentication Errors: Make sure your Client ID and Client Secret are correct
- 404 Not Found: Verify that the specified file exists in your Aspose.Tasks Cloud storage
- Empty Results: Some project files might not have any extended attributes defined
- Timeout Issues: For large project files, consider implementing retry logic

## What You've Learned

In this tutorial, you've learned:
- What extended attributes are in MS Project
- How to authenticate with the Aspose.Tasks Cloud API
- How to retrieve extended attributes information using API calls
- How to implement extended attribute retrieval in Java
- How to process and use the API response in your applications
- The different types of extended attributes available in MS Project

## Further Practice

To strengthen your understanding, try these exercises:
1. Create a function that categorizes extended attributes by their field type
2. Build a simple interface to display extended attributes in a user-friendly way
3. Compare extended attributes across multiple project files to identify common patterns
4. Implement a search feature to find extended attributes by name or alias

## Next Tutorial

Ready to learn more? Continue to the next tutorial: [How to Get a Project Extended Attribute by Index](/working-with-outline-codes-and-extended-attributes/get-extended-attribute-by-index/) to learn how to access detailed information about specific extended attributes.

## Useful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).
