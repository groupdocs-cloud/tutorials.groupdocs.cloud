---
title: Creating Column Charts with Aspose.Slides Cloud API Tutorial
url: /working-with-charts/create-column-chart/
weight: 10
description: Learn how to create column charts in PowerPoint presentations programmatically using Aspose.Slides Cloud API in this step-by-step tutorial.
---

## Tutorial: Creating Column Charts in PowerPoint Presentations

In this tutorial, you'll learn how to create and customize column charts in PowerPoint presentations using Aspose.Slides Cloud API. Column charts are excellent for comparing data across different categories with vertical bars.

## Prerequisites

- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret credentials
- Basic knowledge of REST APIs and your preferred programming language
- A PowerPoint presentation file to work with (or we'll create one)

## The Basics of Column Charts

Column charts display data using vertical bars (columns) where the height of each column corresponds to its value. They're ideal for:
- Comparing data across categories
- Showing changes over time
- Visualizing frequency distributions

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

The response will contain your access token to use in subsequent requests.

### Using SDK (C#)

```csharp
// Initialize the API with your credentials
SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");
```

## Step 2: Prepare Chart Data

For our tutorial, we'll create a simple column chart showing sales data for different product categories across two data series (representing different years).

## Step 3: Create a Column Chart

Now, let's create a column chart using the Aspose.Slides Cloud API:

### Using cURL

First, prepare a JSON file (`chart.json`) with chart configuration:

```json
{
    "type": "Chart",
    "chartType": "ClusteredColumn",
    "x": 100,
    "y": 100,
    "width": 400,
    "height": 400,
    "title": { "text": "Sales Comparison by Category" },
    "categories": [
        { "Value": "Electronics" },
        { "Value": "Clothing" },
        { "Value": "Food" }
    ],
    "series": [
        {
            "name": "2023",
            "dataPointType": "OneValue",
            "dataPoints": [
                { "value": 45000 },
                { "value": 32000 },
                { "value": 21000 }
            ]
        },
        {
            "name": "2024",
            "dataPointType": "OneValue",
            "dataPoints": [
                { "value": 52000 },
                { "value": 36000 },
                { "value": 28000 }
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
dto.ChartType = Chart.ChartTypeEnum.ClusteredColumn;
dto.X = 100;
dto.Y = 100;
dto.Width = 400;
dto.Height = 400;
dto.Title = new ChartTitle { Text = "Sales Comparison by Category" };

// Add categories
dto.Categories = new List<ChartCategory>
{
    new ChartCategory { Value = "Electronics" },
    new ChartCategory { Value = "Clothing" },
    new ChartCategory { Value = "Food" }
};

// Add first data series (2023)
OneValueSeries series1 = new OneValueSeries();
series1.Name = "2023";
series1.DataPoints = new List<OneValueChartDataPoint>
{
    new OneValueChartDataPoint { Value = 45000 },
    new OneValueChartDataPoint { Value = 32000 },
    new OneValueChartDataPoint { Value = 21000 }
};

// Add second data series (2024)
OneValueSeries series2 = new OneValueSeries();
series2.Name = "2024";
series2.DataPoints = new List<OneValueChartDataPoint>
{
    new OneValueChartDataPoint { Value = 52000 },
    new OneValueChartDataPoint { Value = 36000 },
    new OneValueChartDataPoint { Value = 28000 }
};

dto.Series = new List<Series> { series1, series2 };

// Create the chart
Chart chart = (Chart)api.CreateShape("MyPresentation.pptx", 1, dto);
Console.WriteLine($"Chart created with {chart.Categories.Count} categories and {chart.Series.Count} series");
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
dto.chart_type = 'ClusteredColumn'
dto.x = 100
dto.y = 100
dto.width = 400
dto.height = 400

# Add chart title
title = ChartTitle()
title.text = 'Sales Comparison by Category'
dto.title = title

# Add categories
category1 = ChartCategory()
category1.value = 'Electronics'
category2 = ChartCategory()
category2.value = 'Clothing'
category3 = ChartCategory()
category3.value = 'Food'
dto.categories = [category1, category2, category3]

# Add first data series (2023)
series1 = OneValueSeries()
series1.name = '2023'
data_point11 = OneValueChartDataPoint()
data_point11.value = 45000
data_point12 = OneValueChartDataPoint()
data_point12.value = 32000
data_point13 = OneValueChartDataPoint()
data_point13.value = 21000
series1.data_points = [data_point11, data_point12, data_point13]

# Add second data series (2024)
series2 = OneValueSeries()
series2.name = '2024'
data_point21 = OneValueChartDataPoint()
data_point21.value = 52000
data_point22 = OneValueChartDataPoint()
data_point22.value = 36000
data_point23 = OneValueChartDataPoint()
data_point23.value = 28000
series2.data_points = [data_point21, data_point22, data_point23]

dto.series = [series1, series2]

# Create the chart
result = api.create_shape("MyPresentation.pptx", 1, dto)
print(f"Chart created with {len(result.categories)} categories and {len(result.series)} series")
```

## Step 4: Examine the Result

After executing the API request, a column chart will be added to your presentation. The chart will display two series of data across three categories.

## Try It Yourself: Customization Exercise

Now that you've created a basic column chart, try these customizations:

1. Add a third data series for "2025 (Projected)" with values of your choice
2. Change the chart position or size
3. Try using a different column chart type, such as "StackedColumn" or "PercentStackedColumn"

## Troubleshooting Tips

- If you receive an authentication error, ensure your Client ID and Client Secret are correct
- Check that your presentation file exists at the specified location
- Verify that your JSON structure is valid if using the REST API directly
- Ensure that the number of data points in each series matches the number of categories

## What You've Learned

In this tutorial, you've learned how to:
- Create a column chart in a PowerPoint presentation using Aspose.Slides Cloud API
- Define categories and multiple data series
- Customize basic chart properties

## Next Steps

Now that you know how to create column charts, you might want to explore:

- [Tutorial: Creating Pie Charts](/working-with-charts/create-pie-chart/)
- [How to Configure Chart Data Sources](/working-with-charts/chart-data-source/)
- [Tutorial: Setting Chart Axes Properties](/working-with-charts/chart-axis/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
