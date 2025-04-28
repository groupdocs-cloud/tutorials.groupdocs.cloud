---
title: Setting Chart Axes Properties with Aspose.Slides Cloud API Tutorial
url: /working-with-charts/chart-axis/
weight: 60
description: Learn how to customize chart axes in PowerPoint presentations using Aspose.Slides Cloud API in this step-by-step tutorial with practical examples.
---

## Tutorial: Customizing Chart Axes in PowerPoint Presentations

In this tutorial, you'll learn how to customize chart axes in PowerPoint presentations using Aspose.Slides Cloud API. Proper axis configuration is essential for creating clear, informative charts that effectively communicate your data.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Customize horizontal and vertical axes in PowerPoint charts
- Set axis titles, scales, and formatting options
- Apply best practices for axis configuration to enhance data visualization

## Prerequisites

- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret credentials
- Basic knowledge of REST APIs and your preferred programming language
- A PowerPoint presentation with at least one chart
- Understanding of basic chart concepts

## The Importance of Chart Axes

Chart axes provide the framework for interpreting the data in your charts. Proper axis configuration helps your audience understand:
- The scale and range of your data
- Units of measurement
- Categories being compared
- Relative differences between data points

## Step 1: Authentication Setup

Before customizing chart axes, you need to authenticate with the Aspose.Slides Cloud API:

### Using cURL

```bash
curl -v "https://api.aspose.cloud/connect/token" \
  -X POST \
  -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json"
```

Save the access token from the response for use in subsequent requests.

### Using SDK (C#)

```csharp
// Initialize the API with your credentials
SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
```

## Step 2: Understanding Chart Axis Types

Before customizing axes, it's important to understand the different axis types:

1. Horizontal Axis (Category Axis): Typically shows categories or time periods.
2. Vertical Axis (Value Axis): Displays numerical values or measurements.
3. Secondary Axes: Additional axes used for displaying different scales or units.

The Aspose.Slides Cloud API uses specific axis types in its API calls:
- `VerticalAxis`: The primary Y-axis
- `HorizontalAxis`: The primary X-axis
- `SecondaryVerticalAxis`: The secondary Y-axis
- `SecondaryHorizontalAxis`: The secondary X-axis

## Step 3: Customizing a Vertical Axis

Let's modify the vertical axis of an existing chart to improve its readability:

### Using cURL

First, create a JSON file (`vertical_axis.json`) with the axis configuration:

```json
{
  "HasTitle": true,
  "Title": {
    "Text": "Sales (in thousands)"
  },
  "IsAutomaticMaxValue": false,
  "MaxValue": 100,
  "IsAutomaticMinValue": false,
  "MinValue": 0,
  "MajorTickMark": "Outside",
  "MinorTickMark": "None",
  "DisplayUnit": "Thousands"
}
```

Then, make the API request:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/shapes/2/VerticalAxis" \
  -d @vertical_axis.json \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Using SDK (C#)

```csharp
SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

string fileName = "MyPresentation.pptx";
int slideIndex = 1;
int shapeIndex = 2; // Assuming the chart is the second shape on the slide
AxisType axisType = AxisType.VerticalAxis;

// Create the axis configuration
Axis axis = new Axis
{
    HasTitle = true,
    Title = new ChartTitle { Text = "Sales (in thousands)" },
    IsAutomaticMaxValue = false,
    MaxValue = 100,
    IsAutomaticMinValue = false,
    MinValue = 0,
    MajorTickMark = TickMark.Outside,
    MinorTickMark = TickMark.None,
    DisplayUnit = DisplayUnitType.Thousands
};

// Update the axis
Axis updatedAxis = api.SetChartAxis(fileName, slideIndex, shapeIndex, axisType, axis);
Console.WriteLine("Vertical axis updated successfully");
```

### Using SDK (Python)

```python
import asposeslidescloud
from asposeslidescloud.configuration import Configuration
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.axis import Axis
from asposeslidescloud.models.chart_title import ChartTitle
from asposeslidescloud.models import AxisType, TickMark, DisplayUnitType

# Configure API client
configuration = Configuration()
configuration.app_sid = 'YOUR_CLIENT_ID'
configuration.app_key = 'YOUR_CLIENT_SECRET'
api = SlidesApi(configuration)

file_name = "MyPresentation.pptx"
slide_index = 1
shape_index = 2  # Assuming the chart is the second shape on the slide
axis_type = AxisType.VERTICALAXIS

# Create the axis configuration
axis = Axis()
axis.has_title = True
axis.title = ChartTitle()
axis.title.text = "Sales (in thousands)"
axis.is_automatic_max_value = False
axis.max_value = 100
axis.is_automatic_min_value = False
axis.min_value = 0
axis.major_tick_mark = TickMark.OUTSIDE
axis.minor_tick_mark = TickMark.NONE
axis.display_unit = DisplayUnitType.THOUSANDS

# Update the axis
updated_axis = api.set_chart_axis(file_name, slide_index, shape_index, axis_type, axis)
print("Vertical axis updated successfully")
```

## Step 4: Customizing a Horizontal Axis

Now, let's customize the horizontal axis to better display categories:

### Using cURL

Create a JSON file (`horizontal_axis.json`) with the axis configuration:

```json
{
  "HasTitle": true,
  "Title": {
    "Text": "Product Categories"
  },
  "AxisBetweenCategories": true,
  "TickLabelRotationAngle": 45,
  "TickLabelSpacing": 1,
  "MajorTickMark": "Outside"
}
```

Then, make the API request:

```bash
curl -X PUT "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/shapes/2/HorizontalAxis" \
  -d @horizontal_axis.json \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Using SDK (C#)

```csharp
SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

string fileName = "MyPresentation.pptx";
int slideIndex = 1;
int shapeIndex = 2; // Assuming the chart is the second shape on the slide
AxisType axisType = AxisType.HorizontalAxis;

// Create the axis configuration
Axis axis = new Axis
{
    HasTitle = true,
    Title = new ChartTitle { Text = "Product Categories" },
    AxisBetweenCategories = true,
    TickLabelRotationAngle = 45,
    TickLabelSpacing = 1,
    MajorTickMark = TickMark.Outside
};

// Update the axis
Axis updatedAxis = api.SetChartAxis(fileName, slideIndex, shapeIndex, axisType, axis);
Console.WriteLine("Horizontal axis updated successfully");
```

## Step 5: Key Axis Properties Explained

When customizing chart axes, these are the most important properties to consider:

### Title Properties:
- HasTitle: Determines whether the axis has a title.
- Title.Text: The text to display as the axis title.

### Value Range Properties:
- IsAutomaticMinValue/IsAutomaticMaxValue: When set to true, PowerPoint automatically determines the min/max values.
- MinValue/MaxValue: Specify custom minimum and maximum values for the axis.

### Tick Mark Properties:
- MajorTickMark/MinorTickMark: Controls the appearance of tick marks (None, Inside, Outside, Cross).
- TickLabelSpacing: Specifies the interval between tick labels.
- TickLabelRotationAngle: Rotates tick labels to prevent overlap (useful for long category names).

### Display Properties:
- DisplayUnit: Simplifies large numbers (Hundreds, Thousands, Millions, etc.).
- AxisBetweenCategories: When true, places the axis line between categories instead of at the edge.

## Step 6: Verify the Results

After updating the axes, check your presentation to verify that:
- The vertical axis shows a range from 0 to 100 with values displayed in thousands
- The vertical axis has the title "Sales (in thousands)"
- The horizontal axis has the title "Product Categories"
- Category labels are rotated 45 degrees for better readability
- Tick marks appear correctly on both axes

## Try It Yourself: Customization Exercise

Now that you understand the basics of axis customization, try these additional exercises:

1. Add a secondary vertical axis to display a different scale
2. Set logarithmic scaling on the vertical axis for exponential data
3. Customize the grid lines for better data visualization
4. Adjust the axis line formatting (color, style, etc.)

## Troubleshooting Tips

- If your axis changes aren't appearing, make sure you're using the correct shape index for your chart
- Check that you're using the appropriate axis type (VerticalAxis, HorizontalAxis, etc.)
- Verify that your JSON structure is valid if using the REST API directly
- Ensure that min and max values make sense for your data range

## What You've Learned

In this tutorial, you've learned how to:
- Customize vertical and horizontal axes in PowerPoint charts
- Set axis titles, scales, and tick marks
- Control the display of axis values
- Improve chart readability through proper axis configuration

## Next Steps

Now that you know how to customize chart axes, you might want to explore:

- [How to Configure Chart Data Sources](/working-with-charts/chart-data-source/)
- [Tutorial: Creating Column Charts](/working-with-charts/create-column-chart/)
- [Learn to Create Bubble Charts](/working-with-charts/create-bubble-chart/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)

