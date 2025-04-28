---
title: Creating Bubble Charts with Aspose.Slides Cloud API Tutorial
url: /working-with-charts/create-bubble-chart/
weight: 50
description: Learn how to create bubble charts in PowerPoint presentations programmatically with Aspose.Slides Cloud API. This tutorial includes step-by-step code examples.
---

## Tutorial: Creating Bubble Charts in PowerPoint Presentations

In this tutorial, you'll learn how to create and customize bubble charts in PowerPoint presentations using Aspose.Slides Cloud API. Bubble charts extend the concept of scatter charts by adding a third dimension represented by the size of the bubble.

## Prerequisites

- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret credentials
- Basic knowledge of REST APIs and your preferred programming language
- A PowerPoint presentation file to work with

## Estimated Time to Complete: 20 minutes

## The Basics of Bubble Charts

Bubble charts are similar to scatter charts but include a third dimension shown by the size of each bubble. They're ideal for:
- Visualizing data with three variables
- Comparing items across multiple dimensions
- Identifying relationships and patterns in complex datasets
- Showing relative proportions or importance of data points

## Step 1: Authentication Setup

Before creating charts, you need to authenticate with the Aspose.Slides Cloud API:

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

## Step 2: Prepare Chart Data

For our tutorial, we'll create a bubble chart showing data about cities with three dimensions:
- X-axis: Population (in millions)
- Y-axis: Cost of living index
- Bubble size: Average income (in thousands)

## Step 3: Create a Bubble Chart

Now, let's create a bubble chart using the Aspose.Slides Cloud API:

### Using cURL

First, prepare a JSON file (`chart.json`) with chart configuration:

```json
{
    "type": "Chart",
    "chartType": "Bubble",
    "x": 100,
    "y": 100,
    "width": 400,
    "height": 400,
    "title": { "text": "City Comparison" },
    "series": [
        {
            "name": "Series1",
            "dataPointType": "Bubble",
            "dataPoints": [
                { "xValue": 8.5, "yValue": 80, "bubbleSize": 62 },
                { "xValue": 3.2, "yValue": 65, "bubbleSize": 48 },
                { "xValue": 1.8, "yValue": 70, "bubbleSize": 53 },
                { "xValue": 5.6, "yValue": 90, "bubbleSize": 70 }
            ]
        },
        {
            "name": "Series2",
            "dataPointType": "Bubble",
            "dataPoints": [
                { "xValue": 7.2, "yValue": 75, "bubbleSize": 45 },
                { "xValue": 2.5, "yValue": 60, "bubbleSize": 40 },
                { "xValue": 4.0, "yValue": 85, "bubbleSize": 63 },
                { "xValue": 6.3, "yValue": 78, "bubbleSize": 52 }
            ]
        }
    ]              
}
```

Then, make the API request:

```bash
curl -v "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/shapes" \
  -d @chart.json \
  -H "Content-Type: text/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Using SDK (C#)

```csharp
SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

// Create a chart DTO
Chart dto = new Chart();
dto.ChartType = Chart.ChartTypeEnum.Bubble;
dto.X = 100;
dto.Y = 100;
dto.Width = 400;
dto.Height = 400;
dto.Title = new ChartTitle { Text = "City Comparison" };

// Add first data series
BubbleSeries series1 = new BubbleSeries();
series1.Name = "Series1";
series1.DataPoints = new List<BubbleChartDataPoint>
{
    new BubbleChartDataPoint { XValue = 8.5, YValue = 80, BubbleSize = 62 },
    new BubbleChartDataPoint { XValue = 3.2, YValue = 65, BubbleSize = 48 },
    new BubbleChartDataPoint { XValue = 1.8, YValue = 70, BubbleSize = 53 },
    new BubbleChartDataPoint { XValue = 5.6, YValue = 90, BubbleSize = 70 }
};

// Add second data series
BubbleSeries series2 = new BubbleSeries();
series2.Name = "Series2";
series2.DataPoints = new List<BubbleChartDataPoint>
{
    new BubbleChartDataPoint { XValue = 7.2, YValue = 75, BubbleSize = 45 },
    new BubbleChartDataPoint { XValue = 2.5, YValue = 60, BubbleSize = 40 },
    new BubbleChartDataPoint { XValue = 4.0, YValue = 85, BubbleSize = 63 },
    new BubbleChartDataPoint { XValue = 6.3, YValue = 78, BubbleSize = 52 }
};

dto.Series = new List<Series> { series1, series2 };

// Create the chart
Chart chart = (Chart)api.CreateShape("MyPresentation.pptx", 1, dto);
Console.WriteLine($"Bubble chart created with {chart.Series.Count} series");
```

### Using SDK (Python)

```python
import asposeslidescloud
from asposeslidescloud.configuration import Configuration
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.chart import Chart
from asposeslidescloud.models.chart_title import ChartTitle
from asposeslidescloud.models.bubble_series import BubbleSeries
from asposeslidescloud.models.bubble_chart_data_point import BubbleChartDataPoint

# Configure API client
configuration = Configuration()
configuration.app_sid = 'YOUR_CLIENT_ID'
configuration.app_key = 'YOUR_CLIENT_SECRET'
api = SlidesApi(configuration)

# Create chart object
dto = Chart()
dto.chart_type = 'Bubble'
dto.x = 100
dto.y = 100
dto.width = 400
dto.height = 400

# Add chart title
title = ChartTitle()
title.text = 'City Comparison'
dto.title = title

# Add first data series
series1 = BubbleSeries()
series1.name = 'Series1'
data_point11 = BubbleChartDataPoint()
data_point11.x_value = 8.5
data_point11.y_value = 80
data_point11.bubble_size = 62
data_point12 = BubbleChartDataPoint()
data_point12.x_value = 3.2
data_point12.y_value = 65
data_point12.bubble_size = 48
data_point13 = BubbleChartDataPoint()
data_point13.x_value = 1.8
data_point13.y_value = 70
data_point13.bubble_size = 53
data_point14 = BubbleChartDataPoint()
data_point14.x_value = 5.6
data_point14.y_value = 90
data_point14.bubble_size = 70
series1.data_points = [data_point11, data_point12, data_point13, data_point14]

# Add second data series
series2 = BubbleSeries()
series2.name = 'Series2'
data_point21 = BubbleChartDataPoint()
data_point21.x_value = 7.2
data_point21.y_value = 75
data_point21.bubble_size = 45
data_point22 = BubbleChartDataPoint()
data_point22.x_value = 2.5
data_point22.y_value = 60
data_point22.bubble_size = 40
data_point23 = BubbleChartDataPoint()
data_point23.x_value = 4.0
data_point23.y_value = 85
data_point23.bubble_size = 63
data_point24 = BubbleChartDataPoint()
data_point24.x_value = 6.3
data_point24.y_value = 78
data_point24.bubble_size = 52
series2.data_points = [data_point21, data_point22, data_point23, data_point24]

dto.series = [series1, series2]

# Create the chart
result = api.create_shape("MyPresentation.pptx", 1, dto)
print(f"Bubble chart created with {len(result.series)} series")
```

## Step 4: Understanding Bubble Chart Data

A bubble chart in Aspose.Slides Cloud API requires three values for each data point:

1. xValue: Position on the X-axis (horizontal)
2. yValue: Position on the Y-axis (vertical)
3. bubbleSize: Size of the bubble, representing the third dimension of data

The bubble size values are proportional - larger values create larger bubbles. The actual visual size is determined by the chart engine based on the range of all bubble sizes.

## Step 5: Examine the Result

After executing the API request, a bubble chart will be added to your presentation. The chart will display:
- Two series of data points, each with four bubbles
- X-axis showing population values (1.8 to 8.5 million)
- Y-axis showing cost of living index (60 to 90)
- Bubble sizes representing average income (40 to 70 thousand)

## Step 6: Key Bubble Chart Considerations

When creating bubble charts, keep these points in mind:

1. Scale Appropriately: Ensure your axis scales are appropriate for your data range.
2. Size Proportions: The bubbleSize values should be proportionate to maintain visual clarity.
3. Data Representation: Use meaningful dimensions for x-value, y-value, and bubble-size that logically relate to each other.
4. Series Organization: Group related data points in the same series for better visual comparison.

## Try It Yourself: Customization Exercise

Now that you've created a basic bubble chart, try these customizations:

1. Add a third data series with different data points
2. Add titles to the X and Y axes to clarify what they represent
3. Adjust the chart dimensions or position on the slide
4. Modify the bubble sizes to better differentiate between data points

## Troubleshooting Tips

- Ensure all three values (xValue, yValue, bubbleSize) are specified for each data point
- Check that the dataPointType is set to "Bubble" for all series
- Verify that your JSON structure is valid if using the REST API directly
- Make sure bubble sizes are within a reasonable range - values that are too large or too small may cause display issues

## What You've Learned

In this tutorial, you've learned how to:
- Create a bubble chart in a PowerPoint presentation using Aspose.Slides Cloud API
- Configure three-dimensional data with X, Y, and bubble size values
- Set up multiple data series for comparison
- Define basic chart properties and appearance

## Next Steps

Now that you know how to create bubble charts, you might want to explore:

- [How to Configure Chart Data Sources](/working-with-charts/chart-data-source/)
- [Tutorial: Setting Chart Axes Properties](/working-with-charts/chart-axis/)
- [Learn to Create Column Charts](/working-with-charts/create-column-chart/)

## Further Practice

Try creating bubble charts to visualize:
1. Countries by population, GDP, and life expectancy
2. Products by sales volume, price, and customer satisfaction rating
3. Websites by daily visitors, conversion rate, and average time on site

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
