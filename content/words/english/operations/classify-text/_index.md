---
title: Automatically Classify Text into Categories
articleTitle: Text Classification
linktitle: Text Classification

url: /operations/classify-text/
description: Learn how to categorize raw text snippets without documents using Aspose.Words Cloud API with IAB taxonomy and sentiment analysis.
weight: 20
---

# Automatically Classify Text into Categories

In today's data-driven world, organizations receive vast amounts of unstructured text through customer feedback, support tickets, social media mentions, and more. Making sense of this textual information manually is practically impossible. Text classification offers an efficient solution by automatically categorizing text snippets based on their content, helping organizations derive actionable insights.

## What is Text Classification?

Text classification is the process of analyzing text content and assigning it to predefined categories based on its meaning, subject matter, or sentiment. Unlike document classification, which processes entire documents, text classification works with raw text snippets without requiring document creation or storage.

Think of text classification as having an expert reader who can instantly identify what a text is about. For example, determining whether a customer review expresses positive or negative sentiment, or whether a news snippet relates to sports, politics, or entertainment.

## Text Classification vs. Document Classification

While [document classification](/operations/classify-document/) and text classification share the same underlying technology, they serve different use cases:

| Text Classification | Document Classification |
|---------------------|-------------------------|
| Works with raw text snippets | Requires formatted documents |
| No storage requirements | Document must be created and possibly stored |
| Ideal for real-time processing | Better for batch processing |
| Lightweight and faster | More comprehensive analysis |
| Perfect for APIs and microservices | Suited for document management systems |

## Use Cases for Text Classification

Text classification is incredibly versatile and can be applied in numerous scenarios:

- Customer Support: Categorize incoming support tickets to route them to appropriate departments
- Social Media Monitoring: Classify mentions of your brand by topic and sentiment
- Content Moderation: Automatically filter inappropriate content
- News Aggregation: Categorize news articles by topic for personalized feeds
- Survey Analysis: Categorize open-ended survey responses for easier analysis
- Email Filtering: Sort emails by department, priority, or content type

## Supported Taxonomies

Aspose.Words Cloud API currently supports the following taxonomies for text classification:

1. IAB-2 Taxonomy: The Interactive Advertising Bureau's content categorization system, with categories like "Automotive," "Business," "Technology," etc.
2. Sentiment Analysis:
   - 2-Class: Negative/Positive
   - 3-Class: Negative/Neutral/Positive

## How Text Classification Works

When you send text to the API for classification:

1. The API analyzes the text using natural language processing techniques
2. The system identifies key topics, terminology, and patterns
3. It matches these linguistic features against its taxonomy categories
4. The API returns the best matching categories with confidence scores

## Using the Text Classification API

### REST API Endpoint

| Server | Method | Endpoint |
|--------|--------|----------|
| `https://api.aspose.cloud/v4.0` | PUT | `/words/classify` |

### Request Parameters

| Parameter Name | Data Type | Required/Optional | Description |
|----------------|-----------|-------------------|-------------|
| `bestClassesCount` | string | Optional | Number of best matching classes to return |

### Multipart Form Data

| Property Name | Data Type | Required/Optional | Description |
|---------------|-----------|-------------------|-------------|
| `text` | string | Required | The text to classify |

## Step-By-Step Implementation

Let's see how to implement text classification using different programming approaches:

### Using cURL

The simplest way to test the API is using direct REST calls with cURL:

```bash
# First, get the JWT token
curl -v "https://api.aspose.cloud/connect/token" \
-d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded" \
-H "Accept: application/json"

# Now classify text
curl -v "https://api.aspose.cloud/v4.0/words/classify?bestClassesCount=3" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-H "Content-Type: multipart/form-data" \
-H "Accept: application/json" \
-F "text=Electric cars are becoming increasingly popular as battery technology improves. Tesla, Ford, and Volkswagen are all expanding their electric vehicle lineups." \
--output classification_result.json
```

### Using Python SDK

Python developers can use the Aspose.Words Cloud SDK:

```python
# Import necessary modules
import os
from aspose.words.cloud import WordsApi, Configuration

# Set Aspose.Words Cloud credentials
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
words_api = WordsApi(configuration)

# Text to classify
text_to_classify = """
Electric cars are becoming increasingly popular as battery technology improves. 
Tesla, Ford, and Volkswagen are all expanding their electric vehicle lineups.
"""

# Set number of classes to return
best_classes_count = 3

# Execute classification
result = words_api.classify(text_to_classify, best_classes_count)

# Process classification results
print("Classification Results:")
for class_info in result.best_classes:
    print(f"Class: {class_info.class_name}, Score: {class_info.score}")
```

### Using Java SDK

Java developers can use the Aspose.Words Cloud SDK for Java:

```java
import com.aspose.words.cloud.ApiClient;
import com.aspose.words.cloud.ApiException;
import com.aspose.words.cloud.Configuration;
import com.aspose.words.cloud.WordsApi;
import com.aspose.words.cloud.model.*;

public class ClassifyText {
    public static void main(String[] args) throws Exception {
        // Configure API client
        ApiClient apiClient = Configuration.getDefaultApiClient();
        apiClient.setClientId("YOUR_CLIENT_ID");
        apiClient.setClientSecret("YOUR_CLIENT_SECRET");
        
        WordsApi wordsApi = new WordsApi(apiClient);
        
        // Prepare text for classification
        String textToClassify = "Electric cars are becoming increasingly popular as battery technology improves. "
                + "Tesla, Ford, and Volkswagen are all expanding their electric vehicle lineups.";
        
        // Set number of classes to return
        Integer bestClassesCount = 3;
        
        // Classify text
        ClassificationResponse result = wordsApi.classify(textToClassify, bestClassesCount);
        
        // Process results
        System.out.println("Classification Results:");
        for (ClassificationResult classInfo : result.getBestClasses()) {
            System.out.println("Class: " + classInfo.getClassName() + ", Score: " + classInfo.getScore());
        }
    }
}
```

### Using C# SDK

C# developers can use the Aspose.Words Cloud SDK for .NET:

```csharp
using System;
using Aspose.Words.Cloud.Sdk;
using Aspose.Words.Cloud.Sdk.Model;
using Aspose.Words.Cloud.Sdk.Model.Requests;

namespace TextClassificationExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Configure API client
            var config = new Configuration
            {
                ClientId = "YOUR_CLIENT_ID",
                ClientSecret = "YOUR_CLIENT_SECRET"
            };
            
            var wordsApi = new WordsApi(config);
            
            // Text to classify
            var textToClassify = @"Electric cars are becoming increasingly popular as battery technology improves. 
                Tesla, Ford, and Volkswagen are all expanding their electric vehicle lineups.";
            
            // Set number of classes to return
            var bestClassesCount = 3;
            
            // Classify text
            var request = new ClassifyRequest(textToClassify, bestClassesCount);
            var result = wordsApi.Classify(request);
            
            // Process results
            Console.WriteLine("Classification Results:");
            foreach (var classInfo in result.BestClasses)
            {
                Console.WriteLine($"Class: {classInfo.ClassName}, Score: {classInfo.Score}");
            }
        }
    }
}
```

## Interpreting Classification Results

The API returns classification results as a JSON object containing:

1. Best Classes: A list of the top matching categories based on your `bestClassesCount` parameter
2. Class Name: The name of the identified category
3. Score: A confidence score between 0 and 1 indicating how well the text matches the category

For example, classifying our electric car text might return:

```json
{
  "bestClasses": [
    {
      "className": "Automotive",
      "score": 0.87
    },
    {
      "className": "Technology & Computing",
      "score": 0.65
    },
    {
      "className": "Business",
      "score": 0.32
    }
  ]
}
```

This indicates the text is primarily about Automotive (0.87 confidence) with significant relevance to Technology (0.65) and some Business aspects (0.32).

## Tips for Effective Text Classification

- Provide sufficient context: Longer text snippets generally yield more accurate classifications
- Be specific: Vague or ambiguous text may result in lower confidence scores
- Consider multi-classification: Some text naturally belongs to multiple categories
- Review confidence scores: Higher scores (closer to 1.0) indicate stronger matches
- Test with known samples: Validate classification accuracy with pre-categorized text

## Real-World Application Example

Let's consider a practical example: automatically routing customer support tickets.

```python
import os
from aspose.words.cloud import WordsApi, Configuration

# Set Aspose.Words Cloud credentials
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
words_api = WordsApi(configuration)

def route_support_ticket(ticket_text):
    """Route a support ticket based on content classification."""
    # Classify the ticket text
    result = words_api.classify(ticket_text, 1)  # Get the top category
    
    # Map classification to department
    top_category = result.best_classes[0].class_name
    confidence = result.best_classes[0].score
    
    # Routing logic
    if top_category == "Technology & Computing" and confidence > 0.7:
        return "Technical Support"
    elif top_category == "Business" and confidence > 0.7:
        return "Billing Department"
    elif top_category == "Shopping" and confidence > 0.7:
        return "Sales Support"
    else:
        return "General Inquiries"  # Default route

# Example tickets
tickets = [
    "My software keeps crashing whenever I try to save my work. I've already reinstalled it twice.",
    "I need to update my credit card information for my monthly subscription.",
    "Do you offer discounts for educational institutions? We need 50 licenses."
]

# Route each ticket
for i, ticket in enumerate(tickets):
    department = route_support_ticket(ticket)
    print(f"Ticket #{i+1} routed to: {department}")
```

## Going Further

Here are some ways to extend text classification in your applications:

- Multilingual Classification: Process text in different languages
- Custom Classification Models: Train models on your specific text data
- Hierarchical Classification: Categorize text at multiple levels of specificity
- Automated Workflows: Trigger actions based on classification results
- Hybrid Approaches: Combine automatic classification with human verification for critical tasks
- Sentiment Analysis: Analyze customer feedback tone for emotional content

## See Also

- [Classify a Document](/classification/document/) 
- [GitHub Repository](https://github.com/aspose-words-cloud)
- [Product Page](https://products.aspose.cloud/words/)
- [Documentation](https://docs.aspose.cloud/words/)
- [Live Demo](https://products.aspose.app/words/family)
- [API Reference](https://reference.aspose.cloud/words/)
- [Blog](https://blog.aspose.cloud/category/words/)
- [Free Support](https://forum.aspose.cloud/c/words/17)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
