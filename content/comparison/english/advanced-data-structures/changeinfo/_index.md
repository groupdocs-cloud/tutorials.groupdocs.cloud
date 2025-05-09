---
title: Advanced Tutorial Working with Change Info Structure in GroupDocs.Comparison Cloud Tutorial
description: Learn how to manage detailed change information in document comparisons using the ChangeInfo structure in GroupDocs.Comparison Cloud API.
weight: 30
url: /advanced-data-structures/changeinfo/
---

# Advanced Tutorial: Working with ChangeInfo Structure

In this advanced tutorial, you'll learn how to work with the ChangeInfo data structure in GroupDocs.Comparison Cloud API. This structure is crucial for managing detailed information about changes detected during document comparison operations and for implementing acceptance or rejection of specific changes.

## Learning Objectives

- Understand the ChangeInfo data structure and its properties
- Retrieve detailed change information from comparison results
- Process and filter changes based on their type and properties
- Implement change acceptance/rejection workflows
- Understand change coordinates and positioning information

## Prerequisites

- GroupDocs.Comparison Cloud API credentials
- Advanced understanding of document comparison concepts
- Familiarity with RESTful APIs
- Test documents with various types of changes

## Understanding the ChangeInfo Structure

The ChangeInfo data structure represents detailed information about changes detected during comparison operations. It's returned by the `/comparison/changes` API and used by the `/comparison/updates` API.

```javascript
{
    "id": 0,
    "comparisonAction": "None",
    "type": "Inserted",
    "text": "lol",
    "targetText": "Latin (i/?læt?n/, /?læt?n/; Latin: lingua lat?na, IPA: [?l????a la?ti?na]) is a classical language, originally spoken inLatium, Italy, which belongs to the Italic branch of the Indo-European languages.[3] The Latin alphabet is derived from the Etruscan and Greek alphabetslol.",
    "authors": [
      "?GroupDocs"
    ],
    "styleChangeInfo": [],
    "pageInfo": {
      "width": 0,
      "height": 0,
      "pageNumber": 0
    },
    "box": {
      "height": 0,
      "width": 0,
      "x": 0,
      "y": 0
    }
}
```

Key properties include:

| Property | Description |
|---|---|
| id | ID of the change |
| comparisonAction | Action to take (Accept, Reject, None) |
| type | Type of change (Inserted, Deleted, StyleChanged, etc.) |
| text | Source text |
| targetText | Target text |
| authors | Array of authors who made this change |
| pageInfo | Description of the page where the change is found |
| box | Coordinates and dimensions of the change |
| styleChangeInfo | Array of style changes with old and new values |

## Implementing ChangeInfo in Your Application

### Step 1: Retrieving Change Information

First, let's see how to retrieve detailed change information from a comparison result:

<script src="https://gist.github.com/groupdocs-comparison-cloud/4j4abd9cd5678ab31e622222.js"></script>

#### Try it yourself
Compare two versions of a document and retrieve the change information. Print out the changes to understand their structure.

### Step 2: Filtering and Processing Changes

Often, you'll want to filter changes based on their type or other properties:

<script src="https://gist.github.com/groupdocs-comparison-cloud/5k5bce9cd5678ab31e622223.js"></script>

### Step 3: Implementing Change Acceptance/Rejection

One of the most powerful features is the ability to selectively accept or reject changes:

<script src="https://gist.github.com/groupdocs-comparison-cloud/6l6cdf9cd5678ab31e622224.js"></script>

#### Troubleshooting Tip
If changes aren't being applied as expected, verify that you're using the correct change IDs and that the ComparisonAction property is set correctly. The API is case-sensitive.

### Step 4: Working with Change Coordinates

For visual representation of changes, you can use the coordinate information:

<script src="https://gist.github.com/groupdocs-comparison-cloud/7m7deg9cd5678ab31e622225.js"></script>

### Step 5: Handling Style Changes

Style changes require special handling:

<script src="https://gist.github.com/groupdocs-comparison-cloud/8n8efh9cd5678ab31e622226.js"></script>

### Step 6: Creating a Complete Change Management Workflow

Finally, let's put everything together for a complete change management workflow:

<script src="https://gist.github.com/groupdocs-comparison-cloud/9o9fgi9cd5678ab31e622227.js"></script>

## Advanced Change Processing Techniques

For complex documents or workflows, consider these advanced techniques:

### Batch Processing of Changes

When dealing with documents that have many changes, process them in batches:

<script src="https://gist.github.com/groupdocs-comparison-cloud/1p1glj9cd5678ab31e622228.js"></script>

### Automatic Change Classification

Use the change properties to automatically classify changes:

<script src="https://gist.github.com/groupdocs-comparison-cloud/2q2hlk9cd5678ab31e622229.js"></script>

## What You've Learned

In this tutorial, you've learned:
- The structure and purpose of the ChangeInfo data type
- How to retrieve and process change information
- How to implement change acceptance and rejection
- How to work with change coordinates and style information
- Advanced techniques for change processing and management

## Further Practice

Try these exercises to reinforce your learning:
1. Create a visual change viewer that displays changes with their actual coordinates
2. Implement an automated change acceptance system based on rules (e.g., accept all formatting changes but review content changes)
3. Build a change report generator that provides statistics and summaries of changes

## Next Tutorial

Continue your learning journey with our [Tutorial: Creating Powerful Comparisons with ComparisonOptions](/advanced-data-structures/comparisonoptions) tutorial to learn how to configure complete comparison operations.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
