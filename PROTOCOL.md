# Behavioral Protocol: Conditioned Place Preference (CPP) with Closed-Loop Optogenetics

This document describes the behavioral protocol used in the paper.

[中文] 本文档描述论文中使用的行为实验协议。

---

## Animals

- Wild-type Long Evans adult male rats (12–16 weeks old).
- Purchased from Charles River Laboratories.
- Pair-housed under standardized conditions with a 12:12 light cycle.
- Free access to food and water during 7-day acclimation.
- After surgery, individually housed with ad libitum food and dietary gel for 3 days.
- Soft bedding and nesting material provided during recovery.

[中文] 野生型 Long Evans 成年雄性大鼠（12–16 周）。购自 Charles River Laboratories。在标准化条件下配对饲养，12:12 光周期。7 天适应期自由进食饮水。术后单独饲养，自由进食并补充膳食凝胶 3 天。恢复期提供软垫料和筑巢材料。

---

## Surgery and Viral Injection

- Anesthesia: 4% isoflurane.
- Analgesia: Carprofen (0.01 ml/100 g), Buprenorphine (0.05 mg/kg), Lidocaine (0.1 ml).
- Headplate fabricated with 3D printing, copper wire mesh, dental cement.
- Bilateral holes (1.4 mm diameter) drilled for injection and fiber implantation.
- Viral vector: rAAV2/9-CamKIIa-ChrimsonR-mScarlet-KV2.1.
- Concentration: 1 × 10^12 vg/mL.
- Injection rate: 100 nL/min.
- Target coordinates for rostral PPN: -8 AP, ±2 ML, -7.72 DV relative to Bregma and Dura.
- Optical fiber: 200 µm core, NA 0.39, length 10 mm.
- Fiber implanted at same coordinates, approximately 0.1–0.2 mm above the DV injection level.
- Post-surgery recovery: 3 weeks for viral expression.

[中文] 麻醉：4% 异氟烷。镇痛：Carprofen、Buprenorphine、Lidocaine。3D 打印头板、铜网、牙科水泥。双侧钻孔 1.4 mm 用于注射和光纤植入。病毒载体：rAAV2/9-CamKIIa-ChrimsonR-mScarlet-KV2.1。浓度 1×10^12 vg/mL。注射速率 100 nL/min。吻侧 PPN 坐标：-8 AP，±2 ML，-7.72 DV（相对 Bregma 和 Dura）。光纤：200 µm 芯径，NA 0.39，长 10 mm。光纤植入相同坐标，高于注射 DV 水平约 0.1–0.2 mm。术后恢复 3 周以表达病毒。

---

## Optogenetic Stimulation Parameters

- Wavelength: 625 nm
- Frequency: 20 Hz
- Duty cycle: 50%
- Stimulation duration: 3 s per trigger
- Post-stimulation pause: 7 s
- Total cycle: 10 s
- Power at fiber tip: 10–13 mW
- Estimated irradiance: 32–42 mW/mm²

[中文] 波长：625 nm；频率：20 Hz；占空比：50%；每次触发刺激 3 秒；刺激后暂停 7 秒；总周期 10 秒；光纤尖端功率 10–13 mW；估计辐照度 32–42 mW/mm²。

---

## Behavioral Apparatus

Two tasks were used.

[中文] 使用两种行为任务。

### Task A: Open field with visual cue (Rat1 and Rat2)

- Polycarbonate open-field chamber.
- A red color block placed underneath the chamber region serves as a visual cue.
- ROI defined as the region above the red block.
- OpenMV camera sees the whole chamber.

[中文] 任务 A：带视觉线索的开放场（Rat1 和 Rat2）。聚碳酸酯开放场箱。箱体区域下方放置红色色块作为视觉线索。ROI 定义为红色色块上方区域。OpenMV 相机看到整个箱体。

### Task B: Y maze without visual cue (Rat3 and Rat4)

- Y maze with a closed channel.
- One arm designated as ROI.
- No visual cue.
- OpenMV camera field of view physically restricted so that only the ROI arm is visible.

[中文] 任务 B：无视觉线索的 Y 迷宫（Rat3 和 Rat4）。带封闭通道的 Y 迷宫。一臂设为 ROI。无视觉线索。OpenMV 相机视野被物理限制，只让 ROI 臂可见。

---

## Closed-Loop Trigger

The trigger logic is the same for both tasks, but the label used for triggering differs.

[中文] 两种任务的触发逻辑相同，但用于触发的标签不同。

### Task A (Rat1 and Rat2): four-label scheme

- `labels.txt`: `background`, `rat`, `ROI`, `RIR`
- The model detects rat, ROI, and rat-in-ROI separately.
- Trigger class: `RIR` (referred to as `STI` in the paper), index `i == 3`.

[中文] 任务 A（Rat1 和 Rat2）：四标签方案。`labels.txt` 为 background、rat、ROI、RIR。模型分别检测大鼠、ROI 和“大鼠在 ROI 中”。触发类为 `RIR`（论文中称为 `STI`），索引 `i == 3`。

### Task B (Rat3 and Rat4): two-label scheme

- `labels.txt`: `background`, `rat`
- The camera only sees the ROI, so detecting a rat is equivalent to detecting the rat inside the ROI.
- Trigger class: `rat`, index `i == 1`.

[中文] 任务 B（Rat3 和 Rat4）：两标签方案。`labels.txt` 为 background、rat。相机只能看到 ROI，所以检测到大鼠等价于检测到大鼠在 ROI 内。触发类为 `rat`，索引 `i == 1`。

In both cases:

1. OpenMV detects the trigger class.
2. Sends 3.3 V TTL trigger to pulse generator.
3. Pulse generator activates 625 nm laser for 3 s.
4. OpenMV pauses for 10 s (3 s stimulation + 7 s recovery).

[中文] 两种情况下：OpenMV 检测到触发类；发送 3.3 V TTL 触发信号给脉冲发生器；脉冲发生器激活 625 nm 激光 3 秒；OpenMV 暂停 10 秒（3 秒刺激 + 7 秒恢复）。

---

## CPP Protocol

### Habituation (Control Phase)

- Rats placed in the behavioral chamber to explore freely until equal interest in all parts.
- No optogenetic stimulation.

[中文] 习惯化（控制期）：大鼠放入行为箱自由探索，直到对所有区域兴趣均等。无光遗传刺激。

### Training Phase

- OpenMV Cam active with pulse train generator.
- Whenever the rat enters the ROI, optogenetic stimulation is delivered.
- Training duration: 30 min.
- Rest: 1 h in home cage.
- Repeated 2–3 times per rat.

[中文] 训练期：OpenMV 和脉冲发生器开启。每当大鼠进入 ROI，给予光遗传刺激。训练时长 30 分钟。回笼休息 1 小时。每只鼠重复 2–3 次。

### Test Phase

- OpenMV Cam and pulse generator turned off.
- Rats allowed to move freely for 30 min.
- Movement trajectories recorded for analysis.

[中文] 测试期：关闭 OpenMV 和脉冲发生器。大鼠自由活动 30 分钟。记录运动轨迹用于分析。

---

## Data Analysis

- Videos analyzed using DeepLabCut.
- ROI defined as 0 to 1000 along the x-axis.
- Percentage of time spent in ROI calculated from coordinate points.
- Heat maps generated to visualize exploration time.

[中文] 使用 DeepLabCut 分析视频。ROI 定义为 x 轴 0–1000。根据坐标点计算 ROI 停留时间百分比。生成热图可视化探索时间。

---

## Ethics

All animal procedures were approved by the institutional animal care and use committee.

[中文] 所有动物实验程序均经机构动物护理和使用委员会批准。
