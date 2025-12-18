---
title: Java Connection Configuration
description: Details of integration with the Java microservice.
---

# C# Backend <-> Java Microservice Connection

## Architecture Overview

The C# backend delegates computationally intensive Route Optimization (VRP) tasks to a dedicated Java microservice. Communication is asynchronous and hybrid:

- **Sending (C# -> Java):** HTTP REST (POST).
- **Response (Java -> C#):** Asynchronous messaging via RabbitMQ.

---

## 🌐 HTTP Configuration (Sending)

### Service Details

- **Host (Docker Internal):** `apex-java`
- **Port:** 8081 (Configured in DI) / 8080 (Fallback in Proxy)
- **Base URL:** `http://apex-java:8081/`
- **Main Endpoint:** `POST /api/v1/optimize`

### Configuration in `Program.cs`

The HTTP client is registered with the name `JavaOptimizationApi`.
```csharp
builder.Services.AddHttpClient("JavaOptimizationApi", client =>
{
    client.BaseAddress = new Uri("http://apex-java:8081/");
    // Additional headers if needed
});
```

### Call Example (C#)

**Location:** `Services/OptimizationService.cs`
```csharp
public async Task OptimizeRouteAsync(string driverId)
{
    // 1. Build Payload
    var optimizationRequest = new OptimizationRequestDto
    {
        FleetId = $"driver-{driverId}",
        Locations = pendingOrders.Select(o => new LocationDto
        {
            Id = o.Id,
            Latitude = o.Latitude,
            Longitude = o.Longitude
        }).ToList()
    };
    
    // 2. Get Client
    var client = _httpClientFactory.CreateClient("JavaOptimizationApi");
    
    // 3. Serialize and Send
    var jsonContent = JsonSerializer.Serialize(optimizationRequest);
    var content = new StringContent(jsonContent, Encoding.UTF8, "application/json");
    var response = await client.PostAsync("api/v1/optimize", content);
    response.EnsureSuccessStatusCode();
}
```

---

## 🐇 RabbitMQ Configuration (Reception)

The Java service notifies results or commands through a message queue that C# listens to.

### Connection Parameters (`appsettings.json`)
```json
"RabbitMQ": {
  "HostName": "rabbitmq",
  "UserName": "guest",
  "Password": "guest",
  "QueueName": "task_queue" 
}
```

### C# Components

- **RabbitMqConnection.cs:** Maintains persistent connection with the bus.
- **CommandConsumer.cs:** Background Service that listens to the `task_queue` queue.
- **CommandExecutor.cs:** Executes received logic (currently system commands, pending refactoring to domain events).

### Data Flow

1. C# sends request via HTTP.
2. Java processes optimization (may take seconds/minutes).
3. Java publishes message to RabbitMQ.
4. `CommandConsumer` receives message => `CommandExecutor` applies changes (update stop order in DB).

---

## ⚠️ Required Environment Variables

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `RABBITMQ_HOST` | RabbitMQ server host | `rabbitmq` |
| `JAVA_SERVICE_URL` | Java service base URL | `http://apex-java:8081` |