# OpenMV Deployment

This folder contains the OpenMV scripts, labels, and example model for closed-loop optogenetic stimulation.

[中文] 本文件夹包含闭环光遗传刺激所需的 OpenMV 脚本、标签和示例模型。

---

## Files

- `main_roi_trigger.py`: Main script for ROI/event-triggered stimulation.
- `ei_object_detection.py`: Edge Impulse object detection example for testing the model.
- `labels.txt`: Class labels. Order: `background`, `RAT`, `ROI`, `STI`.
- `trained.tflite`: Example FOMO MobileNetV2 model (STI version).

[中文] 文件说明：`main_roi_trigger.py` 主脚本；`ei_object_detection.py` 调试示例；`labels.txt` 标签；`trained.tflite` 示例模型（STI 版本）。

---

## Label Configuration

Default order:

```text
0 background
1 RAT
2 ROI
3 STI
```

Trigger class: `STI`, index `3`.

Script uses:

```python
if (i == 3):
    p.high()
    sensor.skip_frames(time=10000)
```

If your label order is different, change `i == 3` to the index of your trigger class.

[中文] 默认顺序为 `background, RAT, ROI, STI`，触发类 `STI` 在索引 3，脚本用 `i == 3`。如果你的顺序不同，请修改 `i == 3` 为触发类所在行号。

---

## Trigger Logic

When the trigger class `STI` is detected:

1. `p.high()` sends a TTL trigger signal to the pulse generator.
2. `sensor.skip_frames(time=10000)` makes OpenMV wait 10 seconds.
3. During these 10 seconds, the pulse generator outputs 3 seconds of laser stimulation followed by 7 seconds of recovery.

[中文] 触发逻辑：检测到触发类 `STI` 后，P0 发送 TTL 触发信号给脉冲发生器，然后 OpenMV 等待 10 秒。这 10 秒包含 3 秒激光刺激和 7 秒恢复。

---

## Deployment

1. Install OpenMV IDE.
2. Copy `trained.tflite` and `labels.txt` to the OpenMV mass-storage device.
3. Copy `main_roi_trigger.py` to the OpenMV mass-storage device.
4. Wire `P0` to the pulse generator trigger input and `GND` to ground.
5. Run the script from OpenMV IDE.

[中文] 部署步骤：安装 OpenMV IDE；拷贝模型和标签；拷贝主脚本；连接 P0 和 GND；运行脚本。

---

## Notes

- The included `trained.tflite` and `labels.txt` are both the STI version and are matched.
- The complete per-rat models and raw data were not preserved.
- To retrain, use Edge Impulse with FOMO MobileNetV2 0.35 and the label definitions above.

[中文] 注意：包含的 `trained.tflite` 和 `labels.txt` 均为 STI 版本，二者配套；完整每只鼠模型和原始数据未保存；如需重新训练，请使用 Edge Impulse 和 FOMO MobileNetV2 0.35，并按照上述标签定义。
