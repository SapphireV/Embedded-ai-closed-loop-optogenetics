# Embedded AI-Enabled Real-Time Closed-Loop Optogenetics

OpenMV Cam H7 Plus + FOMO MobileNetV2 for real-time rat position/event detection and closed-loop optogenetic stimulation.

[中文] 基于 OpenMV Cam H7 Plus 和 FOMO MobileNetV2 的嵌入式 AI 闭环光遗传学系统，用于实时检测大鼠位置/事件并触发光遗传刺激。

---

## Overview

This repository contains the OpenMV deployment scripts, label definitions, and an example trained model for a low-cost embedded AI platform that integrates real-time machine-vision-based behavioral detection with closed-loop optogenetic stimulation.

The system uses an OpenMV Cam H7 Plus running an onboard FOMO MobileNetV2 object detection model. When a predefined behavioral event is detected (entry into a region of interest, labeled as `STI`), the OpenMV camera sends a TTL trigger signal to a pulse generator, which drives a 625 nm laser for optogenetic stimulation.

[中文] 本仓库包含 OpenMV 部署脚本、标签定义和一个示例模型。系统使用 OpenMV Cam H7 Plus 运行 FOMO MobileNetV2 目标检测模型，检测到预定义行为事件（进入感兴趣区域，标签为 `STI`）后发送 TTL 触发信号，驱动 625 nm 激光进行光遗传刺激。

---

## Paper

**Title:** Embedded AI Enable Real-Time Closed-Loop Optogenetics: Application to Pedunculopontine Nucleus-Induced Motor Arrest and Place Preference

**Journal:** Journal of Neural Engineering

**Status:** Manuscript under review

[中文] 论文目前正在 Journal of Neural Engineering 审稿中。

---

## Repository Structure

```text
.
├── README.md
├── LICENSE
└── openmv/
    ├── README.md
    ├── ei_object_detection.py
    ├── labels.txt
    ├── main_roi_trigger.py
    └── trained.tflite
