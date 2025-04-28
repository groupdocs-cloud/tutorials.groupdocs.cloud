---
title: Set Basic Animations with Aspose.Slides Cloud API Tutorial
url: /animations/set-animation/
description: Learn how to add and configure animations for shapes and text in PowerPoint presentations using Aspose.Slides Cloud API in this step-by-step tutorial.
weight: 20
---

# Tutorial: How to Set Basic Animations with Aspose.Slides Cloud API

## Prerequisites

Before starting this tutorial, make sure you have:

1. An Aspose Cloud account (if you don't have one, [register for a free trial](https://dashboard.aspose.cloud/))
2. Your Client ID and Client Secret credentials from the [Aspose Cloud Dashboard](https://dashboard.aspose.cloud/)
3. A basic understanding of REST APIs and HTTP requests
4. Familiarity with one of the programming languages featured in the examples (cURL, Python, Java, or C#)
5. A PowerPoint presentation with multiple shapes to animate

## Introduction

Adding animations to PowerPoint presentations can significantly enhance their visual appeal and effectiveness. With Aspose.Slides Cloud API, you can programmatically apply various animation effects to shapes and text elements in your slides.

In this tutorial, we'll explore how to set animations using the Aspose.Slides Cloud API. We'll cover both main sequence animations (which play automatically as the slide advances) and interactive animations (which are triggered by clicking on specific shapes).

## Step 1: Authenticate with the Aspose.Slides Cloud API

First, we need to obtain an access token using our client credentials:

```bash
curl -v "https://api.aspose.cloud/connect/token" \
-d "grant_type=client_credentials&client_id=MY_CLIENT_ID&client_secret=MY_CLIENT_SECRET" \
-H "Content-Type: application/x-www-form-urlencoded"
```

Replace `MY_CLIENT_ID` and `MY_CLIENT_SECRET` with your actual credentials.

## Step 2: Set Main Sequence Animations

Let's add two animation effects to the main sequence of the first slide. We'll apply a Fade effect to shape 2 that plays automatically after the previous animation, and a Blink effect to shape 3 that plays when clicked:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
           "MainSequence": [
             {
               "ShapeIndex": 2,
               "Type": "Fade",
               "TriggerType": "AfterPrevious"
             },
             {
               "ShapeIndex": 3,
               "Type": "Blink",
               "TriggerType": "OnClick"
             }
           ]
         }'
```

Replace `YOUR_ACCESS_TOKEN` with the token obtained in Step 1, and `MyPresentation.pptx` with your actual presentation filename.

### Understanding the Response

The API will respond with the complete animation configuration that was set:

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
    },
    {
      "type": "Blink",
      "subtype": "None",
      "presetClassType": "Emphasis",
      "shapeIndex": 3,
      "triggerType": "OnClick",
      "duration": 1.0,
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

Note how the API automatically sets some default values we didn't specify, such as `duration` and `presetClassType`.

## Step 3: Set Interactive Sequence Animations

Now, let's add an interactive animation sequence that will be triggered when shape 2 is clicked. This sequence will apply a Fly effect to shape 4:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
           "InteractiveSequences": [
             {
               "TriggerShapeIndex": 2,
               "Effects": [
                 {
                   "ShapeIndex": 4,
                   "Type": "Fly",
                   "Subtype": "Bottom",
                   "TriggerType": "OnClick"
                 }
               ]
             }
           ]
         }'
```

### Key Points About Interactive Sequences

1. The `TriggerShapeIndex` specifies which shape must be clicked to start the animation sequence
2. Each sequence can contain multiple effects that will be triggered by the same action
3. Effects in interactive sequences can be configured with the same properties as main sequence effects

## Step 4: Combining Main and Interactive Sequences

You can set both main sequence and interactive sequence animations in a single API call. This is useful when you're configuring all animations for a slide at once:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/animation" \
     -H "authorization: Bearer YOUR_ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
           "MainSequence": [
             {
               "ShapeIndex": 2,
               "Type": "Fade",
               "TriggerType": "AfterPrevious"
             },
             {
               "ShapeIndex": 3,
               "Type": "Blink",
               "TriggerType": "OnClick"
             }
           ],
           "InteractiveSequences": [
             {
               "TriggerShapeIndex": 2,
               "Effects": [
                 {
                   "ShapeIndex": 4,
                   "Type": "Fly",
                   "Subtype": "Bottom",
                   "TriggerType": "OnClick"
                 }
               ]
             }
           ]
         }'
```

## Try It Yourself

Now that you understand the basics, try these exercises to reinforce your learning:

1. Add a different animation type (such as Split, Wipe, or Wheel) to a shape on slide 2
2. Create an interactive sequence with multiple effects that trigger from the same shape
3. Experiment with different `TriggerType` values (OnClick, WithPrevious, AfterPrevious)
4. Try setting a longer duration for an animation to make it play more slowly

## Using the SDK

While direct REST API calls work well, using SDKs can make your code more readable and maintainable. Here are examples using official Aspose.Slides Cloud SDKs:

### Python Example

```python
import asposeslidescloud
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.slide_animation import SlideAnimation
from asposeslidescloud.models.effect import Effect
from asposeslidescloud.models.interactive_sequence import InteractiveSequence

# Configure the API client
slides_api = SlidesApi(None, "MY_CLIENT_ID", "MY_CLIENT_SECRET")

# Create animation objects
fade_effect = Effect()
fade_effect.shape_index = 2
fade_effect.type = "Fade"
fade_effect.trigger_type = "AfterPrevious"

blink_effect = Effect()
blink_effect.shape_index = 3
blink_effect.type = "Blink"
blink_effect.trigger_type = "OnClick"

# Create an animation with main sequence effects
slide_animation = SlideAnimation()
slide_animation.main_sequence = [fade_effect, blink_effect]

# Add interactive sequence
fly_effect = Effect()
fly_effect.shape_index = 4
fly_effect.type = "Fly"
fly_effect.subtype = "Bottom"
fly_effect.trigger_type = "OnClick"

interactive_sequence = InteractiveSequence()
interactive_sequence.trigger_shape_index = 2
interactive_sequence.effects = [fly_effect]

slide_animation.interactive_sequences = [interactive_sequence]

# Set the animations on slide 1
result = slides_api.set_animation("MyPresentation.pptx", 1, slide_animation)

print(f"Added {len(result.main_sequence)} main sequence effects and " 
      f"{len(result.interactive_sequences)} interactive sequences")
```

### Java Example

```java
import com.aspose.slides.ApiException;
import com.aspose.slides.api.SlidesApi;
import com.aspose.slides.model.*;

import java.util.Arrays;

public class SetAnimations {
    public static void main(String[] args) throws ApiException {
        SlidesApi slidesApi = new SlidesApi("MY_CLIENT_ID", "MY_CLIENT_SECRET");

        // Create main sequence effects
        Effect fadeEffect = new Effect();
        fadeEffect.setShapeIndex(2);
        fadeEffect.setType(Effect.TypeEnum.FADE);
        fadeEffect.setTriggerType(Effect.TriggerTypeEnum.AFTERPREVIOUS);

        Effect blinkEffect = new Effect();
        blinkEffect.setShapeIndex(3);
        blinkEffect.setType(Effect.TypeEnum.BLINK);
        blinkEffect.setTriggerType(Effect.TriggerTypeEnum.ONCLICK);

        // Create an animation with main sequence effects
        SlideAnimation slideAnimation = new SlideAnimation();
        slideAnimation.setMainSequence(Arrays.asList(fadeEffect, blinkEffect));

        // Add interactive sequence
        Effect flyEffect = new Effect();
        flyEffect.setShapeIndex(4);
        flyEffect.setType(Effect.TypeEnum.FLY);
        flyEffect.setSubtype(Effect.SubtypeEnum.BOTTOM);
        flyEffect.setTriggerType(Effect.TriggerTypeEnum.ONCLICK);

        InteractiveSequence interactiveSequence = new InteractiveSequence();
        interactiveSequence.setTriggerShapeIndex(2);
        interactiveSequence.setEffects(Arrays.asList(flyEffect));

        slideAnimation.setInteractiveSequences(Arrays.asList(interactiveSequence));

        // Set the animations on slide 1
        SlideAnimation result = slidesApi.setAnimation("MyPresentation.pptx", 1, slideAnimation, null, null, null);

        System.out.println("Added " + result.getMainSequence().size() + " main sequence effects and " 
                          + result.getInteractiveSequences().size() + " interactive sequences");
    }
}
```

### C# Example

```csharp
using System;
using System.Collections.Generic;
using Aspose.Slides.Cloud.Sdk;
using Aspose.Slides.Cloud.Sdk.Model;

class Program
{
    static void Main(string[] args)
    {
        SlidesApi slidesApi = new SlidesApi("MY_CLIENT_ID", "MY_CLIENT_SECRET");

        // Create main sequence effects
        var fadeEffect = new Effect
        {
            ShapeIndex = 2,
            Type = Effect.TypeEnum.Fade,
            TriggerType = Effect.TriggerTypeEnum.AfterPrevious
        };

        var blinkEffect = new Effect
        {
            ShapeIndex = 3,
            Type = Effect.TypeEnum.Blink,
            TriggerType = Effect.TriggerTypeEnum.OnClick
        };

        // Create an animation with main sequence effects
        var slideAnimation = new SlideAnimation
        {
            MainSequence = new List<Effect> { fadeEffect, blinkEffect }
        };

        // Add interactive sequence
        var flyEffect = new Effect
        {
            ShapeIndex = 4,
            Type = Effect.TypeEnum.Fly,
            Subtype = Effect.SubtypeEnum.Bottom,
            TriggerType = Effect.TriggerTypeEnum.OnClick
        };

        var interactiveSequence = new InteractiveSequence
        {
            TriggerShapeIndex = 2,
            Effects = new List<Effect> { flyEffect }
        };

        slideAnimation.InteractiveSequences = new List<InteractiveSequence> { interactiveSequence };

        // Set the animations on slide 1
        var result = slidesApi.SetAnimation("MyPresentation.pptx", 1, slideAnimation);

        Console.WriteLine($"Added {result.MainSequence.Count} main sequence effects and " +
                          $"{result.InteractiveSequences.Count} interactive sequences");
    }
}
```

## Animation Types and Settings

PowerPoint supports a wide variety of animation types. Here are some common ones you can use:

### Animation Types
- Entrance Effects: Fade, Fly, Float, Split, Wipe, Zoom
- Emphasis Effects: Blink, Grow/Shrink, Spin, Teeter, Wave
- Exit Effects: Disappear, Fade, Fly, Float, Split, Wipe
- Motion Path Effects: Circle, Diamond, Plus, Square, Up-Down

### Animation Settings
- Type: The animation effect to apply
- Subtype: Variation of the effect (e.g., direction)
- TriggerType: When the animation starts
  - `OnClick`: Plays when the slide is clicked
  - `WithPrevious`: Plays at the same time as the previous animation
  - `AfterPrevious`: Plays after the previous animation finishes
- Duration: Length of the animation in seconds
- TriggerDelayTime: Time to wait before starting the animation

## Common Issues and Troubleshooting

- 404 Not Found: Ensure the presentation file exists at the specified location
- Invalid shape index: Verify that the shape indices you're using exist in the slide
- Animation not appearing: Check that the animation type is compatible with the shape type

## What You've Learned

In this tutorial, you've learned how to:
- Add animations to shapes in PowerPoint presentations
- Configure animation properties like type, trigger, and timing
- Create both main sequence and interactive animations
- Apply different animation effects to various shapes
- Use the Aspose.Slides Cloud API with different programming languages

## Next Steps

Now that you can set basic animations, you're ready to move on to:
- [Tutorial: How to Add Effects to a Main Sequence](/animations/add-effect-main-sequence/) - Learn more advanced techniques for building animation sequences
- [Tutorial: How to Update Animation Effects](/animations/update-animation-effects/) - Modify existing animations

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [Live Demo](https://products.aspose.app/slides/family)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Blog](https://blog.aspose.cloud/category/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
