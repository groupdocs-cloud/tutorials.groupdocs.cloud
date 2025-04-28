---
title: Add Effects to a Main Sequence with Aspose.Slides Cloud API Tutorial
url: /animations/add-effect-main-sequence/
description: Learn how to add animation effects to the main sequence of PowerPoint slides using Aspose.Slides Cloud API in this step-by-step tutorial.
weight: 30
---

# Tutorial: How to Add Effects to a Main Sequence with Aspose.Slides Cloud API

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account (if you don't have one, [register for a free trial](https://dashboard.aspose.cloud/))
2. Your Client ID and Client Secret credentials from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
3. A basic understanding of REST APIs and HTTP requests
4. Familiarity with one of the programming languages featured in the examples (cURL, Python, Java, or C#)
5. A PowerPoint presentation with slides containing multiple shapes

## Introduction

The main animation sequence is the primary collection of animation effects that play during a slide show. Unlike setting all animations at once (as we covered in the [Set Basic Animations tutorial](/animations/set-animation/), sometimes you need to add individual effects to an existing sequence.

In this tutorial, we'll explore how to add new effects to a main animation sequence using the Aspose.Slides Cloud API. This approach is particularly useful when:

- You want to preserve existing animations and just add new ones
- You're building a complex sequence of animations step by step
- You need to add animations programmatically based on dynamic content

## Understanding Main Sequence Animations

The main sequence contains animation effects that play when a slide is viewed. These animations can be configured to play:

- On Click: The animation plays when the slide or another element is clicked
- With Previous: The animation plays at the same time as the previous animation
- After Previous: The animation plays after the previous animation finishes

Each effect in the sequence has properties that control its appearance and behavior, such as type, duration, and delay time.

## Step 1: Authenticate with the Aspose.Slides Cloud API

First, we need to obtain an access token using our client credentials:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-d "grant_type=client_credentials&client_id=MY_CLIENT_ID&client_secret=MY_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded"
```

Replace `MY_CLIENT_ID` and `MY_CLIENT_SECRET` with your actual credentials.

## Step 2: Add an Effect to the Main Sequence

To add an animation effect to the main sequence, we'll use the `CreateAnimationEffect` endpoint. Let's add a Wipe effect to shape 2 on slide 1:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation/mainSequence" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
           "Type": "Wipe",
           "PresetClassType": "Entrance",
           "ShapeIndex": 2,
           "TriggerType": "OnClick"
         }'
```

Replace `YOUR_ACCESS_TOKEN` with the token obtained in Step 1, and `MyPresentation.pptx` with your actual presentation filename.

### Understanding the Request Body

The JSON request body contains several important fields:

- Type: The animation effect to apply (e.g., "Wipe")
- PresetClassType: The category of the effect (e.g., "Entrance", "Emphasis", "Exit")
- ShapeIndex: The 1-based index of the shape to animate
- TriggerType: When the animation should start (e.g., "OnClick", "WithPrevious", "AfterPrevious")

### Example Response

```json
{
  "mainSequence": [
    {
      "type": "Wipe",
      "subtype": "Bottom",
      "presetClassType": "Entrance",
      "shapeIndex": 2,
      "triggerType": "OnClick",
      "duration": 0.5,
      "triggerDelayTime": 0.0
    }
  ],
  "interactiveSequences": [],
  "selfUri": {
    "href": "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation",
    "relation": "self",
    "slideIndex": 1
  }
}
```

## Step 3: Add Another Effect with Different Settings

Now, let's add a second effect to the same slide. This time, we'll add a Spin effect to shape 3, which will play automatically after the previous animation:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation/mainSequence" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
           "Type": "Spin",
           "PresetClassType": "Emphasis",
           "ShapeIndex": 3,
           "TriggerType": "AfterPrevious",
           "Duration": 1.5
         }'
```

Notice that we've added an additional parameter:
- Duration: The length of the animation in seconds (in this case, 1.5 seconds)

## Step 4: Add a Text Animation Effect

Animation effects can be applied to specific paragraphs within text shapes. Let's add a Fade effect to the first paragraph of shape 4:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation/mainSequence" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
           "Type": "Fade",
           "PresetClassType": "Entrance",
           "ShapeIndex": 4,
           "ParagraphIndex": 1,
           "TriggerType": "WithPrevious",
           "TriggerDelayTime": 0.5
         }'
```

In this example, we've introduced additional parameters:
- ParagraphIndex: The 1-based index of the paragraph to animate
- TriggerDelayTime: The delay (in seconds) before the animation starts

## Try It Yourself

Now that you understand the basics, try these exercises to reinforce your learning:

1. Add a different animation type (such as Split, Fly, or Zoom) to a shape on slide 2
2. Create a sequence of animations with varying trigger types to create a choreographed entrance
3. Experiment with different duration values to control the speed of animations
4. Try animating different paragraphs within the same text shape

## Using the SDK

While direct REST API calls work well, using SDKs can make your code more readable and maintainable. Here are examples using official Aspose.Slides Cloud SDKs:

### Python Example

```python
import asposeslidescloud
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.effect import Effect

# Configure the API client
slides_api = SlidesApi(None, "MY_CLIENT_ID", "MY_CLIENT_SECRET")

# Create a Wipe effect for shape 2
wipe_effect = Effect()
wipe_effect.type = "Wipe"
wipe_effect.preset_class_type = "Entrance"
wipe_effect.shape_index = 2
wipe_effect.trigger_type = "OnClick"

# Add the effect to slide 1's main sequence
result = slides_api.create_animation_effect("MyPresentation.pptx", 1, wipe_effect)
print(f"Added {wipe_effect.type} effect to shape {wipe_effect.shape_index}")

# Create a Spin effect for shape 3
spin_effect = Effect()
spin_effect.type = "Spin"
spin_effect.preset_class_type = "Emphasis"
spin_effect.shape_index = 3
spin_effect.trigger_type = "AfterPrevious"
spin_effect.duration = 1.5

# Add the effect to slide 1's main sequence
result = slides_api.create_animation_effect("MyPresentation.pptx", 1, spin_effect)
print(f"Added {spin_effect.type} effect to shape {spin_effect.shape_index}")

# Get all animations to verify
animations = slides_api.get_animation("MyPresentation.pptx", 1)
print(f"Total effects in main sequence: {len(animations.main_sequence)}")
```

### Java Example

```java
import com.aspose.slides.ApiException;
import com.aspose.slides.api.SlidesApi;
import com.aspose.slides.model.Effect;
import com.aspose.slides.model.SlideAnimation;

public class AddMainSequenceEffect {
    public static void main(String[] args) throws ApiException {
        SlidesApi slidesApi = new SlidesApi("MY_CLIENT_ID", "MY_CLIENT_SECRET");

        // Create a Wipe effect for shape 2
        Effect wipeEffect = new Effect();
        wipeEffect.setType(Effect.TypeEnum.WIPE);
        wipeEffect.setPresetClassType(Effect.PresetClassTypeEnum.ENTRANCE);
        wipeEffect.setShapeIndex(2);
        wipeEffect.setTriggerType(Effect.TriggerTypeEnum.ONCLICK);

        // Add the effect to slide 1's main sequence
        SlideAnimation result = slidesApi.createAnimationEffect("MyPresentation.pptx", 1, wipeEffect, null, null, null);
        System.out.println("Added " + wipeEffect.getType() + " effect to shape " + wipeEffect.getShapeIndex());

        // Create a Spin effect for shape 3
        Effect spinEffect = new Effect();
        spinEffect.setType(Effect.TypeEnum.SPIN);
        spinEffect.setPresetClassType(Effect.PresetClassTypeEnum.EMPHASIS);
        spinEffect.setShapeIndex(3);
        spinEffect.setTriggerType(Effect.TriggerTypeEnum.AFTERPREVIOUS);
        spinEffect.setDuration(1.5);

        // Add the effect to slide 1's main sequence
        result = slidesApi.createAnimationEffect("MyPresentation.pptx", 1, spinEffect, null, null, null);
        System.out.println("Added " + spinEffect.getType() + " effect to shape " + spinEffect.getShapeIndex());

        // Get all animations to verify
        SlideAnimation animations = slidesApi.getAnimation("MyPresentation.pptx", 1, null, null, null, null, null);
        System.out.println("Total effects in main sequence: " + animations.getMainSequence().size());
    }
}
```

### C# Example

```csharp
using System;
using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Model;

class Program
{
    static void Main(string[] args)
    {
        SlidesApi slidesApi = new SlidesApi("MY_CLIENT_ID", "MY_CLIENT_SECRET");

        // Create a Wipe effect for shape 2
        var wipeEffect = new Effect
        {
            Type = Effect.TypeEnum.Wipe,
            PresetClassType = Effect.PresetClassTypeEnum.Entrance,
            ShapeIndex = 2,
            TriggerType = Effect.TriggerTypeEnum.OnClick
        };

        // Add the effect to slide 1's main sequence
        var result = slidesApi.CreateAnimationEffect("MyPresentation.pptx", 1, wipeEffect);
        Console.WriteLine($"Added {wipeEffect.Type} effect to shape {wipeEffect.ShapeIndex}");

        // Create a Spin effect for shape 3
        var spinEffect = new Effect
        {
            Type = Effect.TypeEnum.Spin,
            PresetClassType = Effect.PresetClassTypeEnum.Emphasis,
            ShapeIndex = 3,
            TriggerType = Effect.TriggerTypeEnum.AfterPrevious,
            Duration = 1.5
        };

        // Add the effect to slide 1's main sequence
        result = slidesApi.CreateAnimationEffect("MyPresentation.pptx", 1, spinEffect);
        Console.WriteLine($"Added {spinEffect.Type} effect to shape {spinEffect.ShapeIndex}");

        // Get all animations to verify
        var animations = slidesApi.GetAnimation("MyPresentation.pptx", 1);
        Console.WriteLine($"Total effects in main sequence: {animations.MainSequence.Count}");
    }
}
```

## Common Effect Types and Their Uses

### Entrance Effects
- Fade: A simple fade-in effect, good for subtle entrances
- Fly: Slides in from a specified direction, great for dynamic entrances
- Float: Moves in smoothly with a slight bounce at the end
- Wipe: Reveals the object gradually from one edge, like wiping a window
- Zoom: Grows the object from small to full size, good for emphasizing importance

### Emphasis Effects
- Pulse: Makes the object briefly grow and then return to normal size
- Spin: Rotates the object, good for attracting attention
- Teeter: Rocks the object back and forth
- Blink: Makes the object blink briefly
- Wave: Creates a wave-like effect through the object

### Exit Effects
- Disappear: Instantly removes the object
- Fade: Gradually fades the object out
- Fly Out: Slides the object off in a specified direction
- Float Out: Moves the object off screen with a slight bounce
- Shrink & Turn: Shrinks and rotates the object as it exits

## Common Issues and Troubleshooting

- 404 Not Found: Ensure the presentation file exists at the specified location
- Invalid shape index: Verify that the shape indices you're using exist in the slide
- Paragraph index out of range: Check that the text shape has the paragraph index you specified
- Animation not compatible: Not all animation types work with all objects, check compatibility

## What You've Learned

In this tutorial, you've learned how to:
- Add individual animation effects to a main sequence
- Configure animation properties like type, trigger, and timing
- Apply animations to both shapes and text paragraphs
- Create sequences of animations with different timing behaviors
- Use the Aspose.Slides Cloud API with different programming languages

## Next Steps

Now that you can add effects to a main sequence, you're ready to move on to:
- [Tutorial: How to Update Animation Effects](/animations/update-animation-effects/) - Modify existing animation properties

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
