# UAVs Defense

## Advanced AI-Based UAV Detection, Localization & Tracking System

> **UAVs Defense** is an AI-powered real-time UAV detection system developed to detect, identify, localize, and track unmanned aerial vehicles from sensor observations.
>
> The system focuses on combining **deep-learning-based computer vision, multi-object tracking, spatial localization, and multi-sensor observations** to build a reliable and continuously updated representation of UAV activity.
>
> The primary detection pipeline is built around a **YOLO-family object detection model**, with model variants evaluated against the project dataset and target computing environment to determine the optimal balance between detection accuracy and real-time performance.
>
> The project is designed as an extensible research and engineering platform, allowing additional sensing technologies such as **radar and RF/SDR** to be incorporated into the detection and tracking pipeline.

---

## Table of Contents

1. [Overview](#overview)
2. [Problem Statement](#problem-statement)
3. [Project Objectives](#project-objectives)
4. [System Architecture](#system-architecture)
5. [UAV Detection Pipeline](#uav-detection-pipeline)
6. [AI-Based UAV Detection](#ai-based-uav-detection)
7. [Multi-Object Tracking](#multi-object-tracking)
8. [UAV Localization](#uav-localization)
9. [Multi-Sensor Integration](#multi-sensor-integration)
10. [Dataset & Model Training](#dataset--model-training)
11. [Performance Evaluation](#performance-evaluation)
12. [Hardware & Software Requirements](#hardware--software-requirements)
13. [Simulation & Testing](#simulation--testing)
14. [Project Structure](#project-structure)
15. [Development Roadmap](#development-roadmap)
16. [Research Direction](#research-direction)

---

# Overview

The rapid growth of small Unmanned Aerial Vehicles (UAVs) has created increasingly demanding requirements for reliable aerial-object detection and tracking.

Detecting small UAVs in real-world environments is challenging because their appearance can vary significantly depending on:

* Distance from the sensor
* Target size and orientation
* Lighting conditions
* Weather and atmospheric conditions
* Background complexity
* Occlusion
* Motion blur
* Camera characteristics
* UAV platform and configuration

A conventional image-processing approach may struggle to maintain reliable detection across these conditions.

**UAVs Defense approaches the problem using modern deep-learning-based computer vision combined with temporal tracking and spatial localization.**

The core concept is:

```text
                    UAV / Drone
                         │
                         ▼
                  Camera / Sensor
                         │
                         ▼
                Image Preprocessing
                         │
                         ▼
              YOLO-Based Detection
                         │
                  ┌──────┴──────┐
                  │             │
             Bounding Box    Confidence
                  │             │
                  └──────┬──────┘
                         ▼
                Multi-Object Tracking
                         │
                         ▼
                  UAV Localization
                         │
                         ▼
                    Target State
                         │
                         ▼
                  UAV Track Output
```

The system transforms raw sensor data into structured information that can be analyzed, tracked, and visualized in real time.

---

# Problem Statement

Small UAV detection presents several computer-vision and tracking challenges.

Unlike large aircraft, small UAVs may occupy only a small number of pixels when observed from long distances. Their appearance can also change substantially as the viewing angle changes.

A robust detection system must therefore handle:

### Small Objects

Drones can occupy a very small region of an image, making feature extraction and classification difficult.

### Dynamic Targets

UAVs can change velocity, heading, altitude, and direction rapidly.

### Complex Backgrounds

Buildings, trees, birds, vehicles, cables, and other environmental elements can generate visually similar patterns.

### Environmental Variation

Lighting, haze, rain, shadows, low-light conditions, and image quality can significantly affect detection performance.

### Continuous Tracking

Detecting a UAV in one frame is insufficient for a real-time monitoring system. The system must maintain target identity across successive frames.

Therefore, the project focuses on the complete perception pipeline:

> **Detection → Identification → Tracking → Localization**

---

# Project Objectives

The primary objectives of UAVs Defense are:

* Develop a real-time UAV detection system
* Train and evaluate a deep-learning object detector
* Detect multiple UAVs simultaneously
* Estimate detection confidence and location within the image
* Maintain persistent UAV identities across video frames
* Estimate UAV spatial position where sufficient sensor information is available
* Evaluate detection accuracy under different environmental conditions
* Optimize inference performance for real-time operation
* Establish an architecture suitable for future radar and RF/SDR integration

The project emphasizes **measurable performance and reproducible experimentation** rather than relying solely on visual demonstrations.

---

# System Architecture

The system is organized into independent perception stages:

```text
┌───────────────────────────────────────────────────────────────┐
│                       UAVs Defense                            │
│             UAV Detection & Tracking Pipeline                │
├───────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐                                             │
│  │ Camera /     │                                             │
│  │ Sensor Input │                                             │
│  └──────┬───────┘                                             │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                             │
│  │ Preprocessing│                                             │
│  └──────┬───────┘                                             │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                             │
│  │ YOLO-Based   │                                             │
│  │ Detection    │                                             │
│  └──────┬───────┘                                             │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                             │
│  │ Observation  │                                             │
│  │ Generation   │                                             │
│  └──────┬───────┘                                             │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                             │
│  │ Multi-Object │                                             │
│  │ Tracking     │                                             │
│  └──────┬───────┘                                             │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                             │
│  │ Localization │                                             │
│  └──────┬───────┘                                             │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                             │
│  │ UAV Target   │                                             │
│  │ Representation│                                            │
│  └──────────────┘                                             │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

Each stage has a clearly defined responsibility, allowing individual algorithms to be independently evaluated and improved.

---

# UAV Detection Pipeline

The real-time processing pipeline follows:

```text
Sensor Input
     │
     ▼
Frame Acquisition
     │
     ▼
Preprocessing
     │
     ▼
AI Inference
     │
     ▼
UAV Detections
     │
     ├── Bounding Box
     ├── Class
     ├── Confidence
     └── Timestamp
     │
     ▼
Multi-Object Tracking
     │
     ▼
Target State Estimation
     │
     ▼
UAV Localization
     │
     ▼
Tracked UAV
```

A detection represents an observation at a specific moment.

A track represents the system's continuously updated estimate of a UAV over time.

This distinction is important for maintaining stable target identities and handling temporary detection loss.

---

# AI-Based UAV Detection

The primary computer-vision detector uses a **YOLO-family architecture**.

YOLO is well suited to this project because it provides a practical balance between object-detection accuracy and inference speed.

Instead of selecting a model purely based on its generation or size, the project evaluates candidate model configurations using the actual UAV dataset and deployment hardware.

Potential model variants include different model sizes optimized for:

* Maximum inference speed
* Balanced accuracy and performance
* Higher detection accuracy
* Resource-constrained deployment

The final model is selected based on experimental results.

## Detection Output

Each inference produces structured detection information:

```text
Detection
├── class
├── confidence
├── bounding_box
│   ├── x
│   ├── y
│   ├── width
│   └── height
└── timestamp
```

This output is passed to the tracking subsystem.

---

# Multi-Object Tracking

Object detection provides frame-level observations.

Multi-object tracking provides **temporal continuity**.

The tracking subsystem associates detections across consecutive frames and assigns persistent identifiers to individual UAVs.

Conceptually:

```text
Frame N
  │
  ├── UAV A
  ├── UAV B
  └── UAV C
       │
       ▼
   Association
       │
       ▼
Frame N+1
  │
  ├── Track 01
  ├── Track 02
  └── Track 03
```

A tracked target may contain:

```text
UAV Track
├── track_id
├── class
├── confidence
├── bounding_box
├── position
├── velocity
├── heading
├── first_seen
├── last_seen
└── tracking_state
```

Tracking algorithms can use information such as:

* Bounding-box position
* Object appearance
* Motion
* Velocity
* Temporal consistency
* Detection confidence

The tracking subsystem is designed to handle temporary detection loss and maintain target identity whenever sufficient information is available.

---

# UAV Localization

Detection provides an image-space position.

Localization attempts to transform this information into a meaningful spatial representation.

Depending on the available sensor configuration, localization can incorporate:

* Camera calibration
* Camera orientation
* Camera position
* Field of view
* Target image coordinates
* Altitude information
* Radar measurements
* Sensor geometry

The conceptual pipeline is:

```text
Image Coordinates
       │
       ▼
Camera Geometry
       │
       ▼
Sensor Pose
       │
       ▼
Spatial Estimation
       │
       ▼
UAV Position
```

When additional sensors such as radar become available, their measurements can provide complementary spatial information and improve the overall target-state estimation process.

---

# Multi-Sensor Integration

The initial system is centered around AI-based visual detection.

The architecture is intentionally designed to support additional observation sources.

Future sensor inputs may include:

```text
             ┌─────────────┐
             │ EO / IR     │
             │ Camera      │
             └──────┬──────┘
                    │
                    ▼
             Visual Detection
                    │
                    │
      ┌─────────────┴─────────────┐
      │                           │
      ▼                           ▼
   Radar                        RF / SDR
 Observation                  Observation
      │                           │
      └─────────────┬─────────────┘
                    ▼
             Data Association
                    │
                    ▼
              Sensor Fusion
                    │
                    ▼
              Unified UAV
                 Track
```

The multi-sensor architecture allows each sensing modality to contribute its own strengths while maintaining a common target representation.

Potential future fusion techniques include:

* Kalman Filtering
* Extended Kalman Filtering (EKF)
* Measurement association
* Track-to-track correlation
* Confidence fusion
* State estimation

---

# Dataset & Model Training

The UAV detector is trained and evaluated using a dedicated UAV dataset.

Dataset preparation focuses on representative conditions including:

* Different UAV types
* Different viewing angles
* Different distances
* Different backgrounds
* Different lighting conditions
* Different UAV scales
* Multiple UAVs in a single frame
* Partial occlusion

The dataset is divided into independent subsets for:

```text
Dataset
├── Training Set
├── Validation Set
└── Test Set
```

The test set remains isolated from model training and hyperparameter optimization to provide an unbiased evaluation of the final model.

---

# Performance Evaluation

The system is evaluated using both **AI accuracy metrics** and **real-time system metrics**.

## Detection Metrics

| Metric        | Purpose                                                |
| ------------- | ------------------------------------------------------ |
| **Precision** | Measures how many predicted UAV detections are correct |
| **Recall**    | Measures how many actual UAVs are detected             |
| **mAP@50**    | Detection accuracy using IoU threshold of 0.50         |
| **mAP@50-95** | Detection performance across multiple IoU thresholds   |

## Runtime Metrics

| Metric                 | Purpose                                             |
| ---------------------- | --------------------------------------------------- |
| **FPS**                | Real-time processing capability                     |
| **Inference Latency**  | Time required for model inference                   |
| **End-to-End Latency** | Time from frame acquisition to final tracked output |
| **GPU Memory**         | Model resource requirements                         |
| **CPU Utilization**    | Overall processing overhead                         |

## Tracking Metrics

| Metric                   | Purpose                                               |
| ------------------------ | ----------------------------------------------------- |
| **Track Continuity**     | Ability to maintain a target across frames            |
| **Identity Consistency** | Ability to prevent unnecessary track identity changes |
| **Track Loss Rate**      | Frequency of lost target tracks                       |
| **Localization Error**   | Difference between estimated and reference position   |

The final model and configuration are selected based on the measured trade-off between **accuracy, latency, resource consumption, and tracking stability**.

---

# Hardware & Software Requirements

Actual requirements depend on the selected model and deployment target.

A development configuration may include:

### Hardware

* x86_64 CPU
* NVIDIA GPU for accelerated training/inference
* EO/IR or RGB camera
* Optional radar sensor
* Optional SDR / RF sensing hardware

### Software

* Ubuntu Linux
* Python 3.x
* PyTorch
* CUDA
* OpenCV
* YOLO-family framework
* NumPy
* Scientific computing and visualization libraries

Additional dependencies are documented in the project environment configuration.

---

# Simulation & Testing

Simulation and recorded datasets are used to create repeatable testing scenarios before physical deployment.

Testing scenarios may include:

* Single UAV detection
* Multiple UAV detection
* Fast-moving UAVs
* Small/distant UAVs
* Partial occlusion
* Complex backgrounds
* Low-light conditions
* Temporary detection loss
* Multiple simultaneous tracks

The validation workflow follows:

```text
Dataset / Simulation
        │
        ▼
AI Model Evaluation
        │
        ▼
Tracking Evaluation
        │
        ▼
Localization Evaluation
        │
        ▼
Real-Time Pipeline
        │
        ▼
Controlled Hardware Testing
```

---

# Project Structure

The project is organized around the detection pipeline rather than a conventional web-application architecture.

A representative structure is:

```text
Ongoing
```

The actual repository structure may evolve as the implementation develops.

---

# Development Roadmap
Plan: v1.0.0, can be change
```text
Phase 1
Dataset Preparation
        │
        ▼
Phase 2
YOLO Model Training & Benchmarking
        │
        ▼
Phase 3
Real-Time UAV Detection
        │
        ▼
Phase 4
Multi-Object Tracking
        │
        ▼
Phase 5
UAV Localization
        │
        ▼
Phase 6
3D Visualization
        │
        ▼
Phase 7
Radar Integration
        │
        ▼
Phase 8
RF / SDR Integration
        │
        ▼
Phase 9
Multi-Sensor Fusion
```

The development process prioritizes establishing a reliable visual detection and tracking pipeline before introducing additional sensing modalities.

---

# Research Direction

The project explores the intersection of:

* **Artificial Intelligence**
* **Computer Vision**
* **Object Detection**
* **Multi-Object Tracking**
* **Geometric Localization**
* **Sensor Fusion**
* **Real-Time Computing**
* **Embedded and Edge Computing**

The central research question is:

> **How can modern AI-based computer vision and complementary sensor observations be combined to achieve accurate, robust, and real-time UAV detection and tracking under varying operational conditions?**

The project therefore goes beyond training an object-detection model.

It investigates the complete path from **raw sensor data to a persistent, spatially meaningful UAV representation**.

---

# Core Pipeline

```text
┌──────────────────────────────────────────────────────────┐
│                    UAVs DEFENSE                          │
│                                                          │
│   SENSE                                                  │
│     │                                                    │
│     ▼                                                    │
│   DETECT ──────────── YOLO / AI                         │
│     │                                                    │
│     ▼                                                    │
│   IDENTIFY                                               │
│     │                                                    │
│     ▼                                                    │
│   TRACK ───────────── Multi-Object Tracking              │
│     │                                                    │
│     ▼                                                    │
│   LOCALIZE                                               │
│     │                                                    │
│     ▼                                                    │
│   FUSE ────────────── Radar / RF / Other Sensors         │
│     │                                                    │
│     ▼                                                    │
│   VISUALIZE                                              │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

> **UAVs Defense — From AI-powered detection to real-time aerial situational awareness.**
