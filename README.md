# UAVs Defense

## Advanced Multi-Modal UAV Detection, Identification & Tracking Platform

> **UAVs Defense** is an advanced real-time UAV detection and tracking platform focused on the **detection, localization, identification, and continuous tracking of unmanned aerial vehicles (UAVs)**.
>
> The platform combines **AI-powered computer vision, radar observations, Radio-Frequency (RF) sensing, multi-sensor data fusion, and 3D geospatial visualization** to transform heterogeneous sensor observations into a unified and continuously updated representation of aerial targets.
>
> Designed as a modular and extensible engineering platform, UAVs Defense separates **sensor acquisition, AI inference, observation processing, sensor fusion, target tracking, and visualization** into independent processing layers. This architecture enables new sensors, detection models, tracking algorithms, and data sources to be integrated without requiring fundamental changes to the overall system.

---

## Table of Contents

1. [Overview](#overview)
2. [Project Objectives](#project-objectives)
3. [System Architecture](#system-architecture)
4. [UAV Detection Pipeline](#uav-detection-pipeline)
5. [AI-Based UAV Detection](#ai-based-uav-detection)
6. [Radar & RF Observation](#radar--rf-observation)
7. [Multi-Sensor Data Fusion](#multi-sensor-data-fusion)
8. [Multi-Object Tracking](#multi-object-tracking)
9. [3D Geospatial Visualization](#3d-geospatial-visualization)
10. [Technology Stack](#technology-stack)
11. [Simulation & Testing](#simulation--testing)
12. [Performance Evaluation](#performance-evaluation)
13. [Development Roadmap](#development-roadmap)

---

# Overview

The increasing availability of small Unmanned Aerial Vehicles has created new challenges for real-time aerial monitoring and situational awareness.

Small UAVs can operate at different altitudes and distances, perform highly dynamic movements, become temporarily occluded, and appear under significantly different environmental conditions. Their visual, RF, and radar characteristics can also vary depending on the platform, payload, operating environment, and sensor configuration.

A single sensing modality therefore cannot provide complete information in every scenario.

**UAVs Defense addresses this challenge through a multi-modal detection architecture.**

Different sensing technologies contribute complementary information:

| Sensor             | Primary Information                                           |
| ------------------ | ------------------------------------------------------------- |
| **EO / IR Camera** | Visual detection and classification                           |
| **Radar**          | Range, bearing, elevation, position and kinematic information |
| **RF / SDR**       | Radio-frequency observations and signal characteristics       |
| **Fusion Layer**   | Correlation and unified target representation                 |

The platform combines these observations into a common processing pipeline:

```text
                         UAVs Defense
                              │
                 ┌────────────┴────────────┐
                 │                         │
            EO / IR Camera            Radar / RF
                 │                         │
                 ▼                         ▼
          AI-Based Detection        Sensor Observations
                 │                         │
                 └────────────┬────────────┘
                              ▼
                    Observation Processing
                              │
                              ▼
                     Data Association
                              │
                              ▼
                       Sensor Fusion
                              │
                              ▼
                     Multi-Object Tracking
                              │
                              ▼
                       Unified UAV Track
                              │
                              ▼
                    2D / 3D Visualization
```

The objective is not simply to detect an object in an individual sensor stream.

The system is designed to maintain a **consistent and continuously updated representation of aerial targets over time**, while preserving the origin and confidence of the underlying observations.

---

# Project Objectives

The primary objective of UAVs Defense is to build a reliable software platform for **real-time UAV perception and situational awareness**.

The project focuses on:

* Real-time UAV detection from camera streams
* AI-assisted UAV identification and classification
* Multi-object detection and tracking
* Radar observation integration
* RF observation integration
* Cross-sensor observation correlation
* Target state estimation
* Persistent track management
* Real-time 2D and 3D visualization
* Sensor and system monitoring
* Simulation-driven development
* Modular hardware and software integration

The architecture is designed to support both **standalone computer-vision detection** and progressively more advanced multi-sensor configurations.

---

# System Architecture

UAVs Defense is organized into independent processing layers.

```text
┌───────────────────────────────────────────────────────────────┐
│                       UAVs DEFENSE                            │
│              UAV Detection & Tracking Platform               │
├───────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    │
│  │   EO / IR    │    │    Radar     │    │   RF / SDR   │    │
│  │   Camera     │    │    Sensor    │    │    Sensor    │    │
│  └──────┬───────┘    └──────┬───────┘    └──────┬───────┘    │
│         │                   │                   │             │
│         ▼                   ▼                   ▼             │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │                 Sensor Acquisition                      │ │
│  └──────────────────────────┬──────────────────────────────┘ │
│                             │                                 │
│                             ▼                                 │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │             Detection & Observation Processing          │ │
│  └──────────────────────────┬──────────────────────────────┘ │
│                             │                                 │
│                             ▼                                 │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │                 Data Association & Fusion               │ │
│  └──────────────────────────┬──────────────────────────────┘ │
│                             │                                 │
│                             ▼                                 │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │                  Multi-Object Tracking                  │ │
│  └──────────────────────────┬──────────────────────────────┘ │
│                             │                                 │
│                             ▼                                 │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │                2D / 3D Operational View                 │ │
│  └─────────────────────────────────────────────────────────┘ │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

Each layer communicates through explicit interfaces and normalized data structures.

This separation provides several engineering advantages:

* Independent subsystem development
* Easier hardware integration
* Replaceable AI models
* Replaceable tracking algorithms
* Testable processing stages
* Clear data ownership
* Hardware-independent simulation
* Future distributed deployment

---

# UAV Detection Pipeline

The complete UAV detection pipeline follows a continuous perception workflow:

```text
Camera / Sensor Input
        │
        ▼
Data Acquisition
        │
        ▼
Preprocessing
        │
        ▼
AI / Sensor Detection
        │
        ▼
Observation Normalization
        │
        ▼
Data Association
        │
        ▼
Multi-Sensor Fusion
        │
        ▼
Target Tracking
        │
        ▼
Target State
        │
        ▼
2D / 3D Visualization
```

Each stage converts raw sensor information into progressively higher-level information.

For example:

```text
Raw Video
    ↓
Image Frame
    ↓
Object Detection
    ↓
UAV Observation
    ↓
Tracked Target
    ↓
Unified UAV Track
```

This layered approach allows the project to evolve from a standalone AI detector into a complete multi-sensor UAV perception platform.

---

# AI-Based UAV Detection

The computer-vision subsystem is responsible for detecting UAVs from EO/IR or other camera sources.

The project uses a **YOLO-family object detection architecture** as the primary AI detection approach.

Rather than assuming a single model variant is optimal, different model sizes and configurations can be evaluated against the project's UAV dataset and target hardware.

The selection process considers the trade-off between:

* Detection accuracy
* Precision
* Recall
* mAP
* Inference latency
* FPS
* GPU memory consumption
* Target size
* Detection distance
* Environmental conditions

The AI pipeline follows:

```text
Camera Stream
      │
      ▼
Frame Acquisition
      │
      ▼
Image Preprocessing
      │
      ▼
YOLO-Based Detection
      │
      ▼
Bounding Boxes
      │
      ├── Class
      ├── Confidence
      ├── Position
      └── Timestamp
      │
      ▼
Multi-Object Tracking
      │
      ▼
UAV Track
```

The detection model is intentionally isolated from the rest of the system.

This allows different YOLO variants, model versions, training configurations, and inference backends to be benchmarked without changing the surrounding platform.

---

# Radar & RF Observation

## Radar

Radar provides complementary information that is difficult to obtain directly from a conventional camera.

Depending on the connected radar system, observations may include:

* Range
* Bearing
* Elevation
* Position
* Velocity
* Heading
* Detection confidence
* Track information

Radar observations can therefore provide spatial and kinematic information that complements visual detection.

---

## RF / SDR

RF sensing provides an additional observation channel for aerial activity.

Depending on the sensing hardware and processing pipeline, RF observations may contain:

* Frequency
* Signal strength
* Timestamp
* Signal characteristics
* Direction information
* Classification information
* Detection confidence

The RF subsystem is treated as an independent sensor source and integrated into the common observation-processing pipeline.

---

# Multi-Sensor Data Fusion

The fusion subsystem correlates observations originating from different sensors.

For example:

```text
                 Radar Observation
                         │
                  Position / Velocity
                         │
                         ▼
                    ┌─────────┐
                    │         │
Camera Detection ──►│ Fusion  │◄── RF Observation
                    │ Engine  │
                    └────┬────┘
                         │
                         ▼
                  Unified UAV Track
```

The system can use multiple attributes when determining whether observations represent the same physical target, including:

* Spatial proximity
* Temporal consistency
* Velocity consistency
* Heading consistency
* Sensor confidence
* Predicted target state

State-estimation techniques such as **Kalman Filters or Extended Kalman Filters (EKF)** can be used to estimate target states from noisy and asynchronous measurements.

The output is a normalized target representation that can be consumed by the tracking and visualization layers.

---

# Multi-Object Tracking

Detection provides an instantaneous observation.

Tracking provides **temporal continuity**.

The tracking subsystem maintains persistent target identities across successive observations.

A target representation may contain:

```text
UAV Track
├── track_id
├── timestamp
├── position
│   ├── latitude
│   ├── longitude
│   └── altitude
├── velocity
├── heading
├── classification
├── confidence
├── first_seen
├── last_seen
└── sensor_sources
```

The tracker continuously performs:

```text
Prediction
    ↓
Observation Association
    ↓
State Update
    ↓
Confidence Update
    ↓
Track Maintenance
```

This enables the system to maintain stable UAV tracks even when individual sensor observations are intermittent.

---

# 3D Geospatial Visualization

The visualization layer provides a real-time spatial representation of UAV detections and their trajectories.

**CesiumJS** is used as the primary 3D geospatial visualization engine.

The operational view can display:

* UAV positions
* UAV trajectories
* Sensor locations
* Detection observations
* Target metadata
* Track history
* Detection confidence
* Sensor coverage
* Real-time target updates

A 3D environment provides a spatial context in which sensor observations and UAV tracks can be interpreted together.

The visualization layer is designed to consume normalized target and sensor data rather than being directly coupled to individual sensor implementations.

---

# Technology Stack

| Category                 | Technologies                        |
| ------------------------ | ----------------------------------- |
| **Frontend**             | React, TypeScript, Vite             |
| **3D Geospatial**        | CesiumJS, WebGL, 3D Tiles           |
| **Backend**              | Go, Echo                            |
| **AI / Computer Vision** | Python, PyTorch, YOLO-family models |
| **Systems Programming**  | C, C++                              |
| **Communication**        | REST, SSE / WebSocket, MQTT, gRPC   |
| **RF / SDR**             | SDR platforms, RF sensors           |
| **Radar**                | Radar sensor interfaces             |
| **Data Processing**      | Python, Go                          |
| **Storage**              | SQLite / PostgreSQL                 |
| **Deployment**           | Linux, Docker                       |
| **Simulation**           | ROS 2, Gazebo, SITL                 |

---

# Simulation & Testing

Simulation is used to validate detection, tracking, and fusion components before integration with physical sensors.

The testing environment can reproduce scenarios including:

* Multiple UAV targets
* Different flight trajectories
* Sensor noise
* Intermittent observations
* Multiple simultaneous targets
* Sensor latency
* Target appearance and disappearance
* Cross-sensor observations

The development workflow follows:

```text
Simulation
    ↓
Algorithm Validation
    ↓
Software Integration
    ↓
Hardware Integration
    ↓
Controlled Testing
```

Simulation provides repeatable scenarios for evaluating changes to detection, tracking, and fusion algorithms.

---

# Performance Evaluation

UAV detection performance is evaluated across multiple dimensions.

| Category         | Metrics                            |
| ---------------- | ---------------------------------- |
| **Detection**    | Precision, Recall, mAP50, mAP50-95 |
| **Inference**    | FPS, inference latency             |
| **End-to-End**   | Sensor-to-display latency          |
| **Tracking**     | Track continuity, ID consistency   |
| **Localization** | Position estimation error          |
| **Fusion**       | Observation association accuracy   |
| **System**       | Throughput, CPU/GPU utilization    |
| **Scalability**  | Concurrent sensors and UAV tracks  |

Model performance will be benchmarked using a dedicated UAV dataset under representative operating conditions.

The final AI model will be selected based on the measured balance between **detection quality, inference performance, and deployment constraints**, rather than solely selecting the largest or newest model.

---

# Development Roadmap

```text
Phase 1
UAV Dataset & AI Detection
        │
        ▼
Phase 2
Real-Time YOLO Inference
        │
        ▼
Phase 3
Multi-Object Tracking
        │
        ▼
Phase 4
3D UAV Visualization
        │
        ▼
Phase 5
Radar Integration
        │
        ▼
Phase 6
RF / SDR Integration
        │
        ▼
Phase 7
Multi-Sensor Fusion
        │
        ▼
Phase 8
Distributed Detection Architecture
```

The platform is designed to evolve incrementally, beginning with **AI-based UAV detection** and progressively incorporating additional sensing modalities and tracking capabilities.

---

# Project Vision

UAVs Defense is designed as more than a standalone object-detection application.

The project explores how **AI perception, real-time systems, sensor integration, data fusion, tracking algorithms, and 3D geospatial computing** can be combined into a unified UAV detection platform.

The core engineering pipeline is:

> **Detect → Localize → Identify → Fuse → Track → Visualize**

By separating perception, sensor integration, fusion, tracking, and visualization into independent layers, the platform provides a scalable foundation for developing increasingly capable **real-time UAV detection and situational-awareness systems**.

The ultimate goal is to build a system in which heterogeneous sensor data can be transformed into a **reliable, continuously updated, and spatially contextualized representation of UAV activity**.
