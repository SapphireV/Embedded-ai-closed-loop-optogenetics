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

openmv/main_roi_trigger.py: Main OpenMV script for ROI/event-triggered optogenetic stimulation.

openmv/ei_object_detection.py: Edge Impulse OpenMV object detection example, useful for testing the model and labels.

openmv/labels.txt: Class labels used by the model.

openmv/trained.tflite: Example TensorFlow Lite model for demonstration.

[中文] main_roi_trigger.py 是主实验脚本；ei_object_detection.py 是官方检测示例，用于调试；labels.txt 是类别标签；trained.tflite 是示例模型。

## System Overview

OpenMV Cam H7 Plus
   ↓  real-time detection of RAT / ROI / STI
3.3 V TTL trigger
   ↓
Pulse generator + LED driver
   ↓
625 nm laser
   ↓
Optical fiber → rostral PPN
   ↓
Motor arrest + place preference

Stimulation parameters used in the study:

Wavelength: 625 nm

Frequency: 20 Hz

Duty cycle: 50%

Stimulation duration: 3 s per trigger

Post-stimulation pause: 7 s

Total OpenMV wait time: 10 s

[中文] 系统流程：OpenMV 检测事件 → TTL 触发 → 脉冲发生器 → 625 nm 激光 → 光纤 → 吻侧 PPN → 运动停止与位置偏好。刺激参数：625 nm，20 Hz，50% 占空比，每次 3 秒，之后暂停 7 秒，OpenMV 总共等待 10 秒。

## Label Configuration
The script main_roi_trigger.py uses labels.txt to map class indices. The default order is:

0 background
1 RAT
2 ROI
3 STI
With this order, the trigger class STI is at index 3, so the script uses:

if (i == 3):
    p.high()
    sensor.skip_frames(time=10000)
If your Edge Impulse training used a different label order, simply:

Open labels.txt and find the line number of your trigger class (starting from 0).

Open main_roi_trigger.py and change i == 3 to that line number.

[中文] labels.txt 的顺序决定类别索引。默认顺序为 background, RAT, ROI, STI，触发类 STI 在索引 3，所以脚本用 i == 3。如果你的标签顺序不同，找到触发类的行号（从 0 开始），把脚本里的 i == 3 改成对应数字即可。

## Model Availability
This repository includes one example trained.tflite for demonstration.

The complete set of per-rat models and raw video recordings are not available because they were not preserved during the original study.

To reproduce the full pipeline, users can retrain FOMO MobileNetV2 models using Edge Impulse with their own labeled frames, following the protocol and label definitions described below.

[中文] 本仓库只提供一个示例 trained.tflite。完整的每只鼠模型和原始视频录像未保存，因此无法提供。用户可以使用 Edge Impulse 按照下面的协议和标签定义，用自己的标注帧重新训练 FOMO MobileNetV2 模型。

## Training Protocol Summary
Platform: Edge Impulse

Model: FOMO MobileNetV2 0.35, INT8 quantized

Input size: 160 × 160 × 3

Classes: background, RAT, ROI, STI

Training images: approximately 150–250 manually annotated frames per model

Evaluation metric: F1 score, threshold 95%

[中文] 训练协议：Edge Impulse，FOMO MobileNetV2 0.35，INT8 量化，输入 160×160×3，类别包括 background、RAT、ROI、STI，每模型约 150–250 张标注帧，F1 阈值 95%。

## Data Availability
Raw video recordings and DeepLabCut-extracted trajectory files were not preserved during the original study and are therefore not available.

The repository provides:

OpenMV deployment scripts

Label definitions

Training protocol

Example model for demonstration

[中文] 原始视频和 DeepLabCut 轨迹文件未保存，因此无法提供。本仓库提供 OpenMV 部署脚本、标签定义、训练协议和示例模型。

## Quick Start
Install OpenMV IDE.

Copy trained.tflite and labels.txt to the OpenMV Cam mass-storage device.

Copy main_roi_trigger.py to the OpenMV Cam.

Connect OpenMV P0 to the pulse generator trigger input.

Connect OpenMV GND to the pulse generator GND.

Run main_roi_trigger.py in OpenMV IDE.

[中文] 快速开始：安装 OpenMV IDE；把 trained.tflite 和 labels.txt 拷贝到 OpenMV 存储设备；把 main_roi_trigger.py 拷贝进去；连接 P0 到脉冲发生器触发输入，GND 共地；在 OpenMV IDE 中运行脚本。

## Hardware
Hardware documentation, including BOM, wiring diagram, CAD files, assembly notes, calibration, and laser safety, will be added in a future update.

Key hardware used in this study:

OpenMV Cam H7 Plus

Pulse generator (NWT6000 25MHz–6GHz or equivalent)

LED driver (LEDD1B or equivalent)

625 nm laser

Optical fiber (200 µm core, NA 0.39, 10 mm)

Polycarbonate behavioral chamber or Y maze

[中文] 硬件文档（BOM、接线图、CAD、装配、校准、激光安全）将在后续更新中补充。本研究使用的主要硬件包括：OpenMV Cam H7 Plus、脉冲发生器、LED 驱动器、625 nm 激光、光纤、聚碳酸酯行为箱或 Y 迷宫。

## Citation
If you use this code or the example model, please cite the paper:

Embedded AI Enable Real-Time Closed-Loop Optogenetics:
Application to Pedunculopontine Nucleus-Induced Motor Arrest and Place Preference
Journal of Neural Engineering, under review.
[中文] 如果使用本代码或示例模型，请引用上述论文。

## License
Code in this repository is released under the MIT License.
See the LICENSE file for details.

[中文] 本仓库代码采用 MIT 许可。详见 LICENSE 文件。

## Contact
For questions, please open an issue or contact the corresponding author.

[中文] 如有问题，请开 issue 或联系通讯作者。
