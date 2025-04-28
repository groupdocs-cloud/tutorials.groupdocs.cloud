---
title: Using AI-Powered Name API with Aspose.Email Cloud Tutorial
url: /email-cloud/name-api/
description: Learn how to use the AI-powered Name API in Aspose.Email Cloud to detect gender, format names, compare names, and more in this comprehensive tutorial.
weight: 16
---

## Learning Objectives

In this tutorial, you'll learn how to:
- Detect a person's gender based on their name
- Format names according to specific patterns
- Compare names to determine similarity
- Expand names into possible variants
- Complete names based on initials or prefixes
- Parse names from email addresses
- Implement these AI capabilities in multiple programming languages

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account (sign up at [dashboard.aspose.cloud](https://dashboard.aspose.cloud/))
2. Your Aspose Cloud credentials (Client ID and Client Secret)
3. The Aspose.Email Cloud SDK for your preferred programming language installed
4. Basic understanding of name processing concepts

## Introduction to AI-Powered Name API

The Name API is an artificial intelligence feature developed by Aspose that provides powerful capabilities for processing and analyzing personal names. This modern approach offers numerous functions for working with names in various contexts.

The AI-powered Name API can be particularly useful for:
- Customer relationship management (CRM) systems
- User registration forms
- Email personalization
- Contact management systems
- Data cleansing and standardization
- Identity verification workflows

## Tutorial Steps

### Step 1: Set Up Your Development Environment

First, ensure you have the Aspose.Email Cloud SDK installed for your preferred programming language. If you haven't done this yet, refer to our [SDK Setup Tutorial](/email-cloud/sdk-setup/).

### Step 2: Initialize the API Client

Create and configure the API client with your credentials:

```python
# Python example
import os
from asposeemail.api_client import ApiClient
from asposeemail.api.email_cloud import EmailCloud
from asposeemail.configuration import Configuration

# Configure API key authorization
configuration = Configuration(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET"
)

# Create API client
api = EmailCloud(configuration)
```

### Step 3: Detect a Person's Gender by Name

One of the most useful capabilities of the Name API is gender detection. This can help personalize communications by using the correct pronouns and salutations:

```python
# Python
from asposeemail.models import AiNameGenderizeRequest

# Detect gender for a given name
name = "John Cane"
result = api.ai.name.genderize(
    AiNameGenderizeRequest(name))

# Print the detected gender
if result.value and len(result.value) > 0:
    # Get the first (most likely) hypothesis
    first_hypothesis = result.value[0]
    print(f"Name: {name}")
    print(f"Detected gender: {first_hypothesis.gender}")
    print(f"Score: {first_hypothesis.score}")
    
    # If there are multiple hypotheses, show them all
    if len(result.value) > 1:
        print("\nAll hypotheses:")
        for i, hypothesis in enumerate(result.value):
            print(f"{i+1}. Gender: {hypothesis.gender}, Score: {hypothesis.score}")
else:
    print("No gender could be determined")
```

Let's look at this example in other programming languages:

### C# Example

```csharp
using System;
using System.Linq;
using Aspose.Email.Cloud.Sdk.Api;
using Aspose.Email.Cloud.Sdk.Model;

// Initialize API client
var api = new EmailCloud("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Detect gender for a given name
string name = "John Cane";
var result = await api.Ai.Name.GenderizeAsync(
    new AiNameGenderizeRequest(name));

// Print the detected gender
if (result.Value != null && result.Value.Count > 0)
{
    // Get the first (most likely) hypothesis
    var firstHypothesis = result.Value.First();
    Console.WriteLine($"Name: {name}");
    Console.WriteLine($"Detected gender: {firstHypothesis.Gender}");
    Console.WriteLine($"Score: {firstHypothesis.Score}");
    
    // If there are multiple hypotheses, show them all
    if (result.Value.Count > 1)
    {
        Console.WriteLine("\nAll hypotheses:");
        for (int i = 0; i < result.Value.Count; i++)
        {
            var hypothesis = result.Value[i];
            Console.WriteLine($"{i+1}. Gender: {hypothesis.Gender}, Score: {hypothesis.Score}");
        }
    }
}
else
{
    Console.WriteLine("No gender could be determined");
}
```


### Java Example

```java
import com.aspose.email.cloud.sdk.api.EmailCloud;
import com.aspose.email.cloud.sdk.model.*;
import java.util.List;

// Initialize API client
EmailCloud api = new EmailCloud("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Detect gender for a given name
String name = "John Cane";
ListResponseOfAiNameGenderHypothesis result = api.ai().name().genderize(
    new AiNameGenderizeRequest().name(name));

// Print the detected gender
List<AiNameGenderHypothesis> hypotheses = result.getValue();
if (hypotheses != null && !hypotheses.isEmpty()) {
    // Get the first (most likely) hypothesis
    AiNameGenderHypothesis firstHypothesis = hypotheses.get(0);
    System.out.println("Name: " + name);
    System.out.println("Detected gender: " + firstHypothesis.getGender());
    System.out.println("Score: " + firstHypothesis.getScore());
    
    // If there are multiple hypotheses, show them all
    if (hypotheses.size() > 1) {
        System.out.println("\nAll hypotheses:");
        for (int i = 0; i < hypotheses.size(); i++) {
            AiNameGenderHypothesis hypothesis = hypotheses.get(i);
            System.out.println((i+1) + ". Gender: " + hypothesis.getGender() + 
                               ", Score: " + hypothesis.getScore());
        }
    }
} else {
    System.out.println("No gender could be determined");
}
```


The API analyzes the name and returns a list of gender hypotheses, each with a confidence score. The first hypothesis in the list is typically the most likely.

### Step 4: Compare Names to Determine Similarity

The Name API can compare two names to determine if they likely belong to the same person. This is extremely useful for duplicate detection and verification processes:

```python
# Python
from asposeemail.models import AiNameMatchRequest

# Define two names to compare
first_name = "John Michael Cane"
second_name = "Cane J."

# Compare the names
result = api.ai.name.match(
    AiNameMatchRequest(first_name, second_name))

# Print the results
print(f"Comparing '{first_name}' with '{second_name}':")
print(f"Similarity score: {result.similarity}")

# A score above 0.5 typically indicates the same person
if result.similarity > 0.5:
    print("These names likely belong to the same person")
else:
    print("These names likely belong to different people")

# If there are mismatches, show them
if result.mismatches:
    print("\nMismatches found:")
    for mismatch in result.mismatches:
        print(f"- {mismatch}")
```

### C# Example

```csharp
// Define two names to compare
string firstName = "John Michael Cane";
string secondName = "Cane J.";

// Compare the names
var result = await api.Ai.Name.MatchAsync(
    new AiNameMatchRequest(firstName, secondName));

// Print the results
Console.WriteLine($"Comparing '{firstName}' with '{secondName}':");
Console.WriteLine($"Similarity score: {result.Similarity}");

// A score above 0.5 typically indicates the same person
if (result.Similarity > 0.5)
{
    Console.WriteLine("These names likely belong to the same person");
}
else
{
    Console.WriteLine("These names likely belong to different people");
}

// If there are mismatches, show them
if (result.Mismatches != null && result.Mismatches.Count > 0)
{
    Console.WriteLine("\nMismatches found:");
    foreach (var mismatch in result.Mismatches)
    {
        Console.WriteLine($"- {mismatch}");
    }
}
```


The API returns a similarity score between 0 and 1, where higher values indicate greater similarity. It also provides details about specific mismatches between the names.

### Step 5: Format Names According to a Pattern

The Name API can format names according to specific patterns, which is useful for standardizing name presentation across applications:

```python
# Python
from asposeemail.models import AiNameFormatRequest

# Define a name to format
name = "Mr. John Michael Cane"

# Format the name according to a pattern
# Format specifiers:
# %t - Title (Mr., Mrs., etc.)
# %f - First name
# %m - Middle name(s)
# %L - Last name
# %i - Initials
format_pattern = "%t%L%f%m"  # Example: "Mr. Cane J. M."

result = api.ai.name.format(
    AiNameFormatRequest(
        name=name,
        format=format_pattern))

# Print the formatted name
print(f"Original name: {name}")
print(f"Formatted name: {result.name}")
```

### Java Example

```java
// Define a name to format
String name = "Mr. John Michael Cane";

// Format the name according to a pattern
// Format specifiers:
// %t - Title (Mr., Mrs., etc.)
// %f - First name
// %m - Middle name(s)
// %L - Last name
// %i - Initials
String formatPattern = "%t%L%f%m";  // Example: "Mr. Cane J. M."

AiNameFormatted result = api.ai().name().format(
    new AiNameFormatRequest()
        .name(name)
        .format(formatPattern));

// Print the formatted name
System.out.println("Original name: " + name);
System.out.println("Formatted name: " + result.getName());
```


Format patterns use special placeholders to indicate different parts of the name:
- `%t` - Title (Mr., Mrs., etc.)
- `%f` - First name
- `%m` - Middle name(s)
- `%L` - Last name
- `%i` - Initials

This allows for flexible name formatting to match your application's requirements.

### Step 6: Expand a Name into Possible Variants

The Name API can generate a list of possible name variants:

```python
# Python
from asposeemail.models import AiNameExpandRequest

# Define a name to expand
name = "Smith Bobby"

# Expand the name into possible variants
result = api.ai.name.expand(
    AiNameExpandRequest(name))

# Print the expanded name variants
print(f"Original name: {name}")
print(f"Expanded into {len(result.names)} variants:")

for i, variant in enumerate(result.names):
    print(f"{i+1}. {variant.name} (Score: {variant.score})")

# You can also filter for specific variants
print("\nSome notable variants:")
for variant in result.names:
    if "Mr." in variant.name or "B." in variant.name:
        print(f"- {variant.name}")
```

### C# Example

```csharp
// Define a name to expand
string name = "Smith Bobby";

// Expand the name into possible variants
var result = await api.Ai.Name.ExpandAsync(
    new AiNameExpandRequest(name));

// Print the expanded name variants
Console.WriteLine($"Original name: {name}");
Console.WriteLine($"Expanded into {result.Names.Count} variants:");

for (int i = 0; i < result.Names.Count; i++)
{
    var variant = result.Names[i];
    Console.WriteLine($"{i+1}. {variant.Name} (Score: {variant.Score})");
}

// You can also filter for specific variants
Console.WriteLine("\nSome notable variants:");
foreach (var variant in result.Names)
{
    if (variant.Name.Contains("Mr.") || variant.Name.Contains("B."))
    {
        Console.WriteLine($"- {variant.Name}");
    }
}
```


This functionality is useful for applications that need to search for a person's name in different formats, such as finding matches in databases or documents where the name might be written in different ways.

### Step 7: Complete a Name Based on Initials or Prefixes

The Name API can suggest completions for partial names or initials:

```python
# Python
from asposeemail.models import AiNameCompleteRequest

# Define a name prefix to complete
prefix = "Dav"

# Get name completions
result = api.ai.name.complete(
    AiNameCompleteRequest(prefix))

# Print the possible completions
print(f"Possible completions for '{prefix}':")

for i, completion in enumerate(result.names):
    full_name = prefix + completion.name
    print(f"{i+1}. {full_name} (Score: {completion.score})")
```

### Java Example

```java
// Define a name prefix to complete
String prefix = "Dav";

// Get name completions
AiNameWeightedVariants result = api.ai().name().complete(
    new AiNameCompleteRequest().name(prefix));

// Print the possible completions
System.out.println("Possible completions for '" + prefix + "':");

for (int i = 0; i < result.getNames().size(); i++) {
    AiNameWeighted completion = result.getNames().get(i);
    String fullName = prefix + completion.getName();
    System.out.println((i+1) + ". " + fullName + " (Score: " + completion.getScore() + ")");
}
```


This feature is particularly useful for autocomplete functionality in forms or search fields, similar to the recipient field in email clients.

### Step 8: Parse Names from Email Addresses

The Name API can extract and parse names from email addresses:

```python
# Python
from asposeemail.models import AiNameParseEmailAddressRequest
import functools  # For reduce function

# Define an email address to parse
email_address = "john-cane@gmail.com"

# Parse the name from the email address
result = api.ai.name.parse_email_address(
    AiNameParseEmailAddressRequest(email_address))

# Process the extracted values
if result.value and len(result.value) > 0:
    # Combine all extracted name components
    extracted_components = []
    for extracted in result.value:
        extracted_components.extend(extracted.name)
    
    # Print all extracted components
    print(f"Parsed name components from {email_address}:")
    for component in extracted_components:
        print(f"- {component.category}: {component.value}")
    
    # Find specific components
    given_name = next((x for x in extracted_components if x.category == 'GivenName'), None)
    surname = next((x for x in extracted_components if x.category == 'Surname'), None)
    
    if given_name and surname:
        print(f"\nFull name: {given_name.value} {surname.value}")
else:
    print(f"Could not parse a name from {email_address}")
```

### C# Example

```csharp
// Define an email address to parse
string emailAddress = "john-cane@gmail.com";

// Parse the name from the email address
var result = await api.Ai.Name.ParseEmailAddressAsync(
    new AiNameParseEmailAddressRequest(emailAddress));

// Process the extracted values
if (result.Value != null && result.Value.Count > 0)
{
    // Combine all extracted name components
    var extractedComponents = new List<AiNameExtractedComponent>();
    foreach (var extracted in result.Value)
    {
        extractedComponents.AddRange(extracted.Name);
    }
    
    // Print all extracted components
    Console.WriteLine($"Parsed name components from {emailAddress}:");
    foreach (var component in extractedComponents)
    {
        Console.WriteLine($"- {component.Category}: {component.Value}");
    }
    
    // Find specific components
    var givenName = extractedComponents.FirstOrDefault(x => x.Category == "GivenName");
    var surname = extractedComponents.FirstOrDefault(x => x.Category == "Surname");
    
    if (givenName != null && surname != null)
    {
        Console.WriteLine($"\nFull name: {givenName.Value} {surname.Value}");
    }
}
else
{
    Console.WriteLine($"Could not parse a name from {emailAddress}");
}
```


This feature is particularly useful for systems that receive email addresses but need to address users by their actual names. It can intelligently extract first and last names from various email address formats.

### Step 9: Parse a Full Name into Components

You can also parse a complete name into its components:

```python
# Python
from asposeemail.models import AiNameParseRequest

# Define a name to parse
full_name = "Dr. John Michael Cane Jr."

# Parse the name into components
result = api.ai.name.parse(
    AiNameParseRequest(full_name))

# Print the parsed components
if result.value and len(result.value) > 0:
    print(f"Parsed components from '{full_name}':")
    for extracted in result.value:
        for component in extracted.name:
            print(f"- {component.category}: {component.value}")
else:
    print(f"Could not parse components from '{full_name}'")
```

### Java Example

```java
// Define a name to parse
String fullName = "Dr. John Michael Cane Jr.";

// Parse the name into components
ListResponseOfAiNameExtracted result = api.ai().name().parse(
    new AiNameParseRequest().name(fullName));

// Print the parsed components
if (result.getValue() != null && !result.getValue().isEmpty()) {
    System.out.println("Parsed components from '" + fullName + "':");
    for (AiNameExtracted extracted : result.getValue()) {
        for (AiNameExtractedComponent component : extracted.getName()) {
            System.out.println("- " + component.getCategory() + ": " + component.getValue());
        }
    }
} else {
    System.out.println("Could not parse components from '" + fullName + "'");
}
```


This parsing functionality breaks down a full name into its constituent parts (title, first name, middle name, last name, suffix, etc.), which can be useful for database storage or form processing.

## Practical Application Examples

Let's look at some real-world examples of how to use the Name API in applications:

### Example 1: Personalized Email Greeting Generator

```python
# Python
def generate_personalized_greeting(api, full_name):
    """Generate a personalized email greeting based on name analysis."""
    # Detect gender
    gender_result = api.ai.name.genderize(AiNameGenderizeRequest(full_name))
    gender = gender_result.value[0].gender if gender_result.value else "Unknown"
    
    # Parse name components
    parse_result = api.ai.name.parse(AiNameParseRequest(full_name))
    
    # Extract components
    first_name = None
    title = None
    if parse_result.value and len(parse_result.value) > 0:
        components = parse_result.value[0].name
        first_name = next((c.value for c in components if c.category == "GivenName"), None)
        title = next((c.value for c in components if c.category == "Title"), None)
    
    # Default to full name if parsing fails
    first_name = first_name or full_name
    
    # Generate appropriate greeting
    if title:
        return f"Dear {title} {full_name},"
    elif gender == "Male":
        return f"Dear Mr. {full_name},"
    elif gender == "Female":
        return f"Dear Ms. {full_name},"
    else:
        return f"Dear {first_name},"

# Usage
greeting = generate_personalized_greeting(api, "John Smith")
print(greeting)  # "Dear Mr. John Smith," or "Dear John,"
```

### Example 2: Duplicate Contact Detector

```python
# Python
def check_for_duplicate_contacts(api, contacts):
    """
    Check a list of contact names for potential duplicates.
    Returns pairs of names that might be the same person.
    """
    potential_duplicates = []
    
    # Compare each pair of names
    for i in range(len(contacts)):
        for j in range(i + 1, len(contacts)):
            name1 = contacts[i]
            name2 = contacts[j]
            
            # Compare names
            result = api.ai.name.match(AiNameMatchRequest(name1, name2))
            
            # If similarity is high, consider them potential duplicates
            if result.similarity > 0.7:  # Higher threshold for more confidence
                potential_duplicates.append((name1, name2, result.similarity))
    
    return potential_duplicates

# Usage
contacts = [
    "John M. Smith",
    "J. Smith",
    "Mary Johnson",
    "Johnson, Mary",
    "Robert Brown",
    "Bob Brown"
]

duplicates = check_for_duplicate_contacts(api, contacts)
for name1, name2, similarity in duplicates:
    print(f"Potential duplicate: '{name1}' and '{name2}' (Similarity: {similarity:.2f})")
```

## Troubleshooting Tips

If you encounter issues with the Name API:

1. Unexpected gender detection:
   - Remember that gender detection is based on statistical analysis and may not be accurate for all names
   - Some names are gender-neutral or have different gender associations in different cultures
   - Try using more complete names for better accuracy

2. Low similarity scores:
   - Consider cultural and formatting differences in names
   - Names with nicknames or abbreviated parts may have lower similarity scores
   - Try normalizing names before comparison

3. Name parsing issues:
   - Uncommon name formats might not parse correctly
   - Names from certain cultures may follow different patterns
   - Provide more context when possible (like title or suffix)

4. Rate limiting:
   - The API has rate limits, so batch your requests when processing many names
   - Consider caching results for frequently used names

## What You've Learned

In this tutorial, you've learned how to:
- Detect a person's gender based on their name
- Format names according to specific patterns
- Compare names to determine similarity
- Expand names into possible variants
- Complete names based on initials or prefixes
- Parse names from email addresses
- Implement these AI capabilities in your applications

## Further Practice

To reinforce your learning:
1. Build a name standardization system for a contact database
2. Create an intelligent form that suggests name completions as users type
3. Develop a duplicate contact detection feature
4. Implement personalized email greeting generation
5. Create a name parser for importing contacts from various sources

## Next Steps

Consider exploring these related tutorials:
- [Quick Start With Email Client](/email-cloud/quick-start-with-email-client/) - Apply name processing to email communications

## Helpful Resources

- [Product Page](https://products.aspose.cloud/email/)
- [Documentation](https://docs.aspose.cloud/email/)
- [Live Demo](https://products.aspose.app/email/family)
- [API Reference](https://reference.aspose.cloud/email/)
- [Blog](https://blog.aspose.cloud/category/email/)
- [Free Support](https://forum.aspose.cloud/c/email/9/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out to us on the [Aspose.Email Cloud forum](https://forum.aspose.cloud/c/email/9/).