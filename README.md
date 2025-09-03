# 🌐 Gateway Service (Doctor Appointment System)

The **Gateway Service** acts as the single entry point for the Doctor Appointment System.  
It uses **Spring Cloud Gateway** for intelligent routing, load balancing, and security.

------------------------------------------------------------------------

## 📌 Responsibilities

- Routes external client requests to the appropriate microservice.
- Provides load balancing via Eureka service discovery (`lb://` URIs).
- Acts as an API Gateway for all services in the system.
- Centralized point for authentication, authorization, and request filtering.
- Simplifies client access (users only call the gateway, not each microservice directly).

------------------------------------------------------------------------

## 🏗️ Architecture

    Client --> Gateway Service --> Eureka Discovery --> Target Microservice
        - /users/**  --> User Service (lb://user-service)
        - /appointments/**  --> Appointment Service (lb://appointment-service)

------------------------------------------------------------------------

## ⚙️ Configuration Example

application.properties:

    spring.application.name=gateway-service
    server.port=8080

    eureka.client.service-url.defaultZone=http://localhost:8761/eureka
    eureka.instance.prefer-ip-address=true

    # User Service Route
    spring.cloud.gateway.routes[0].id=user-service
    spring.cloud.gateway.routes[0].uri=lb://user-service
    spring.cloud.gateway.routes[0].predicates[0]=Path=/users/**

    # Appointment Service Route
    spring.cloud.gateway.routes[1].id=appointment-service
    spring.cloud.gateway.routes[1].uri=lb://appointment-service
    spring.cloud.gateway.routes[1].predicates[0]=Path=/appointments/**

------------------------------------------------------------------------

## 🚀 Usage

- Start **Eureka Server** first.
- Run **Gateway Service** (it will register with Eureka).
- Run **User Service** and **Appointment Service**.
- Access APIs through the gateway:

    - `http://localhost:8080/users/register`
    - `http://localhost:8080/users/login`
    - `http://localhost:8080/appointments/book`

------------------------------------------------------------------------

## 📜 Notes

- The gateway hides internal service URLs and exposes only unified endpoints.
- If new microservices are added, configure additional routes in the gateway.
- Security filters (like JWT validation) can be applied at the gateway level for consistency.
