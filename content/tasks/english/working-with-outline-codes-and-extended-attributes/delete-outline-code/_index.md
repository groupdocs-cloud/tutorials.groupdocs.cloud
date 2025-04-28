---
title: How to Delete a Project Outline Code in Aspose.Tasks Cloud Tutorial
description: Learn how to safely remove outline codes from MS Project files using Aspose.Tasks Cloud API in this practical step-by-step tutorial for developers
url: /working-with-outline-codes-and-extended-attributes/delete-outline-code/
weight: 30
---

## Learning to Delete a Project Outline Code

In this tutorial, you'll learn how to delete a specific outline code from a MS Project file using the Aspose.Tasks Cloud API. Removing unused or obsolete outline codes helps maintain clean project structures and can be necessary when reorganizing project categorization schemes.

### Learning Objectives

By the end of this tutorial, you will be able to:
- Make API calls to safely delete a project outline code
- Understand the implications of removing outline codes
- Implement outline code deletion in your preferred programming language
- Verify successful deletion through API responses
- Handle potential errors during the deletion process

## Prerequisites

Before starting this tutorial, make sure you have:

1. Completed the previous tutorials on [Getting Outline Codes Information](/working-with-outline-codes-and-extended-attributes/get-outline-codes/) and [Getting a Specific Outline Code by Index](/working-with-outline-codes-and-extended-attributes/get-outline-code-by-index/)
2. An [Aspose Cloud account](https://dashboard.aspose.cloud/)
3. Valid Aspose.Tasks Cloud API credentials (Client ID and Client Secret)
4. A MS Project file with outline codes stored in your Aspose Cloud Storage

## Understanding Outline Code Deletion

Deleting an outline code permanently removes it from the project file. Before proceeding with deletion, consider these important points:

- Deletion is irreversible - once deleted, the outline code cannot be recovered
- Any task or resource assignments using this outline code will lose this categorization
- References to the outline code in filters, groups, or views may be affected
- Custom fields that rely on the outline code may no longer work correctly

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

### Step 2: Identify the Outline Code to Delete

Before deletion, it's good practice to verify which outline code you want to delete:

#### Using cURL

```bash
curl -X GET "https://api.aspose.cloud/v3.0/tasks/YourProjectFile.mpp/outlineCodes" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Review the response to identify the correct index of the outline code you want to delete.

### Step 3: Delete the Outline Code

Now let's delete a specific outline code by its index:

#### Using cURL

```bash
curl -X DELETE "https://api.aspose.cloud/v3.0/tasks/YourProjectFile.mpp/outlineCodes/1" \
     -H "accept: application/json" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

Replace `YOUR_ACCESS_TOKEN` with the token received in Step 1, and adjust the file name and index as needed.

### Step 4: Implement in Node.js

Let's see how to implement this in Node.js:

```javascript
// Tutorial Code Example: Deleting an Outline Code in Node.js
const axios = require('axios');

async function deleteOutlineCode() {
    try {
        // Step 1: Authenticate and get token
        const authResponse = await axios({
            method: 'post',
            url: 'https://api.aspose.cloud/connect/token',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded',
                'Accept': 'application/json'
            },
            data: 'grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET'
        });
        
        const token = authResponse.data.access_token;
        
        // Step 2: Define file name and outline code index
        const fileName = 'ExtendedAttribute.mpp';
        const outlineCodeIndex = 1; // The index of the outline code to delete
        
        // Step 3: Make the delete request
        const deleteResponse = await axios({
            method: 'delete',
            url: `https://api.aspose.cloud/v3.0/tasks/${encodeURIComponent(fileName)}/outlineCodes/${outlineCodeIndex}`,
            headers: {
                'Accept': 'application/json',
                'Authorization': `Bearer ${token}`
            }
        });
        
        // Step 4: Process the response
        console.log('Response Status:', deleteResponse.data.Code);
        console.log('Message:', deleteResponse.data.Status);
        
        // Step 5: Verify deletion by trying to get the outline codes list again
        const verifyResponse = await axios({
            method: 'get',
            url: `https://api.aspose.cloud/v3.0/tasks/${encodeURIComponent(fileName)}/outlineCodes`,
            headers: {
                'Accept': 'application/json',
                'Authorization': `Bearer ${token}`
            }
        });
        
        console.log('Remaining outline codes:', verifyResponse.data.outlineCodes.list.length);
        console.log('Verification successful!');
        
    } catch (error) {
        if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            console.error('Error status:', error.response.status);
            console.error('Error details:', error.response.data);
        } else if (error.request) {
            // The request was made but no response was received
            console.error('No response received:', error.request);
        } else {
            // Something happened in setting up the request that triggered an Error
            console.error('Error setting up request:', error.message);
        }
    }
}

// Run the function
deleteOutlineCode();
```

### Step 5: Handle Response and Verify Deletion

After sending the DELETE request, the API will return a response indicating success or failure:

```json
{
  "Code": "200",
  "Status": "OK"
}
```

To verify that the outline code has been successfully deleted, you can make another GET request to list all outline codes and confirm that the deleted index is no longer present.

### Try It Yourself

Now that you understand how to delete an outline code, try implementing this feature in your own application:

1. Set up authentication with your Aspose Cloud credentials
2. Retrieve all outline codes to identify which one to delete
3. Make the API call to delete the selected outline code
4. Verify the deletion by retrieving the updated list of outline codes
5. Handle any potential errors gracefully

## Troubleshooting Tips

- 404 Not Found: Verify that the specified file exists in your storage
- Invalid Index: Ensure that the outline code index you're trying to delete exists
- File Access Issues: Make sure the file isn't locked or in use
- Permission Issues: Verify you have the necessary permissions to modify the file

## What You've Learned

In this tutorial, you've learned:
- How to safely delete a project outline code
- The importance of verifying which outline code to delete
- How to implement outline code deletion in Node.js
- How to verify successful deletion
- Best practices for error handling during deletion operations

## Further Practice

To strengthen your understanding, try these exercises:
1. Create a function that checks if an outline code is in use (by tasks or resources) before deletion
2. Implement a backup mechanism that saves the outline code information before deletion
3. Build a simple user interface that lists outline codes and allows selective deletion

## Next Tutorial

Ready to learn more? Continue to the next tutorial: [How to Create Reports in PDF Format](/working-with-outline-codes-and-extended-attributes/create-report-pdf/) to learn how to generate professional PDF reports from your project data.

## Useful Resources

- [Product Page](https://products.aspose.cloud/tasks/)
- [Documentation](https://docs.aspose.cloud/tasks/)
- [API Reference](https://reference.aspose.cloud/tasks/)
- [Free Support](https://forum.aspose.cloud/c/tasks/16/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post them on our [support forum](https://forum.aspose.cloud/c/tasks/16/).
- [