---
title: Creating Scattered Charts with Aspose.Slides Cloud API Tutorial
url: /working-with-charts/create-scattered-chart/
weight: 40
description: Learn how to create scatter charts in PowerPoint presentations using Aspose.Slides Cloud API in this step-by-step programming tutorial with code examples.
---

## Tutorial: Creating Scattered Charts in PowerPoint Presentations

In this tutorial, you'll learn how to create and customize scattered charts (also known as scatter plots or XY charts) in PowerPoint presentations using Aspose.Slides Cloud API. Scatter charts are ideal for showing relationships between two variables and identifying potential correlations or patterns.

## Prerequisites

- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret credentials
- Basic knowledge of REST APIs and your preferred programming language
- A PowerPoint presentation file to work with

## The Basics of Scattered Charts

Scatter charts plot data points using Cartesian coordinates to show the relationship between two variables. Each data point has two values: X (horizontal axis) and Y (vertical axis). They're ideal for:
- Showing correlation between variables
- Identifying data clusters and outliers
- Illustrating distribution patterns

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

For our tutorial, we'll create a scatter chart showing the relationship between hours studied and exam scores, with two data series representing different student groups.

## Step 3: Create a Scattered Chart

Now, let's create a scatter chart using the Aspose.Slides Cloud API:

### Using cURL

First, prepare a JSON file (`chart.json`) with chart configuration:

```json
{
    "type": "Chart",
    "chartType": "ScatterWithSmoothLines",
    "x": 100,
    "y": 100,
    "width": 400,
    "height": 400,
    "title": { "text": "Study Hours vs. Exam Score" },
    "series": [
        {
            "name": "Group A",
            "dataPointType": "Scatter",
            "dataPoints": [
                { "xvalue": 1, "yvalue": 65 },
                { "xvalue": 2, "yvalue": 70 },
                { "xvalue": 3, "yvalue": 80 },
                { "xvalue": 4, "yvalue": 85 },
                { "xvalue": 5, "yvalue": 90 }
            ]
        },
        {
            "name": "Group B",
            "dataPointType": "Scatter",
            "dataPoints": [
                { "xvalue": 1, "yvalue": 60 },
                { "xvalue": 2, "yvalue": 65 },
                { "xvalue": 3, "yvalue": 70 },
                { "xvalue": 4, "yvalue": 75 },
                { "xvalue": 5, "yvalue": 80 }
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
dto.ChartType = Chart.ChartTypeEnum.ScatterWithSmoothLines;
dto.X = 100;
dto.Y = 100;
dto.Width = 400;
dto.Height = 400;
dto.Title = new ChartTitle { Text = "Study Hours vs. Exam Score" };

// Add first data series (Group A)
ScatterSeries series1 = new ScatterSeries();
series1.Name = "Group A";
series1.DataPoints = new List<ScatterChartDataPoint>
{
    new ScatterChartDataPoint { XValue = 1, YValue = 65 },
    new ScatterChartDataPoint { XValue = 2, YValue = 70 },
    new ScatterChartDataPoint { XValue = 3, YValue = 80 },
    new ScatterChartDataPoint { XValue = 4, YValue = 85 },
    new ScatterChartDataPoint { XValue = 5, YValue = 90 }
};

// Add second data series (Group B)
ScatterSeries series2 = new ScatterSeries();
series2.Name = "Group B";
series2.DataPoints = new List<ScatterChartDataPoint>
{
    new ScatterChartDataPoint { XValue = 1, YValue = 60 },
    new ScatterChartDataPoint { XValue = 2, YValue = 65 },
    new ScatterChartDataPoint { XValue = 3, YValue = 70 },
    new ScatterChartDataPoint { XValue = 4, YValue = 75 },
    new ScatterChartDataPoint { XValue = 5, YValue = 80 }
};

dto.Series = new List<Series> { series1, series2 };

// Create the chart
Chart chart = (Chart)api.CreateShape("MyPresentation.pptx", 1, dto);
Console.WriteLine($"Scatter chart created with {chart.Series.Count} series");
```

### Using SDK (Python)

```python
import asposeslidescloud
from asposeslidescloud.configuration import Configuration
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.chart import Chart
from asposeslidescloud.models.chart_title import ChartTitle
from asposeslidescloud.models.scatter_series import ScatterSeries
from asposeslidescloud.models.scatter_chart_data_point import ScatterChartDataPoint

# Configure API client
configuration = Configuration()
configuration.app_sid = 'YOUR_CLIENT_ID'
configuration.app_key = 'YOUR_CLIENT_SECRET'
api = SlidesApi(configuration)

# Create chart object
dto = Chart()
dto.chart_type = 'ScatterWithSmoothLines'
dto.x = 100
dto.y = 100
dto.width = 400
dto.height = 400

# Add chart title
title = ChartTitle()
title.text = 'Study Hours vs. Exam Score'
dto.title = title

# Add first data series (Group A)
series1 = ScatterSeries()
series1.name = 'Group A'
data_point11 = ScatterChartDataPoint()
data_point11.x_value = 1
data_point11.y_value = 65
data_point12 = ScatterChartDataPoint()
data_point12.x_value = 2
data_point12.y_value = 70
data_point13 = ScatterChartDataPoint()
data_point13.x_value = 3
data_point13.y_value = 80
data_point14 = ScatterChartDataPoint()
data_point14.x_value = 4
data_point14.y_value = 85
data_point15 = ScatterChartDataPoint()
data_point15.x_value = 5
data_point15.y_value = 90
series1.data_points = [data_point11, data_point12, data_point13, data_point14, data_point15]

# Add second data series (Group B)
series2 = ScatterSeries()
series2.name = 'Group B'
data_point21 = ScatterChartDataPoint()
data_point21.x_value = 1
data_point21.y_value = 60
data_point22 = ScatterChartDataPoint()
data_point22.x_value = 2
data_point22.y_value = 65
data_point23 = ScatterChartDataPoint()
data_point23.x_value = 3
data_point23.y_value = 70
data_point24 = ScatterChartDataPoint()
data_point24.x_value = 4
data_point24.y_value = 75
data_point25 = ScatterChartDataPoint()
data_point25.x_value = 5
data_point25.y_value = 80
series2.data_points = [data_point21, data_point22, data_point23, data_point24, data_point25]

dto.series = [series1, series2]

# Create the chart
result = api.create_shape("MyPresentation.pptx", 1, dto)
print(f"Scatter chart created with {len(result.series)} series")
```

## Step 4: Understand Scatter Chart Types

Aspose.Slides Cloud API supports several types of scatter charts:

1. Scatter - Simple data points without lines
2. ScatterWithSmoothLines - Data points connected with smooth curves (used in our example)
3. ScatterWithStraightLines - Data points connected with straight lines
4. ScatterWithSmoothLinesAndMarkers - Similar to ScatterWithSmoothLines but with visible markers
5. ScatterWithStraightLinesAndMarkers - Similar to ScatterWithStraightLines but with visible markers

To change the chart type, simply modify the `chartType` property in your request.

## Step 5: Examine the Result

After executing the API request, a scatter chart will be added to your presentation. The chart will display:
- Two series representing Group A and Group B
- X-axis showing study hours (1-5)
- Y-axis showing exam scores (60-90)
- Points connected by smooth lines

## Try It Yourself: Customization Exercise

Now that you've created a basic scatter chart, try these customizations:

1. Change the chart type to `ScatterWithStraightLinesAndMarkers`
2. Add a third data series with different points
3. Adjust the chart title to reflect your specific data
4. Modify the axis scales to better fit your data range

## Troubleshooting Tips

- Ensure both xvalue and yvalue are specified for each data point
- Check that the dataPointType is set to "Scatter" for all series
- Verify that your JSON structure is valid if using the REST API directly
- Make sure your data points form a logical pattern for smooth lines

## What You've Learned

In this tutorial, you've learned how to:
- Create a scatter chart in a PowerPoint presentation using Aspose.Slides Cloud API
- Configure multiple data series with X and Y values
- Choose appropriate scatter chart types for your data
- Define basic appearance properties for your chart

## Next Steps

Now that you know how to create scatter charts, you might want to explore:

- [Tutorial: Creating Bubble Charts](/working-with-charts/create-bubble-chart/)
- [How to Configure Chart Data Sources](/working-with-charts/chart-data-source/)
- [Tutorial: Setting Chart Axes Properties](/working-with-charts/chart-axis/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
