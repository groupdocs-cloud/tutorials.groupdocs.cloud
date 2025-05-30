---
title: How to Work with Lists in GroupDocs.Assembly Cloud Tutorial
url: /working-with-lists/
weight: 3
description: Learn how to dynamically generate bulleted, numbered, and custom lists in your document templates with this step-by-step GroupDocs.Assembly Cloud tutorial.
---

# Tutorial: How to Work with Lists in GroupDocs.Assembly Cloud

## Overview

In this tutorial, you'll learn how to work with lists in GroupDocs.Assembly Cloud, creating dynamic document templates that can generate various types of lists. Lists are fundamental elements in documents that help organize information in a readable and structured format.

## Learning Objectives

By the end of this tutorial, you will be able to:

- Create in-paragraph lists for embedding items within text
- Generate dynamic bulleted lists with custom formatting
- Implement numbered lists with automatic numbering
- Control nested list numbering with restart features
- Apply conditional formatting to list items
- Create multi-level lists with custom styling

## Prerequisites

Before starting this tutorial, you should have:

- A GroupDocs.Assembly Cloud account (sign up for a [free trial](https://dashboard.groupdocs.cloud/#/apps))
- Basic understanding of template syntax and data bands
- Familiarity with JSON or XML data sources
- Your development environment set up (any language with the GroupDocs.Assembly Cloud SDK installed)

## Understanding List Types in Document Templates

Documents support several list types, each with distinct visual presentations and use cases:

1. In-paragraph lists - Items embedded within paragraph text
2. Bulleted lists - Items preceded by bullet symbols (•, ◦, ■, etc.)
3. Numbered lists - Items with sequential numbers or letters
4. Multi-level lists - Nested lists with hierarchical structure

Let's explore how to implement each type using GroupDocs.Assembly Cloud templates.

## Sample Data Source

Throughout this tutorial, we'll use a sample product catalog as our data source:

### XML Data Source

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-ProductsData.xml"></script>

### JSON Data Source

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-ProductsData.json"></script>

## Creating an In-Paragraph List

In-paragraph lists embed items within continuous text, using separators like commas or semicolons.

### Template Syntax

```
We provide support for the following products: <<foreach [in products]>><<[IndexOf() != 0 ? ", " : ""]>><<[ProductName]>><</foreach>>.
```

### Result

```
We provide support for the following products: LG Nexus 5, Nokia Lumia 5801, Huawie Mate S, Moto Style.
```

### How It Works

1. The `foreach` tag iterates through all products in the data source
2. The `IndexOf()` method determines if it's the first item (index 0)
3. For non-first items, a comma and space are added before the product name
4. Each product name is inserted using the `<<[ProductName]>>` expression

### Try it yourself

Create a simple template with an in-paragraph list of your own data. Try using different separators like semicolons or custom text.

## Generating a Bulleted List

Bulleted lists use symbols to visually separate items in a vertical list.

### Template Syntax

```
•  <<foreach [in products]>><<[ProductName]>>
<</foreach>>
```

### Result

```
•  LG Nexus 5
•  Nokia Lumia 5801
•  Huawie Mate S
•  Moto Style
```

### How It Works

1. The bullet character (•) is placed at the beginning of the template line
2. The `foreach` tag iterates through all products
3. Each product name is placed on a new line with the bullet inheriting from the template

### Try it yourself

Create a bulleted list with different bullet symbols:
- Use standard bullets (•)
- Try squares (■) or other Unicode symbols
- Apply custom indentation to your list

## Implementing a Numbered List

Numbered lists automatically assign sequential numbers to items.

### Template Syntax

```
1. <<foreach [in products]>><<[ProductName]>>
<</foreach>>
```

### Result

```
1. LG Nexus 5
2. Nokia Lumia 5801
3. Huawie Mate S
4. Moto Style
```

### How It Works

1. Start with a numbered item in your template (1.)
2. The `foreach` tag iterates through the products
3. Document processors automatically continue the numbering sequence

### C# SDK Implementation Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-NumberedListCSharp.cs"></script>

### Python SDK Implementation Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-NumberedListPython.py"></script>

## Working with Nested Lists and Numbering Restart

When working with nested lists, you may need to restart numbering for each outer list item.

### Template Syntax

```
<<foreach [order in Orders]>><<[order.ClientName]>><<[order.ClientAddress]>>
1. <<restartNum>><<foreach [service in order.Services]>><<[service.Name]>>  
<</foreach>><</foreach>>
```

### How It Works

1. The outer `foreach` loop iterates through orders
2. For each order, client information is displayed
3. The `<<restartNum>>` tag ensures numbering starts from 1 for each order's services
4. The inner `foreach` loop then lists all services with fresh numbering

### Try it yourself

Create a template with nested list structures that use the `restartNum` tag to control numbering behavior.

## Applying Conditional Formatting to List Items

You can dynamically apply different formatting to list items based on conditions.

### Template Syntax

```
We provide support for the following products:
1. <<foreach [in products]>><<if [IndexOf() % 2 == 1]>><span style="background-color:#ffa69e;color:#000;"><<[ProductName]>></span>
2. <<else>><span style="background-color:#b8f2e6;color:#000;"><<[ProductName]>></span><<</if>><</foreach>>
```

### Result

This would produce a list with alternating background colors:
- Odd items have a pink background
- Even items have a teal background

### How It Works

1. The `foreach` tag iterates through all products
2. The `if` condition checks if the item index modulo 2 equals 1 (odd index)
3. Different style spans are applied based on the condition
4. The product name is displayed with the appropriate styling

### REST API Implementation with cURL

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-ConditionalFormattingCurl.sh"></script>

## Advanced List Techniques

### Creating Custom-Formatted Multi-Level Lists

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-MultiLevelList.cs"></script>

### Combining Lists with Tables

Lists can be combined with tables to create structured document layouts:

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-ListsWithTables.cs"></script>

## Troubleshooting Common Issues

When working with lists in templates, you might encounter these common issues:

1. Incorrect list indentation - Ensure proper nesting levels in your template
2. Numbering sequence errors - Check for proper use of the `restartNum` tag
3. Formatting inconsistencies - Verify that your conditional formatting is properly closed
4. Paragraph break problems - Pay attention to paragraph breaks in your templates

> Tip: Always test your templates with small data sets first, then scale up to your complete data source.

## What You've Learned

In this tutorial, you've learned how to:
- Create different types of lists (in-paragraph, bulleted, numbered)
- Control list numbering with the `restartNum` tag
- Apply conditional formatting to list items
- Implement advanced list techniques for complex documents

## Further Practice

To reinforce your learning, try these exercises:

1. Create a multi-level list that shows product categories and their items
2. Implement a numbered list with custom number formatting (Roman numerals or letters)
3. Build a conditionally formatted list that highlights special items based on price or inventory

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/assembly/)
- [Documentation](https://docs.groupdocs.cloud/assembly/)
- [Live Demo](https://products.groupdocs.app/assembly/family)
- [API Reference](https://reference.groupdocs.cloud/assembly/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.assembly-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/assembly/15/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
