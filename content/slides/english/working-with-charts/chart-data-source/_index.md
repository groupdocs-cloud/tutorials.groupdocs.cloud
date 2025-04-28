---
title: Configuring Chart Data Sources with Aspose.Slides Cloud API Tutorial
url: /working-with-charts/chart-data-source/
weight: 70
description: Learn how to configure different data sources for PowerPoint charts using Aspose.Slides Cloud API in this comprehensive step-by-step tutorial.
---

## Tutorial: Configuring Chart Data Sources in PowerPoint Presentations

In this tutorial, you'll learn how to configure data sources for charts in PowerPoint presentations using Aspose.Slides Cloud API. Understanding how to properly set up chart data sources gives you more flexibility and control over your data visualization.

## Prerequisites

- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret credentials
- Basic knowledge of REST APIs and your preferred programming language
- Understanding of charts and Excel workbooks
- A PowerPoint presentation to work with

## Estimated Time to Complete: 30 minutes

## Understanding Chart Data Sources

In PowerPoint charts, data can come from different sources. The Aspose.Slides Cloud API supports two main types of data sources:

1. Workbook: Data is stored in an embedded Excel workbook, and the chart references specific cells.
2. Literals: Data values are stored directly with the chart (cached within the presentation).

## Step 1: Authentication Setup

Before configuring chart data sources, you need to authenticate with the Aspose.Slides Cloud API:

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

## Step 2: Creating a Chart with Workbook Data Source

Let's create a column chart that gets its data from specific cells in the embedded workbook:

### Using cURL

First, create a JSON file (`chart_workbook.json`) with the chart configuration:

```json
{
    "type": "Chart",
    "chartType": "ClusteredColumn",
    "x": 20,
    "y": 20,
    "width": 400,
    "height": 300,
    "dataSourceForCategories": {
        "type": "workbook",
        "worksheetIndex": 1,
        "columnIndex": 1,
        "rowIndex": 2
    },
    "categories": [
        { "Value": "Category1" },
        { "Value": "Category2" },
        { "Value": "Category3" }
    ],
    "series": [
        {
            "dataPointType": "OneValue",
            "dataSourceForSeriesName": {
                "type": "workbook",
                "worksheetIndex": 1,
                "columnIndex": 2,
                "rowIndex": 1
            },
            "name": "Series1",
            "dataSourceForValues": {
                "type": "workbook",
                "worksheetIndex": 1,
                "columnIndex": 2,
                "rowIndex": 2
            },
            "dataPoints": [
                { "value": 40 },
                { "value": 50 },
                { "value": 70 }
            ]
        },
        {
            "dataPointType": "OneValue",
            "dataSourceForSeriesName": {
                "type": "workbook",
                "worksheetIndex": 1,
                "columnIndex": 3,
                "rowIndex": 1
            },
            "name": "Series2",
            "dataSourceForValues": {
                "type": "workbook",
                "worksheetIndex": 1,
                "columnIndex": 3,
                "rowIndex": 2
            },
            "dataPoints": [
                { "value": 55 },
                { "value": 35 },
                { "value": 90 }
            ]
        }
    ]              
}
```

Then, make the API request:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/shapes" \
  -d @chart_workbook.json \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Using SDK (C#)

```csharp
SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

string fileName = "MyPresentation.pptx";
int slideIndex = 1;

// Create a chart with workbook data source
Chart dto = new Chart
{
    ChartType = Chart.ChartTypeEnum.ClusteredColumn,
    X = 20,
    Y = 20,
    Width = 400,
    Height = 300,
    
    // Configure data source for categories
    DataSourceForCategories = new Workbook
    {
        WorksheetIndex = 1,
        ColumnIndex = 1,
        RowIndex = 2
    },
    
    // Add categories (initial values)
    Categories = new List<ChartCategory>
    {
        new ChartCategory { Value = "Category1" },
        new ChartCategory { Value = "Category2" },
        new ChartCategory { Value = "Category3" }
    },
    
    Series = new List<Series>
    {
        // First series
        new OneValueSeries
        {
            Name = "Series1",
            
            // Configure data source for series name
            DataSourceForSeriesName = new Workbook
            {
                WorksheetIndex = 1,
                ColumnIndex = 2,
                RowIndex = 1
            },
            
            // Configure data source for values
            DataSourceForValues = new Workbook
            {
                WorksheetIndex = 1,
                ColumnIndex = 2,
                RowIndex = 2
            },
            
            // Initial data points
            DataPoints = new List<OneValueChartDataPoint>
            {
                new OneValueChartDataPoint { Value = 40 },
                new OneValueChartDataPoint { Value = 50 },
                new OneValueChartDataPoint { Value = 70 }
            }
        },
        
        // Second series
        new OneValueSeries
        {
            Name = "Series2",
            
            // Configure data source for series name
            DataSourceForSeriesName = new Workbook
            {
                WorksheetIndex = 1,
                ColumnIndex = 3,
                RowIndex = 1
            },
            
            // Configure data source for values
            DataSourceForValues = new Workbook
            {
                WorksheetIndex = 1,
                ColumnIndex = 3,
                RowIndex = 2
            },
            
            // Initial data points
            DataPoints = new List<OneValueChartDataPoint>
            {
                new OneValueChartDataPoint { Value = 55 },
                new OneValueChartDataPoint { Value = 35 },
                new OneValueChartDataPoint { Value = 90 }
            }
        }
    }
};

// Create the chart
Chart chart = (Chart)api.CreateShape(fileName, slideIndex, dto);
Console.WriteLine("Chart created with workbook data source");
```

### Using SDK (Python)

```python
import asposeslidescloud
from asposeslidescloud.configuration import Configuration
from asposeslidescloud.apis.slides_api import SlidesApi
from asposeslidescloud.models.chart import Chart
from asposeslidescloud.models.workbook import Workbook
from asposeslidescloud.models.chart_category import ChartCategory
from asposeslidescloud.models.one_value_series import OneValueSeries
from asposeslidescloud.models.one_value_chart_data_point import OneValueChartDataPoint

# Configure API client
configuration = Configuration()
configuration.app_sid = 'YOUR_CLIENT_ID'
configuration.app_key = 'YOUR_CLIENT_SECRET'
api = SlidesApi(configuration)

file_name = "MyPresentation.pptx"
slide_index = 1

# Create chart object
chart = Chart()
chart.chart_type = 'ClusteredColumn'
chart.x = 20
chart.y = 20
chart.width = 400
chart.height = 300

# Configure data source for categories
categories_data_source = Workbook()
categories_data_source.worksheet_index = 1
categories_data_source.column_index = 1
categories_data_source.row_index = 2
chart.data_source_for_categories = categories_data_source

# Add categories (initial values)
category1 = ChartCategory()
category1.value = "Category1"
category2 = ChartCategory()
category2.value = "Category2"
category3 = ChartCategory()
category3.value = "Category3"
chart.categories = [category1, category2, category3]

# Create first series
series1 = OneValueSeries()
series1.name = "Series1"

# Configure data source for series name
series1_name_data_source = Workbook()
series1_name_data_source.worksheet_index = 1
series1_name_data_source.column_index = 2
series1_name_data_source.row_index = 1
series1.data_source_for_series_name = series1_name_data_source

# Configure data source for values
series1_values_data_source = Workbook()
series1_values_data_source.worksheet_index = 1
series1_values_data_source.column_index = 2
series1_values_data_source.row_index = 2
series1.data_source_for_values = series1_values_data_source

# Add data points
data_point1 = OneValueChartDataPoint()
data_point1.value = 40
data_point2 = OneValueChartDataPoint()
data_point2.value = 50
data_point3 = OneValueChartDataPoint()
data_point3.value = 70
series1.data_points = [data_point1, data_point2, data_point3]

# Create second series
series2 = OneValueSeries()
series2.name = "Series2"

# Configure data source for series name
series2_name_data_source = Workbook()
series2_name_data_source.worksheet_index = 1
series2_name_data_source.column_index = 3
series2_name_data_source.row_index = 1
series2.data_source_for_series_name = series2_name_data_source

# Configure data source for values
series2_values_data_source = Workbook()
series2_values_data_source.worksheet_index = 1
series2_values_data_source.column_index = 3
series2_values_data_source.row_index = 2
series2.data_source_for_values = series2_values_data_source

# Add data points
data_point1 = OneValueChartDataPoint()
data_point1.value = 55
data_point2 = OneValueChartDataPoint()
data_point2.value = 35
data_point3 = OneValueChartDataPoint()
data_point3.value = 90
series2.data_points = [data_point1, data_point2, data_point3]

chart.series = [series1, series2]

# Create the chart
result = api.create_shape(file_name, slide_index, chart)
print("Chart created with workbook data source")
```

## Step 3: Understanding Workbook Data Sources

When using the workbook data source type, you specify these key parameters:

1. worksheetIndex: The index of the worksheet in the embedded Excel workbook (1-based).
2. columnIndex: The column index (1-based, where A=1, B=2, etc.).
3. rowIndex: The row index (1-based).

These parameters define the first cell in the data range. For example, if you set:
```
worksheetIndex: 1
columnIndex: 2
rowIndex: 3
```
This refers to cell B3 on the first worksheet as the starting point for your data range.

You can specify separate data sources for:
- Categories (horizontal axis labels)
- Series names (legend entries)
- Data values (plotted values)

The data in each range is read sequentially from the starting cell, based on the number of categories or data points.

## Step 4: Creating a Chart with Literals Data Source

Now, let's create a chart that uses literals as its data source:

### Using cURL

First, create a JSON file (`chart_literals.json`) with the chart configuration:

```json
{
    "type": "Chart",
    "chartType": "ClusteredColumn",
    "x": 20,
    "y": 20,
    "width": 400,
    "height": 300,
    "dataSourceForCategories": {
        "type": "literals"
    },
    "categories": [
        { "Value": "Category1" },
        { "Value": "Category2" },
        { "Value": "Category3" }
    ],
    "series": [
        {
            "dataPointType": "OneValue",
            "dataSourceForSeriesName": {
                "type": "literals"
            },
            "name": "Series1",
            "dataSourceForValues": {
                "type": "literals"
            },
            "dataPoints": [
                { "value": 20 },
                { "value": 50 },
                { "value": 30 }
            ]
        },
        {
            "dataPointType": "OneValue",
            "dataSourceForSeriesName": {
                "type": "literals"
            },
            "name": "Series2",
            "dataSourceForValues": {
                "type": "literals"
            },
            "dataPoints": [
                { "value": 30 },
                { "value": 10 },
                { "value": 60 }
            ]
        }
    ]              
}
```

Then, make the API request:

```bash
curl -X POST "https://api.aspose.cloud/v3.0/slides/MyPresentation.pptx/slides/1/shapes" \
  -d @chart_literals.json \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Using SDK (C#)

```csharp
SlidesApi api = new SlidesApi("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET");

string fileName = "MyPresentation.pptx";
int slideIndex = 1;

// Create a chart with literals data source
Chart dto = new Chart
{
    ChartType = Chart.ChartTypeEnum.ClusteredColumn,
    X = 20,
    Y = 20,
    Width = 400,
    Height = 300,
    
    // Configure data source for categories
    DataSourceForCategories = new Literals(),
    
    // Add categories
    Categories = new List<ChartCategory>
    {
        new ChartCategory { Value = "Category1" },
        new ChartCategory { Value = "Category2" },
        new ChartCategory { Value = "Category3" }
    },
    
    Series = new List<Series>
    {
        // First series
        new OneValueSeries
        {
            Name = "Series1",
            
            // Configure data source for series name
            DataSourceForSeriesName = new Literals(),
            
            // Configure data source for values
            DataSourceForValues = new Literals(),
            
            // Data points
            DataPoints = new List<OneValueChartDataPoint>
            {
                new OneValueChartDataPoint { Value = 20 },
                new OneValueChartDataPoint { Value = 50 },
                new OneValueChartDataPoint { Value = 30 }
            }
        },
        
        // Second series
        new OneValueSeries
        {
            Name = "Series2",
            
            // Configure data source for series name
            DataSourceForSeriesName = new Literals(),
            
            // Configure data source for values
            DataSourceForValues = new Literals(),
            
            // Data points
            DataPoints = new List<OneValueChartDataPoint>
            {
                new OneValueChartDataPoint { Value = 30 },
                new OneValueChartDataPoint { Value = 10 },
                new OneValueChartDataPoint { Value = 60 }
            }
        }
    }
};

// Create the chart
Chart chart = (Chart)api.CreateShape(fileName, slideIndex, dto);
Console.WriteLine("Chart created with literals data source");
```

## Step 5: Understanding Literals Data Source

When using the literals data source type, the values you provide in the categories and data points are used directly in the chart. This means:

1. The values are stored with the chart inside the PowerPoint presentation.
2. There's no connection to an external or embedded data source.
3. To update the values, you would need to modify the chart directly.

Literals are simpler to set up but offer less flexibility for data that might change frequently.

## Step 6: Choosing Between Workbook and Literals

Here are some guidelines to help you decide which data source type is best for your scenario:

### Use Workbook Data Source when:
- You need to maintain a connection to specific worksheet cells
- The data might be updated later by modifying the workbook
- You want to leverage Excel formulas or data processing
- You're working with large datasets that are already in Excel format
- You need to maintain consistency between multiple charts

### Use Literals Data Source when:
- You have simple, static data that won't change frequently
- You want to avoid the complexity of workbook cell references
- The chart doesn't need to be linked to external data
- You're creating a one-off visualization with specific values
- Performance is more important than maintainability

## Step 7: Key Considerations for Data Sources

When working with chart data sources in Aspose.Slides Cloud API, keep these points in mind:

1. Data Organization: For workbook sources, ensure your Excel data is properly organized with headers and consistent data types.

2. Cell Addressing: Remember that both column and row indices are 1-based (not 0-based).

3. Initial Values: Even when using workbook data sources, you should provide initial values in the categories and data points arrays.

4. Data Ranges: The data range starts at the specified cell and extends based on the number of categories and data points.

5. Mixed Approaches: You can mix approaches, using workbook sources for some elements and literals for others.

## Try It Yourself: Customization Exercise

Now that you understand the two data source types, try these exercises:

1. Create a chart where categories use a workbook source but series values use literals
2. Modify an existing chart to change its data source type
3. Create a chart with multiple series, each using a different worksheet as its data source

## Troubleshooting Tips

- If your chart isn't showing data correctly, verify that your workbook cell references are accurate
- Check that you've specified the correct worksheet index (remember it's 1-based)
- Ensure your data types in the workbook match what the chart expects
- Verify that your JSON structure is valid if using the REST API directly
- For workbook sources, confirm that the data exists in the specified range

## What You've Learned

In this tutorial, you've learned how to:
- Configure charts to use workbook cells as data sources
- Set up charts with literal values as data sources
- Understand the differences between workbook and literals approaches
- Choose the appropriate data source type for different scenarios

## Next Steps

Now that you know how to configure chart data sources, you might want to explore:

- [Tutorial: Setting Chart Axes Properties](/working-with-charts/chart-axis/)
- [Learn to Create Column Charts](/working-with-charts/create-column-chart/)
- [Tutorial: Creating Sunburst Charts](/working-with-charts/create-sunburst-chart/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
