---
title: Delete a Main Sequence Animation Tutorial
url: /animations/delete-main-sequence/
description: Learn how to delete the main animation sequence from PowerPoint slides using Aspose.Slides Cloud API, while preserving interactive animations.
weight: 100
---

## Learning Objectives

In this tutorial, you'll learn how to programmatically delete a main animation sequence from PowerPoint slides using Aspose.Slides Cloud API. By the end of this guide, you'll be able to:

- Understand the difference between main and interactive animation sequences
- Remove an entire main animation sequence while preserving interactive sequences
- Verify the removal through response validation
- Implement this functionality in different programming languages

## Prerequisites

Before beginning this tutorial, ensure you have:

1. An Aspose Cloud account with an active subscription
2. Your Client ID and Client Secret credentials
3. A PowerPoint presentation with at least one animation sequence
4. Basic understanding of REST API concepts
5. Familiarity with your preferred programming language

## Understanding Main Sequences vs. Interactive Sequences

In PowerPoint presentations, animations are organized into sequences:

- Main Sequence: Animations that play automatically during the presentation, often advancing with slide transitions or clicks.
- Interactive Sequences: Animations triggered by specific user actions, like clicking on a particular shape.

This tutorial focuses on deleting the main sequence while leaving any interactive sequences intact.

## Step-by-Step Guide

### Step 1: Authenticate with the API

Before performing any operations, you'll need to authenticate with the Aspose.Slides Cloud API:

```bash
# Request an access token
curl -X POST "https://api.aspose.cloud/connect/token" \
     -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
     -H "Content-Type: application/x-www-form-urlencoded"
```

This will provide you with an access token to use in subsequent requests.

### Step 2: Identify the Target Slide

To delete a main sequence, you need to know:
- The name of your presentation file
- The slide index (1-based) containing the main sequence

For example, if you want to delete the main sequence from the first slide of "MyPresentation.pptx", you would use:
- name = "MyPresentation.pptx"
- slideIndex = 1

### Step 3: Delete the Main Sequence

Now, make the DELETE request to remove the main sequence:

#### cURL Example

```bash
curl -X DELETE "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation/mainSequence" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN"
```

#### Try it yourself

Replace `YOUR_ACCESS_TOKEN`, and if needed, change the presentation name and slide index to match your specific scenario.

### Step 4: Understand the Response

After successful deletion, the API will return a JSON response showing the updated animation structure. Here's an example response:

```json
{
  "mainSequence": [],
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
    },
    {
      "effects": [
        {
          "type": "Split",
          "subtype": "HorizontalIn",
          "presetClassType": "Entrance",
          "shapeIndex": 5,
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
- The `mainSequence` array is now empty
- The `interactiveSequences` remain unchanged
- The `selfUri` with a link to access all animations on this slide

### Step 5: Implement in Your Application

Here are examples of how to implement this functionality in various programming languages:

#### C# Example

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

        // Delete the main sequence
        SlideAnimation slideAnimation = slidesApi.DeleteAnimationMainSequence(fileName, slideIndex);

        // Verify the result
        int mainSequenceEffectCount = slideAnimation.MainSequence.Count;
        int interactiveSequenceCount = slideAnimation.InteractiveSequences.Count;

        Console.WriteLine("Number of effects in the main sequence: " + mainSequenceEffectCount);
        Console.WriteLine("Number of interactive sequences: " + interactiveSequenceCount);
    }
}
```

#### Python Example

```python
from asposeslidescloud.apis.slides_api import SlidesApi

# Setup API client with your credentials
slides_api = SlidesApi(None, "YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET")

# Define parameters
file_name = "MyPresentation.pptx"
slide_index = 1

# Delete the main sequence
slide_animation = slides_api.delete_animation_main_sequence(file_name, slide_index)

# Verify the result
main_sequence_effect_count = len(slide_animation.main_sequence)
interactive_sequence_count = len(slide_animation.interactive_sequences)

print("Number of effects in the main sequence:", main_sequence_effect_count)
print("Number of interactive sequences:", interactive_sequence_count)
```

#### Node.js Example

```javascript
const cloudSdk = require("asposeslidescloud");

// Setup API client with your credentials
const slidesApi = new cloudSdk.SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Define parameters
const fileName = "MyPresentation.pptx";
const slideIndex = 1;

// Delete the main sequence
slidesApi.deleteAnimationMainSequence(fileName, slideIndex)
    .then(slideAnimation => {
        // Verify the result
        const mainSequenceEffectCount = slideAnimation.body.mainSequence.length;
        const interactiveSequenceCount = slideAnimation.body.interactiveSequences.length;

        console.log("Number of effects in the main sequence:", mainSequenceEffectCount);
        console.log("Number of interactive sequences:", interactiveSequenceCount);
    });
```

## Troubleshooting

### Common Issues and Solutions

1. Authentication Errors
   - Ensure your Client ID and Client Secret are correct
   - Verify that your access token is still valid (they typically expire after 1 hour)

2. 404 Not Found Errors
   - Check that the presentation file exists in the specified location
   - Verify that the slide index is valid (remember it's 1-based, not 0-based)

3. No Changes in Response
   - If the main sequence was already empty, the operation will succeed but show no changes
   - Verify that you're examining the correct slide for animations

## What You've Learned

In this tutorial, you've learned how to:
- Authenticate with the Aspose.Slides Cloud API
- Delete a main animation sequence from a PowerPoint slide
- Preserve interactive sequences during main sequence deletion
- Process and interpret the API response
- Implement this functionality in multiple programming languages

## Further Practice

To reinforce your learning, try these exercises:
1. Write code to check if a main sequence exists before attempting to delete it
2. Create a utility function that deletes main sequences from multiple slides
3. Implement a backup solution that stores the main sequence properties before deletion

## Next Steps

Now that you know how to delete a main sequence, you might want to learn how to [delete all animations](/animations/delete-animations/) or [work with paragraph animations](/animations/paragraph-animation/).

## Learning Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [Aspose.Slides Cloud Forum](https://forum.aspose.cloud/c/slides/15)
