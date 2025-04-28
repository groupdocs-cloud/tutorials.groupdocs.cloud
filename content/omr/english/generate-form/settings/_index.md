---
title: Working with Form Generation Settings Tutorial
weight: 35
url: /generate-form/settings/
description: Learn how to customize the appearance and layout of your OMR forms by mastering form generation settings in the Aspose.OMR Cloud API.
---

# Tutorial: Working with Form Generation Settings

## Learning Objectives

In this tutorial, you'll learn how to:
- Configure paper size and orientation for different form requirements
- Customize fonts to improve readability and aesthetics
- Adjust bubble colors and sizes for different user groups
- Set page margins for optimal form layout
- Apply settings strategically to enhance form usability
- Troubleshoot common settings-related issues

## Prerequisites

Before starting this tutorial, you should:
- Have an [Aspose Cloud account](https://dashboard.aspose.cloud/#/apps)
- Have completed the [Getting Started](/generate-form/getting-started/) tutorial
- Be familiar with [sending form source code](/generate-form/send-source-code/) for generation
- Have a basic understanding of form design and layout principles

## Introduction

The appearance and layout of your OMR forms significantly impact their usability and recognition accuracy. Aspose.OMR Cloud API provides various settings that let you customize how your forms look and function. In this tutorial, we'll explore these settings in detail and learn how to apply them effectively.

## Practical Scenario

You work for an educational institute that needs to create different types of forms:
- Assessments for young children (grades 1-3)
- Standard exams for high school students
- Professional certification tests for adults

Each audience requires different form layouts and visual designs for optimal usability.

## Step 1: Understanding the Settings Object

When generating a form, you provide a `Settings` object in your API request that controls various aspects of the form's appearance. Here's a basic example:

```json
"Settings": {
  "PaperSize": "A4",
  "Orientation": "Vertical",
  "PageMarginLeft": 50,
  "FontFamily": "Arial",
  "FontSize": 12,
  "FontStyle": "Regular",
  "BubbleColor": "Black",
  "BubbleSize": "Normal"
}
```

Let's examine each of these settings in detail.

## Step 2: Configuring Paper Size and Orientation

### Paper Size Options

The `PaperSize` setting determines the physical dimensions of your form. Aspose.OMR Cloud supports three standard paper sizes:

| Option  | Dimensions (mm) | Dimensions (pixels) | Common Use |
|---------|----------------|-------------------|------------|
| `A4`    | 210 × 297 mm   | 2480 × 3508 px    | Standard international size |
| `Letter`| 215.9 × 279.4 mm | 2551 × 3295 px  | Common in US and Canada |
| `Legal` | 215.9 × 355.6 mm | 2551 × 4205 px  | Longer forms with many questions |

### Orientation Options

The `Orientation` setting determines whether your form is portrait or landscape:

| Option      | Description | Best For |
|-------------|-------------|----------|
| `Vertical`  | Portrait orientation | Forms with many short questions |
| `Horizontal`| Landscape orientation | Forms with fewer, longer questions or tables |

> Try it yourself: Generate the same form with different paper sizes and orientations to observe how the layout adapts.

## Step 3: Working with Fonts

### Font Family

The `FontFamily` setting specifies the font used throughout your form (unless explicitly overridden in the source code). Make sure to use only [supported fonts](/omr/appendices/fonts/).

Popular options include:
- `Arial` - Clean, professional appearance
- `Times New Roman` - Traditional, academic look
- `Verdana` - High readability on printed materials
- `Courier New` - Monospaced, good for code or tabular data

### Font Size

The `FontSize` setting determines the text size in pixels. Recommended values:
- 10-12px for standard text
- 14-16px for section headers
- 18-24px for form titles

### Font Style

The `FontStyle` setting controls the text appearance:
- `Regular` - Standard weight text
- `Bold` - Emphasized text
- `Italic` - Slanted text (use sparingly)
- `Underline` - Underlined text (best for headers)
- `Strikeout` - Text with a line through it (rarely used)

> Learning checkpoint: Which font settings would you choose for a kindergarten assessment form? What about a professional certification exam?

## Step 4: Customizing Bubbles

### Bubble Color

The `BubbleColor` setting determines the color of answer bubbles on your form. Choose from [supported color codes](/omr/appendices/colors/) such as:
- `Black` - Maximum contrast, best for scanning
- `Red` - High visibility, good for proofreading
- `Blue` - Professional appearance
- `Green` - Softer appearance, reduces test anxiety

### Bubble Size

The `BubbleSize` setting controls the diameter of answer bubbles:

| Option | Size (pixels) | Best For |
|--------|--------------|----------|
| `Extrasmall` | 40px | Very compact forms |
| `Small` | 50px | Standard forms with space constraints |
| `Normal` | 60px | General purpose forms |
| `Large` | 80px | Forms for older children or seniors |
| `Extralarge` | 100px | Forms for young children |

> Tip: Larger bubbles are easier to fill but take up more space. For young children or elderly users, larger bubbles are recommended.

## Step 5: Setting Page Margins

The `PageMarginLeft` setting controls the width of the left margin in pixels. This is useful for:
- Forms that will be bound or placed in folders
- Accommodating pre-printed letterhead
- Creating space for hole punches
- Ensuring proper scanning alignment

Typical values range from 30 to 100 pixels.

## Step 6: Creating Audience-Specific Forms

Now let's apply these settings to create forms for different audiences:

### For Young Children (Grades 1-3)

```json
"Settings": {
  "PaperSize": "Letter",
  "Orientation": "Vertical",
  "PageMarginLeft": 60,
  "FontFamily": "Comic Sans MS",
  "FontSize": 14,
  "FontStyle": "Regular",
  "BubbleColor": "Blue",
  "BubbleSize": "Extralarge"
}
```

Key considerations:
- Larger, rounded font that resembles handwriting
- Extra large bubbles for easier marking
- Blue bubbles to create a friendlier appearance
- Larger font size for better readability

### For High School Students

```json
"Settings": {
  "PaperSize": "A4",
  "Orientation": "Vertical",
  "PageMarginLeft": 50,
  "FontFamily": "Arial",
  "FontSize": 12,
  "FontStyle": "Regular",
  "BubbleColor": "Black",
  "BubbleSize": "Normal"
}
```

Key considerations:
- Professional, clean font
- Standard bubble size for efficient space usage
- Black bubbles for maximum clarity
- Standard font size balancing readability and space efficiency

### For Professional Certification

```json
"Settings": {
  "PaperSize": "Legal",
  "Orientation": "Horizontal",
  "PageMarginLeft": 40,
  "FontFamily": "Times New Roman",
  "FontSize": 11,
  "FontStyle": "Regular",
  "BubbleColor": "Black",
  "BubbleSize": "Small"
}
```

Key considerations:
- Traditional, academic font
- Smaller bubbles for space efficiency
- Landscape orientation for complex questions
- Legal paper size for accommodating more content

> Try it yourself: Generate the same form with these three different settings configurations and compare the results.

## Step 7: Overriding Settings in Source Code

While the `Settings` object provides global defaults, you can override specific settings for individual elements in your form source code:

```
?text=Form Title
	font_size=24
	font_style=bold
	align=center
?empty_line=
?text=Section 1: Multiple Choice
	font_size=16
	font_style=bold
?answer=Question 1,Option A,Option B,Option C,Option D
	bubble_size=large
```

In this example:
- The title uses size 24 bold text, overriding the global settings
- The section header uses size 16 bold text
- Question 1 uses large bubbles, overriding the global bubble size

## Troubleshooting Common Issues

### Issue: Form Elements Are Cut Off

Possible causes:
- Paper size too small for content
- Insufficient margins
- Font size too large

Solutions:
- Switch to a larger paper size
- Reduce font size
- Increase margins
- Change orientation to horizontal

### Issue: Poor Recognition Accuracy

Possible causes:
- Bubble size too small
- Low-contrast bubble color
- Improper printing scaling

Solutions:
- Use black bubbles for maximum contrast
- Increase bubble size
- Ensure forms are printed at 100% scale

### Issue: Form Looks Unprofessional

Possible causes:
- Inconsistent font usage
- Poor spacing between elements
- Inappropriate font style

Solutions:
- Use a single professional font (Arial or Times New Roman)
- Maintain consistent spacing with empty lines
- Use bold sparingly, only for headers and titles

> Learning checkpoint: If a form printed with standard settings has recognition issues, which settings would you adjust first?

## What You've Learned

In this tutorial, you learned how to:
- Configure paper size and orientation for different form types
- Select appropriate fonts for different audiences
- Customize bubble appearance to improve usability
- Set page margins for optimal layout
- Apply different settings for specific user groups
- Troubleshoot common settings-related issues

## Further Practice

Try these exercises to reinforce your learning:

1. Generate the same form with three different paper sizes and compare the results
2. Create a form with progressively larger bubble sizes to see the impact on layout
3. Experiment with different font families and styles to find optimal combinations
4. Generate forms with varying left margins to understand their impact

## Next Steps

Now that you understand how to configure form settings, you're ready to explore [using the Aspose.OMR Cloud SDK](/generate-form/using-sdk/) for easier form generation integration.

## Additional Resources

- [Product Page](https://products.aspose.cloud/omr/)
- [Documentation](https://docs.aspose.cloud/omr/)
- [Live Demo](https://products.aspose.app/omr/family)
- [API Reference](https://reference.aspose.cloud/omr/)
- [Blog](https://blog.aspose.cloud/category/omr/)
- [Free Support](https://forum.aspose.cloud/c/omr/8/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [support forum](https://forum.aspose.cloud/c/omr/8/).
