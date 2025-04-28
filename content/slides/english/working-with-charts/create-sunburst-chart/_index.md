---
title: Creating Sunburst Charts with Aspose.Slides Cloud API Tutorial
url: /working-with-charts/create-sunburst-chart/
weight: 30
description: Learn how to create hierarchical sunburst charts in PowerPoint presentations using Aspose.Slides Cloud API in this comprehensive tutorial with examples.
---

## Tutorial: Creating Sunburst Charts in PowerPoint Presentations

In this tutorial, you'll learn how to create and customize sunburst charts in PowerPoint presentations using Aspose.Slides Cloud API. Sunburst charts are powerful visualization tools that display hierarchical data in a multi-level circular format.

## Prerequisites

- An Aspose Cloud account with an active subscription
- Your Client ID and Client Secret credentials
- Basic knowledge of REST APIs and your preferred programming language
- A PowerPoint presentation file to work with

## The Basics of Sunburst Charts

Sunburst charts (also known as ring charts or multilevel pie charts) display hierarchical data as concentric rings. Each ring represents a level in the hierarchy, with the innermost ring representing the highest level. They're ideal for:
- Visualizing hierarchical structures
- Showing part-to-whole relationships at multiple levels
- Displaying complex organizational or data relationships

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

## Step 2: Understanding Hierarchical Data Structure

For sunburst charts, you need to properly structure your data to represent hierarchy. Let's use an example of company revenue by region, country, and product category.

The key components for a sunburst chart are:
1. level: Indicates the hierarchy level (1 is outermost, higher numbers are inner rings)
2. parentCategories: Specifies parent categories for nested items
3. value: The data value for the category

## Step 3: Create a Sunburst Chart

Now, let's create a sunburst chart using the Aspose.Slides Cloud API:

### Using cURL

First, prepare a JSON file (`chart.json`) with chart configuration:

```json
{
    "type": "Chart",
    "chartType": "Sunburst",
    "x": 100,
    "y": 100,
    "width": 400,
    "height": 400,
    "title": { "hasTitle": true, "text": "Revenue by Region and Category" },
    "categories": [
        { "value": "Electronics", "level": 3, "parentCategories": [ "USA", "North America" ] },
        { "value": "Clothing", "level": 3, "parentCategories": [ "USA", "North America" ] },
        { "value": "Food", "level": 3, "parentCategories": [ "Canada", "North America" ] },
        { "value": "Electronics", "level": 3, "parentCategories": [ "UK", "Europe" ] },
        { "value": "USA", "level": 2, "parentCategories": [ "North America" ] },
        { "value": "Canada", "level": 2, "parentCategories": [ "North America" ] },
        { "value": "UK", "level": 2, "parentCategories": [ "Europe" ] },
        { "value": "North America", "level": 1 },
        { "value": "Europe", "level": 1 }
    ],
    "series": [
        {
            "dataPointType": "OneValue",
            "dataPoints": [
                { "value": 30 },
                { "value": 25 },
                { "value": 15 },
                { "value": 20 },
                { "value": 0 },
                { "value": 0 },
                { "value": 0 },
                { "value": 0 },
                { "value": 0 }
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
dto.ChartType = Chart.ChartTypeEnum.Sunburst;
dto.X = 100;
dto.Y = 100;
dto.Width = 400;
dto.Height = 400;
dto.Title = new ChartTitle { Text = "Revenue by Region and Category", HasTitle = true };

// Add categories - hierarchical structure
dto.Categories = new List<ChartCategory>
{
    // Level 3 (innermost) - Product categories with their parent regions/continents
    new ChartCategory { Value = "Electronics", Level = 3, ParentCategories = new List<string> { "USA", "North America" } },
    new ChartCategory { Value = "Clothing", Level = 3, ParentCategories = new List<string> { "USA", "North America" } },
    new ChartCategory { Value = "Food", Level = 3, ParentCategories = new List<string> { "Canada", "North America" } },
    new ChartCategory { Value = "Electronics", Level = 3, ParentCategories = new List<string> { "UK", "Europe" } },
    
    // Level 2 - Countries with their parent continents
    new ChartCategory { Value = "USA", Level = 2, ParentCategories = new List<string> { "North America" } },
    new ChartCategory { Value = "Canada", Level = 2, ParentCategories = new List<string> { "North America" } },
    new ChartCategory { Value = "UK", Level = 2, ParentCategories = new List<string> { "Europe" } },
    
    // Level 1 (outermost) - Continents
    new ChartCategory { Value = "North America", Level = 1 },
    new ChartCategory { Value = "Europe", Level = 1 }
};

// Add data series
OneValueSeries series = new OneValueSeries();
series.DataPoints = new List<OneValueChartDataPoint>
{
    // Values for leaf nodes (level 3)
    new OneValueChartDataPoint { Value = 30 }, // Electronics in USA
    new OneValueChartDataPoint { Value = 25 }, // Clothing in USA
    new OneValueChartDataPoint { Value = 15 }, // Food in Canada
    new OneValueChartDataPoint { Value = 20 }, // Electronics in UK
    
    // Parent nodes (levels 1-2) have zero value as they aggregate children
    new OneValueChartDataPoint { Value = 0 }, // USA
    new OneValueChartDataPoint { Value = 0 }, // Canada
    new OneValueChartDataPoint { Value = 0 }, // UK
    new OneValueChartDataPoint { Value = 0 }, // North America
    new OneValueChartDataPoint { Value = 0 }  // Europe
};
dto.Series = new List<Series> { series };

// Create the chart
Chart chart = (Chart)api.CreateShape("MyPresentation.pptx", 1, dto);
Console.WriteLine($"Sunburst chart created with {chart.Categories.Count} categories");
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
dto.chart_type = 'Sunburst'
dto.x = 100
dto.y = 100
dto.width = 400
dto.height = 400

# Add chart title
title = ChartTitle()
title.text = 'Revenue by Region and Category'
title.has_title = True
dto.title = title

# Add categories - hierarchical structure
category1 = ChartCategory()
category1.value = "Electronics"
category1.level = 3
category1.parent_categories = ["USA", "North America"]

category2 = ChartCategory()
category2.value = "Clothing"
category2.level = 3
category2.parent_categories = ["USA", "North America"]

category3 = ChartCategory()
category3.value = "Food"
category3.level = 3
category3.parent_categories = ["Canada", "North America"]

category4 = ChartCategory()
category4.value = "Electronics"
category4.level = 3
category4.parent_categories = ["UK", "Europe"]

category5 = ChartCategory()
category5.value = "USA"
category5.level = 2
category5.parent_categories = ["North America"]

category6 = ChartCategory()
category6.value = "Canada"
category6.level = 2
category6.parent_categories = ["North America"]

category7 = ChartCategory()
category7.value = "UK"
category7.level = 2
category7.parent_categories = ["Europe"]

category8 = ChartCategory()
category8.value = "North America"
category8.level = 1

category9 = ChartCategory()
category9.value = "Europe"
category9.level = 1

dto.categories = [category1, category2, category3, category4, 
                  category5, category6, category7, category8, category9]

# Add data series
series = OneValueSeries()

# Values for leaf nodes (level 3)
data_point1 = OneValueChartDataPoint()
data_point1.value = 30  # Electronics in USA

data_point2 = OneValueChartDataPoint()
data_point2.value = 25  # Clothing in USA

data_point3 = OneValueChartDataPoint()
data_point3.value = 15  # Food in Canada

data_point4 = OneValueChartDataPoint()
data_point4.value = 20  # Electronics in UK

# Parent nodes (levels 1-2) have zero value as they aggregate children
data_point5 = OneValueChartDataPoint()
data_point5.value = 0  # USA

data_point6 = OneValueChartDataPoint()
data_point6.value = 0  # Canada

data_point7 = OneValueChartDataPoint()
data_point7.value = 0  # UK

data_point8 = OneValueChartDataPoint()
data_point8.value = 0  # North America

data_point9 = OneValueChartDataPoint()
data_point9.value = 0  # Europe

series.data_points = [data_point1, data_point2, data_point3, data_point4,
                      data_point5, data_point6, data_point7, data_point8, data_point9]
dto.series = [series]

# Create the chart
result = api.create_shape("MyPresentation.pptx", 1, dto)
print(f"Sunburst chart created with {len(result.categories)} categories")
```

## Step 4: Understanding the Sunburst Chart Structure

A sunburst chart in Aspose.Slides Cloud API has several key components:

1. Categories with Levels: Each category has a level property that determines its position in the hierarchy.
   - Level 1: Outermost ring (highest level in hierarchy)
   - Level 2, 3, etc.: Inner rings (descending levels in hierarchy)

2. Parent Categories: Each category (except the outermost ones) must specify its parent categories in order, from immediate parent to highest ancestor.

3. Data Points: Values should be assigned to the leaf nodes (innermost categories). Parent categories typically have zero values as they represent the sum of their children.

## Step 5: Verify the Result

After executing the API request, a sunburst chart will be added to your presentation. The chart will display:
- The outermost ring showing continents (North America, Europe)
- The middle ring showing countries (USA, Canada, UK)
- The innermost ring showing product categories (Electronics, Clothing, Food)

The size of each segment represents the corresponding revenue value.

## Try It Yourself: Customization Exercise

Now that you've created a basic sunburst chart, try these customizations:

1. Add a third continent with countries and product categories
2. Add more product categories to existing countries
3. Adjust the colors or add data labels to enhance readability

## Troubleshooting Tips

- Ensure your hierarchy is properly defined with consistent levels
- Verify that each category (except level 1) has the correct parent categories listed
- Check that the order of data points corresponds exactly to the order of categories
- Make sure you've specified level values correctly (1 for outermost, increasing for inner rings)

## What You've Learned

In this tutorial, you've learned how to:
- Create a sunburst chart in a PowerPoint presentation using Aspose.Slides Cloud API
- Define hierarchical data with multiple levels
- Establish parent-child relationships between categories
- Assign data values to the appropriate categories

## Next Steps

Now that you know how to create sunburst charts, you might want to explore:

- [Tutorial: Creating Scattered Charts](/working-with-charts/create-scattered-chart/)
- [How to Create Bubble Charts](/working-with-charts/create-bubble-chart/)
- [Tutorial: Setting Chart Axes Properties](/working-with-charts/chart-axis/)

## Helpful Resources

- [Product Page](https://products.aspose.cloud/slides/)
- [Documentation](https://docs.aspose.cloud/slides/)
- [API Reference](https://reference.aspose.cloud/slides/)
- [Free Support](https://forum.aspose.cloud/c/slides/15)
- [Free Trial](https://dashboard.aspose.cloud/#/apps)
- [support forum](https://forum.aspose.cloud/c/slides/15)
