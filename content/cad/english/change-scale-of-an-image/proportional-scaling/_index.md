---
title: Proportional vs Non-Proportional Scaling Tutorial
url: /change-scale-of-an-image/proportional-scaling/
weight: 50
description: Learn to control aspect ratio when scaling CAD drawings with Aspose.CAD Cloud API in this intermediate tutorial
---

# Tutorial: Proportional vs Non-Proportional Scaling

## Learning Objectives

In this tutorial, you'll learn:
- The difference between proportional and non-proportional scaling
- How to maintain aspect ratio when resizing CAD drawings
- When to use each scaling approach
- How to implement both scaling methods using Aspose.CAD Cloud API

## Prerequisites

Before starting this tutorial, you should have:
- Completed the [Working with Different Output Formats](/change-scale-of-an-image/output-formats/) tutorial
- An Aspose Cloud account with an active subscription or trial
- Your Client ID and Client Secret from the Aspose Cloud dashboard
- A CAD drawing (DWG, DXF, etc.) on your local machine or in cloud storage
- Basic understanding of aspect ratio and image proportions

## Introduction

When scaling CAD drawings, one of the most important considerations is whether to maintain the original aspect ratio. Depending on your requirements, you might need to:

- Preserve proportions to maintain the visual accuracy of the drawing
- Adjust proportions to fit specific layout requirements or aspect ratios
- Scale only one dimension while calculating the other to maintain proportions

In this tutorial, we'll explore both proportional and non-proportional scaling approaches using Aspose.CAD Cloud API, and help you understand when each approach is appropriate.

## Understanding Aspect Ratio

Aspect ratio is the proportional relationship between the width and height of an image. It's typically expressed as two numbers separated by a colon (e.g., 4:3, 16:9, 1:1).

For CAD drawings, maintaining the correct aspect ratio is often critical to ensure that:
- Circular elements remain circular (not oval)
- Squares remain square (not rectangular)
- The overall drawing isn't visually distorted
- Measurements and proportions remain accurate

## Proportional Scaling

Proportional scaling (also called uniform scaling) maintains the original aspect ratio of the drawing, ensuring that relative dimensions stay consistent.

### When to Use Proportional Scaling

Use proportional scaling when:
- Visual accuracy is important
- The drawing contains circles, arcs, or other elements that would be noticeably distorted
- You need to create thumbnails that accurately represent the original drawing
- You're preparing technical documentation where proportions matter

### Implementing Proportional Scaling

There are two main approaches to implementing proportional scaling:

#### Method 1: Specify Only One Dimension

The simplest way to ensure proportional scaling is to specify only one dimension (either width or height) and let the system calculate the other dimension automatically.

However, the Aspose.CAD Cloud API currently requires both dimensions to be specified, so we'll need to calculate the second dimension ourselves.

#### Method 2: Calculate Both Dimensions

To maintain the aspect ratio while specifying both dimensions:

1. Determine the aspect ratio of the original drawing
2. Calculate new dimensions that maintain this ratio
3. Set the new width and height accordingly

Let's see how to implement this in practice.

## Step 1: Determine the Original Drawing Dimensions

Before scaling, we need to know the original dimensions of the CAD drawing. We can use the `GET /cad/{name}/properties` endpoint to retrieve this information:

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/sample.dxf/properties" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

The response will include the width and height of the drawing:

```json
{
  "Width": "199",
  "Height": "130",
  "...": "..."
}
```

## Step 2: Calculate Proportional Dimensions

Once we have the original dimensions, we can calculate new dimensions that maintain the same aspect ratio. Here's a simple formula:

If you want to specify a target width:
```
newHeight = originalHeight * (targetWidth / originalWidth)
```

If you want to specify a target height:
```
newWidth = originalWidth * (targetHeight / originalHeight)
```

Let's implement this in Python:

```python
# Tutorial Code Example - Proportional Scaling in Python
import os
import asposecadcloud
from asposecadcloud.apis.cad_api import CadApi
from asposecadcloud.api_client import ApiClient
from asposecadcloud.configuration import Configuration
import json

# Get your client_id and client_secret from https://dashboard.aspose.cloud/
configuration = Configuration(client_id="YOUR_CLIENT_ID", client_secret="YOUR_CLIENT_SECRET")
api_client = ApiClient(configuration)

# Create API instance
api = CadApi(api_client)

try:
    # Specify the drawing name in storage
    name = "sample.dxf"
    
    # Get drawing properties
    properties = api.get_drawing_properties(name)
    
    # Parse the properties
    if hasattr(properties, 'width') and hasattr(properties, 'height'):
        original_width = float(properties.width)
        original_height = float(properties.height)
        
        # Calculate aspect ratio
        aspect_ratio = original_width / original_height
        
        # Target width (you can change this)
        target_width = 800
        
        # Calculate height to maintain proportions
        target_height = int(target_width / aspect_ratio)
        
        print(f"Original dimensions: {original_width}x{original_height}")
        print(f"New dimensions: {target_width}x{target_height}")
        
        # Specify output format
        output_format = "pdf"
        
        # Call the API with proportional dimensions
        response = api.get_image_resize(name, output_format, target_width, target_height)
        
        # Save the resized drawing
        with open("proportionally_resized.pdf", 'wb') as f:
            f.write(response)
        
        print("Successfully resized the drawing proportionally.")
    else:
        print("Error: Could not retrieve drawing dimensions")
    
except Exception as e:
    print("Error: " + str(e))
```

## Step 3: Implement Proportional Scaling API Calls

Now, let's see how to implement proportional scaling in different programming languages:

### C# Implementation

```csharp
// Tutorial Code Example - Proportional Scaling in C#
using System;
using System.IO;
using Aspose.CAD.Cloud.Sdk.Api;
using Aspose.CAD.Cloud.Sdk.Model;
using Aspose.CAD.Cloud.Sdk.Model.Requests;

namespace Aspose.CAD.Cloud.Examples
{
    class Program
    {
        static void Main(string[] args)
        {
            // Get your ClientID and ClientSecret from https://dashboard.aspose.cloud/
            string clientId = "YOUR_CLIENT_ID";
            string clientSecret = "YOUR_CLIENT_SECRET";
            
            // Create API instance
            var api = new CadApi(clientId, clientSecret);
            
            // The drawing name in storage
            var name = "sample.dxf";
            
            try
            {
                // Get drawing properties
                var propertiesRequest = new GetDrawingPropertiesRequest(name);
                var properties = api.GetDrawingProperties(propertiesRequest);
                
                // Parse the properties
                if (!string.IsNullOrEmpty(properties.Width) && !string.IsNullOrEmpty(properties.Height))
                {
                    float originalWidth = float.Parse(properties.Width);
                    float originalHeight = float.Parse(properties.Height);
                    
                    // Calculate aspect ratio
                    float aspectRatio = originalWidth / originalHeight;
                    
                    // Target width (you can change this)
                    int targetWidth = 800;
                    
                    // Calculate height to maintain proportions
                    int targetHeight = (int)(targetWidth / aspectRatio);
                    
                    Console.WriteLine($"Original dimensions: {originalWidth}x{originalHeight}");
                    Console.WriteLine($"New dimensions: {targetWidth}x{targetHeight}");
                    
                    // Specify output format
                    var outputFormat = "pdf";
                    
                    // Create request
                    var request = new GetImageResizeRequest(name, outputFormat, targetWidth, targetHeight);
                    
                    // Call the API
                    using (var response = api.GetImageResize(request))
                    {
                        // Save the result to a file
                        using (var output = File.Create("proportionally_resized.pdf"))
                        {
                            response.CopyTo(output);
                        }
                        
                        Console.WriteLine("Successfully resized the drawing proportionally.");
                    }
                }
                else
                {
                    Console.WriteLine("Error: Could not retrieve drawing dimensions");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error: " + ex.Message);
            }
        }
    }
}
```

### Node.js Implementation

```javascript
// Tutorial Code Example - Proportional Scaling in Node.js
const fs = require('fs');
const request = require('request');

// Get your client_id and client_secret from https://dashboard.aspose.cloud/
const clientId = 'YOUR_CLIENT_ID';
const clientSecret = 'YOUR_CLIENT_SECRET';

// Function to get the access token
function getAccessToken() {
    return new Promise((resolve, reject) => {
        request.post({
            url: 'https://api.aspose.cloud/connect/token',
            form: {
                grant_type: 'client_credentials',
                client_id: clientId,
                client_secret: clientSecret
            },
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            }
        }, (error, response, body) => {
            if (error) {
                reject(error);
            } else {
                const data = JSON.parse(body);
                resolve(data.access_token);
            }
        });
    });
}

// Function to get drawing properties
function getDrawingProperties(token, name) {
    return new Promise((resolve, reject) => {
        request.get({
            url: `https://api.aspose.cloud/v3.0/cad/${name}/properties`,
            headers: {
                'Authorization': `Bearer ${token}`
            }
        }, (error, response, body) => {
            if (error) {
                reject(error);
            } else {
                resolve(JSON.parse(body));
            }
        });
    });
}

// Function to resize the drawing proportionally
async function resizeDrawingProportionally() {
    try {
        // Get the access token
        const token = await getAccessToken();
        
        // Specify the drawing name in storage
        const name = 'sample.dxf';
        
        // Get the drawing properties
        const properties = await getDrawingProperties(token, name);
        
        if (properties.Width && properties.Height) {
            const originalWidth = parseFloat(properties.Width);
            const originalHeight = parseFloat(properties.Height);
            
            // Calculate aspect ratio
            const aspectRatio = originalWidth / originalHeight;
            
            // Target width (you can change this)
            const targetWidth = 800;
            
            // Calculate height to maintain proportions
            const targetHeight = Math.round(targetWidth / aspectRatio);
            
            console.log(`Original dimensions: ${originalWidth}x${originalHeight}`);
            console.log(`New dimensions: ${targetWidth}x${targetHeight}`);
            
            // Specify output format
            const outputFormat = 'pdf';
            
            // Create the API URL
            const url = `https://api.aspose.cloud/v3.0/cad/${name}/resize?outputFormat=${outputFormat}&newWidth=${targetWidth}&newHeight=${targetHeight}`;
            
            // Make the API call
            request.get({
                url: url,
                headers: {
                    'Authorization': `Bearer ${token}`
                },
                encoding: null  // Important for binary data
            }, (error, response, body) => {
                if (error) {
                    console.error('Error:', error);
                } else {
                    // Save the resized drawing
                    fs.writeFileSync('proportionally_resized.pdf', body);
                    console.log('Successfully resized the drawing proportionally.');
                }
            });
        } else {
            console.error('Error: Could not retrieve drawing dimensions');
        }
    } catch (error) {
        console.error('Error:', error);
    }
}

// Execute the resize operation
resizeDrawingProportionally();
```

## Non-Proportional Scaling

Non-proportional scaling (also called non-uniform scaling) changes the width and height independently, which can alter the aspect ratio of the drawing.

### When to Use Non-Proportional Scaling

Use non-proportional scaling when:
- You need to fit the drawing into a specific layout with fixed dimensions
- The original proportions are not critical to preserve
- You want to deliberately emphasize one dimension
- You're creating diagrams where exact proportions aren't necessary

### Implementing Non-Proportional Scaling

Implementing non-proportional scaling is straightforward with Aspose.CAD Cloud API. Simply specify the exact width and height you want, without calculating proportional dimensions:

```bash
curl -v "https://api.aspose.cloud/v3.0/cad/sample.dxf/resize?outputFormat=pdf&newWidth=800&newHeight=400" \
-X GET \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
-o non_proportional_resized.pdf
```

In this example, we're setting a width of 800 pixels and a height of 400 pixels, regardless of the original aspect ratio.

## Hybrid Approach: Fixed Maximum Dimensions

Sometimes, you want to ensure a drawing fits within certain maximum dimensions while maintaining its proportions. This is a common approach for thumbnails or preview images.

In this approach:
1. Determine the limiting dimension (width or height)
2. Scale proportionally based on that dimension

Here's a Python implementation:

```python
# Tutorial Code Example - Maximum Dimensions Scaling in Python

# ... (Same setup code as before)

try:
    # Specify the drawing name in storage
    name = "sample.dxf"
    
    # Get drawing properties
    properties = api.get_drawing_properties(name)
    
    # Parse the properties
    if hasattr(properties, 'width') and hasattr(properties, 'height'):
        original_width = float(properties.width)
        original_height = float(properties.height)
        
        # Maximum dimensions allowed
        max_width = 800
        max_height = 600
        
        # Calculate scaling factors for both dimensions
        width_scale = max_width / original_width
        height_scale = max_height / original_height
        
        # Use the smaller scaling factor to ensure the image fits within both constraints
        scale = min(width_scale, height_scale)
        
        # Calculate new dimensions
        target_width = int(original_width * scale)
        target_height = int(original_height * scale)
        
        print(f"Original dimensions: {original_width}x{original_height}")
        print(f"New dimensions: {target_width}x{target_height}")
        
        # Specify output format
        output_format = "pdf"
        
        # Call the API with calculated dimensions
        response = api.get_image_resize(name, output_format, target_width, target_height)
        
        # Save the resized drawing
        with open("max_dimensions_resized.pdf", 'wb') as f:
            f.write(response)
        
        print("Successfully resized the drawing to fit maximum dimensions.")
    else:
        print("Error: Could not retrieve drawing dimensions")
    
except Exception as e:
    print("Error: " + str(e))
```

## Try It Yourself: Comparing Scaling Approaches

Now, let's experiment with different scaling approaches and observe the results:

1. Choose a CAD drawing with a clear aspect ratio (e.g., a drawing with circles or squares)
2. Resize the drawing using the following approaches:
   - Proportional scaling (maintaining aspect ratio)
   - Non-proportional scaling (fixed width and height)
   - Maximum dimensions scaling (fit within constraints)
3. Compare the visual results and note any distortions or issues

## Which Approach Should You Choose?

The best scaling approach depends on your specific requirements:

- For technical accuracy: Always use proportional scaling to maintain the integrity of the drawing
- For web thumbnails: Use maximum dimensions scaling to create consistent galleries
- For fixed layout requirements: Use non-proportional scaling, but be aware of potential distortion
- For printing: Typically proportional scaling, possibly with margins or borders to fit standard paper sizes

## Troubleshooting Scaling Issues

If you encounter problems with your scaling operations, consider these solutions:

1. Distorted Elements: If circular or square elements appear distorted, you're likely using non-proportional scaling. Switch to proportional scaling.

2. Image Doesn't Fit: If the image doesn't fit your required space, consider using the maximum dimensions approach rather than forcing specific dimensions.

3. Unexpected Results: Double-check your aspect ratio calculations, especially when parsing string values from properties.

4. Calculation Precision: Be aware of rounding issues when calculating dimensions. Consider using floats for calculations and integers only for the final API call.

## Learning Checkpoint

Test your understanding of the concepts covered:

1. What is the formula to calculate a new height while maintaining aspect ratio, if you know the target width?
2. When would you choose non-proportional scaling over proportional scaling?
3. How would you ensure a drawing fits within a 1000x800 area while maintaining its proportions?
4. What problems might occur if you use non-proportional scaling on a CAD drawing with circles?

## What You've Learned

In this tutorial, you've learned:
- The difference between proportional and non-proportional scaling
- How to calculate dimensions to maintain aspect ratio
- When to use each scaling approach
- How to implement different scaling strategies using Aspose.CAD Cloud API

## Next Steps

Now that you understand how to control aspect ratio when scaling, you're ready to learn how to implement these concepts in a complete application.

Continue to the next tutorial: [SDK Implementation in C#](/change-scale-of-an-image/sdk-csharp/) to learn how to integrate scaling operations into a C# application.

## Further Practice

To reinforce your understanding:
- Create a utility function that calculates proportional dimensions based on a target width or height
- Build a simple application that provides options for different scaling approaches
- Experiment with extremely wide or tall drawings to see how each scaling approach handles them
- Create a visual comparison of the same drawing scaled using different methods

## Helpful Resources

- [Product Page](https://products.aspose.cloud/cad/)
- [Documentation](https://docs.aspose.cloud/cad/)
- [Live Demo](https://products.aspose.app/cad/family)
- [API Reference](https://reference.aspose.cloud/cad/)
- [Blog](https://blog.aspose.cloud/category/cad/)
- [Free Support](https://forum.aspose.cloud/c/cad/28/)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)

Have questions about this tutorial? Post them on the [Aspose.CAD Cloud Forum](https://forum.aspose.cloud/c/cad/28/) for quick assistance!