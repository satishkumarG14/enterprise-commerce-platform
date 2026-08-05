# Enterprise Commerce Platform

A production-inspired, cloud-native e-commerce platform built using Java 21, Spring Boot, Spring Cloud, React, Docker, Kubernetes, Kafka, and modern DevOps practices.

This project is designed as a portfolio showcasing enterprise architecture, microservices, security, event-driven communication, observability, and cloud-native deployment.

---

## Architecture

```
                    +----------------+
                    | React Frontend |
                    +-------+--------+
                            |
                            |
                    +-------v--------+
                    | API Gateway    |
                    +-------+--------+
                            |
        +-------------------+-------------------+
        |                   |                   |
        |                   |                   |
+-------v------+    +--------v-------+   +------v-------+
| Auth Service |    | User Service   |   | Product Svc  |
+--------------+    +----------------+   +--------------+
        |                   |                  |
        +-------------------+------------------+
                            |
                    Apache Kafka Events
                            |
        +-----------+--------+--------+----------+
        |           |                 |          |
+-------v------+ +--v-----------+ +---v-----+ +--v------------+
| Inventory    | | Order        | | Payment | | Notification  |
+--------------+ +--------------+ +---------+ +---------------+
                            |
                     Reporting Service
```

---

## Technology Stack

### Backend

- Java 21
- Spring Boot 3.x
- Spring Cloud
- Spring Security
- Spring Data JPA
- Hibernate
- Kafka
- Maven

### Database

- PostgreSQL
- Redis

### Frontend

- React
- TypeScript
- Material UI

### DevOps

- Docker
- Kubernetes
- GitHub Actions
- SonarQube
- Prometheus
- Grafana
- Zipkin

---

## Microservices

- API Gateway
- Authentication Service
- User Service
- Product Service
- Inventory Service
- Order Service
- Payment Service
- Notification Service
- Reporting Service

---

## Features

- JWT Authentication
- Role Based Authorization
- API Gateway
- Service Discovery
- Event Driven Architecture
- Distributed Tracing
- Circuit Breaker
- Centralized Logging
- Health Monitoring
- OpenAPI Documentation
- Docker Deployment
- Kubernetes Deployment

---

## Project Status

Current Phase:

Sprint 0 – Foundation

- ✅ Repository Setup
- ✅ Parent Maven Project
- ✅ Common Module
- ✅ Gateway Service

Upcoming

- Authentication Service
- User Service
- Product Service
- Kafka Integration
- Docker
- Kubernetes
- CI/CD
- Monitoring

---

## Build

```bash
cd backend
mvn clean install
```

Run Gateway

```bash
cd gateway
mvn spring-boot:run
```

---

## License

MIT License