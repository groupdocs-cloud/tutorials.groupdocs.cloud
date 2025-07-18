---
title: "GroupDocs Viewer Cloud Metered Consumption - Complete Docker Setup"
linktitle: "Metered Consumption Tutorial"
description: "Learn how to implement and monitor GroupDocs Viewer Cloud metered consumption with Docker deployments. Step-by-step tutorial with code examples for cost optimization."
keywords: "GroupDocs Viewer Cloud metered consumption, Docker GroupDocs Viewer, metered license tracking, consumption monitoring API, GroupDocs Cloud usage tracking"
weight: 10
url: /getting-started/metered-consumption/
date: "2025-01-02"
lastmod: "2025-01-02"
categories: ["Getting Started"]
tags: ["metered-consumption", "docker", "monitoring", "cost-optimization"]
---

# Complete Guide to GroupDocs Viewer Cloud Metered Consumption with Docker

## Why Metered Consumption Matters for Your Document Processing Pipeline

If you're running a document processing service at scale, you've probably wondered: "How much am I actually spending on document viewing?" or "Which departments are using the most resources?" That's exactly what GroupDocs Viewer Cloud metered consumption tracking helps you answer.

Whether you're building an enterprise document portal, a customer-facing file viewer, or integrating document processing into your existing applications, understanding your consumption patterns isn't just nice to have—it's essential for controlling costs and optimizing performance.

In this comprehensive tutorial, you'll learn how to set up metered consumption tracking for GroupDocs Viewer Cloud using Docker, implement real-time monitoring, and build cost optimization strategies that actually work in production environments.

## What You'll Learn in This Tutorial

By the end of this guide, you'll be able to:
- Set up a Docker version of GroupDocs Viewer Cloud with metered licensing
- Implement automated consumption tracking and monitoring
- Create alerts for usage thresholds (before you hit budget limits)
- Optimize your consumption to reduce costs without sacrificing functionality
- Build comprehensive reporting for stakeholders and cost allocation

## Prerequisites: What You Need Before Starting

Before diving into the implementation, make sure you have:
- A GroupDocs Cloud account with metered license credentials
- Docker installed and running on your system
- Basic familiarity with REST APIs and your preferred programming language
- Development environment with the GroupDocs.Viewer Cloud SDK installed

Don't worry if you're new to metered licensing—we'll explain everything step by step.

## Understanding Metered Consumption in GroupDocs Viewer Cloud

Here's the thing about metered consumption: it's not just about tracking usage—it's about understanding patterns. Unlike fixed licensing, metered consumption gives you granular visibility into exactly how your document processing resources are being used.

Think of it like your electricity bill. Instead of paying a flat rate regardless of usage, you pay based on actual consumption. This means you can identify peak usage times, optimize processing workflows, and allocate costs accurately across departments or projects.

## 1. Setting Up Docker Version of GroupDocs Viewer Cloud

Let's start with the foundation—getting your Docker environment properly configured for metered consumption tracking.

### Why Docker for GroupDocs Viewer Cloud?

Docker deployment gives you several advantages for metered consumption tracking:
- Complete control over your environment
- Easier integration with existing monitoring systems
- Better security for sensitive documents
- Simplified scaling based on consumption patterns

### Step-by-Step Docker Setup

Here's how to get your Docker environment running with metered consumption enabled:

1. **Pull the Official Image**: First, grab the latest GroupDocs.Viewer Cloud Docker image from the registry
2. **Configure License Mounting**: Set up proper volume mounting for your license files
3. **Enable Consumption Tracking**: Configure the container to expose consumption endpoints
4. **Verify Connectivity**: Test that everything's working correctly

### Docker Setup Commands

```bash
# Pull the GroupDocs.Viewer Cloud Docker image
docker pull groupdocs/viewer-cloud

# Run the container with proper configuration
docker run -d -p 8080:80 -p 8081:443 \
  -e "GROUPDOCS_LIC_PATH=/licenses" \
  -v /local/path/to/licenses:/licenses \
  groupdocs/viewer-cloud
```

**Pro Tip**: Always use specific version tags in production instead of 'latest' to avoid unexpected changes that might affect your consumption tracking.

### Verifying Your Setup

After starting the container, you should be able to access:
- API endpoint: `http://localhost:8080/v2.0/viewer`
- Health check: `http://localhost:8080/v2.0/viewer/health`
- Consumption endpoint: `http://localhost:8080/v2.0/viewer/consumption`

If any of these don't respond, check your Docker logs: `docker logs [container_id]`

## 2. Implementing Metered License Consumption Tracking

Now comes the interesting part—actually retrieving and working with your consumption data. This is where you'll get insights into how your application is really performing.

### Understanding the Consumption API Response

The consumption API returns two key metrics:
- **Credits**: The number of processing credits consumed
- **Quantity**: The total megabytes of documents processed

These metrics help you understand both the volume and complexity of your document processing workload.

### Basic Consumption Retrieval

Let's start with a simple example to get your consumption data:

### cURL Example

```bash
curl -v "http://localhost:8080/v2.0/viewer/consumption" \
  -X GET \
  -H "Accept: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### SDK Implementation Examples

Here's how to implement consumption tracking using the official SDKs:

#### C# Implementation

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using System;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class GetMeteredConsumptionExample
    {
        public static void Run()
        {
            // Authenticate with the API
            var configuration = new Configuration(Common.MyAppSid, Common.MyAppKey);
            
            // The base URL should point to your Docker deployment
            configuration.ApiBaseUrl = "http://localhost:8080/v2.0";
            
            var apiInstance = new LicenseApi(configuration);

            try
            {
                // Execute the request to get consumption data
                var response = apiInstance.GetConsumptionCredit();
                
                // Process the consumption information
                Console.WriteLine($"Credits consumed: {response.Credit}");
                Console.WriteLine($"Megabytes processed: {response.Quantity}");
                
                // Calculate average MB per credit
                if (response.Credit > 0)
                {
                    double mbPerCredit = response.Quantity / response.Credit;
                    Console.WriteLine($"Average MB per credit: {mbPerCredit:F2}");
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error retrieving consumption data: " + e.Message);
            }
        }
    }
}
```

#### Java Implementation

```java
package examples;

import com.groupdocs.cloud.viewer.api.*;
import com.groupdocs.cloud.viewer.client.ApiException;
import com.groupdocs.cloud.viewer.client.Configuration;
import com.groupdocs.cloud.viewer.model.*;

public class GetMeteredConsumptionExample {

    public static void main(String[] args) {
        // Authenticate with the API
        String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";
        
        // Create configuration with Docker deployment URL
        Configuration configuration = new Configuration(clientId, clientSecret);
        configuration.setApiBaseUrl("http://localhost:8080/v2.0");
        
        // Create API instance
        LicenseApi apiInstance = new LicenseApi(configuration);
        
        try {
            // Execute the request to get consumption data
            ConsumptionResult response = apiInstance.getConsumptionCredit();
            
            // Process the consumption information
            System.out.println("Credits consumed: " + response.getCredit());
            System.out.println("Megabytes processed: " + response.getQuantity());
            
            // Calculate average MB per credit
            if (response.getCredit() > 0) {
                double mbPerCredit = response.getQuantity() / response.getCredit();
                System.out.printf("Average MB per credit: %.2f%n", mbPerCredit);
            }
        } catch (ApiException e) {
            System.err.println("Error retrieving consumption data: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

#### Python Implementation

```python
import groupdocs_viewer_cloud
from groupdocs_viewer_cloud import Configuration

# Authenticate with the API
app_sid = "YOUR_CLIENT_ID"
app_key = "YOUR_CLIENT_SECRET"

# Create configuration with Docker deployment URL
configuration = groupdocs_viewer_cloud.Configuration(app_sid, app_key)
configuration.api_base_url = "http://localhost:8080/v2.0"

# Create API instance
api_instance = groupdocs_viewer_cloud.LicenseApi(configuration)

try:
    # Execute the request to get consumption data
    response = api_instance.get_consumption_credit()
    
    # Process the consumption information
    print(f"Credits consumed: {response.credit}")
    print(f"Megabytes processed: {response.quantity}")
    
    # Calculate average MB per credit
    if response.credit > 0:
        mb_per_credit = response.quantity / response.credit
        print(f"Average MB per credit: {mb_per_credit:.2f}")
    
except groupdocs_viewer_cloud.ApiException as e:
    print(f"Error retrieving consumption data: {e.message}")
```

### Testing Your Implementation

Once you've implemented the basic consumption retrieval, test it with these steps:

1. **Replace the Credentials**: Update the example with your actual Client ID and Client Secret
2. **Update the Base URL**: Point to your Docker deployment's address
3. **Run the Code**: Execute and verify that consumption data is retrieved successfully
4. **Analyze the Results**: Look at the credit-to-megabyte ratio to understand your processing efficiency

The credit-to-megabyte ratio is particularly interesting—it tells you how "complex" your documents are. Simple text documents will have a different ratio than complex PDFs with images and formatting.

## 3. Building a Comprehensive Consumption Monitoring System

Getting consumption data once is useful, but building a system that tracks changes over time? That's where the real insights come from.

### Why Historical Tracking Matters

Here's what I've learned from implementing consumption tracking in production environments: the patterns matter more than the absolute numbers. You want to know:
- When do you hit peak usage? (So you can optimize processing schedules)
- Which types of documents consume the most resources?
- Are there unusual spikes that might indicate issues?
- How does your usage correlate with business metrics?

### Implementing Time-Series Monitoring

Let's build a robust monitoring system that captures consumption data over time:

```csharp
using GroupDocs.Viewer.Cloud.Sdk.Api;
using GroupDocs.Viewer.Cloud.Sdk.Client;
using System;
using System.Threading.Tasks;
using System.Collections.Generic;
using System.Data.SQLite;
using System.Threading;

namespace GroupDocs.Viewer.Cloud.Examples
{
    class ConsumptionMonitoringService
    {
        private readonly Configuration configuration;
        private readonly string dbPath;
        private Timer timer;
        
        public ConsumptionMonitoringService(string clientId, string clientSecret, string apiBaseUrl, string dbPath)
        {
            // Initialize configuration
            configuration = new Configuration(clientId, clientSecret)
            {
                ApiBaseUrl = apiBaseUrl
            };
            
            this.dbPath = dbPath;
            
            // Create database if not exists
            CreateDatabaseIfNotExists();
        }
        
        public void StartMonitoring(TimeSpan interval)
        {
            // Start timer to collect data at specified interval
            timer = new Timer(async _ => await CollectConsumptionData(), null, TimeSpan.Zero, interval);
            
            Console.WriteLine($"Monitoring started with interval: {interval}");
        }
        
        public void StopMonitoring()
        {
            timer?.Dispose();
            Console.WriteLine("Monitoring stopped");
        }
        
        private async Task CollectConsumptionData()
        {
            try
            {
                var apiInstance = new LicenseApi(configuration);
                var response = apiInstance.GetConsumptionCredit();
                
                // Store data in database
                StoreConsumptionData(response.Credit, response.Quantity);
                
                Console.WriteLine($"[{DateTime.Now}] Data collected - Credits: {response.Credit}, MB: {response.Quantity}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error collecting consumption data: {ex.Message}");
            }
        }
        
        private void CreateDatabaseIfNotExists()
        {
            try
            {
                // Create SQLite database if not exists
                SQLiteConnection.CreateFile(dbPath);
                
                using (var connection = new SQLiteConnection($"Data Source={dbPath};Version=3;"))
                {
                    connection.Open();
                    
                    // Create table for consumption data
                    string sql = @"
                    CREATE TABLE IF NOT EXISTS consumption_history (
                        id INTEGER PRIMARY KEY AUTOINCREMENT,
                        timestamp DATETIME NOT NULL,
                        credits REAL NOT NULL,
                        megabytes REAL NOT NULL
                    )";
                    
                    using (var command = new SQLiteCommand(sql, connection))
                    {
                        command.ExecuteNonQuery();
                    }
                }
                
                Console.WriteLine("Database initialized successfully");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error creating database: {ex.Message}");
            }
        }
        
        private void StoreConsumptionData(decimal credits, decimal megabytes)
        {
            try
            {
                using (var connection = new SQLiteConnection($"Data Source={dbPath};Version=3;"))
                {
                    connection.Open();
                    
                    string sql = @"
                    INSERT INTO consumption_history (timestamp, credits, megabytes)
                    VALUES (@timestamp, @credits, @megabytes)";
                    
                    using (var command = new SQLiteCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@timestamp", DateTime.Now);
                        command.Parameters.AddWithValue("@credits", (double)credits);
                        command.Parameters.AddWithValue("@megabytes", (double)megabytes);
                        command.ExecuteNonQuery();
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error storing consumption data: {ex.Message}");
            }
        }
        
        public List<ConsumptionRecord> GetConsumptionHistory(DateTime startDate, DateTime endDate)
        {
            var records = new List<ConsumptionRecord>();
            
            try
            {
                using (var connection = new SQLiteConnection($"Data Source={dbPath};Version=3;"))
                {
                    connection.Open();
                    
                    string sql = @"
                    SELECT timestamp, credits, megabytes
                    FROM consumption_history
                    WHERE timestamp BETWEEN @startDate AND @endDate
                    ORDER BY timestamp";
                    
                    using (var command = new SQLiteCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@startDate", startDate);
                        command.Parameters.AddWithValue("@endDate", endDate);
                        
                        using (var reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                records.Add(new ConsumptionRecord
                                {
                                    Timestamp = Convert.ToDateTime(reader["timestamp"]),
                                    Credits = Convert.ToDecimal(reader["credits"]),
                                    Megabytes = Convert.ToDecimal(reader["megabytes"])
                                });
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error retrieving consumption history: {ex.Message}");
            }
            
            return records;
        }
    }
    
    class ConsumptionRecord
    {
        public DateTime Timestamp { get; set; }
        public decimal Credits { get; set; }
        public decimal Megabytes { get; set; }
    }
}
```

### Example Usage and Reporting

Here's how to use the monitoring system in practice:

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Initialize the monitoring service
        var service = new ConsumptionMonitoringService(
            "YOUR_CLIENT_ID",
            "YOUR_CLIENT_SECRET",
            "http://localhost:8080/v2.0",
            "consumption_data.db"
        );
        
        // Start monitoring with 1-hour interval
        service.StartMonitoring(TimeSpan.FromHours(1));
        
        Console.WriteLine("Press any key to stop monitoring...");
        Console.ReadKey();
        
        // Stop monitoring
        service.StopMonitoring();
        
        // Generate report for the last 7 days
        var endDate = DateTime.Now;
        var startDate = endDate.AddDays(-7);
        
        var history = service.GetConsumptionHistory(startDate, endDate);
        
        Console.WriteLine($"\nConsumption Report ({startDate} to {endDate}):");
        Console.WriteLine("Timestamp\t\tCredits\t\tMegabytes");
        Console.WriteLine("--------------------------------------------------");
        
        foreach (var record in history)
        {
            Console.WriteLine($"{record.Timestamp}\t{record.Credits}\t\t{record.Megabytes}");
        }
        
        // Calculate total consumption
        var totalCredits = history.Sum(r => r.Credits);
        var totalMegabytes = history.Sum(r => r.Megabytes);
        
        Console.WriteLine("--------------------------------------------------");
        Console.WriteLine($"Total:\t\t\t{totalCredits}\t\t{totalMegabytes}");
    }
}
```

### What This Monitoring System Gives You

With this system in place, you'll be able to:
- **Identify Usage Patterns**: See when your application processes the most documents
- **Detect Anomalies**: Spot unusual spikes that might indicate issues or attacks
- **Plan Capacity**: Understand growth trends to plan for scaling
- **Allocate Costs**: Generate department-specific reports for cost allocation

## 4. Advanced Cost Optimization Strategies

Now that you have monitoring in place, let's talk about optimization. These strategies can significantly reduce your consumption costs without impacting user experience.

### Document Caching Strategy

One of the most effective ways to reduce consumption is implementing smart caching. Here's why: if you're processing the same document multiple times, you're paying for the same work repeatedly.

```csharp
public class DocumentCache
{
    private readonly Dictionary<string, byte[]> cache = new Dictionary<string, byte[]>();
    private readonly int maxCacheSize;
    
    public DocumentCache(int maxCacheSize = 100)
    {
        this.maxCacheSize = maxCacheSize;
    }
    
    public bool TryGetDocument(string documentId, out byte[] documentData)
    {
        return cache.TryGetValue(documentId, out documentData);
    }
    
    public void CacheDocument(string documentId, byte[] documentData)
    {
        // Implement cache eviction if size exceeds maximum
        if (cache.Count >= maxCacheSize)
        {
            // Simple LRU implementation: remove first item
            var firstKey = cache.Keys.First();
            cache.Remove(firstKey);
        }
        
        cache[documentId] = documentData;
    }
}
```

### Batch Processing for Efficiency

Instead of processing documents one by one, batch processing can reduce overhead and improve efficiency:

```csharp
public class BatchProcessor
{
    private readonly List<DocumentTask> queue = new List<DocumentTask>();
    private readonly int batchSize;
    private readonly Timer timer;
    
    public BatchProcessor(int batchSize = 10, int processingIntervalSeconds = 60)
    {
        this.batchSize = batchSize;
        timer = new Timer(_ => ProcessBatch(), null, 
            TimeSpan.FromSeconds(processingIntervalSeconds), 
            TimeSpan.FromSeconds(processingIntervalSeconds));
    }
    
    public void QueueDocument(string documentId, Action<byte[]> callback)
    {
        lock (queue)
        {
            queue.Add(new DocumentTask { DocumentId = documentId, Callback = callback });
        }
    }
    
    private void ProcessBatch()
    {
        List<DocumentTask> batchToProcess;
        
        lock (queue)
        {
            batchToProcess = queue.Take(batchSize).ToList();
            queue.RemoveRange(0, Math.Min(batchSize, queue.Count));
        }
        
        if (batchToProcess.Count == 0)
            return;
            
        Console.WriteLine($"Processing batch of {batchToProcess.Count} documents");
        
        // Here you would implement actual batch processing
        // ...
        
        // Simulating processing and calling callbacks
        foreach (var task in batchToProcess)
        {
            // Simulate document data
            var documentData = new byte[1024];
            task.Callback(documentData);
        }
    }
    
    private class DocumentTask
    {
        public string DocumentId { get; set; }
        public Action<byte[]> Callback { get; set; }
    }
}
```

### Document Compression for Reduced Transfer

Implementing compression can significantly reduce the data transfer volume:

```csharp
public static class DocumentCompressor
{
    public static byte[] Compress(byte[] data)
    {
        using (var compressedStream = new MemoryStream())
        using (var gzipStream = new GZipStream(compressedStream, CompressionLevel.Optimal))
        {
            gzipStream.Write(data, 0, data.Length);
            gzipStream.Close();
            return compressedStream.ToArray();
        }
    }
    
    public static byte[] Decompress(byte[] compressedData)
    {
        using (var compressedStream = new MemoryStream(compressedData))
        using (var gzipStream = new GZipStream(compressedStream, CompressionMode.Decompress))
        using (var resultStream = new MemoryStream())
        {
            gzipStream.CopyTo(resultStream);
            return resultStream.ToArray();
        }
    }
}
```

### Practical Optimization Tips

Based on real-world implementations, here are some additional strategies that work:

1. **Smart Preprocessing**: Filter out unnecessary document pages before processing
2. **Quality Settings**: Use appropriate quality settings for different use cases
3. **Format Optimization**: Convert documents to more efficient formats when possible
4. **Peak Hour Shifting**: Schedule bulk processing during off-peak hours if possible

## 5. Proactive Consumption Alerting System

Here's something I wish I had implemented from day one: proactive alerts. Instead of finding out you've exceeded your budget after the fact, set up alerts that give you time to react.

### Building an Intelligent Alert System

```csharp
public class ConsumptionAlertService
{
    private readonly Configuration configuration;
    private readonly decimal warningThreshold;
    private readonly decimal criticalThreshold;
    private readonly List<Action<ConsumptionAlert>> alertHandlers = new List<Action<ConsumptionAlert>>();
    
    public ConsumptionAlertService(
        string clientId, 
        string clientSecret, 
        string apiBaseUrl,
        decimal warningThreshold = 70, 
        decimal criticalThreshold = 90)
    {
        // Initialize configuration
        configuration = new Configuration(clientId, clientSecret)
        {
            ApiBaseUrl = apiBaseUrl
        };
        
        this.warningThreshold = warningThreshold;
        this.criticalThreshold = criticalThreshold;
    }
    
    public void RegisterAlertHandler(Action<ConsumptionAlert> handler)
    {
        alertHandlers.Add(handler);
    }
    
    public void CheckConsumption(decimal monthlyLimitCredits)
    {
        try
        {
            var apiInstance = new LicenseApi(configuration);
            var consumption = apiInstance.GetConsumptionCredit();
            
            // Calculate percentage of monthly limit
            decimal percentUsed = (consumption.Credit / monthlyLimitCredits) * 100;
            
            // Determine alert level
            AlertLevel level = AlertLevel.Normal;
            
            if (percentUsed >= criticalThreshold)
                level = AlertLevel.Critical;
            else if (percentUsed >= warningThreshold)
                level = AlertLevel.Warning;
                
            // Create alert if not normal
            if (level != AlertLevel.Normal)
            {
                var alert = new ConsumptionAlert
                {
                    Timestamp = DateTime.Now,
                    Level = level,
                    CreditsUsed = consumption.Credit,
                    MonthlyLimit = monthlyLimitCredits,
                    PercentUsed = percentUsed,
                    Message = $"License consumption is at {percentUsed:F1}% of monthly limit ({consumption.Credit} of {monthlyLimitCredits} credits used)"
                };
                
                // Notify all handlers
                foreach (var handler in alertHandlers)
                {
                    handler(alert);
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error checking consumption for alerts: {ex.Message}");
        }
    }
    
    public enum AlertLevel
    {
        Normal,
        Warning,
        Critical
    }
    
    public class ConsumptionAlert
    {
        public DateTime Timestamp { get; set; }
        public AlertLevel Level { get; set; }
        public decimal CreditsUsed { get; set; }
        public decimal MonthlyLimit { get; set; }
        public decimal PercentUsed { get; set; }
        public string Message { get; set; }
    }
}
```

### Alert Handler Implementation Examples

Here are practical examples of how to handle different types of alerts:

```csharp
// Email Alert Handler
static void EmailAlertHandler(ConsumptionAlertService.ConsumptionAlert alert)
{
    Console.WriteLine($"Sending email alert: {alert.Level} - {alert.Message}");
    
    // Implement your email sending logic here
    // Example using System.Net.Mail:
    /*
    using (var client = new SmtpClient("smtp.example.com"))
    {
        client.Credentials = new NetworkCredential("username", "password");
        var message = new MailMessage(
            "alerts@yourcompany.com",
            "admin@yourcompany.com",
            $"GroupDocs Viewer {alert.Level} Alert",
            alert.Message
        );
        client.Send(message);
    }
    */
}

// Slack Alert Handler
static void SlackAlertHandler(ConsumptionAlertService.ConsumptionAlert alert)
{
    Console.WriteLine($"Sending Slack alert: {alert.Level} - {alert.Message}");
    
    // Implement your Slack webhook integration here
    // Example using HttpClient:
    /*
    using (var client = new HttpClient())
    {
        var payload = new
        {
            text = $"*{alert.Level} Alert*: {alert.Message}",
            channel = "#alerts",
            username = "GroupDocs Monitor"
        };
        
        var content = new StringContent(JsonConvert.SerializeObject(payload), Encoding.UTF8, "application/json");
        client.PostAsync("https://hooks.slack.com/services/YOUR_WEBHOOK_URL", content).Wait();
    }
    */
}

// Log Alert Handler
static void LogAlertHandler(ConsumptionAlertService.ConsumptionAlert alert)
{
    string logMessage = $"[{alert.Timestamp}] {alert.Level}: {alert.Message}";
    Console.WriteLine(logMessage);
    
    // Append to log file
    File.AppendAllText("consumption_alerts.log", logMessage + Environment.NewLine);
}
```

### Setting Up Your Alert System

Here's a complete example of how to implement the alert system:

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Initialize the alert service
        var alertService = new ConsumptionAlertService(
            "YOUR_CLIENT_ID",
            "YOUR_CLIENT_SECRET",
            "http://localhost:8080/v2.0",
            warningThreshold: 70,   // Alert at 70% usage
            criticalThreshold: 90   // Critical alert at 90% usage
        );
        
        // Register alert handlers
        alertService.RegisterAlertHandler(EmailAlertHandler);
        alertService.RegisterAlertHandler(SlackAlertHandler);
        alertService.RegisterAlertHandler(LogAlertHandler);
        
        // Check consumption with a monthly limit of 10,000 credits
        alertService.CheckConsumption(10000);
        
        // In production, you'd typically set this up as a scheduled task
        // that runs daily or hourly
    }
    
    // Alert handlers defined here...
}
```

### Why This Alert System Works

This approach is effective because:
- **Multiple Channels**: You get notified through email, Slack, and logs
- **Threshold-Based**: Different actions for different severity levels
- **Proactive**: Alerts before you hit your limit, not after
- **Customizable**: Easy to adjust thresholds based on your needs

## Troubleshooting Common Issues

After implementing metered consumption tracking in multiple environments, here are the most common issues you'll encounter and how to solve them:

### Docker Container Issues

**Problem**: "Can't connect to GroupDocs Viewer Docker container"
**What's happening**: Usually a network configuration or port mapping issue
**Solution**: 
1. Check if the container is actually running: `docker ps`
2. Verify port mapping: `docker port [container_name]`
3. Test basic connectivity: `curl http://localhost:8080/v2.0/viewer/health`
4. Check Docker logs: `docker logs [container_name]`

### Authentication Problems

**Problem**: "Getting 401 Unauthorized errors when calling the API"
**What's happening**: Usually credential or token issues
**Solution**:
1. Verify your Client ID and Client Secret are correct
2. Check if your JWT token is properly generated and not expired
3. Ensure you're using the correct API base URL
4. Test authentication with a simple API call first

### Missing Consumption Data

**Problem**: "Consumption endpoint returns null or empty data"
**What's happening**: Metered license not properly configured
**Solution**:
1. Verify your license file is correctly mounted in the Docker container
2. Check that your license actually supports metered consumption
3. Ensure the GROUPDOCS_LIC_PATH environment variable points to the right location
4. Test with a different API endpoint to confirm basic functionality

### Inaccurate Consumption Reporting

**Problem**: "Consumption numbers don't match what I expect"
**What's happening**: Could be caching, CDN, or multiple processing instances
**Solution**:
1. Check if you have any caching layers that might be serving cached results
2. Verify you're not double-counting consumption from multiple instances
3. Look for any CDN or proxy services that might affect API calls
4. Compare consumption patterns with actual document processing logs

### Performance and Latency Issues

**Problem**: "Consumption API calls are slow or timing out"
**What's happening**: Network issues or resource constraints
**Solution**:
1. Check your Docker container resource allocation (CPU, memory)
2. Monitor network latency between your application and Docker container
3. Consider implementing connection pooling for API calls
4. Add retry logic with exponential backoff

### Database Connection Problems (Monitoring System)

**Problem**: "SQLite database errors in monitoring system"
**What's happening**: File permissions or concurrent access issues
**Solution**:
1. Ensure your application has write permissions to the database directory
2. Use connection pooling to handle concurrent access
3. Implement proper exception handling for database operations
4. Consider using a more robust database for high-volume scenarios

## Real-World Implementation Best Practices

Based on production deployments, here are some practices that will save you headaches:

### Start Small, Scale Gradually

Don't try to implement everything at once. Start with basic consumption tracking, then add monitoring, then optimization. This approach helps you understand the patterns before building complex systems.

### Set Realistic Alert Thresholds

If you set your warning threshold too low (like 50%), you'll get alert fatigue. Start with 70% for warnings and 90% for critical alerts, then adjust based on your usage patterns.

### Plan for Peak Usage

Most applications have predictable usage patterns. If you're in a business environment, you might see peaks during business hours. Plan your monitoring and alerting around these patterns.

### Keep Historical Data

Don't just track current consumption—keep at least 3-6 months of historical data. This helps with trend analysis and capacity planning.

## Advanced Scenarios and Use Cases

### Multi-Tenant Applications

If you're building a multi-tenant application, you'll want to track consumption per tenant:

```csharp
public class TenantConsumptionTracker
{
    private readonly Dictionary<string, decimal> tenantConsumption = new Dictionary<string, decimal>();
    
    public void TrackTenantUsage(string tenantId, decimal credits)
    {
        if (tenantConsumption.ContainsKey(tenantId))
            tenantConsumption[tenantId] += credits;
        else
            tenantConsumption[tenantId] = credits;
    }
    
    public Dictionary<string, decimal> GetTenantReport()
    {
        return new Dictionary<string, decimal>(tenantConsumption);
    }
}
```

### Cost Allocation by Department

For enterprise scenarios, you might need to allocate costs by department or project:

```csharp
public class DepartmentCostAllocator
{
    public void AllocateCosts(List<ConsumptionRecord> records, Dictionary<string, string> userToDepartment)
    {
        var departmentUsage = new Dictionary<string, decimal>();
        
        foreach (var record in records)
        {
            // Logic to determine which department used the credits
            // This would depend on your application's user tracking
        }
        
        // Generate department-specific reports
    }
}
```

### Integration with Business Intelligence Tools

Consider integrating your consumption data with BI tools like Power BI, Tableau, or Grafana for advanced analytics and visualization.

## Performance Optimization Tips

### Reduce API Call Frequency

Instead of checking consumption after every document operation, batch your checks:

```csharp
public class BatchedConsumptionChecker
{
    private int operationCount = 0;
    private readonly int checkInterval = 50; // Check every 50 operations
    
    public void OnDocumentProcessed()
    {
        operationCount++;
        
        if (operationCount >= checkInterval)
        {
            CheckConsumption();
            operationCount = 0;
        }
    }
}
```

### Cache Consumption Data

For high-frequency applications, cache consumption data for a few minutes to reduce API load:

```csharp
public class CachedConsumptionService
{
    private ConsumptionResult cachedResult;
    private DateTime lastCheck;
    private readonly TimeSpan cacheTimeout = TimeSpan.FromMinutes(5);
    
    public ConsumptionResult GetConsumption()
    {
        if (cachedResult == null || DateTime.Now - lastCheck > cacheTimeout)
        {
            // Fetch fresh data
            var api = new LicenseApi(configuration);
            cachedResult = api.GetConsumptionCredit();
            lastCheck = DateTime.Now;
        }
        
        return cachedResult;
    }
}
```

## Security Considerations

### Protect Your Credentials

Never hardcode credentials in your source code. Use environment variables or secure configuration management:

```csharp
public class SecureConfiguration
{
    public static Configuration GetConfiguration()
    {
        var clientId = Environment.GetEnvironmentVariable("GROUPDOCS_CLIENT_ID");
        var clientSecret = Environment.GetEnvironmentVariable("GROUPDOCS_CLIENT_SECRET");
        
        if (string.IsNullOrEmpty(clientId) || string.IsNullOrEmpty(clientSecret))
            throw new InvalidOperationException("Missing required environment variables");
            
        return new Configuration(clientId, clientSecret);
    }
}
```

### Secure Your Docker Deployment

1. **Use non-root users** in your Docker containers
2. **Limit network exposure** to only necessary ports
3. **Regularly update** your Docker images
4. **Monitor access logs** for unusual patterns

### Protect Consumption Data

Consumption data can be sensitive business information. Ensure:
- Database connections are encrypted
- Access to consumption reports is properly authenticated
- Historical data is backed up securely

## Next Steps and Advanced Topics

Now that you have the foundation in place, consider these advanced implementations:

### 1. Build a Consumption Dashboard

Create a web-based dashboard using your preferred framework (React, Vue, Angular) to visualize consumption trends, costs, and alerts in real-time.

### 2. Implement Predictive Analytics

Use your historical data to predict future consumption and automatically adjust processing schedules or alert thresholds.

### 3. Create Custom Metrics

Develop application-specific metrics like "cost per user" or "consumption per document type" that align with your business KPIs.

### 4. Automate Scaling

Integrate consumption data with your infrastructure automation to automatically scale Docker containers based on usage patterns.

### 5. Build Cost Optimization Rules

Implement automatic optimization rules that adjust processing parameters based on consumption trends and cost targets.

## Frequently Asked Questions

**Q: How often should I check consumption data?**
A: For most applications, checking every hour is sufficient. For high-volume applications, you might want to check every 15-30 minutes. Avoid checking more frequently than every 5 minutes to prevent API rate limiting.

**Q: What's a typical credit-to-megabyte ratio?**
A: This varies significantly based on document types. Simple text documents might process at 0.1-0.5 credits per MB, while complex PDFs with images could use 2-5 credits per MB. Monitor your specific patterns to establish baselines.

**Q: Can I get consumption data for specific time periods?**
A: The GroupDocs API provides cumulative consumption data. To track specific periods, you'll need to implement the historical tracking system shown in this tutorial.

**Q: How do I handle consumption tracking in a load-balanced environment?**
A: Implement a centralized tracking service that all instances can report to, or use the approach shown in this tutorial with a shared database for storing consumption history.

**Q: What happens if I exceed my monthly limit?**
A: This depends on your license agreement. Some licenses have hard limits that stop processing, others allow overages with additional charges. Check your specific license terms and implement appropriate alerts well before limits are reached.

## Additional Resources and Support

Want to dive deeper? Here are some valuable resources:

- [Product Overview](https://products.groupdocs.cloud/viewer/) - Complete feature overview and pricing information
- [API Documentation](https://docs.groupdocs.cloud/viewer/) - Comprehensive API reference and guides  
- [Live Demo](https://products.groupdocs.app/viewer/family) - Try GroupDocs Viewer features online
- [API Reference UI](https://reference.groupdocs.cloud/viewer/) - Interactive API documentation
- [Developer Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/) - Latest updates and tutorials
- [Community Support](https://forum.groupdocs.cloud/c/viewer/9) - Get help from the community and GroupDocs team
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps) - Start your free trial today
