---
title: Applying Revisions with Options in GroupDocs.Comparison Cloud Tutorial
description: Learn how to process document revisions programmatically using the ApplyRevisionsOptions structure in GroupDocs.Comparison Cloud API.
url: /advanced-data-structures/applyrevisionsoptions/
weight: 20
---

# Tutorial: Applying Revisions with ApplyRevisionsOptions

In this tutorial, you'll learn how to use the ApplyRevisionsOptions data structure in GroupDocs.Comparison Cloud API to programmatically apply or reject revisions in documents. This is particularly useful for automating document review workflows and managing tracked changes.

## Learning Objectives

- Understand the ApplyRevisionsOptions structure and its components
- Learn how to retrieve revision information from documents
- Implement selective acceptance or rejection of revisions
- Create efficient document revision workflows
- Save documents with processed revisions

## Prerequisites

- GroupDocs.Comparison Cloud API credentials
- Advanced understanding of document revision concepts
- Familiarity with track changes in word processing documents
- Test documents with revisions for practice

## Understanding the ApplyRevisionsOptions Structure

The ApplyRevisionsOptions data structure defines options for applying revisions using the "PUT" /comparison/revisions API method. It allows you to specify which revisions to accept or reject in a document.

```javascript
{
  'SourceFile': {
    'FilePath': 'source_files/word/source_with_revs.docx'
  },
  'Revisions': [
    {
      'Id': 0,
      'action': 'Accept'
    },
    {
      'Id': 1,
      'action': 'Accept'
    },
  ],
  'OutputPath': 'output/result.docx'
}
```

Key components include:

| Component | Description |
|---|---|
| SourceFile | Information about the source file (using the FileInfo structure) |
| Revisions | Array of revision settings (using the RevisionInfo structure) |
| OutputPath | Path to the output document |

## Implementing ApplyRevisionsOptions in Your Application

### Step 1: Retrieving Revision Information

Before you can apply revisions, you need to know what revisions exist in the document:

<script src="https://gist.github.com/groupdocs-comparison-cloud/2a2rwv9cd5678ab31e622239.js"></script>

#### Try it yourself
Retrieve the revision information from a document with tracked changes and examine the structure of the results.

### Step 2: Creating a Basic Revision Application Configuration

Now, let's create a basic configuration to apply some revisions:

<script src="https://gist.github.com/groupdocs-comparison-cloud/3b3sxw9cd5678ab31e622240.js"></script>

### Step 3: Selectively Accepting and Rejecting Revisions

In real-world scenarios, you'll often want to selectively accept or reject revisions based on certain criteria:

<script src="https://gist.github.com/groupdocs-comparison-cloud/4c4tyx9cd5678ab31e622241.js"></script>

#### Troubleshooting Tip
If revisions aren't being applied as expected, verify that you're using the correct revision IDs and that the action values are correctly spelled ("Accept" or "Reject"). The API is case-sensitive.

### Step 4: Handling Different Types of Revisions

Different types of revisions (insertions, deletions, formatting changes) may require different handling:

<script src="https://gist.github.com/groupdocs-comparison-cloud/5d5uzy9cd5678ab31e622242.js"></script>

### Step 5: Creating a Complete Revision Application Workflow

Finally, let's put everything together for a comprehensive revision application workflow:

<script src="https://gist.github.com/groupdocs-comparison-cloud/6e6vza9cd5678ab31e622243.js"></script>

## Implementing Revision Strategies

Let's explore some common revision management strategies:

### Author-Based Revision Management

Accept or reject revisions based on their authors:

<script src="https://gist.github.com/groupdocs-comparison-cloud/7f7wab9cd5678ab31e622244.js"></script>

### Time-Based Revision Management

Process revisions based on when they were made:

<script src="https://gist.github.com/groupdocs-comparison-cloud/8g8xcb9cd5678ab31e622245.js"></script>

### Content-Based Revision Management

Make decisions based on the content of the revisions:

<script src="https://gist.github.com/groupdocs-comparison-cloud/9h9yde9cd5678ab31e622246.js"></script>

## What You've Learned

In this tutorial, you've learned:
- How to configure the ApplyRevisionsOptions structure for processing document revisions
- How to retrieve revision information from documents
- How to selectively accept or reject revisions
- How to implement different revision management strategies
- How to create complete revision processing workflows

## Further Practice

Try these exercises to reinforce your learning:
1. Create a revision management system that follows specific rules (e.g., accept all formatting changes but review content changes)
2. Build a batch processing system that applies the same revision rules to multiple documents
3. Implement a revision audit system that logs all revision decisions

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/comparison/)
- [Documentation](https://docs.groupdocs.cloud/comparison/)
- [Live Demo](https://products.groupdocs.app/comparison/family)
- [API Reference](https://reference.groupdocs.cloud/comparison/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.comparison-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/annotation/12/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
