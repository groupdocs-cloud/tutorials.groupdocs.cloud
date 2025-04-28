---
title: Customizing Recognition Settings for PDF Documents Tutorial
weight: 50
url: /recognize-pdf/recognition-settings/
description: Learn how to optimize PDF text extraction by fine-tuning recognition parameters in Aspose.OCR Cloud to improve accuracy for different document types.
---

# Tutorial: Customizing Recognition Settings for PDF Documents

## Learning Objectives

By the end of this tutorial, you will be able to:
- Understand each recognition parameter and its impact on OCR results
- Select optimal settings for different types of PDF documents
- Implement pre-processing techniques to improve recognition accuracy
- Fine-tune language settings for multi-language documents
- Configure output formats based on your specific needs
- Test and compare different settings to achieve optimal results

## Prerequisites

Before starting this tutorial, make sure you have:
- Completed the [Getting Started with PDF Recognition](/recognize-pdf/getting-started/) tutorial
- Familiarity with the basic PDF recognition workflow
- Your Aspose Cloud account credentials
- Several test PDF documents with different characteristics (low quality scans, different languages, small fonts, etc.)

## Understanding Recognition Parameters

Aspose.OCR Cloud provides numerous settings to optimize the recognition process for different types of documents. Let's examine each parameter in detail to understand when and how to use them effectively.

## Step 1: Language Settings

The `language` parameter specifies which language model to use for recognition. This is one of the most critical parameters for accurate results.

```json
"settings": {
  "language": "English"
}
```

Aspose.OCR Cloud supports [multiple languages](/ocr/supported-languages/), including:
- English
- French
- German
- Spanish
- Portuguese
- Italian
- Russian
- Chinese
- Japanese
- And many others

### When to Adjust Language Settings:

- Single Language: Set to the specific language of your document
- Mixed Languages: For documents with multiple languages, choose the predominant language or use a more generic setting
- Technical Documents: For documents with specialized terminology, choose the appropriate base language

### Try it yourself:
Process the same PDF with different language settings and compare the results, especially for text containing specialized terms or proper nouns.

## Step 2: Image Correction Settings

Scanned documents often have imperfections that can affect recognition accuracy. Aspose.OCR Cloud provides several image correction options:

### Skew Correction

The `makeSkewCorrect` parameter automatically fixes tilted pages:

```json
"settings": {
  "makeSkewCorrect": true
}
```

- Set to `true` for documents scanned at a slight angle (15 degrees or less)
- Set to `false` for perfectly aligned scans to save processing time

### Manual Rotation

The `rotate` parameter allows you to specify a rotation angle (in degrees) for significantly rotated pages:

```json
"settings": {
  "rotate": 90
}
```

Common values:
- `0` - No rotation (default)
- `90` - Rotate 90 degrees clockwise (for landscape pages)
- `180` - Rotate 180 degrees (for upside-down pages)
- `270` or `-90` - Rotate 90 degrees counterclockwise

### Contrast Correction

The `makeContrastCorrection` parameter automatically enhances image contrast:

```json
"settings": {
  "makeContrastCorrection": true
}
```

- Set to `true` for faded or low-contrast scans
- Set to `false` for documents with good contrast

### Binarization

The `makeBinarization` parameter converts the image to black and white:

```json
"settings": {
  "makeBinarization": true
}
```

- Set to `true` for grayscale or colored documents with simple text
- Set to `false` for documents where color information is important

### Upsampling

The `makeUpsampling` parameter intelligently increases image resolution:

```json
"settings": {
  "makeUpsampling": true
}
```

- Set to `true` for low-resolution scans or documents with small fonts
- Set to `false` for high-quality scans to save processing time

### Learning Checkpoint:
For a faded receipt scanned at a slight angle, which image correction settings would you enable?

<details>
<summary>Answer</summary>
You would set <code>makeSkewCorrect: true</code> to fix the tilt and <code>makeContrastCorrection: true</code> to enhance the faded text. You might also consider <code>makeBinarization: true</code> to improve text clarity.
</details>

## Step 3: Advanced Processing Settings

### Spell Check

The `makeSpellCheck` parameter automatically corrects common OCR errors:

```json
"settings": {
  "makeSpellCheck": true
}
```

- Set to `true` for general documents where spelling accuracy is important
- Set to `false` for technical documents with specialized terminology that might be incorrectly "corrected"

### Document Structure Analysis

The `dsrMode` parameter controls how the document structure is analyzed:

```json
"settings": {
  "dsrMode": "Regions"
}
```

Options include:
- `Regions` - Best for documents with mixed content (text, tables, images)
- `Document` - Best for standard text documents with simple layouts
- `TextInCells` - Best for tables and forms
- `None` - No structure analysis, treats everything as continuous text

### DSR Confidence

The `dsrConfidence` parameter sets the threshold for content block detection:

```json
"settings": {
  "dsrConfidence": "Default"
}
```

Options include:
- `Default` - Balanced approach
- `Low` - Detects more blocks but may include noise
- `High` - Only detects clear, well-defined blocks

## Step 4: Output Format Settings

The `resultType` parameter specifies the format of the recognition results:

```json
"settings": {
  "resultType": "Text"
}
```

Options include:
- `Text` - Plain text without formatting
- `Pdf` - Searchable PDF with recognized text
- `TextAndPdf` - Both plain text and searchable PDF
- `Json` - JSON format with detailed position information for each word

### Try it yourself:
Process a document with tables using different `dsrMode` settings and compare how the structure is preserved in the results.

## Step 5: Creating Profiles for Different Document Types

Based on our understanding of each parameter, let's create optimized profiles for common document types:

### General Office Documents

```json
{
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "makeContrastCorrection": true,
    "makeSpellCheck": true,
    "dsrMode": "Document",
    "resultType": "Pdf"
  }
}
```

### Low-Quality Scans

```json
{
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "makeContrastCorrection": true,
    "makeBinarization": true,
    "makeUpsampling": true,
    "dsrMode": "Regions",
    "dsrConfidence": "Low",
    "resultType": "Text"
  }
}
```

### Tables and Forms

```json
{
  "settings": {
    "language": "English",
    "makeSkewCorrect": true,
    "makeContrastCorrection": true,
    "makeSpellCheck": false,
    "dsrMode": "TextInCells",
    "resultType": "Json"
  }
}
```

### Multi-language Documents

```json
{
  "settings": {
    "language": "French",  // Primary language
    "makeSkewCorrect": true,
    "makeContrastCorrection": true,
    "makeSpellCheck": false,  // Disable to prevent incorrect corrections
    "dsrMode": "Document",
    "resultType": "Text"
  }
}
```

## Step 6: Testing and Comparing Results

To find the optimal settings for your specific documents, it's important to test different configurations and compare the results.

Let's implement a simple testing framework:

```javascript
// Pseudocode for testing different settings
async function compareSettings(pdfFile, settingsArray) {
  const results = [];
  
  for (const settings of settingsArray) {
    // Submit the PDF with current settings
    const taskId = await submitPdf(pdfFile, settings);
    
    // Wait for processing to complete
    const result = await pollForResults(taskId);
    
    // Calculate accuracy metrics
    const accuracy = calculateAccuracy(result);
    
    results.push({
      settings,
      accuracy,
      result
    });
  }
  
  // Sort by accuracy
  results.sort((a, b) => b.accuracy - a.accuracy);
  
  // Return the best settings and all results for comparison
  return {
    bestSettings: results[0].settings,
    allResults: results
  };
}
```

### Accuracy Metrics to Consider:

1. Character Error Rate (CER): Percentage of incorrectly recognized characters
2. Word Error Rate (WER): Percentage of incorrectly recognized words
3. Structure Preservation: How well tables, paragraphs, and formatting are maintained
4. Processing Time: Duration of the recognition process

### Try it yourself:
Create a test set of at least three different types of PDF documents. For each document, test at least three different settings profiles and compare the results based on accuracy and processing time.

## Best Practices for Optimizing Recognition Settings

Based on extensive testing with various document types, here are some best practices:

1. Start with the basics: Always set the correct language and basic image correction (skew and contrast)

2. Iterative refinement: Start with default settings, then adjust one parameter at a time to isolate its impact

3. Document-specific profiles: Create specific profiles for different document types in your workflow

4. Balance accuracy and speed: More preprocessing generally improves accuracy but increases processing time

5. Test with representative samples: Use actual documents from your workflow for testing, not idealized examples

6. Consider post-processing: For specialized documents, sometimes minimal OCR settings with custom post-processing yields better results

## Troubleshooting Common Issues

| Issue | Possible Causes | Solutions |
|-------|----------------|-----------|
| Missing characters | Small font, low contrast | Enable `makeUpsampling` and `makeContrastCorrection` |
| Garbled text | Wrong language setting | Set the correct language or try a more generic language |
| Merged paragraphs | Incorrect structure analysis | Try different `dsrMode` settings |
| Slow processing | Too many enabled features | Disable features not needed for your document type |
| Poor table recognition | Incorrect structure mode | Use `dsrMode: "TextInCells"` for tables |

## What You've Learned

Congratulations! In this tutorial, you've learned how to:
- Understand and configure each recognition parameter
- Create optimized profiles for different document types
- Test and compare different settings
- Apply best practices for improved recognition results
- Troubleshoot common recognition issues

## Further Practice

To reinforce what you've learned, try these exercises:
1. Create a settings profile for historical documents with old typography
2. Develop a preprocessing pipeline that automatically selects optimal settings based on document characteristics
3. Compare recognition results with commercial desktop OCR software

## Helpful Resources

- [Product Page](https://products.aspose.cloud/ocr/)
- [Documentation](https://docs.aspose.cloud/ocr/)
- [Live Demo](https://products.aspose.app/ocr/family)
- [API Reference](https://reference.aspose.cloud/ocr/)
- [Blog](https://blog.aspose.cloud/category/ocr/)
- [Free Support](https://forum.aspose.cloud/c/ocr/12/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Feel free to post in our [support forum](https://forum.aspose.cloud/c/ocr/12/) for assistance!