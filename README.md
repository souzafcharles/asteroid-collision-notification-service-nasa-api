![GitHub language count](https://img.shields.io/github/languages/count/souzafcharles/asteroid-collision-notification-service-nasa-api)
![GitHub top language](https://img.shields.io/github/languages/top/souzafcharles/asteroid-collision-notification-service-nasa-api)
![GitHub](https://img.shields.io/github/license/souzafcharles/asteroid-collision-notification-service-nasa-api)
![GitHub last commit](https://img.shields.io/github/last-commit/souzafcharles/asteroid-collision-notification-service-nasa-api)

# Asteroid Collision Notification Service NASA API

## 1. Architecture & Event Flow
<p align="justify"> The system architecture integrates synchronous data retrieval from NASA's NeoWs API with an asynchronous event-driven workflow for asteroid alert processing. The design ensures scalability, fault isolation, and reliable delivery through Kafka as the event backbone. The Notification Service persists processed events in MySQL and dispatches email alerts, maintaining clear separation of responsibilities across components. </p>

```
┌──────────────────────────────────────────────────────────────┐
│                      NASA Open APIs                          │
│   Asteroids – NeoWs (Near Earth Object Web Service)          │
└──────────────────────────────────────────────────────────────┘
                             ↑
                             │ 1. Synchronous
                             │
┌──────────────────────────────────────────────────────────────┐
│                Asteroids Alerting Service                    │
└──────────────────────────────────────────────────────────────┘
                             │
                             │ 2. Asynchronous
                             ↓
                   ┌──────────────────┐
                   │       Kafka      │
                   └──────────────────┘
                             │
                             │ 3. Event Consumption
                             ↓
┌──────────────────────────────────────────────────────────────┐
│                     Notification Service                     │
└──────────────────────────────────────────────────────────────┘
              ↑                                 │
              │                                 │ Sends Email
              │                                 ↓
      ┌──────────────────┐              ┌──────────────────┐
      │    PostgreSQL    │              │       Email      │
      └──────────────────┘              └──────────────────┘


```

<p align="justify"> The event flow begins with a synchronous request to NASA's NeoWs API to retrieve near-Earth object information. Once processed, potential asteroid threats are published asynchronously to Kafka, allowing high-throughput and decoupled event propagation. The Notification Service consumes these messages, stores alert data in MySQL, and triggers outbound email notifications to end users. </p>

