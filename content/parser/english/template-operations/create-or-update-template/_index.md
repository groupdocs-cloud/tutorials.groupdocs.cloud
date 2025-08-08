---
id: "create-or-update-template-tutorial"
url: /template-operations/create-or-update-template/
title: Learn to Create or Update Templates in GroupDocs.Parser Cloud Tutorial
weight: 1
description: This tutorial teaches you how to create and update document parsing templates using GroupDocs.Parser Cloud API to extract structured data from your documents.
---

# Tutorial: Learn to Create or Update Templates in GroupDocs.Parser Cloud

## Learning Objectives

In this tutorial, you'll learn how to create and update document parsing templates using the GroupDocs.Parser Cloud API. By the end, you'll be able to:

- Define custom templates for extracting data from documents
- Create templates with fields for specific data points
- Add tables to your templates for structured data extraction
- Save templates for future use in parsing operations
- Update existing templates with new fields or parameters

Estimated completion time: 20-25 minutes

## Prerequisites

Before starting this tutorial, make sure you have:

1. A GroupDocs.Parser Cloud account (if not, [register for a free trial](https://dashboard.groupdocs.cloud/#/apps))
2. Your Client ID and Client Secret from the [dashboard](https://dashboard.groupdocs.cloud/applications)
3. Basic understanding of REST API concepts
4. Familiarity with your preferred programming language (C#, Java, or cURL)
5. Development environment set up for your chosen SDK

## Understanding Templates in Document Parsing

Templates are powerful tools that define patterns for extracting specific data from documents. They're especially useful when you need to extract data from similarly structured documents like invoices, contracts, or forms.

### Why Use Templates?

Templates allow you to:
- Extract data from fixed positions in a document
- Find data linked to other elements in the document
- Define tables for structured data extraction
- Process batches of similarly structured documents consistently

## Creating Your First Template

Let's start by creating a simple template that extracts an address and company name from a document.

### Step 1: Plan Your Template Structure

First, identify what data you need to extract:
1. Address - located at a fixed position
2. Company Name - positioned below the address
3. Totals Table - containing financial summary data

### Step 2: Obtain Authentication Token

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

### Step 3: Define Template Fields

Now, let's create a template with fields for the address and company name:

#### Try it yourself:

Use the following cURL command to create a new template:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/template" \
-X PUT \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"Template\": {
        \"Fields\": [
            {
                \"FieldName\": \"Address\",
                \"FieldPosition\": {
                    \"FieldPositionType\": \"Fixed\",
                    \"Rectangle\": {
                        \"Position\": {
                            \"X\": 13.0,
                            \"Y\": 35.0
                        },
                        \"Size\": {
                            \"Height\": 10.0,
                            \"Width\": 100.0
                        }
                    },
                    \"MatchCase\": false,
                    \"IsLeftLinked\": false,
                    \"IsRightLinked\": false,
                    \"IsTopLinked\": false,
                    \"IsBottomLinked\": false,
                    \"AutoScale\": false
                }
            },
            {
                \"FieldName\": \"Company\",
                \"FieldPosition\": {
                    \"FieldPositionType\": \"Linked\",
                    \"MatchCase\": false,
                    \"LinkedFieldName\": \"Address\",
                    \"IsLeftLinked\": false,
                    \"IsRightLinked\": false,
                    \"IsTopLinked\": false,
                    \"IsBottomLinked\": true,
                    \"SearchArea\": {
                        \"Height\": 15.0,
                        \"Width\": 100.0
                    },
                    \"AutoScale\": true
                }
            }
        ],
        \"Tables\": [
            {
                \"TableName\": \"Totals\",
                \"DetectorParameters\": {
                    \"Rectangle\": {
                        \"Position\": {
                            \"X\": 300.0,
                            \"Y\": 385.0
                        },
                        \"Size\": {
                            \"Height\": 220.0,
                            \"Width\": 65.0
                        }
                    }
                }
            }
        ]
    },
    \"TemplatePath\": \"templates/my_first_template.json\"
}"
```

### Step 4: Understanding the Template Structure

Let's break down what each part of the template does:

1. Fixed Position Field (Address):
   - Positioned at coordinates X:13, Y:35
   - Covers an area of width 100 and height 10
   - Uses absolute positioning on the page

2. Linked Field (Company):
   - Linked to the "Address" field
   - Positioned below the address (IsBottomLinked: true)
   - Search area of width 100 and height 15
   - Will automatically scale based on the size of the linked field

3. Table (Totals):
   - Located within a rectangle at position X:300, Y:385
   - Covers an area with width 65 and height 220

## SDK Implementation

Let's implement the same template creation using the SDK of your choice.

### C# Example

{{< gist groupdocscloud 39135fbf5cfb74deeeae6c47eafb2473 Parser_CSharp_Create_Update_Template_Tutorial.cs >}}

### Java Example

{{< gist groupdocscloud c8b8e01a52ef2bae6fa5d78aba152238 Parser_Java_Create_Update_Template_Tutorial.java >}}

## Updating an Existing Template

Now let's learn how to update an existing template by adding a new field.

### Step 5: Add a New Field to Your Template

To update a template, use the same API endpoint but specify the path of the existing template:

```bash
curl -v "https://api.groupdocs.cloud/v1.0/parser/template" \
-X PUT \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d "{
    \"Template\": {
        \"Fields\": [
            {
                \"FieldName\": \"Address\",
                \"FieldPosition\": {
                    \"FieldPositionType\": \"Fixed\",
                    \"Rectangle\": {
                        \"Position\": {
                            \"X\": 13.0,
                            \"Y\": 35.0
                        },
                        \"Size\": {
                            \"Height\": 10.0,
                            \"Width\": 100.0
                        }
                    },
                    \"MatchCase\": false
                }
            },
            {
                \"FieldName\": \"Company\",
                \"FieldPosition\": {
                    \"FieldPositionType\": \"Linked\",
                    \"MatchCase\": false,
                    \"LinkedFieldName\": \"Address\",
                    \"IsBottomLinked\": true,
                    \"SearchArea\": {
                        \"Height\": 15.0,
                        \"Width\": 100.0
                    },
                    \"AutoScale\": true
                }
            },
            {
                \"FieldName\": \"InvoiceNumber\",
                \"FieldPosition\": {
                    \"FieldPositionType\": \"Regex\",
                    \"Regex\": \"Invoice #(\\\\d+)\",
                    \"MatchCase\": false
                }
            }
        ],
        \"Tables\": [
            {
                \"TableName\": \"Totals\",
                \"DetectorParameters\": {
                    \"Rectangle\": {
                        \"Position\": {
                            \"X\": 300.0,
                            \"Y\": 385.0
                        },
                        \"Size\": {
                            \"Height\": 220.0,
                            \"Width\": 65.0
                        }
                    }
                }
            }
        ]
    },
    \"TemplatePath\": \"templates/my_first_template.json\"
}"
```

Notice we've added an "InvoiceNumber" field that uses a regular expression to find the invoice number.

## Troubleshooting Tips

### Common Issues and Solutions

1. Authentication Errors:
   - Ensure your Client ID and Client Secret are correct
   - Check that your JWT token hasn't expired

2. Template Not Saving:
   - Verify that the storage path exists
   - Ensure all required fields have valid values

3. Field Not Found During Parsing:
   - Double-check coordinates and dimensions
   - Try expanding the search area for linked fields
   - Test regex patterns separately before using them in templates

## What You've Learned

In this tutorial, you've learned how to:
- Create a new document parsing template
- Define fixed-position fields for specific data extraction
- Create linked fields that relate to other elements
- Add tables to your templates
- Update existing templates with new fields
- Implement template operations using cURL, C#, and Java

## Further Practice

To reinforce your learning, try these exercises:
1. Create a template that extracts data from an invoice PDF
2. Add a regex field to find dates in various formats
3. Create a template with multiple tables on different pages
4. Update an existing template to add search capabilities for email addresses

## Next Steps

Now that you know how to create and update templates, continue your learning journey with our next tutorial: [How to Retrieve Templates]({{< ref "/template-operations/get-template" >}}).

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/parser/)
- [Documentation](https://docs.groupdocs.cloud/parser/)
- [Live Demo](https://products.groupdocs.app/parser/family)
- [API Reference](https://reference.groupdocs.cloud/parser/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.parser-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/parser/19/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to ask in our [support forum](https://forum.groupdocs.cloud/c/parser/19/).
