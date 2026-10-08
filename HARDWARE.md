# Hardware

This document summarizes the hardware used in the study.

[中文] 本文档总结研究中使用的硬件。

---

## System Overview

```text
OpenMV Cam H7 Plus
   ↓  real-time detection of RAT / ROI / STI or RIR
3.3 V TTL trigger
   ↓
Pulse generator + LED driver
   ↓
625 nm laser
   ↓
Optical fiber → rostral PPN
   ↓
Motor arrest + place preference
```

[中文] 系统流程：OpenMV 检测事件 → 3.3V TTL 触发 → 脉冲发生器 + LED 驱动器 → 625 nm 激光 → 光纤 → 吻侧 PPN → 运动停止与位置偏好。

---

## 1. Core Computing and Vision

| Item | Specification |
|---|---|
| Embedded camera | OpenMV Cam H7 Plus |
| Processor | STM32H743II ARM Cortex M7 |
| Model | FOMO MobileNetV2 0.35, INT8 |
| Input size | 160 × 160 × 3 |
| Output | Heatmap for object localization |
| Trigger output | 3.3 V TTL via I/O pin |
| Trigger function | `p.high()` |
| End-to-end latency | ~770 ms |
| Runtime | Cortex-M4F, 80 MHz |

[中文] 核心计算与视觉模块：OpenMV Cam H7 Plus，STM32H743II ARM Cortex M7，FOMO MobileNetV2 0.35 INT8，输入 160×160×3，输出热图，3.3V TTL 触发，端到端延迟约 770 ms。

---

## 2. Stimulation and Light Delivery

| Item | Specification |
|---|---|
| Pulse generator | TorLabs M625F2 (NWT6000, 25MHz–6GHz) |
| LED driver | LEDD1B LED Driver |
| Laser wavelength | 625 nm |
| Maximum output power | 13 mW at fiber tip |
| Frequency | 20 Hz |
| Duty cycle | 50% |
| Stimulation duration | 3 s per trigger |
| Post-stimulation pause | 7 s |
| Total cycle | 10 s |
| Power at fiber tip | 10–13 mW |
| Estimated irradiance | 32–42 mW/mm² |
| Fiber | RWD black ceramic ferrule 01.25, 200 µm core, NA 0.39, length 10 mm |
| Shielding cable | OpenMV → pulse generator |

[中文] 刺激与光传输模块：脉冲发生器 TorLabs M625F2（NWT6000，25MHz–6GHz），LED 驱动器 LEDD1B，625 nm 激光，光纤尖端最大输出 13 mW，20 Hz，50% 占空比，每次 3 秒，暂停 7 秒，总周期 10 秒，光纤尖端功率 10–13 mW，辐照度 32–42 mW/mm²，光纤为 RWD black ceramic ferrule 01.25，200 µm 芯径，NA 0.39，长 10 mm，屏蔽线连接 OpenMV 与脉冲发生器。

---

## 3. Surgical and Implant Hardware

| Item | Specification |
|---|---|
| Headplate | 3D printed (design not preserved) |
| Copper mesh | ~5 × 5 cm, crown scaffold |
| Dental cement | Tetric EvoFlow or equivalent |
| Adhesive | Cyanolit glue |
| Drill bit | 1.4 mm diameter (FST 19008-14) |
| Viral vector | rAAV2/9-CamKIIa-ChrimsonR-mScarlet-KV2.1 (Addgene 124651) |
| Viral concentration | 1 × 10^12 vg/mL |
| Injection rate | 100 nL/min |
| Target coordinates | Rostral PPN: -8 AP, ±2 ML, -7.72 DV relative to Bregma and Dura |
| Fiber implant | Same coordinates, 0.1–0.2 mm above DV injection level |
| Fiber descent speed | 1 mm/min |
| Post-injection wait | 10 min |
| Capillary retraction speed | 5 mm/min |
| Post-surgery recovery | 3 weeks |

[中文] 手术与植入硬件：头板为 3D 打印（设计未保存），铜网约 5×5 cm，牙科水泥 Tetric EvoFlow，Cyanolit 胶，1.4 mm 钻头（FST 19008-14），病毒 rAAV2/9-CamKIIa-ChrimsonR-mScarlet-KV2.1（Addgene 124651），浓度 1×10^12 vg/mL，注射速率 100 nL/min，吻侧 PPN 坐标 -8 AP，±2 ML，-7.72 DV（相对 Bregma 和 Dura），光纤植入同坐标高于注射点 0.1–0.2 mm，光纤下降 1 mm/min，注射后停留 10 分钟，毛细管回撤 5 mm/min，术后恢复 3 周。

Note: The 3D-printed headplate design was not preserved. In the actual experiments, the OpenMV camera was suspended above the behavioral chamber using a rope and a frame. The camera field of view was adjusted by changing the frame position and rope length.

[中文] 注意：3D 打印头板设计未保存。实际实验中，OpenMV 相机通过绳子和架子悬挂在行为箱上方。通过改变架子位置和绳子长度来调整相机视野。

---

## 4. Behavioral Apparatus

| Item | Specification |
|---|---|
| Behavioral chamber | Polycarbonate |
| Open field | Transparent box, red block underneath |
| Y maze | Closed channel, one arm as ROI |
| Visual cue | Red color block |
| Background | White (polycarbonate box setting) |
| Camera position | Suspended above the chamber using a rope and frame |
| Behavioral recording camera | Standard surveillance camera |

[中文] 行为装置：聚碳酸酯行为箱，开放场透明箱体下方可放红色色块，Y 迷宫封闭通道一臂为 ROI，视觉线索为红色色块，背景为白色，相机通过绳子和架子悬挂在行为箱上方，行为记录相机为普通监控摄像头。

### Camera Field of View

- Rat1 and Rat2: the camera field of view covered the entire behavioral chamber.
- Rat3 and Rat4: the camera field of view was restricted to only the left arm of the Y maze (the ROI), so detecting a rat was equivalent to detecting it inside the ROI.

[中文] 相机视野：Rat1 和 Rat2 的相机视野覆盖整个行为箱。Rat3 和 Rat4 的相机视野被限制为只包含 Y 迷宫左臂（ROI），因此检测到大鼠就等价于检测到大鼠在 ROI 内。

### Camera Mounting

The OpenMV camera was suspended above the behavioral chamber using a rope and a frame. The camera field of view was adjusted by changing the frame position and rope length.

[中文] 相机安装：OpenMV 相机通过绳子和架子悬挂在行为箱上方。通过改变架子位置和绳子长度来调整相机视野。

---

## 5. Anesthesia and Analgesia

| Item | Specification |
|---|---|
| Anesthesia | 4% isoflurane (Attane vet. 1000 mg/g) |
| Heating pad | 37°C |
| Carprofen | 0.01 ml / 100 g body weight (Rimadyl, 50 g/ml) |
| Buprenorphine | 0.05 mg/kg |
| Lidocaine | 0.1 ml |

[中文] 麻醉与镇痛：4% 异氟烷（Attane vet. 1000 mg/g），37°C 加热垫，Carprofen 0.01 ml/100 g（Rimadyl，50 g/ml），Buprenorphine 0.05 mg/kg，Lidocaine 0.1 ml。

---

## 6. Wiring

```text
OpenMV P0  →  Pulse generator Trigger IN
OpenMV GND →  Pulse generator GND
Pulse generator OUT → LED Driver IN
LED Driver → 625 nm laser
Laser → optical fiber → rat brain
```

[中文] 接线：OpenMV P0 接脉冲发生器触发输入，GND 共地，脉冲发生器输出接 LED 驱动器，LED 驱动器接 625 nm 激光，激光经光纤进入大鼠脑内。

A wiring diagram is available in the paper.

[中文] 接线图见论文。

---

## 7. Safety

Follow institutional guidelines for laser safety and animal ethics.

[中文] 激光安全与动物伦理遵循机构指南。

---

## 8. Notes

- The OpenMV camera was placed vertically above the behavioral chamber.
- Environmental factors such as lighting, angle, and camera field of view were standardized.
- For the polycarbonate box setting, a white background was used for optimal detection.
- The system supports both wired and wireless optogenetics in principle.
- The complete per-rat models and raw video data were not preserved.

[中文] 注意：OpenMV 相机垂直置于行为箱上方；照明、角度和相机视野等环境因素标准化；聚碳酸酯箱设置中使用白色背景；该系统原则上支持有线与无线光遗传学；完整的每只鼠模型和原始视频数据未保存。
