---
title: Creating Pie Charts with Aspose.Slides Cloud API Tutorial
url: /working-with-charts/create-pie-chart/
weight: 20
description: Step-by-step tutorial on creating pie charts in PowerPoint presentations using Aspose.Slides Cloud API with practical examples and code samples.
---

## Tutorial: Creating Pie Charts in PowerPoint Presentations

In this tutorial, you'll learn how to create and customize pie charts in PowerPoint presentations using Aspose.Slides Cloud API. Pie charts are perfect for showing proportional relationships between data in a simple, visual way.

## Learning Objectives

By the end of this tutorial, you'll be able to:
- Create a pie chart in a PowerPoint presentation
- Customize pie chart properties including colors and labels
- Format the pie chart for better data visualization

## The Basics of Pie Charts

Pie charts represent data as slices of a circle, where each slice's size is proportional to the value it represents. They're ideal for:
- Showing parts of a whole
- Displaying percentage distributions
- Visualizing proportional data

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

For our tutorial, we'll create a pie chart showing market share distribution for different product categories.

## Step 3: Create a Pie Chart

Now, let's create a pie chart using the Aspose.Slides Cloud API:

### Using cURL

First, prepare a JSON file (`chart.json`) with chart configuration:

```json
{
    "type": "Chart",
    "chartType": "Pie",
    "x": 100,
    "y": 100,
    "width": 400,
    "height": 400,
    "title": { "text": "Market Share Distribution" },
    "categories": [
        { "Value": "Electronics" },
        { "Value": "Clothing" },
        { "Value": "Food" }
    ],
    "series": [
        {
            "dataPointType": "OneValue",
            "isColorVaried": true,
            "dataPoints": [
                { "value": 45 },
                { "value": 32 },
                { "value": 23 }
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
dto.ChartType = Chart.ChartTypeEnum.Pie;
dto.X = 100;
dto.Y = 100;
dto.Width = 400;
dto.Height = 400;
dto.Title = new ChartTitle { Text = "Market Share Distribution" };

// Add categories
dto.Categories = new List<ChartCategory>
{
    new ChartCategory { Value = "Electronics" },
    new ChartCategory { Value = "Clothing" },
    new ChartCategory { Value = "Food" }
};

// Add data series
OneValueSeries series = new OneValueSeries();
series.IsColorVaried = true; // Each slice gets a different color
series.DataPoints = new List<OneValueChartDataPoint>
{
    new OneValueChartDataPoint { Value = 45 },
    new OneValueChartDataPoint { Value = 32 },
    new OneValueChartDataPoint { Value = 23 }
};
dto.Series = new List<Series> { series };

// Create the chart
Chart chart = (Chart)api.CreateShape("MyPresentation.pptx", 1, dto);
Console.WriteLine($"Pie chart created with {chart.Categories.Count} categories");
```

### Using SDK (Python)

```python
import asposeslidescloud
from asposeslidescloud.configuration import Configuration
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.chart import Chart
from asposeslidescloud.models.chart_title import ChartTitle
from asposeslidescloud.models.chart_category import ChartCategory
from asposeslidescloud.models.one_value_series import OneValueSeries
from asposeslidescloud.models.one_value_chart_data_point import OneValueChartDataPoint

# Configure API client
configuration = Configuration()
configuration.app_sid = 'YOUR_CLIENT_ID'
configuration.app_key = 'YOUR_CLIENT_SECRET'
api = SlidesApi(configuration)

# Create chart object
dto = Chart()
dto.chart_type = 'Pie'
dto.x = 100
dto.y = 100
dto.width = 400
dto.height = 400

# Add chart title
title = ChartTitle()
title.text = 'Market Share Distribution'
dto.title = title

# Add categories
category1 = ChartCategory()
category1.value = 'Electronics'
category2 = ChartCategory()
category2.value = 'Clothing'
category3 = ChartCategory()
category3.value = 'Food'
dto.categories = [category1, category2, category3]

# Add data series
series = OneValueSeries()
series.is_color_varied = True  # Each slice gets a different color
data_point1 = OneValueChartDataPoint()
data_point1.value = 45
data_point2 = OneValueChartDataPoint()
data_point2.value = 32
data_point3 = OneValueChartDataPoint()
data_point3.value = 23
series.data_points = [data_point1, data_point2, data_point3]
dto.series = [series]

# Create the chart
result = api.create_shape("MyPresentation.pptx", 1, dto)
print(f"Pie chart created with {len(result.categories)} categories")
```

## Step 4: Key Pie Chart Features

A pie chart in Aspose.Slides Cloud API has several important properties:

1. isColorVaried: When set to `true`, each slice of the pie gets a different color.
2. dataPointType: For pie charts, this should be set to "OneValue" since each category has only one value.
3. categories: These are the labels for each slice of the pie.
4. dataPoints: These are the values that determine the size of each slice.

## Step 5: Verify the Result

After executing the API request, a pie chart will be added to your presentation. The chart will display three slices representing the market share of Electronics (45%), Clothing (32%), and Food (23%).

## Try It Yourself: Customization Exercise

Now that you've created a basic pie chart, try these customizations:

1. Add a fourth category to your pie chart
2. Explode one of the slices to make it stand out
3. Add data labels to show values or percentages on each slice

## Troubleshooting Tips

- Ensure your data values add up to a logical total (often 100 for percentage-based pie charts)
- Make sure you've set `isColorVaried` to true if you want different colors for each slice
- Check that your JSON structure is valid if using the REST API directly
- Verify that the number of data points matches the number of categories

## What You've Learned

In this tutorial, you've learned how to:
- Create a pie chart in a PowerPoint presentation using Aspose.Slides Cloud API
- Set categories and data points for the pie chart
- Configure basic pie chart appearance properties

## Next Steps

Now that you know how to create pie charts, you might want to explore:

- [Tutorial: Creating Sunburst Charts](/working-with-charts/create-sunburst-chart/)
- [How to Create Scattered Charts](/working-with-charts/create-scattered-chart/)
- [Tutorial: Setting Chart Axes Properties](/working-with-charts/chart-axis/)

## Further Practice

Try creating a pie chart that shows:
1. The distribution of time spent on different activities in a day
2. Budget allocation across different departments
3. Sales distribution by region

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
