# UAVs Defense

## AI-Based UAV Detection, Localization & Tracking System

> **UAVs Defense** is an AI-based UAV perception research project focused on the real-time detection, identification, localization, and continuous tracking of Unmanned Aerial Vehicles (UAVs).

The current implementation is intentionally **software-first**. It focuses on deep-learning-based computer vision and multi-object tracking using publicly available UAV datasets, recorded video, and software-based simulation as the primary sources of sensor data.

The system is designed around a **replaceable AI detection-engine architecture**. The detector is treated as the perception engine of the application: an existing model can be used as-is, fine-tuned on UAV-specific data, optimized for deployment, modified, or replaced by another model without changing the rest of the application.

The architecture can later incorporate additional sensing modalities such as **EO/IR, radar, and RF/SDR** when physical hardware becomes available.

---

## Table of Contents

1. [Overview](#overview)
2. [Project Objectives](#project-objectives)
3. [Current Scope](#current-scope)
4. [System Architecture](#system-architecture)
5. [UAV Detection Pipeline](#uav-detection-pipeline)
6. [AI Detection Engine](#ai-detection-engine)
7. [Detection Engine Interface](#detection-engine-interface)
8. [Model Selection & Benchmarking](#model-selection--benchmarking)
9. [Multi-Object Tracking](#multi-object-tracking)
10. [UAV Localization](#uav-localization)
11. [Simulation & Data Sources](#simulation--data-sources)
12. [Dataset & Model Training](#dataset--model-training)
13. [Performance Evaluation](#performance-evaluation)
14. [Software Requirements](#software-requirements)
15. [Project Structure](#project-structure)
16. [Development Roadmap](#development-roadmap)
17. [Future Multi-Sensor Integration](#future-multi-sensor-integration)
18. [Research Direction](#research-direction)
19. [Core Concept](#core-concept)

---

# Overview

Small Unmanned Aerial Vehicles present a challenging computer-vision problem.

A UAV may occupy only a small number of pixels when observed at long distances, while changes in viewing angle, lighting, background, motion, compression, and environmental conditions can significantly affect its appearance.

A reliable UAV detection system must therefore address several problems simultaneously:

- Small-object detection
- Real-time inference
- Multiple simultaneous targets
- Target identity preservation
- Temporary detection loss
- Motion and trajectory estimation
- Environmental variation
- Spatial localization
- Computational resource constraints

**UAVs Defense focuses on solving these problems through an AI-driven perception pipeline.**

The initial system operates on software-accessible data rather than requiring dedicated physical sensing hardware.

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
                ┌───────────────────────┐
                │   AI Detection Engine │
                │                       │
                │ RT-DETR / YOLOX /     │
                │ YOLO / Custom Model   │
                └───────────┬───────────┘
                            │
                            ▼
                     UAV Detections
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
                      UAV Track Output
```

The application is intentionally decoupled from any single model. The detection engine may be upgraded, optimized, or replaced as experimentation progresses.

---

# Project Objectives

The project focuses on developing a complete UAV perception pipeline capable of:

- Detecting UAVs from video and image data
- Identifying UAV objects using deep-learning models
- Detecting multiple UAVs simultaneously
- Estimating detection confidence
- Maintaining persistent target identities
- Tracking UAV movement across frames
- Estimating target position and motion
- Evaluating detection performance quantitatively
- Comparing different AI detection engines
- Optimizing AI inference for real-time operation
- Supporting repeatable software-based experiments
- Providing an architecture suitable for future sensor integration

The primary focus is **UAV perception**, rather than UAV mitigation or interception.

---

# Current Scope

The current project is intentionally focused on software-based development.

## Current Development Areas

- UAV image and video processing
- Object detection
- Dataset preparation
- Model training and fine-tuning
- Detection-engine benchmarking
- Multi-object tracking
- Detection confidence analysis
- Video-based target tracking
- Software simulation
- Runtime performance benchmarking

## Current Data Sources

The project currently relies on:

- Publicly available UAV datasets
- Public UAV videos
- Recorded video data
- Synthetic or simulated data
- Software-generated sensor observations

This allows the detection and tracking algorithms to be developed and evaluated without dedicated physical UAV-detection hardware.

## Future Hardware Integration

The architecture is prepared for future integration with:

- EO/IR cameras
- Radar systems
- SDR / RF sensors
- Other aerial sensing systems

These components are **future integration targets and are not assumed to be physically available in the current implementation**.

---

# System Architecture

The application is structured around independent perception stages.

```text
┌─────────────────────────────────────────────────────────────────┐
│                         UAVs DEFENSE                            │
│                                                                 │
│                 AI-Based UAV Perception System                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌───────────────────┐                                         │
│   │ Dataset / Video   │                                         │
│   │ / Simulation      │                                         │
│   └─────────┬─────────┘                                         │
│             │                                                   │
│             ▼                                                   │
│   ┌───────────────────┐                                         │
│   │ Frame Acquisition │                                         │
│   └─────────┬─────────┘                                         │
│             │                                                   │
│             ▼                                                   │
│   ┌───────────────────┐                                         │
│   │ Image             │                                         │
│   │ Preprocessing     │                                         │
│   └─────────┬─────────┘                                         │
│             │                                                   │
│             ▼                                                   │
│   ┌──────────────────────────────────────┐                      │
│   │          AI Detection Engine         │                      │
│   │                                      │                      │
│   │ RT-DETR / RT-DETRv2 / YOLOX / YOLO │                      │
│   │ Custom / Optimized Deployment Model │                      │
│   └──────────────────┬───────────────────┘                      │
│                      │                                          │
│                      ▼                                          │
│             ┌─────────────────┐                                 │
│             │ UAV Detections  │                                 │
│             └────────┬────────┘                                 │
│                      │                                          │
│                      ▼                                          │
│             ┌─────────────────┐                                 │
│             │ Object Tracking │                                 │
│             └────────┬────────┘                                 │
│                      │                                          │
│                      ▼                                          │
│             ┌─────────────────┐                                 │
│             │ Target State    │                                 │
│             │ Estimation      │                                 │
│             └────────┬────────┘                                 │
│                      │                                          │
│                      ▼                                          │
│             ┌─────────────────┐                                 │
│             │ UAV Localization│                                 │
│             └────────┬────────┘                                 │
│                      │                                          │
│                      ▼                                          │
│                UAV Track Output                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

The architecture separates **detection, tracking, and localization** so that each stage can be independently evaluated and improved.

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
AI Detection Engine
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

The pipeline should operate on:

- Individual images
- Recorded video
- Live video streams

The same detection output contract is used regardless of the underlying model.

---

# AI Detection Engine

## Design Principle

The AI model is treated as a **replaceable detection engine**, not as the application itself.

The project should not be architecturally locked to a single model family.

An existing model can be:

1. Used directly as a baseline
2. Fine-tuned on a UAV dataset
3. Optimized for latency and memory
4. Exported to an inference runtime
5. Modified at the architecture or training level
6. Replaced by a different detector
7. Replaced by a custom model developed specifically for UAV detection

Conceptually:

```text
                    Detection Engine
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
       RT-DETR           YOLOX            YOLO
          │                │                │
          └────────────────┼────────────────┘
                           │
                           ▼
                    Common Output
                           │
                           ▼
                    Tracking Layer
```

The rest of the application should not depend on model-specific output formats.

## Initial Candidate Models

The project begins with several model families rather than assuming one model is automatically the best:

### RT-DETR / RT-DETRv2

RT-DETR is the primary open baseline for the initial benchmark because the official implementation is available under Apache-2.0. The official repository includes PyTorch implementations and RT-DETRv2, and documents deployment paths including ONNX Runtime, TensorRT, and OpenVINO.

The model is particularly useful as a research baseline because it can be trained on custom data and later optimized without changing the higher-level application architecture.

### YOLOX

YOLOX is included as another Apache-2.0 detector family and provides a useful real-time comparison against the transformer-based RT-DETR family.

### Ultralytics YOLO

Ultralytics YOLO can be evaluated as a practical benchmark because its ecosystem provides mature training, tracking, export, and deployment workflows.

However, the licensing model is different from Apache-2.0. Ultralytics currently offers AGPL-3.0 and Enterprise licensing, so the project records the model and license used for each experiment rather than assuming that all YOLO usage has the same redistribution terms.

## Model Selection Principle

The project does **not** select a detector from popularity alone.

The final detector is selected from measured performance on the project's UAV dataset and deployment target.

```text
Candidate Models
      │
      ▼
Same Dataset
      │
      ▼
Same Evaluation Protocol
      │
      ├── Accuracy
      ├── Small-UAV Recall
      ├── Precision
      ├── mAP
      ├── Latency
      ├── FPS
      ├── Memory
      └── Power / Resource Usage
      │
      ▼
Experimental Comparison
      │
      ▼
Selected Baseline
      │
      ├── Fine-tune
      ├── Optimize
      ├── Modify
      └── Replace
```

---

# Detection Engine Interface

The application should expose an internal model-independent interface.

Conceptually:

```python
class DetectionEngine:
    def load(self, model_path: str) -> None:
        ...

    def warmup(self) -> None:
        ...

    def detect(self, frame):
        ...

    def close(self) -> None:
        ...
```

The exact implementation may evolve, but the principle remains the same:

> **The application consumes detections, not model-specific inference objects.**

Every detector converts its native output into a common representation.

```text
Model Output
    │
    ▼
Detection Adapter
    │
    ▼
Common Detection[]
```

A common detection can contain:

```text
Detection
├── class_id
├── class_name
├── confidence
├── bounding_box
│   ├── x
│   ├── y
│   ├── width
│   └── height
└── timestamp
```

This allows the application to change from:

```text
RT-DETR
```

to:

```text
YOLOX
```

or:

```text
custom detector
```

without rewriting the tracking, API, visualization, or future sensor-fusion layers.

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

- Maintain target identity
- Estimate target movement
- Handle temporary detection loss
- Reduce unnecessary identity changes
- Build target trajectories
- Provide continuous target-state updates

Possible tracking engines can be evaluated independently from the detector.

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

- Camera intrinsic parameters
- Camera position
- Camera orientation
- Field of view
- Target altitude
- Ground reference
- External sensor measurements

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

## Public UAV Datasets

Used for:

- Model training
- Validation
- Benchmarking
- Error analysis

## Public UAV Videos

Used for:

- Real-time inference experiments
- Multi-object tracking
- Long-duration tracking
- Detection robustness testing

## Synthetic Data

Used to generate controlled scenarios where target:

- Position
- Velocity
- Trajectory
- Appearance
- Number of objects

can be controlled.

## Software Simulation

Simulation provides a repeatable environment for evaluating the complete perception pipeline before physical hardware integration.

---

# Dataset & Model Training

The AI detector is trained and evaluated using a dedicated UAV dataset.

Dataset preparation includes:

- Image collection
- Annotation
- Data cleaning
- Class definition
- Dataset balancing
- Train/validation/test splitting
- Data augmentation
- Small-object analysis

A typical dataset structure is:

```text
Dataset
├── train/
├── val/
└── test/
```

The test set remains separate from training and model-selection experiments to provide an unbiased evaluation of the final detector.

## UAV-Specific Data Considerations

Because distant UAVs may occupy very few pixels, the dataset should intentionally include:

- Small and distant UAVs
- Different image resolutions
- Different viewing angles
- Sky-only backgrounds
- Urban backgrounds
- Vegetation
- Clouds
- Haze
- Motion blur
- Compression artifacts
- Different lighting conditions
- Partial or occluded UAVs
- Multiple simultaneous UAVs

The project should also report performance by target scale rather than relying only on a single overall mAP value.

---

# Performance Evaluation

The project evaluates both **AI detection quality** and **real-time performance**.

## Detection Metrics

| Metric               | Description                                             |
| -------------------- | ------------------------------------------------------- |
| **Precision**        | Percentage of predicted UAV detections that are correct |
| **Recall**           | Percentage of actual UAVs successfully detected         |
| **mAP@50**           | Detection performance at IoU threshold 0.50             |
| **mAP@50-95**        | Detection performance across multiple IoU thresholds    |
| **Small-UAV Recall** | Detection recall for small/distant UAV targets          |

## Runtime Metrics

| Metric                 | Description                                   |
| ---------------------- | --------------------------------------------- |
| **FPS**                | Frames processed per second                   |
| **Inference Latency**  | Time required for AI inference                |
| **End-to-End Latency** | Time from input frame to final tracked output |
| **GPU Memory**         | Memory required by the detector               |
| **CPU Utilization**    | Processing overhead                           |
| **Model Size**         | Storage footprint of the deployed model       |

## Tracking Metrics

| Metric                   | Description                                         |
| ------------------------ | --------------------------------------------------- |
| **Track Continuity**     | Ability to maintain a target over time              |
| **Identity Consistency** | Stability of target identities                      |
| **Track Loss Rate**      | Frequency of lost tracks                            |
| **Position Error**       | Difference between estimated and reference position |

## Benchmark Protocol

All candidate detectors should be evaluated using the same:

- Dataset split
- Input resolution policy
- Hardware
- Confidence thresholds
- Evaluation scripts
- Video sequences
- Reporting methodology

This makes model comparisons meaningful.

---

# Software Requirements

The current project is primarily software-based.

## Operating System

- Ubuntu Linux
- Windows + WSL

## AI / Computer Vision

- Python 3.x
- PyTorch
- OpenCV
- NumPy
- Model-specific runtime / framework
- Detection and tracking evaluation tools

## Optional Simulation

- ROS 2
- Gazebo
- SITL
- Synthetic data-generation tools

## Hardware Acceleration

GPU acceleration is recommended for model training and real-time inference but is not strictly required for initial development and experimentation.

Deployment targets may later include:

- Desktop GPU
- Industrial GPU workstation
- NVIDIA Jetson-class edge hardware
- Other accelerators supported by the selected inference runtime

---

# Project Structure

The repository is organized around independent perception components:

```text
uavs-defense/
│
├── ai/
│   ├── engines/
│   │   ├── base.py
│   │   ├── rtdetr/
│   │   ├── yolox/
│   │   ├── yolo/
│   │   └── custom/
│   │
│   ├── adapters/
│   │   └── detection.py
│   │
│   ├── tracking/
│   │   ├── bytetrack/
│   │   └── ...
│   │
│   ├── localization/
│   │
│   └── evaluation/
│
├── datasets/
│   ├── raw/
│   ├── processed/
│   └── splits/
│
├── models/
│   ├── checkpoints/
│   ├── exports/
│   └── benchmarks/
│
├── inference/
│   ├── video/
│   └── stream/
│
├── backend/
│   ├── api/
│   └── websocket/
│
├── frontend/
│   └── web/
│
├── simulation/
│
├── scripts/
│
├── docs/
│   ├── model-selection.md
│   ├── benchmark.md
│   └── architecture.md
│
└── README.md
```

The exact structure can evolve as implementation moves from experimentation into deployment.

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
Detection Engine Abstraction
        │
        ▼
Phase 4
Baseline Model Evaluation
        │
        ├── RT-DETR / RT-DETRv2
        ├── YOLOX
        └── YOLO-family benchmark
        │
        ▼
Phase 5
UAV-Specific Fine-Tuning
        │
        ▼
Phase 6
Model Benchmarking
        │
        ▼
Phase 7
Real-Time UAV Detection
        │
        ▼
Phase 8
Multi-Object Tracking
        │
        ▼
Phase 9
UAV Localization
        │
        ▼
Phase 10
Web / Mobile Visualization
        │
        ▼
Phase 11
3D Visualization
        │
        ▼
Phase 12
Simulated Multi-Sensor Fusion
        │
        ▼
Phase 13
Physical Sensor Integration
```

The project follows a software-first strategy:

> **Develop → Benchmark → Fine-Tune → Optimize → Validate → Integrate Hardware**

The detection engine is expected to evolve throughout this process.

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
         EO / IR        Radar       RF / SDR
          Camera        Sensor        Sensor
             │            │            │
             ▼            ▼            ▼
          Visual        Radar          RF
        Observation   Observation   Observation
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

- Radar-camera fusion
- RF-camera correlation
- Track-to-track association
- Extended Kalman Filtering
- Sensor confidence fusion
- Multi-modal target classification
- Improved spatial localization

---

# Research Direction

The project explores the intersection of:

- Artificial Intelligence
- Computer Vision
- Object Detection
- Multi-Object Tracking
- Geometric Localization
- Sensor Fusion
- Real-Time Computing
- Edge AI

The central research question is:

> **How can modern AI-based computer vision be used to achieve accurate, robust, and real-time UAV detection and tracking under varying environmental and operational conditions?**

The project starts from the fundamental perception problem:

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
       ↓
Situational Awareness
```

The longer-term direction is to build a perception layer that can evolve from a single-camera software system into a multi-modal aerial sensing system.

---

# Core Concept

The detector is an **engine**, not the entire vehicle.

```text
┌───────────────────────────────────────────────────────────┐
│                       UAVs DEFENSE                        │
│                                                           │
│  DATA                                                     │
│   │                                                       │
│   ▼                                                       │
│  DETECT ─────────── Replaceable AI Engine                │
│   │                 ├── RT-DETR / RT-DETRv2              │
│   │                 ├── YOLOX                            │
│   │                 ├── YOLO                              │
│   │                 └── Custom / Optimized Model         │
│   │                                                       │
│   ▼                                                       │
│  IDENTIFY                                                  │
│   │                                                       │
│   ▼                                                       │
│  TRACK ───────────── Multi-Object Tracking                │
│   │                                                       │
│   ▼                                                       │
│  LOCALIZE                                                  │
│   │                                                       │
│   ▼                                                       │
│  ANALYZE                                                    │
│   │                                                       │
│   ▼                                                       │
│  VISUALIZE                                                  │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

The engineering principle is:

> **Choose an engine → establish a baseline → train it for UAVs → benchmark it → optimize it → replace it when a stronger approach is justified.**

This prevents the rest of the application from becoming coupled to a single AI model.

---

# Model & License References

The project records the exact model, weights, codebase, and license used for every benchmark.

- **RT-DETR official repository:** https://github.com/lyuwenyu/RT-DETR
- **YOLOX official repository:** https://github.com/Megvii-BaseDetection/YOLOX
- **Ultralytics documentation and licensing:** https://docs.ultralytics.com/

Licensing must be reviewed for the exact model, weights, code, dataset, and intended distribution model before any production or commercial deployment.

---

> **UAVs Defense — From AI-powered visual detection to real-time UAV situational awareness.**

The project starts entirely from software-accessible data and simulation while maintaining an architecture capable of evolving toward real-world multi-sensor UAV detection as additional hardware becomes available.
