---
id: "get-template-tutorial"
url: /template-operations/get-template/
title: "Tutorial: How to Retrieve Templates in GroupDocs.Parser Cloud"
productName: "GroupDocs.Parser Cloud"
weight: 2
description: "Learn how to retrieve document parsing templates using GroupDocs.Parser Cloud API in this step-by-step tutorial for developers."
keywords: "parser cloud tutorial, get template, retrieve template, template tutorial, groupdocs tutorial, document parsing"
toc: True
---

# Tutorial: How to Retrieve Templates in GroupDocs.Parser Cloud

## Learning Objectives

In this tutorial, you'll learn how to retrieve document parsing templates that you've previously created with the GroupDocs.Parser Cloud API. By the end, you'll be able to:

- Access templates stored in your cloud storage
- Use retrieved templates for document parsing operations
- Handle template retrieval through both REST API calls and SDK implementations
- Troubleshoot common template retrieval issues

Estimated completion time: 15-20 minutes

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Parser Cloud account (if not, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret from the [dashboard](https://dashboard.groupdocs.cloud/applications)
3. At least one template saved in your storage (see our [Create Template Tutorial]({{< ref "/template-operations/create-or-update-template" >}}))
4. Basic understanding of REST API concepts
5. Familiarity with your preferred programming language (C#, Java, or cURL)

## Why Retrieve Templates?

Templates are valuable assets for document parsing operations. Once created, you can reuse them across multiple parsing jobs. Retrieving templates allows you to:

- Use the same extraction patterns across multiple documents
- Ensure consistency in your parsing operations
- Save time by not having to redefine extraction rules
- Build a library of templates for different document types

## Retrieving a Template

Let's walk through the process of retrieving a template from your storage.

### Step 1: Obtain Authentication Token

Before making any API calls, you need to authenticate with the GroupDocs.Parser Cloud API:

```bash
# First get JSON Web Token
curl -v "https://api.groupdocs.cloud/connect/token" \
-X POST \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"
```

Save the JWT token from the response for use in subsequent API calls.

### Step 2: Retrieve the Template

Now, let's retrieve a template using the GET Template endpoint:

#### Try it yourself:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/template" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"TemplatePath\": \"templates/template_1.json\"
}"
```

This request will return the template content as a JSON response. You can then use this template in your document parsing operations.

### Step 3: Understanding the Response

The API response will contain the complete template definition including:

- Fields with their positions and properties
- Tables with their detection parameters
- All configuration needed for data extraction

Let's examine a sample response:

```json
{
    "fields": [
        {
            "fieldName": "Address",
            "fieldPosition": {
                "fieldPositionType": "Fixed",
                "rectangle": {
                    "position": {
                        "x": 13.0,
                        "y": 35.0
                    },
                    "size": {
                        "height": 10.0,
                        "width": 100.0
                    }
                },
                "matchCase": false,
                "isLeftLinked": false,
                "isRightLinked": false,
                "isTopLinked": false,
                "isBottomLinked": false,
                "autoScale": false
            }
        },
        {
            "fieldName": "Company",
            "fieldPosition": {
                "fieldPositionType": "Linked",
                "matchCase": false,
                "linkedFieldName": "Address",
                "isLeftLinked": false,
                "isRightLinked": false,
                "isTopLinked": false,
                "isBottomLinked": true,
                "searchArea": {
                    "height": 15.0,
                    "width": 100.0
                },
                "autoScale": true
            }
        }
    ],
    "tables": [
        {
            "tableName": "Totals",
            "detectorParameters": {
                "rectangle": {
                    "position": {
                        "x": 300.0,
                        "y": 385.0
                    },
                    "size": {
                        "height": 220.0,
                        "width": 65.0
                    }
                }
            }
        }
    ]
}
```

## SDK Implementation

Let's implement the same template retrieval using the SDK of your choice.

### C# Example

{{< gist groupdocscloud 39135fbf5cfb74deeeae6c47eafb2473 Parser_CSharp_Get_Template_Tutorial.cs >}}

### Java Example

{{< gist groupdocscloud c8b8e01a52ef2bae6fa5d78aba152238 Parser_Java_Get_Template_Tutorial.java >}}

## Using the Retrieved Template

After retrieving the template, you can use it in the Parse operation. Here's how you can incorporate the template in your parsing workflow:

### Step 4: Parse a Document Using the Retrieved Template

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/parse" \
-X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"FileInfo\": {
        \"FilePath\": \"invoices/sample.pdf\"
    },
    \"Template\": {
        \"TemplatePath\": \"templates/template_1.json\"
    }
}"
```

This will parse the document using the fields and tables defined in your template.

## Troubleshooting Tips

### Common Issues and Solutions

1. Template Not Found:
   - Verify that the template path is correct
   - Check that the storage name is correct (if not using default storage)
   - Ensure the template file exists in your storage

2. Authentication Errors:
   - Ensure your Client ID and Client Secret are correct
   - Check that your JWT token hasn't expired

3. Invalid Template Format:
   - Confirm that the template was created correctly
   - Verify that the template is a valid JSON file

### Learning Checkpoint

Let's verify your understanding with a few questions:

1. What is the HTTP method used for retrieving a template?
   - Answer: POST

2. What are the required parameters for the Get Template endpoint?
   - Answer: TemplatePath (and optionally StorageName if not using the default storage)

3. What can you do with a retrieved template?
   - Answer: Use it in Parse operations to extract data from documents

## What You've Learned

In this tutorial, you've learned how to:
- Retrieve templates from your GroupDocs.Parser Cloud storage
- Understand the structure of retrieved templates
- Implement template retrieval using cURL, C#, and Java
- Use retrieved templates in your document parsing workflow
- Troubleshoot common template retrieval issues

## Further Practice

To reinforce your learning, try these exercises:
1. Retrieve templates from different storage locations
2. Create a template library by retrieving and listing all available templates
3. Implement error handling for template retrieval in your application
4. Create a workflow that retrieves a template, checks if it exists, and creates it if it doesn't

## Next Steps

Now that you know how to retrieve templates, continue your learning journey with our next tutorial: [Deleting Unused Templates]({{< ref "/template-operations/delete-template" >}}).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to ask in our [support forum](https://forum.groupdocs.cloud/c/parser/19/). We're here to help you master template-based document parsing!
