---
title: Managing Track Changes in Word Documents
articleTitle: Accept or Reject Document Revisions
linktitle: Track Changes

url: /operations/track-changes/
description: Learn how to programmatically manage tracked changes in Word documents using Aspose.Words Cloud API. Accept or reject revisions with simple API calls.
weight: 40
---

# Managing Track Changes in Word Documents

When collaborating on documents, the Track Changes feature in Word is essential for reviewing edits made by different contributors. As a developer, you may need to programmatically manage these changes — accepting or rejecting revisions based on specific criteria or as part of an automated workflow.

Aspose.Words Cloud API provides powerful endpoints to handle tracked changes in Word documents, allowing you to:

- Accept all revisions in a document
- Reject all revisions in a document
- Selectively manage individual revisions

In this tutorial, we'll explore how to use these operations in real-world scenarios.

## Understanding Track Changes

Before diving into the API calls, let's understand what happens when you work with tracked changes:

When changes are tracked in a Word document, each modification (insertion, deletion, formatting change, etc.) is stored as a "revision" within the document. These revisions contain:

- The content that was changed
- The type of change (insertion, deletion, etc.)
- Who made the change
- When the change was made

When you accept a revision, the change becomes permanent part of the document. When you reject a revision, the document reverts to its state before that change was made.

## Accepting All Revisions in a Document

Let's start with a common scenario: you've received a document with numerous tracked changes that you want to accept all at once.

### REST API Details

To accept all revisions in a document, use the following API endpoint:

| Server | Method | Endpoint |
|--------|--------|----------|
| `https://api.aspose.cloud/v4.0` | PUT | `/words/online/put/revisions/acceptAll` |

This operation can be performed on a document that's either already in cloud storage or provided directly in the request.

### Request Parameters

When making the API call, you can use these parameters:

| Parameter Name | Data Type | Required/Optional | Description |
|----------------|-----------|-------------------|-------------|
| `loadEncoding` | string | Optional | Encoding to use when loading the document |
| `password` | string | Optional | Password for protected documents |
| `encryptedPassword` | string | Optional | Encrypted password for protected documents |
| `destFileName` | string | Optional | Name for the resulting document after the operation |

For the request body, use `multipart/form-data` with:

| Property Name | Data Type | Required/Optional | Description |
|---------------|-----------|-------------------|-------------|
| `document` | string(binary) | Required | The document file |

### Example: Accepting All Revisions

Imagine you're building a document approval system where, after all reviewers have added their comments and edits, a final approval process accepts all changes. Here's how you would implement this:

#### Using cURL

```bash
# Note: The JWT token is obtained by following the quickstart guide
curl -X PUT "https://api.aspose.cloud/v4.0/words/online/put/revisions/acceptAll" \
-H "accept: application/json" \
-H "Authorization: Bearer <JWT_TOKEN>" \
-H "Content-Type: multipart/form-data" \
-F "document=@ReviewedDocument.docx" \
-o "FinalDocument.docx"
```

#### Using Python SDK

```python
# Import the SDK components
import os
import sys
from asposeworkscloud import WordsApi, ApiClient, Configuration, models

# Set up authentication
configuration = Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API client
api_client = ApiClient(configuration)
words_api = WordsApi(api_client)

# Open the document file
with open("ReviewedDocument.docx", "rb") as file:
    document_data = file.read()

# Accept all revisions
result = words_api.accept_all_revisions_online(
    document=document_data
)

# Save the result
with open("FinalDocument.docx", "wb") as file:
    file.write(result.document)

print("Successfully accepted all revisions in the document.")
```

## Rejecting All Revisions in a Document

In some scenarios, you might need to reject all proposed changes in a document. For example, if a review cycle needs to be restarted or if the changes don't meet quality requirements.

### REST API Details

To reject all revisions in a document, use:

| Server | Method | Endpoint |
|--------|--------|----------|
| `https://api.aspose.cloud/v4.0` | PUT | `/words/online/put/revisions/rejectAll` |

The parameters for this endpoint are identical to the acceptAll endpoint shown above.

### Example: Rejecting All Revisions

Here's how you would implement a feature to reject all changes in a document:

#### Using Java SDK

```java
// Import the SDK components
import com.aspose.words.cloud.ApiClient;
import com.aspose.words.cloud.ApiException;
import com.aspose.words.cloud.Configuration;
import com.aspose.words.cloud.model.*;
import com.aspose.words.cloud.api.WordsApi;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class RejectAllRevisions {
    public static void main(String[] args) throws ApiException, IOException {
        // Set up authentication
        ApiClient apiClient = new Configuration(
            "YOUR_CLIENT_ID",
            "YOUR_CLIENT_SECRET"
        ).buildApiClient();
        
        WordsApi wordsApi = new WordsApi(apiClient);
        
        // Read the document
        File document = new File("ReviewedDocument.docx");
        byte[] documentData = Files.readAllBytes(document.toPath());
        
        // Reject all revisions
        RejectAllRevisionsOnlineResponse response = wordsApi.rejectAllRevisionsOnline(
            new RejectAllRevisionsOnlineRequest(documentData, null, null, null, null)
        );
        
        // Save the result
        try (FileOutputStream outputStream = new FileOutputStream("CleanDocument.docx")) {
            outputStream.write(response.getDocument());
        }
        
        System.out.println("Successfully rejected all revisions in the document.");
    }
}
```

## Best Practices for Managing Track Changes

When working with track changes operations, consider these best practices:

1. Always back up original documents before performing accept/reject operations, as these changes are permanent.

2. Consider user permissions in your application design. Only authorized users should be able to accept or reject changes.

3. Provide clear feedback to users about what happened during the operation, such as the number of revisions that were processed.

4. Implement selective revision management for more granular control when needed, rather than accepting or rejecting all changes at once.

5. Use the appropriate content type in your requests. For most operations with Aspose.Words Cloud, use `multipart/form-data` when sending document files.

## Going Further

You can extend your document revision management implementation with these ideas:

- Create a revision approval workflow that routes documents to specific users for review.
- Implement a hybrid approach that selectively accepts revisions from certain users while rejecting others.
- Add a comment summary that logs all changes that were accepted or rejected for auditing purposes.
- Build a document comparison feature that shows users the differences between the original and revised versions.

## Helpful Resources

- [Product Page](https://products.aspose.cloud/words/)
- [Documentation](https://docs.aspose.cloud/words/)
- [Live Demo](https://products.aspose.app/words/family)
- [API Reference](https://reference.aspose.cloud/words/)
- [Blog](https://blog.aspose.cloud/category/words/)
- [Free Support](https://forum.aspose.cloud/c/words/17)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
