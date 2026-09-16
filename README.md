# UAVs Defense

## AI-Based UAV Detection, Localization & Tracking System

> **UAVs Defense** is an AI-based UAV detection and tracking research project focused on the real-time detection, identification, localization, and continuous tracking of Unmanned Aerial Vehicles (UAVs).
>
> The current implementation focuses on **deep-learning-based computer vision and multi-object tracking**, using publicly available UAV datasets, recorded video, and software-based simulation environments as the primary sources of sensor data.
>
> The system is designed with an extensible architecture that can later incorporate additional sensing modalities such as **radar, RF/SDR, and EO/IR sensors** when physical hardware becomes available.

---

## Table of Contents

1. [Overview](#overview)
2. [Project Objectives](#project-objectives)
3. [Current Scope](#current-scope)
4. [System Architecture](#system-architecture)
5. [UAV Detection Pipeline](#uav-detection-pipeline)
6. [AI-Based UAV Detection](#ai-based-uav-detection)
7. [Multi-Object Tracking](#multi-object-tracking)
8. [UAV Localization](#uav-localization)
9. [Simulation & Data Sources](#simulation--data-sources)
10. [Future Multi-Sensor Integration](#future-multi-sensor-integration)
11. [Dataset & Model Training](#dataset--model-training)
12. [Performance Evaluation](#performance-evaluation)
13. [Software Requirements](#software-requirements)
14. [Project Structure](#project-structure)
15. [Development Roadmap](#development-roadmap)
16. [Research Direction](#research-direction)

---

# Overview

Small Unmanned Aerial Vehicles present a challenging computer-vision problem.

A UAV may occupy only a small number of pixels when observed at long distances, while changes in viewing angle, lighting, background, motion, and environmental conditions can significantly affect its appearance.

A reliable UAV detection system must therefore address several problems simultaneously:

* Small-object detection
* Real-time inference
* Multiple simultaneous targets
* Target identity preservation
* Temporary detection loss
* Motion and trajectory estimation
* Environmental variation
* Spatial localization

**UAVs Defense focuses on solving these problems through an AI-driven perception pipeline.**

The current system uses software-accessible data sources rather than requiring dedicated physical sensing hardware.

```text
              Public Dataset / Video
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
                UAV Detections
                       │
                       ▼
              Multi-Object Tracking
                       │
                       ▼
               Target Estimation
                       │
                       ▼
               UAV Localization
                       │
                       ▼
                UAV Track Output
```

The architecture is designed so that simulated and recorded sensor data can eventually be replaced or supplemented by physical sensor interfaces.

---

# Project Objectives

The project focuses on developing a complete UAV perception pipeline capable of:

* Detecting UAVs from video and image data
* Identifying UAV objects using deep-learning models
* Detecting multiple UAVs simultaneously
* Estimating detection confidence
* Maintaining persistent target identities
* Tracking UAV movement across frames
* Estimating target position and motion
* Evaluating detection performance quantitatively
* Optimizing AI inference for real-time operation
* Supporting repeatable software-based experiments
* Providing an architecture suitable for future sensor integration

The primary focus is **UAV perception**, rather than UAV mitigation or interception.

---

# Current Scope

The current project is intentionally focused on software-based development.

### Currently Implemented / Evaluated

* UAV image and video processing
* YOLO-based object detection
* Dataset preparation
* Model training and evaluation
* Multi-object tracking
* Detection confidence analysis
* Video-based target tracking
* Software simulation
* Performance benchmarking

### Current Data Sources

The project currently relies on:

* Publicly available UAV datasets
* Public UAV videos
* Recorded video data
* Synthetic or simulated data
* Software-generated sensor observations

This approach allows the detection and tracking algorithms to be developed and evaluated without requiring dedicated physical UAV-detection hardware.

### Future Hardware Integration

The architecture is prepared for future integration with:

* EO/IR cameras
* Radar systems
* SDR / RF sensors
* Other aerial sensing systems

These components are **future integration targets and are not assumed to be physically available in the current implementation**.

---

# System Architecture

The current architecture focuses on the computer-vision perception pipeline.

```text
┌─────────────────────────────────────────────────────────────┐
│                       UAVs DEFENSE                          │
│                                                             │
│              AI-Based UAV Detection System                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────────────┐                                      │
│  │ Dataset / Video   │                                      │
│  │ / Simulation      │                                      │
│  └─────────┬─────────┘                                      │
│            │                                                │
│            ▼                                                │
│  ┌───────────────────┐                                      │
│  │ Frame Acquisition │                                      │
│  └─────────┬─────────┘                                      │
│            │                                                │
│            ▼                                                │
│  ┌───────────────────┐                                      │
│  │ Image             │                                      │
│  │ Preprocessing     │                                      │
│  └─────────┬─────────┘                                      │
│            │                                                │
│            ▼                                                │
│  ┌───────────────────┐                                      │
│  │ YOLO-Based        │                                      │
│  │ UAV Detection     │                                      │
│  └─────────┬─────────┘                                      │
│            │                                                │
│            ▼                                                │
│  ┌───────────────────┐                                      │
│  │ Multi-Object      │                                      │
│  │ Tracking          │                                      │
│  └─────────┬─────────┘                                      │
│            │                                                │
│            ▼                                                │
│  ┌───────────────────┐                                      │
│  │ Target State      │                                      │
│  │ Estimation        │                                      │
│  └─────────┬─────────┘                                      │
│            │                                                │
│            ▼                                                │
│  ┌───────────────────┐                                      │
│  │ UAV Localization  │                                      │
│  └─────────┬─────────┘                                      │
│            │                                                │
│            ▼                                                │
│       UAV Track Output                                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

The architecture separates detection from tracking and localization so that each stage can be independently evaluated.

---

# UAV Detection Pipeline

The core processing pipeline is:

```text
Input Video / Dataset
        │
        ▼
Frame Extraction
        │
        ▼
Image Preprocessing
        │
        ▼
YOLO Inference
        │
        ▼
UAV Detection
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
Target State
        │
        ├── Position
        ├── Velocity
        └── Track Identity
        │
        ▼
Localization / Visualization
```

The pipeline is designed to operate both on individual images and continuous video streams.

---

# AI-Based UAV Detection

The primary detection approach uses a **YOLO-family object detection architecture**.

YOLO is selected because it provides a practical foundation for real-time object detection while supporting different model sizes and deployment configurations.

The project does not assume that the newest or largest model is automatically the best model.

Instead, candidate models are evaluated using:

* Precision
* Recall
* mAP
* Inference latency
* FPS
* GPU memory usage
* Detection performance on small UAVs
* Performance under different visual conditions

The final model is selected based on experimental results and deployment constraints.

## Detection Output

A single inference produces structured UAV observations:

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

These observations are then passed to the tracking subsystem.

---

# Multi-Object Tracking

Object detection provides information about a UAV at a specific frame.

Tracking provides **temporal continuity across frames**.

The tracking subsystem associates detections over time and assigns persistent identities to individual targets.

```text
Frame N
 ├── UAV
 ├── UAV
 └── UAV
      │
      ▼
 Detection Association
      │
      ▼
Frame N + 1
 ├── Track 01
 ├── Track 02
 └── Track 03
```

A tracked UAV may contain:

```text
UAV Track
├── track_id
├── class
├── confidence
├── bounding_box
├── position
├── velocity
├── first_seen
├── last_seen
└── tracking_state
```

Tracking enables the system to:

* Maintain target identity
* Estimate target movement
* Handle temporary detection loss
* Reduce unnecessary identity changes
* Build target trajectories
* Provide continuous target state updates

---

# UAV Localization

The initial computer-vision system primarily operates in **image coordinates**.

A detected UAV can be represented using:

```text
Image Space
├── Center X
├── Center Y
├── Width
└── Height
```

To estimate physical-world coordinates, additional information is required.

Potential localization inputs include:

* Camera intrinsic parameters
* Camera position
* Camera orientation
* Field of view
* Target altitude
* Ground reference
* External sensor measurements

The conceptual transformation is:

```text
Image Coordinates
       │
       ▼
Camera Calibration
       │
       ▼
Camera Pose
       │
       ▼
Geometric Estimation
       │
       ▼
World Coordinates
```

In the current software-based implementation, localization can be evaluated using simulated or known reference information.

More advanced geographic localization can be introduced when appropriate sensor and calibration data become available.

---

# Simulation & Data Sources

Because dedicated UAV-detection hardware is not currently available, the project uses software-accessible data sources to reproduce realistic detection scenarios.

These include:

### Public UAV Datasets

Used for:

* Model training
* Validation
* Benchmarking
* Error analysis

### Public UAV Videos

Used for:

* Real-time inference experiments
* Multi-object tracking
* Long-duration tracking
* Detection robustness testing

### Synthetic Data

Used to generate controlled scenarios where target:

* Position
* Velocity
* Trajectory
* Appearance
* Number of objects

can be controlled.

### Software Simulation

Simulation provides a repeatable environment for evaluating the complete perception pipeline before physical hardware integration.

---

# Future Multi-Sensor Integration

The current project is centered on computer vision.

The architecture is intentionally designed to support future sensor integration.

```text
                       UAV
                        │
           ┌────────────┼────────────┐
           │            │            │
           ▼            ▼            ▼
        EO / IR       Radar       RF / SDR
        Camera        Sensor       Sensor
           │            │            │
           ▼            ▼            ▼
       Visual       Radar         RF
     Observation  Observation  Observation
           │            │            │
           └────────────┼────────────┘
                        ▼
                Data Association
                        │
                        ▼
                 Sensor Fusion
                        │
                        ▼
                  UAV Track
```

These sensors are part of the **future architecture**, not requirements for the current project.

Potential future research areas include:

* Radar-camera fusion
* RF-camera correlation
* Track-to-track association
* Extended Kalman Filtering
* Sensor confidence fusion
* Multi-modal target classification
* Improved spatial localization

---

# Dataset & Model Training

The AI model is trained using a dedicated UAV dataset.

Dataset preparation includes:

* Image collection
* Annotation
* Data cleaning
* Class definition
* Dataset balancing
* Train/validation/test splitting
* Data augmentation

A typical dataset structure is:

```text
Dataset
├── train/
├── val/
└── test/
```

The test set is kept separate from model training and optimization to provide an unbiased evaluation of the final detector.

---

# Performance Evaluation

The project evaluates both **AI detection quality** and **real-time performance**.

## Detection Metrics

| Metric        | Description                                             |
| ------------- | ------------------------------------------------------- |
| **Precision** | Percentage of predicted UAV detections that are correct |
| **Recall**    | Percentage of actual UAVs successfully detected         |
| **mAP@50**    | Detection performance at IoU threshold 0.50             |
| **mAP@50-95** | Detection performance across multiple IoU thresholds    |

## Runtime Metrics

| Metric                 | Description                                   |
| ---------------------- | --------------------------------------------- |
| **FPS**                | Frames processed per second                   |
| **Inference Latency**  | Time required for AI inference                |
| **End-to-End Latency** | Time from input frame to final tracked output |
| **GPU Memory**         | Memory required by the model                  |
| **CPU Utilization**    | Processing overhead                           |

## Tracking Metrics

| Metric                   | Description                                         |
| ------------------------ | --------------------------------------------------- |
| **Track Continuity**     | Ability to maintain a target over time              |
| **Identity Consistency** | Stability of target identities                      |
| **Track Loss Rate**      | Frequency of lost tracks                            |
| **Position Error**       | Difference between estimated and reference position |

The final model is selected based on the measured trade-off between **accuracy, inference speed, resource consumption, and tracking stability**.

---

# Software Requirements

The current project is primarily software-based.

A typical development environment includes:

### Operating System

* Ubuntu Linux
* Windows + WSL

### AI / Computer Vision

* Python 3.x
* PyTorch
* OpenCV
* YOLO-family framework
* NumPy

### Optional Simulation

* ROS 2
* Gazebo
* SITL
* Synthetic data generation tools

### Hardware Acceleration

GPU acceleration is recommended for model training and real-time inference but is not strictly required for development or experimentation.

---

# Project Structure

The project is organized around the UAV perception pipeline:

```text
update later
```

The structure can evolve as additional research components are introduced.

---

# Development Roadmap

```text
Phase 1
Public Dataset Collection
        │
        ▼
Phase 2
Dataset Preparation & Annotation
        │
        ▼
Phase 3
YOLO Model Training
        │
        ▼
Phase 4
Model Benchmarking
        │
        ▼
Phase 5
Real-Time UAV Detection
        │
        ▼
Phase 6
Multi-Object Tracking
        │
        ▼
Phase 7
UAV Localization
        │
        ▼
Phase 8
3D Visualization
        │
        ▼
Phase 9
Simulated Multi-Sensor Fusion
        │
        ▼
Phase 10
Physical Sensor Integration
```

The project intentionally follows a **software-first development strategy**:

> **Develop → Simulate → Benchmark → Validate → Integrate Hardware**

This allows the core algorithms to be developed and evaluated before dedicated sensing hardware is introduced.

---

# Research Direction

The project explores the intersection of:

* Artificial Intelligence
* Computer Vision
* Object Detection
* Multi-Object Tracking
* Geometric Localization
* Sensor Fusion
* Real-Time Computing
* Edge AI

The central research question is:

> **How can modern AI-based computer vision be used to achieve accurate, robust, and real-time UAV detection and tracking under varying environmental and operational conditions?**

The project begins with the fundamental perception problem:

```text
Raw Image / Video
       ↓
UAV Detection
       ↓
Object Identification
       ↓
Multi-Object Tracking
       ↓
Position & Motion Estimation
       ↓
UAV Track
```

and provides an extensible foundation for future research into multi-modal aerial sensing.

---

# Core Concept

```text
┌──────────────────────────────────────────────────────┐
│                   UAVs DEFENSE                      │
│                                                      │
│  DATA                                                │
│   │                                                  │
│   ▼                                                  │
│  DETECT ─────────────── YOLO / AI                    │
│   │                                                  │
│   ▼                                                  │
│  IDENTIFY                                             │
│   │                                                  │
│   ▼                                                  │
│  TRACK ─────────────── Multi-Object Tracking         │
│   │                                                  │
│   ▼                                                  │
│  LOCALIZE                                             │
│   │                                                  │
│   ▼                                                  │
│  ANALYZE                                              │
│   │                                                  │
│   ▼                                                  │
│  VISUALIZE                                            │
│                                                      │
└──────────────────────────────────────────────────────┘
```

> **UAVs Defense — From AI-powered visual detection to real-time UAV situational awareness.**

The project starts entirely from software-accessible data and simulation, while maintaining an architecture capable of evolving toward real-world multi-sensor UAV detection as additional hardware becomes available.
