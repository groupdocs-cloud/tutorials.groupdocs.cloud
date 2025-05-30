---
title: How to Work with Charts in GroupDocs.Assembly Cloud Tutorial
weight: 5
url: /working-with-charts/
description: Learn how to dynamically generate and customize charts in your document templates with this step-by-step GroupDocs.Assembly Cloud tutorial.
---

# Tutorial: How to Work with Charts in GroupDocs.Assembly Cloud

## Overview

In this tutorial, you'll learn how to work with charts in GroupDocs.Assembly Cloud, creating dynamic document templates that can generate various types of data visualizations. Charts are powerful tools for presenting data in a visual format that makes trends and patterns immediately apparent.

## Learning Objectives

By the end of this tutorial, you will be able to:

- Bind charts to dynamic data sources
- Configure different chart types (line, column, bar, pie, etc.)
- Customize chart appearance with dynamic titles and labels
- Set dynamic chart colors based on data values
- Apply filtering to chart series
- Create advanced multi-series charts

## Prerequisites

Before starting this tutorial, you should have:

- A GroupDocs.Assembly Cloud account (sign up for a [free trial](https://dashboard.groupdocs.cloud/#/apps))
- Basic understanding of template syntax and data processing
- Familiarity with chart types and their appropriate use cases
- Your development environment set up (any language with the GroupDocs.Assembly Cloud SDK installed)

## Understanding Chart Components in Templates

Charts in document templates contain several components that can be customized:

1. Chart Title - The main heading of the chart
2. Series - Data sets represented in the chart 
3. Data Points - Individual values within a series
4. Axes - The horizontal and vertical reference lines
5. Legend - The key explaining what each series represents

GroupDocs.Assembly Cloud provides special tags to bind these components to your data source.

## Binding a Chart to a Data Source

The fundamental step in chart generation is binding it to your data source using chart series tags.

### Basic Chart Setup Process

1. Add a chart to your template at the desired location
2. Configure the chart's appearance (type, style, size)
3. Add required chart series and configure their appearance
4. Add a title to the chart if needed
5. Add template tags to make the chart dynamic

### The Chart Tag Syntax

To declare a chart that will be populated with data dynamically, use these special tags:

- `foreach` tag in the chart title (opening tag only)
- `x` tags for x-axis values
- `y` tags for y-axis values

### Sample Data Source

For this tutorial, we'll use a sales data source:

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-SalesChartData.json"></script>

## Creating a Simple Column Chart

Let's start with a basic column chart showing monthly sales.

### Chart Setup

1. Insert a column chart in your document
2. Set the chart title to: `<<foreach [in sales]>>Monthly Sales`
3. Add a single series with the name: `<<y [Amount]>>`
4. Add `<<x [Month]>>` to the chart title after the `foreach` tag

### How It Works

1. The `foreach` tag in the title iterates through your data source
2. The `x` tag defines what data will appear on the x-axis (months)
3. The `y` tag defines what data will be displayed as column heights (sales amounts)

### C# SDK Implementation Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-SimpleChartCSharp.cs"></script>

## Customizing Chart Titles and Labels Dynamically

You can make your charts more informative by dynamically setting titles and labels.

### Chart Setup

1. Insert a column chart in your document
2. Set the chart title to: `<<foreach [in sales]>>Sales for <<[Year]>>`
3. Add a series with the name: `<<y [Amount]>> in <<[Currency]>>`
4. Set the x-axis title to: `<<[Period]>>`

### How It Works

1. The expression `<<[Year]>>` in the title is replaced with the year value from your data
2. The y-axis series name includes both the amount and the currency
3. The x-axis title is set dynamically based on the period value (Monthly, Quarterly, etc.)

### Python SDK Implementation Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-DynamicTitlesPython.py"></script>

## Excluding Chart Series Conditionally

You can dynamically include or exclude series from your charts based on conditions.

### Chart Setup

1. Insert a chart with multiple series in your document
2. Add the `removeif` tag to any series you want to conditionally exclude
3. Add the condition inside the `removeif` tag

Example series name with conditional removal:
```
<<removeif [TotalSales < 10000]>><<y [RegionSales]>>Region A
```

### How It Works

1. The `removeif` tag evaluates the condition `[TotalSales < 10000]`
2. If the condition returns `true`, the series is removed from the chart
3. If the condition returns `false`, the series remains in the chart

### Java SDK Implementation Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-ConditionalSeriesJava.java"></script>

## Working with Chart Colors

You can dynamically set colors for chart series and individual data points.

### Setting Series Colors

Use the `seriesColor` tag to define the color of an entire series:

```
<<seriesColor [colorExpression]>><<y [data]>>Series Name
```

The color expression can return:
- A string with a known color name (e.g., "red")
- An integer RGB value (e.g., 0xFF0000 for red)
- A value of the Color type

### Setting Point Colors

Use the `pointColor` tag to color individual points in a series:

```
<<pointColor [colorExpression]>><<y [data]>>Series Name
```

### Dynamic Coloring Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-DynamicColors.cs"></script>

### REST API Implementation with cURL

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-ChartColorsCurl.sh"></script>

## Creating Different Chart Types

GroupDocs.Assembly Cloud supports various chart types. Let's explore how to create some common ones.

### Line Chart Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-LineChart.cs"></script>

### Pie Chart Example

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-PieChart.cs"></script>

### Bubble Chart Example

For bubble charts, you'll need to use the `size` tag to define the bubble size:

```
<<foreach [in data]>><<x [XValue]>><<y [YValue]>><<size [SizeValue]>>Product Performance
```

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-BubbleChart.cs"></script>

## Complete Example: Sales Performance Dashboard

Let's create a comprehensive sales dashboard with multiple charts:

### Template Setup

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-CompleteChartDashboard.cs"></script>

### Implementation Using C# SDK

<script src="https://gist.github.com/groupdocs-assembly-cloud/a1b2c3d4e5f6g7h8i9j0.js?file=TutorialCodeExample-DashboardImplementation.cs"></script>

## Troubleshooting Common Issues

When working with charts in templates, you might encounter these common issues:

1. Chart not showing data - Ensure your `x` and `y` tags are properly configured
2. Missing series - Check that your `removeif` conditions aren't unintentionally removing series
3. Incorrect color application - Verify your color expressions return valid values
4. Axis scaling problems - Consider using custom axis settings in your original chart template

> Tip: Always test your charts with sample data of the same structure and scale as your production data.

## What You've Learned

In this tutorial, you've learned how to:
- Bind charts to dynamic data sources
- Customize chart titles, series, and colors dynamically
- Conditionally include or exclude chart series
- Create different types of charts (column, line, pie, bubble)
- Build comprehensive chart dashboards

## Further Practice

To reinforce your learning, try these exercises:

1. Create a multi-series line chart showing trends over time with dynamic coloring
2. Implement a pie chart that highlights the largest segment with a different color
3. Build a column chart with year-over-year comparison and conditional formatting
4. Create a dashboard with complementary charts showing different aspects of the same data

## Next Tutorial in the Learning Path

Ready to continue your journey? Check out our [Tutorial: Working with Other Elements](/working-with-other-elements/) to learn how to work with hyperlinks, bookmarks, checkboxes, and barcodes in your documents.

## Helpful Resources

- [Product Page](https://products.groupdocs.cloud/assembly/)
- [Documentation](https://docs.groupdocs.cloud/assembly/)
- [Live Demo](https://products.groupdocs.app/assembly/family)
- [API Reference](https://reference.groupdocs.cloud/assembly/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.assembly-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/assembly/15/)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)
