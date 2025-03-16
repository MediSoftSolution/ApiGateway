# APIGateway - Hospital System Microservices

This is the **API Gateway** for the **Hospital System Microservices**. It acts as the central entry point for all incoming requests and routes them to the appropriate microservice.

## 📌 Overview

The **API Gateway** is a critical component of the Hospital System. It provides:

- **Routing**: All incoming requests are routed to the respective microservice (e.g., HospitalService, IdentityService, PaymentService, etc.).
- **Load Balancing**: Distributes incoming requests across available instances of each microservice.
- **Security**: Ensures secure access by managing authentication and authorization.
- **API Gateway Pattern**: Acts as a single point of entry for clients, reducing the complexity for client applications.
- **Service Discovery**: Integrated with **Consul** for dynamic service discovery.

---

## 🔧 Technologies Used
- **Backend**: C# - .NET 8
- **API Gateway**: Ocelot
- **Authentication**: Integrated with IdentityService for secure authentication
- **Routing**: Custom routing rules defined for each microservice
- **Service Discovery**: Consul for dynamic microservice discovery

---

## 🚀 Features
- **Routing requests** to the correct microservice based on URL patterns and service names.
- **User Authentication**: Validates tokens using IdentityService.
- **API Gateway Pattern**: Centralized access point for all services.
- **Service Aggregation**: Combines responses from multiple microservices into a single response when needed.
- **Service Discovery with Consul**: Automatically discovers and registers available microservices.

---

## 🔧 Configuration

### Consul Integration

The **APIGateway** uses **Consul** for service discovery. Consul helps the gateway dynamically discover the available microservices instead of relying on static configurations.

#### 1. **Consul Configuration in Ocelot**

To configure **Consul** in **Ocelot**, ensure you have the correct setup in your **`ocelot.json`** file.

Example `ocelot.json` with Consul integration:

```json
{
  "ReRoutes": [
    {
      "DownstreamPathTemplate": "/hospitalservice/{everything}",
      "UpstreamPathTemplate": "/hospitalservice/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "hospitalservice",
          "Port": 80
        }
      ],
      "UpstreamHttpMethod": [ "Get", "Post" ]
    },
    {
      "DownstreamPathTemplate": "/identityservice/{everything}",
      "UpstreamPathTemplate": "/identityservice/{everything}",
      "DownstreamScheme": "http",
      "DownstreamHostAndPorts": [
        {
          "Host": "identityservice",
          "Port": 80
        }
      ],
      "UpstreamHttpMethod": [ "Post", "Put" ]
    }
  ],
  "GlobalConfiguration": {
    "BaseUrl": "http://localhost:5000",
    "ServiceDiscoveryProvider": {
      "Host": "localhost",
      "Port": 8500,
      "Type": "Consul"
    }
  }
}
