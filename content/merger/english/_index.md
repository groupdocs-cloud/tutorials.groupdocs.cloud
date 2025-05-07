---
title: GroupDocs.Merger Cloud Comprehensive Document Manipulation API
weight: 2
url: /merger/
description: Learn how to seamlessly join, split, rearrange, rotate, remove, and extract pages from various document formats using the powerful GroupDocs.Merger Cloud API with step-by-step instructions and code examples.
---

# GroupDocs.Merger Cloud Tutorials

## Introduction

Welcome to the GroupDocs.Merger Cloud tutorials. This comprehensive guide provides detailed instructions for developers who want to integrate document merging, splitting, and manipulation capabilities into their applications using our REST API.

## Getting Started

### [Getting Started Tutorials](/merger/getting-started/)

Step-by-step tutorials for developers to learn how to use GroupDocs.Merger Cloud API - from basic operations to advanced document manipulation.

### [Document Management Tutorials](/merger/document-management/)

Step-by-step tutorials for developers to learn how to manage documents with GroupDocs.Merger Cloud API

### [Document Page Processes Tutorials](/merger/page-processes/)

Learn document page manipulation with these step-by-step tutorials for GroupDocs.Merger Cloud API
## Authentication

All API requests require authentication. GroupDocs.Merger Cloud uses OAuth 2.0 for secure API access.

```csharp
// Example authentication code
string clientId = "YOUR_CLIENT_ID";
string clientSecret = "YOUR_CLIENT_SECRET";
string token = GetAuthToken(clientId, clientSecret);
```

## API Reference

This guide provides detailed examples for each API endpoint. Each section includes:

- Endpoint description
- Request parameters
- Response format
- Code samples in multiple languages (C#, Java, PHP, Python, Ruby, Node.js)
- Error handling

## Error Handling

The API uses standard HTTP status codes to indicate success or failure:

- 200 - Success
- 400 - Bad request
- 401 - Authentication failure
- 404 - Resource not found
- 500 - Server error

## SDK Usage Examples

### Merge Documents Example

```csharp
// Example code for merging PDF files
var mergeOptions = new MergeOptions
{
    JoinItems = new List<JoinItem>
    {
        new JoinItem { FilePath = "document1.pdf" },
        new JoinItem { FilePath = "document2.pdf" }
    },
    OutputPath = "merged-output.pdf"
};

var response = api.Merge(new MergeRequest(mergeOptions));
```

### Split Document Example

```csharp
// Example code for splitting a document
var splitOptions = new SplitOptions
{
    FilePath = "document.pdf",
    Mode = SplitMode.Pages,
    Pages = new List<int> { 2, 4, 6 },
    OutputPath = "split-output"
};

var response = api.Split(new SplitRequest(splitOptions));
```

## Best Practices

- Cache authentication tokens to avoid repeated authentication requests
- Use appropriate file formats for your specific use case
- Implement error handling to manage API exceptions
- Optimize document size before uploading for better performance


## Additional Resources

- [API Reference](https://apireference.groupdocs.cloud/merger/)
- [GitHub SDK Repositories](https://github.com/groupdocs-merger-cloud)
- [Code Examples](https://github.com/groupdocs-merger-cloud/groupdocs-merger-cloud-examples)
- [Documentation Home](https://docs.groupdocs.cloud/merger/)
- [Blog](https://blog.groupdocs.cloud/category/merger/)
