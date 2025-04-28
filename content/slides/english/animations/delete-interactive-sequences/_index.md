---
title: Delete Interactive Animation Sequences Tutorial
url: /animations/delete-interactive-sequences/
description: Learn how to delete interactive animation sequences from PowerPoint slides using Aspose.Slides Cloud API with step-by-step code examples and tips.
weight: 60
---

## Learning Objectives

In this tutorial, you'll learn how to programmatically delete interactive animation sequences from PowerPoint slides using Aspose.Slides Cloud API. By the end of this guide, you'll be able to:

- Remove all interactive sequences at once while keeping main sequences intact
- Delete specific interactive sequences by index
- Verify the deletion through response validation
- Implement these operations in different programming languages

## Prerequisites

Before beginning this tutorial, ensure you have:

1. An Aspose Cloud account with an active subscription
2. Your Client ID and Client Secret credentials
3. A PowerPoint presentation with interactive animation sequences
4. Basic understanding of REST API concepts
5. Familiarity with your preferred programming language

## Understanding Interactive Sequences

Interactive sequences in PowerPoint are animations triggered by specific user actions, such as clicking on a shape. Unlike main sequences that follow a predetermined order, interactive sequences are event-driven. This tutorial shows you how to selectively remove these interactive elements while maintaining other animations if desired.

## Step-by-Step Guide

### Step 1: Authenticate with the API

Before deleting interactive sequences, you'll need to authenticate with the Aspose.Slides Cloud API:

```bash
# Request an access token
curl -X POST "https://api.aspose.cloud/connect/token" \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded"
```

This will provide you with an access token to use in subsequent requests.

### Step 2: Identify Your Target

You have two options for deleting interactive sequences:

1. Delete all interactive sequences from a slide
2. Delete a specific interactive sequence by its index

For either operation, you'll need:
- The name of your presentation file
- The slide index (1-based) containing the sequences

For deleting a specific sequence, you'll also need:
- The sequence index (1-based) of the interactive sequence to delete

### Step 3: Delete Interactive Sequences

#### Option 1: Delete ALL Interactive Sequences

To remove all interactive sequences from a slide:

```bash
curl -X DELETE "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation/interactiveSequences" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### Option 2: Delete a SPECIFIC Interactive Sequence

To remove just one interactive sequence (e.g., the second sequence):

```bash
curl -X DELETE "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation/interactiveSequences/2" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### Try it yourself

Replace `YOUR_ACCESS_TOKEN`, and if needed, change the presentation name and indexes to match your specific scenario.

### Step 4: Understand the Response

After successful deletion, the API will return a JSON response showing the updated animation structure. Here's an example response after deleting the second interactive sequence:

```json
{
  "mainSequence": [
    {
      "type": "Fade",
      "subtype": "None",
      "presetClassType": "Entrance",
      "shapeIndex": 2,
      "duration": 0.5,
      "triggerDelayTime": 0.0
    }
  ],
  "interactiveSequences": [
    {
      "effects": [
        {
          "type": "Fly",
          "subtype": "Bottom",
          "presetClassType": "Entrance",
          "shapeIndex": 4,
          "triggerType": "OnClick",
          "duration": 0.5,
          "triggerDelayTime": 0.0
        }
      ],
      "triggerShapeIndex": 3
    }
  ],
  "selfUri": {
    "href": "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation",
    "relation": "self",
    "slideIndex": 1
  }
}
```

Note how the response shows:
- The `mainSequence` remains intact
- The `interactiveSequences` array now has only one sequence (the first one)
- The sequence that was at index 2 has been removed

### Step 5: Implement in Your Application

Here are examples of how to implement this functionality in various programming languages:

#### C# Example (Delete a Specific Interactive Sequence)

```csharp
using System;
using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Model;

class Application
{
    static void Main(string[] args)
    {
        // Setup API client with your credentials
        SlidesApi slidesApi = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

        // Define parameters
        string fileName = "MyPresentation.pptx";
        int slideIndex = 1;
        int sequenceIndex = 2; // Delete the second interactive sequence

        // Delete the specific interactive sequence
        SlideAnimation slideAnimation = slidesApi.DeleteAnimationInteractiveSequence(
            fileName, slideIndex, sequenceIndex);

        // Verify the result
        int mainSequenceEffectCount = slideAnimation.MainSequence.Count;
        int interactiveSequenceCount = slideAnimation.InteractiveSequences.Count;

        Console.WriteLine("Number of effects in the main sequence: " + mainSequenceEffectCount);
        Console.WriteLine("Number of interactive sequences: " + interactiveSequenceCount);
    }
}
```

#### Python Example (Delete All Interactive Sequences)

```python
from asposeslidescloud.apis.slides_api import SlidesApi

# Setup API client with your credentials
slides_api = SlidesApi(None, "YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET")

# Define parameters
file_name = "MyPresentation.pptx"
slide_index = 1

# Delete all interactive sequences
slide_animation = slides_api.delete_animation_interactive_sequences(file_name, slide_index)

# Verify the result
main_sequence_effect_count = len(slide_animation.main_sequence)
interactive_sequence_count = len(slide_animation.interactive_sequences)

print("Number of effects in the main sequence:", main_sequence_effect_count)
print("Number of interactive sequences:", interactive_sequence_count)  # Should be 0
```

#### Node.js Example (Delete a Specific Interactive Sequence)

```javascript
const cloudSdk = require("asposeslidescloud");

// Setup API client with your credentials
const slidesApi = new cloudSdk.SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Define parameters
const fileName = "MyPresentation.pptx";
const slideIndex = 1;
const sequenceIndex = 1; // Delete the first interactive sequence

// Delete the specific interactive sequence
slidesApi.deleteAnimationInteractiveSequence(fileName, slideIndex, sequenceIndex)
    .then(slideAnimation => {
        // Verify the result
        const mainSequenceEffectCount = slideAnimation.body.mainSequence.length;
        const interactiveSequenceCount = slideAnimation.body.interactiveSequences.length;

        console.log("Number of effects in the main sequence:", mainSequenceEffectCount);
        console.log("Number of interactive sequences:", interactiveSequenceCount);
    });
```

## Understanding Sequence Indexing

When working with interactive sequences, remember these important points:

1. Indexes are 1-based: The first sequence is index 1, not 0
2. Indexes shift after deletion: If you delete sequence 1, what was sequence 2 becomes the new sequence 1
3. Check before deleting: It's good practice to check the number of interactive sequences before attempting to delete by index

## Troubleshooting

### Common Issues and Solutions

1. Authentication Errors
   - Ensure your Client ID and Client Secret are correct
   - Verify that your access token is still valid (they typically expire after 1 hour)

2. 404 Not Found Errors
   - Check that the presentation file exists in the specified location
   - Verify that the slide and sequence indexes are valid (they are 1-based, not 0-based)

3. Index Out of Range Errors
   - Confirm the number of sequences before attempting to delete by index
   - Remember that indexes shift after deletion, so adjust accordingly

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the Aspose.Slides Cloud API
- Delete all interactive sequences from a PowerPoint slide
- Delete a specific interactive sequence by index
- Process and interpret the API response
- Handle sequence indexing correctly
- Implement this functionality in multiple programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Create a utility function that deletes interactive sequences that trigger from a specific shape
2. Implement a smart deletion function that checks if a sequence exists before attempting to delete it
3. Write code to remove all interactive sequences while preserving specific ones

## Next Steps

Now that you know how to delete interactive sequences, you might want to learn how to [delete effects from an interactive sequence](/animations/delete-effects/) or [create new interactive sequences](/animations/create-interactive-sequence/).

## Learning Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [Aspose.Slides Cloud Forum](https://forum.aspose.cloud/c/slides/15)
