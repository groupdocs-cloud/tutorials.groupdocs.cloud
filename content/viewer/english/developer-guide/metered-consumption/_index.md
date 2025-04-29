---
title: Implementing Metered Consumption for GroupDocs.Viewer Cloud Tutorial
url: /developer-guide/metered-consumption/
description: Learn how to implement and monitor metered license consumption in GroupDocs.Viewer Cloud with Docker deployments in this step-by-step tutorial.
---

# Tutorial: Implementing Metered Consumption for GroupDocs.Viewer Cloud

## Learning Objectives

In this tutorial, you'll learn how to:
- Set up a Docker version of GroupDocs.Viewer Cloud
- Implement metered license tracking
- Monitor license consumption
- Retrieve consumption metrics
- Analyze usage patterns

## Prerequisites

Before you begin this tutorial, you need:
- A GroupDocs Cloud account with metered license
- Docker installed on your system
- Basic understanding of Docker containers
- Familiarity with REST APIs and your preferred programming language
- Development environment with the respective GroupDocs.Viewer Cloud SDK installed

## Introduction

Monitoring license consumption is critical for organizations that leverage document processing services. GroupDocs.Viewer Cloud's Docker version provides metered licensing, allowing you to track exact usage and optimize costs.

In this tutorial, we'll walk through the process of implementing metered consumption tracking in a self-hosted Docker deployment of GroupDocs.Viewer Cloud. We'll create a scenario where you're building an enterprise document portal and need to monitor license consumption for reporting and cost allocation.

Let's learn how to implement metered consumption tracking using the GroupDocs.Viewer Cloud API.

## 1. Setting Up Docker Version of GroupDocs.Viewer Cloud

First, let's understand how to set up a Docker version of GroupDocs.Viewer Cloud, which is necessary for implementing metered consumption.

### Step-by-Step Instructions

1. Ensure Docker is installed and running on your system
2. Pull the GroupDocs.Viewer Cloud Docker image from the container registry
3. Configure the container with your metered license
4. Start the container with the proper port mappings
5. Verify the service is running correctly

### Docker Setup Example

```bash
# Pull the GroupDocs.Viewer Cloud Docker image
docker pull groupdocs/viewer-cloud

# Run the container with proper configuration
docker run -d -p 8080:80 -p 8081:443 \
  -e "GROUPDOCS_LIC_PATH=/licenses" \
  -v /local/path/to/licenses:/licenses \
  groupdocs/viewer-cloud
```

## 2. Retrieving Metered License Consumption

Now, let's learn how to retrieve metered license consumption data from your GroupDocs.Viewer Cloud Docker deployment.

### Step-by-Step Instructions

1. Authenticate with the GroupDocs.Viewer Cloud API
2. Create a request to the consumption endpoint
3. Execute the request to retrieve consumption data
4. Process and analyze the consumption information

### cURL Example

```bash
curl -v "http://localhost:8080/v2.0/viewer/consumption" \
  -X GET \
  -H "Accept: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### SDK Examples

Let's see how to retrieve metered consumption using various SDKs:

#### C# Example

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

#### Java Example

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

#### Python Example

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

### Try It Yourself

1. Replace the base URL with your Docker deployment's address
2. Replace the authentication credentials with your actual values
3. Run the code and verify that the consumption data is retrieved
4. Analyze the credit consumption and megabytes processed

## 3. Monitoring Consumption Over Time

Let's learn how to implement a solution for tracking consumption over time to identify trends and optimize usage.

### Step-by-Step Instructions

1. Create a scheduled task to collect consumption data periodically
2. Store historical consumption data in a database
3. Implement analytics to track usage patterns
4. Create dashboards or reports to visualize consumption trends

### Implementation Example (C#)

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

### Example Usage

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

### Try It Yourself

1. Implement the monitoring service in your preferred language
2. Schedule the monitoring to run at appropriate intervals for your needs
3. Create a simple dashboard to visualize consumption trends
4. Use the historical data to predict future consumption and costs

## 4. Optimizing Consumption and Cost

Let's explore strategies for optimizing your metered license consumption to minimize costs while maximizing the value of GroupDocs.Viewer Cloud.

### Optimization Strategies

#### 1. Document Caching

Implement caching for rendered documents to avoid reprocessing the same documents multiple times.

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

#### 2. Batch Processing

Process documents in batches to optimize API usage and reduce overhead.

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

#### 3. Document Compression

Implement compression for document storage to reduce the data transfer volume.

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

### Try These Optimization Techniques

1. Implement document caching to avoid reprocessing the same documents
2. Use batch processing for bulk document operations
3. Apply compression techniques to reduce data transfer volume
4. Track consumption metrics before and after implementing optimizations

## 5. Implementing Consumption Alerting

Let's create a system for alerting when consumption reaches certain thresholds, helping you proactively manage license usage.

### Alert Implementation Example

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

### Alert Handler Examples

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

### Example Usage

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

### Try It Yourself

1. Implement the alert service with appropriate thresholds for your needs
2. Configure alert handlers for your organization's communication channels
3. Set up scheduled tasks to check consumption regularly
4. Test the alerts by temporarily lowering the thresholds

## Common Issues and Troubleshooting

When working with metered consumption, you might encounter these common issues:

1. Docker Connectivity Issues
   - Problem: Unable to connect to Docker deployment
   - Solution: Verify Docker container is running; check network settings and port mapping

2. Authentication Errors
   - Problem: API returns 401 Unauthorized
   - Solution: Check your Client ID and Client Secret; ensure JWT token is valid

3. Consumption Data Not Available
   - Problem: API returns error when retrieving consumption data
   - Solution: Verify metered license is properly configured in Docker container

4. Inaccurate Consumption Reporting
   - Problem: Consumption data doesn't match expected usage
   - Solution: Check for caching or CDN usage that might be affecting actual API calls

## What You've Learned

In this tutorial, you've learned how to:
- Set up a Docker version of GroupDocs.Viewer Cloud
- Implement metered license tracking
- Monitor license consumption over time
- Optimize consumption to reduce costs
- Set up alerts for proactive consumption management

You now have the essential skills to implement a comprehensive metered consumption monitoring system for your GroupDocs.Viewer Cloud deployment.

## Further Practice

To reinforce your learning, try these exercises:
1. Create a detailed consumption dashboard with historical trends
2. Implement cost allocation by user or department
3. Develop usage forecasting based on historical patterns
4. Implement automatic scaling of resources based on consumption patterns

## Resources

- [Product Page](https://products.groupdocs.cloud/viewer/)
- [Documentation](https://docs.groupdocs.cloud/viewer/)
- [Live Demo](https://products.groupdocs.app/viewer/family)
- [API Reference UI](https://reference.groupdocs.cloud/viewer/)
- [Blog](https://blog.groupdocs.cloud/categories/groupdocs.viewer-cloud-product-family/)
- [Free Support](https://forum.groupdocs.cloud/c/viewer/9)
- [Free Trial](https://dashboard.groupdocs.cloud/#/apps)

Have questions about this tutorial? Feel free to reach out on our [support forum](https://forum.groupdocs.cloud/c/viewer/9).