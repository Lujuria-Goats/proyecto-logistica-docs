---
title: API Specifications
description: Documentation of Endpoints and data models.
---

# API REST Specifications - ApexVision Backend

## General Information

- **Base URL:** `/api`
- **Authentication:** JWT (Bearer Token)
  - **Header:** `Authorization: Bearer <token>`
- **Formats:** JSON (CamelCase by default)

---

## 🔐 Authentication (AuthController)

### POST /api/Auth/login

Logs in and obtains a JWT token.

**Request Body:**
```json
{
  "email": "admin@apexvision.com",
  "password": "Password123!"
}
```

**Response (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "expiration": "2025-12-18T10:00:00Z",
  "role": "Admin",
  "userId": 1
}
```

### POST /api/Auth/register

Registers a new user (Generally internal or Admin).

---

## 📦 Order Management (OrdersController)

### POST /api/Orders

**[Admin]** Creates a new order.

**Request:**
```json
{
  "description": "Package X Delivery",
  "address": "123 Street",
  "latitude": 4.6097,
  "longitude": -74.0817,
  "requiresEvidence": true
}
```

### GET /api/Orders

**[Admin]** Gets all orders in the system.

### GET /api/Orders/my-route

**[Driver]** Gets orders assigned to the authenticated driver.

### POST /api/Orders/{id}/complete

**[Driver]** Completes an order by uploading photographic evidence.

- **Content-Type:** `multipart/form-data`
- **Form Data:**
  - `file`: (Image file)

### POST /api/Orders/my-route/optimize

**[Driver]** Requests optimization of the current route from the Java service.

---

## 🚚 Route Management (RoutesController)

### POST /api/Routes/save

Saves the current list of orders as a reusable route.

**Request:**
```json
{
  "routeName": "North Route - Monday",
  "orderIds": [101, 102, 105]
}
```

### GET /api/Routes/all

**[Admin]** Gets all saved routes (own and drivers').

### GET /api/Routes/saved

**[Driver]** Gets my saved routes.

### POST /api/Routes/saved/{routeId}/load

**[Driver]** Loads a saved route (reactivates orders for delivery).

### POST /api/Routes/saved/{routeId}/assign

**[Admin]** Assigns a route template to a specific driver.

**Request:**
```json
{
  "driverPhoneNumber": "+573001234567"
}
```

---

## 🧩 Main Data Models (DTOs)

### OrderDto

Main structure of an order in responses.
```json
{
  "id": 1,
  "description": "Express Delivery",
  "address": "123 Evergreen Terrace",
  "latitude": 4.123,
  "longitude": -74.123,
  "status": "Pending", 
  "driverId": 5,
  "evidenceUrl": "https://cloudinary..."
}
```

### SaveRouteDto

Structure for saving routes.
```json
{
  "routeName": "Route Name",
  "orderIds": [1, 2, 3],
  "driverId": 10 
}
```

---

## 🔗 Swagger

Complete interactive documentation is available at:  
`/swagger/index.html` (or `/swagger` depending on configuration).