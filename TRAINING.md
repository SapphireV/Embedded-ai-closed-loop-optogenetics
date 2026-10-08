# Training Protocol

This document summarizes the training pipeline for the FOMO MobileNetV2 models used in the paper.

[中文] 本文档总结论文中使用的 FOMO MobileNetV2 模型训练流程。

---

## Platform

- Edge Impulse (https://www.edgeimpulse.com)
- Project example: https://studio.edgeimpulse.com/studio/367850

[中文] 训练平台为 Edge Impulse，项目示例链接见上。

---

## Model

- Architecture: FOMO MobileNetV2 0.35
- Quantization: INT8
- Input size: 160 × 160 × 3
- Output: heatmap for object localization

[中文] 模型架构：FOMO MobileNetV2 0.35；量化：INT8；输入：160×160×3；输出：用于目标定位的热图。

---

## Two Labeling Schemes

This study used two different labeling schemes depending on the behavioral task.

[中文] 本研究根据行为任务使用了两种不同的标签方案。

### Scheme A: Four labels (Rat1 and Rat2)

Used for the open-field task with a red visual cue. The model learned to distinguish rat, ROI, and "rat in ROI" as separate classes.

[中文] 方案 A：四标签（Rat1 和 Rat2）。用于带有红色视觉线索的开放场任务。模型学会区分大鼠、ROI 和“大鼠在 ROI 中”这三个不同类别。

`labels.txt` for Scheme A:

```text
background
rat
ROI
RIR
```

- `background`: background
- `rat`: rat detected
- `ROI`: region of interest detected
- `RIR`: rat in region, i.e., rat inside the ROI
- Trigger class: `RIR`, index `i == 3`

[中文] 方案 A 的 `labels.txt`：background、rat、ROI、RIR。触发类为 `RIR`，索引 `i == 3`。

In the paper, the trigger class is referred to as `STI` (stimulation-triggering indicator). In the actual training logs for Rat1 and Rat2, it was labeled as `RIR`.

[中文] 论文中把触发类称为 `STI`（刺激触发指示器）。但在 Rat1 和 Rat2 的实际训练日志中，它被标记为 `RIR`。

### Scheme B: Two labels (Rat3 and Rat4)

Used for the Y maze task without visual cues. The OpenMV camera field of view was physically restricted so that only the ROI was visible. Therefore, whenever the rat appeared in the camera frame, it was already inside the ROI.

[中文] 方案 B：两标签（Rat3 和 Rat4）。用于无视觉线索的 Y 迷宫任务。OpenMV 相机的视野被物理限制，只让 ROI 区域进入画面。因此，只要大鼠出现在画面中，就等价于它已经进入了 ROI。

`labels.txt` for Scheme B:

```text
background
rat
```

- `background`: background
- `rat`: rat detected
- Trigger class: `rat`, index `i == 1`

[中文] 方案 B 的 `labels.txt`：background、rat。触发类为 `rat`，索引 `i == 1`。

Because the camera only sees the ROI, detecting a rat is equivalent to detecting the rat inside the ROI. No `ROI` or `RIR` label is needed.

[中文] 因为相机只能看到 ROI，所以检测到大鼠就等价于检测到大鼠在 ROI 内。不需要 `ROI` 或 `RIR` 标签。

---

## Summary of Labeling Schemes

| Scheme | Rats | Labels | Trigger class | Trigger index |
|---|---|---|---|---|
| A | Rat1, Rat2 | background, rat, ROI, RIR | RIR | i == 3 |
| B | Rat3, Rat4 | background, rat | rat | i == 1 |

[中文] 标签方案汇总：方案 A 用于 Rat1、Rat2，四个标签，触发类 RIR，索引 i == 3；方案 B 用于 Rat3、Rat4，两个标签，触发类 rat，索引 i == 1。

The script logic is the same in both cases: when the trigger class is detected, P0 sends a TTL trigger to the pulse generator, and OpenMV waits 10 seconds. The only difference is which class index is used as the trigger.

[中文] 两种方案中脚本逻辑相同：检测到触发类后，P0 发送 TTL 触发信号给脉冲发生器，OpenMV 等待 10 秒。唯一区别是触发类所在的索引不同。

---

## Training Data

- Approximately 160 labeled images for training and 40 labeled images for testing.
- Each rat had a separate model trained for its specific environment.
- Each model used approximately 150–250 manually annotated frames.
- Bounding boxes were used to annotate target objects.
- Environmental factors such as lighting, angle, and camera field of view were standardized.
- For the polycarbonate box setting, a white background was used for optimal performance.

[中文] 约 160 张标注图像用于训练，40 张用于测试。每只鼠针对其特定环境单独训练模型。每个模型使用约 150–250 张手动标注帧。使用边界框标注目标物体。照明、角度和相机视野等环境因素均标准化。聚碳酸酯箱设置中使用白色背景以获得最佳性能。

---

## Evaluation

- Metric: F1 Score
- F1 threshold: 95%
- Accuracy threshold: > 95%
- Validation accuracy: 97.1%
- Test accuracy: 91.1%
- For the STI/RIR class (INT8 quantized models), F1 scores ranged from 0.979 to 1.0.
- Precision and recall were consistently above 0.95.

[中文] 评估指标：F1 分数；F1 阈值：95%；准确率阈值：>95%；验证准确率：97.1%；测试准确率：91.1%。对于 STI/RIR 类（INT8 量化模型），F1 分数为 0.979–1.0。精确率和召回率持续高于 0.95。

---

## Training Dynamics

- Models were trained for 60 epochs.
- Precision, recall, and F1 scores increased rapidly during early epochs and stabilized above 0.94 from epoch 10 onward.
- Training and validation losses decreased steadily, indicating good generalization and no overfitting.

[中文] 模型训练 60 个 epoch。精确率、召回率和 F1 分数在早期迅速上升，从第 10 个 epoch 起稳定在 0.94 以上。训练损失和验证损失稳步下降，表明泛化良好且无过拟合。

---

## Deployment

- The trained model is packaged as a `.zip` file and deployed to the OpenMV4 Cam H7 Plus via USB.
- OpenMV IDE is used to run the script that integrates object recognition with laser triggering.
- The model file `trained.tflite` and `labels.txt` must be copied to the OpenMV mass-storage device.

[中文] 训练后的模型打包为 `.zip` 文件，通过 USB 部署到 OpenMV4 Cam H7 Plus。使用 OpenMV IDE 运行集成目标识别与激光触发的脚本。模型文件 `trained.tflite` 和 `labels.txt` 必须拷贝到 OpenMV 存储设备。

---

## Latency

- End-to-end latency: approximately 770 ms.
- Includes frame capture, inference, and TTL triggering.
- Runs on a quantized INT8 model on a Cortex-M4F (80 MHz).

[中文] 端到端延迟约 770 ms，包括帧捕获、推理和 TTL 触发。在 Cortex-M4F（80 MHz）上运行量化 INT8 模型。

---

## Retraining Instructions

To retrain a model for your own experiment:

1. Decide which labeling scheme fits your task:
   - Scheme A: if the camera sees more than the ROI, label `background`, `rat`, `ROI`, `RIR`.
   - Scheme B: if the camera only sees the ROI, label `background`, `rat`.
2. Collect video frames from your behavioral setup.
3. Label objects using bounding boxes with the chosen classes.
4. Create an Edge Impulse project and select FOMO MobileNetV2 0.35.
5. Set input size to 160 × 160 × 3.
6. Train for 60 epochs or until metrics stabilize.
7. Export the model as `trained.tflite` and `labels.txt`.
8. Deploy to OpenMV Cam H7 Plus.
9. Adjust `i == 3` in `main_roi_trigger.py` to match your trigger class index.

[中文] 重新训练步骤：先决定用哪种标签方案；方案 A 用四个标签，方案 B 用两个标签；采集视频帧；用边界框标注；创建 Edge Impulse 项目，选择 FOMO MobileNetV2 0.35；输入 160×160×3；训练 60 epoch 或直到指标稳定；导出 `trained.tflite` 和 `labels.txt`；部署到 OpenMV；根据触发类索引修改 `main_roi_trigger.py` 中的 `i == 3`。

---

## Notes

- The complete per-rat models and raw video data were not preserved.
- The repository includes one example model for demonstration (STI / Scheme A).
- The model framework is scalable: a generalized model can be trained with more subjects and varied conditions.

[中文] 完整的每只鼠模型和原始视频数据未保存。仓库包含一个示例模型用于演示（STI / 方案 A）。该模型框架可扩展：通过更多受试者和不同条件可以训练通用模型。
