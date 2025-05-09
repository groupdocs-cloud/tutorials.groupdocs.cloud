---
title: Working with ItemsStyle Data Structure in GroupDocs.Comparison Cloud Tutorial
description: Learn how to customize the appearance of document comparison differences using the ItemsStyle data structure in GroupDocs.Comparison Cloud API.
weight: 80
url: /advanced-data-structures/itemsstyle/
---

# Tutorial: Working with ItemsStyle Data Structure

In this tutorial, you'll learn how to use the ItemsStyle data structure to customize the appearance of comparison differences in GroupDocs.Comparison Cloud API. By the end, you'll be able to create visually distinctive comparison results that highlight changes according to your specific requirements.

## Learning Objectives

- Understand the ItemsStyle structure and its properties
- Configure visual styling for inserted, deleted, and changed content
- Apply custom separators for comparison differences
- Create different styling profiles for various types of document comparisons

## Prerequisites

- GroupDocs.Comparison Cloud API credentials
- Basic understanding of document comparison concepts
- Familiarity with JSON structures
- A development environment with your preferred language

## Understanding the ItemsStyle Structure

The ItemsStyle data structure is used for styling comparison differences in the result document. It allows you to customize how inserted, deleted, and changed content appears.

```javascript
{
  "FontColor": "string",
  "HighlightColor": "string",
  "BeginSeparatorString": "string",
  "EndSeparatorString": "string",
  "Bold": true,
  "Italic": true,
  "StrikeThrough": true,
  "Underline": true
}
```

Key properties include:

| Property | Description |
|---|---|
| FontColor | Font color for changed components (RGB color string converted to integer) |
| HighlightColor | Highlight color for changed components |
| BeginSeparatorString | Start tag for changed components |
| EndSeparatorString | End tag for changed components |
| Bold | Bold style for changed components |
| Italic | Italic style for changed components |
| StrikeThrough | Strike through style for changed components |
| Underline | Underline style for changed components |

## Implementing ItemsStyle in Your Application

### Step 1: Creating Basic Style Configurations

Let's start by creating basic style configurations for inserted, deleted, and changed content:

<script src="https://gist.github.com/groupdocs-comparison-cloud/5d7fba9cd5678ab31e622208.js"></script>

#### Try it yourself
Create three different ItemsStyle objects with distinct visual appearances and test them in a comparison operation.

### Step 2: Working with Color Properties

Colors are represented as string values of RGB colors converted to integers. Here's how to work with them:

<script src="https://gist.github.com/groupdocs-comparison-cloud/6e8gba9cd5678ab31e622209.js"></script>

#### Troubleshooting Tip
If colors don't appear as expected, verify that you're using the correct string representation of RGB values. The API expects integers as strings, not hex color codes.

### Step 3: Using Separator Strings

Separator strings can be used to clearly mark the beginning and end of changes:

<script src="https://gist.github.com/groupdocs-comparison-cloud/7f9hca9cd5678ab31e622210.js"></script>

### Step 4: Creating a Complete Styling Configuration

Now, let's put it all together to create a comprehensive styling configuration:

<script src="https://gist.github.com/groupdocs-comparison-cloud/8j0kda9cd5678ab31e622211.js"></script>

## What You've Learned

In this tutorial, you've learned:
- How to configure the visual appearance of comparison differences
- How to work with color properties in the ItemsStyle structure
- How to apply custom separators to mark changes
- How to create comprehensive styling configurations for different change types

## Further Practice

Try these exercises to reinforce your learning:
1. Create a "track changes" style that mimics the appearance of Microsoft Word's track changes feature
2. Implement a styling configuration that optimizes for printing (high contrast, clear markings)
3. Design a style configuration for accessibility (considering color blindness and other visual impairments)

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
