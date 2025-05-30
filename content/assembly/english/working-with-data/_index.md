---
title: Learn to Work with Data in GroupDocs.Assembly Cloud Tutorial
url: /working-with-data"
weight: 2
description: Learn how to filter, sort, group and format data in your document templates with this step-by-step GroupDocs.Assembly Cloud tutorial.
---

# Tutorial: Learn to Work with Data in GroupDocs.Assembly Cloud

## Overview

In this tutorial, you'll learn how to work with data in GroupDocs.Assembly Cloud, applying powerful data processing techniques to create dynamic document templates. This is a fundamental skill for effective document automation and report generation.

## Learning Objectives

By the end of this tutorial, you will be able to:

- Define and use variables in your templates
- Apply LINQ-based extension methods for data operations
- Filter, sort, and group data within your templates 
- Format numeric, date-time, and string values
- Apply custom formatting options to your template data

## Prerequisites

Before starting this tutorial, you should have:

- A GroupDocs.Assembly Cloud account (sign up for a [free trial](https://dashboard.groupdocs.cloud/#/apps))
- Basic understanding of C# and LINQ concepts
- Familiarity with document template structure
- Your development environment set up (any language with the GroupDocs.Assembly Cloud SDK installed)

## Using Variables in Templates

Variables allow you to store and reuse values in your templates, making them more efficient and readable. This approach is particularly useful when you need to calculate expensive expression values just once and reuse them throughout your template.

### How to Define Variables

To declare a variable in your template, use the `var` tag with the following syntax:

```
<<var [variableType variableName = variableValue]>>
```

After declaring a variable, you can access its value using the variable's name.

### Try it yourself

Let's create a simple example that demonstrates variable declaration and usage:

```
<<var [s = "Hello, "]>><<[s]>><<var [s = "World!"]>><<[s]>>
```

This template will output "Hello, World!" in your document. Notice how the variable `s` is first assigned "Hello, " and then reassigned to "World!".

> Note: You can redefine a variable's value using another `var` tag, but you cannot change its type once defined.

## Using Extension Methods in Templates

GroupDocs.Assembly Cloud provides powerful extension methods to manipulate data within your templates. Let's explore how to use them effectively.

### Extension Methods of Iteration Variables

The two primary extension methods for iteration variables are:

- `IndexOf()` - Returns a zero-based index (0, 1, 2, ...) of a sequence element
- `NumberOf()` - Returns a one-based index (1, 2, 3, ...) of a sequence element

#### Example: Using IndexOf() for Comma Separation

```
The items are: <<foreach [item in items]>><<[item.IndexOf() != 0? ", ": ""]>><<[item]>><</foreach>>.
```

Given an array of `["item1", "item2", "item3"]`, this would produce:
```
The items are: item1, item2, item3.
```

#### Example: Using NumberOf() for Item Numbering

```
<<foreach [item in items]>><<[item.NumberOf()]>>. <<[item]>>
<</foreach>>
```

This would produce:
```
1. item1
2. item2
3. item3
```

### Using Enumeration Extension Methods

GroupDocs.Assembly Cloud supports many LINQ-style extension methods for data manipulation. These methods allow you to perform operations like filtering, sorting, grouping, and aggregating data.

Here's a GitHub Gist with examples of the most commonly used enumeration methods:

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-EnumerationMethods.cs"></script>

## Data Formatting

Formatting is a critical aspect of document generation. GroupDocs.Assembly Cloud provides flexible options for formatting various data types.

### Basic Formatting Syntax

The complete syntax for formatting expressions is:

```
<<[expression]:"format" -html>>
```

Where:
- `expression` is the value to be formatted
- `format` is an optional format string
- `-html` is an optional switch to treat the result as HTML

### Formatting Numeric and DateTime Expressions

For numeric and date-time values, you can specify standard .NET format strings:

```
<<[dateValue]:"yyyy.MM.dd">> <!-- Formats date as 2023.05.15 -->
<<[price]:"C2">> <!-- Formats number as currency with 2 decimal places -->
```

### Additional Number Formats

GroupDocs.Assembly Cloud provides several specialized number formats:

| Format | Description | Example |
|--------|-------------|---------|
| alphabetic | Formats integer as uppercase letter | A, B, C |
| roman | Formats integer as Roman numeral | I, II, III |
| ordinal | Adds ordinal suffix | 1st, 2nd, 3rd |
| cardinal | Converts to text representation | One, Two, Three |

### Try it yourself

Let's create a template that demonstrates various formatting options:

```
Date: <<[orderDate]:"MMMM d, yyyy">>
Amount: <<[amount]:"C2">>
Invoice #: <<[invoiceNumber]:alphabetic>>
Page <<[pageNumber]:ordinal>> of <<[totalPages]>>
```

## Practical Example: Filtering and Grouping Data

Let's put everything together in a practical example. We'll create a sales report that:
1. Groups sales by product category
2. Filters out products with zero sales
3. Calculates totals for each category
4. Formats currency values appropriately

### Template Code

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-SalesReport.cs"></script>

### Implementation in Different Languages

#### C# SDK Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-CSharp.cs"></script>

#### Python SDK Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-Python.py"></script>

#### Java SDK Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-Java.java"></script>

#### REST API with cURL

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-cURL.sh"></script>

## Troubleshooting Common Issues

When working with data processing in templates, you might encounter these common issues:

1. Incorrect data type conversion - Ensure your variables are properly typed and convert data as needed.
2. LINQ expression errors - Check your lambda expressions for correct syntax and use proper collection methods.
3. Formatting errors - Verify that your format strings follow the expected pattern for each data type.
4. Null reference errors - Always check for null values, especially when working with nested properties.

> Tip: Use conditional logic (`if` tags) to handle potential null values and provide fallback formatting.

## What You've Learned

In this tutorial, you've learned how to:
- Define and use variables in templates
- Apply extension methods to manipulate iteration variables
- Use enumeration extension methods for data operations
- Format different data types with standard and custom formats
- Create practical templates with filtering, sorting, and grouping

## Further Practice

To reinforce your learning, try these exercises:

1. Create a template that groups customer orders by month and calculates monthly totals
2. Build a report that filters products by price range and applies different formatting to different ranges
3. Develop a template that uses both IndexOf() and NumberOf() for custom listing formats

## Next Tutorial in the Learning Path

Ready to continue your journey? Head to our [Tutorial: Working with Lists](/working-with-lists/) to learn how to create dynamic bulleted, numbered, and custom lists in your documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/assembly/)
- [Documentation](https://docs.groupdocs.cloud/assembly/)
- [Live Demo](https://products.groupdocs.app/assembly/family)
- [API Reference](https://reference.groupdocs.cloud/assembly/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.assembly-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/assembly/15/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
