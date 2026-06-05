# GC10-NET Dataset Repository

Processed YOLO-format datasets supporting **MMA-Net** steel-surface defect detection experiments (NEU-DET, GC10-DET, Crack-Seg) and Experiment **E6** cross-domain / leave-one-class-out evaluation.

**Repository:** https://github.com/gg123654/GC10-NET

---

## Overview

This repository bundles publicly sourced industrial defect benchmarks that have been re-organised into a unified YOLO layout with fixed train/validation/test splits. It is intended for reproducible training and evaluation of lightweight surface-defect detectors in intelligent manufacturing inspection.

| Sub-dataset | Task | Classes | Images (approx.) | Size (approx.) |
|-------------|------|---------|------------------|----------------|
| `GC10-DET/` | Object detection | 7 | 2,001 | ~801 MB |
| `NEU-DET/` | Object detection | 6 | 1,732 | ~23 MB |
| `Crack-Seg/` | Instance segmentation | 1 (crack) | 3,917 | ~112 MB |
| `E6-cache/` | Pre-built E6 training cache | 1 (defect) | — | ~1.4 GB |
| `1/` | Fig. 21 failure-case exports | — | — | ~6 MB |

**Total size:** ~2.3 GB

---

## GC10-DET

The [GC10-DET](https://github.com/lvxiaomin/GC10-DET) benchmark (Lv et al., 2020) contains grayscale steel-sheet surface images collected in real industrial environments. This release retains **seven** defect categories with bounding-box annotations in YOLO format.

| ID | Class name | Abbrev. | Description |
|----|------------|---------|-------------|
| 0 | `crescent_gap` | Cg | Crescent-shaped gap |
| 1 | `oil_spot` | Os | Oil spot |
| 2 | `punching` | Pu | Punched hole |
| 3 | `silk_spot` | Ss | Silk spot |
| 4 | `waist_fold` | Wf | Waist folding |
| 5 | `water_spot` | Ws | Water spot |
| 6 | `weld_line` | Wl | Welding line |

**Split (80 / 10 / 10):**

| Split | Images |
|-------|--------|
| train | 1,599 |
| val | 200 |
| test | 202 |

**Config file:** `GC10-DET/dataset.yaml`

**Citation:**

```bibtex
@article{lv2020deep,
  title={Deep Metallic Surface Defect Detection: The New Benchmark and Detection Network},
  author={Lv, Xiaoming and Duan, Fei and Kang, Wenxiong and Li, Jing},
  journal={Entropy},
  volume={22},
  number={8},
  pages={908},
  year={2020}
}
```

---

## NEU-DET

The [NEU-DET](https://github.com/chaoxu650/NEU-DET) benchmark is derived from the NEU surface-defect database (Song et al., 2013). Six defect types are included with YOLO bounding-box labels.

| ID | Class name |
|----|------------|
| 0 | `crazing` |
| 1 | `inclusion` |
| 2 | `patches` |
| 3 | `pitted_surface` |
| 4 | `rolled-in_scale` |
| 5 | `scratches` |

**Split (80 / 10 / 10):**

| Split | Images |
|-------|--------|
| train | 1,440 |
| val | 180 |
| test | 112 |

**Config file:** `NEU-DET/dataset.yaml`

**Citation:**

```bibtex
@article{song2013noise,
  title={Noise robustness of texture features for metal surface defect classification},
  author={Song, Kaixiang and Yan, Yunhui},
  journal={Journal of Intelligent Manufacturing},
  volume={24},
  number={2},
  pages={371--379},
  year={2013}
}
```

---

## Crack-Seg

Crack patch data in YOLO segmentation format, adapted from the [Ultralytics Crack-Seg](https://docs.ultralytics.com/datasets/segment/crack-seg/) dataset. Used for supplementary crack-detection validation (Experiment E6).

| ID | Class name |
|----|------------|
| 0 | `crack` |

**Config file:** `Crack-Seg/dataset.yaml`

---

## E6 Experiment Cache

Pre-processed YOLO caches for Experiment E6 (cross-domain generalisation):

| Sub-folder | Purpose | Config |
|------------|---------|--------|
| `E6-cache/strip/` | NEU-DET + GC10-DET merged, single-class `defect` | `e6_strip.yaml` |
| `E6-cache/loo_no_rs/` | Leave-one-class-out: NEU `rolled-in_scale` held out | `e6_loo_no_rs.yaml` |

---

## Directory Structure

```
GC10-NET/
├── README.md
├── e6_strip.yaml
├── e6_loo_no_rs.yaml
├── GC10-DET/
│   ├── dataset.yaml
│   ├── images/{train,val,test}/
│   └── labels/{train,val,test}/
├── NEU-DET/
│   ├── dataset.yaml
│   ├── images/{train,val,test}/
│   └── labels/{train,val,test}/
├── Crack-Seg/
│   ├── dataset.yaml
│   ├── images/{train,val}/
│   └── labels/{train,val}/
├── E6-cache/
│   ├── strip/
│   └── loo_no_rs/
└── 1/
    ├── Fig21.svg
    └── panels/          # qualifying failure-case source images
```

---

## YOLO Label Format

**Detection** (GC10-DET, NEU-DET, E6-cache):

```
<class_id> <x_center> <y_center> <width> <height>
```

All coordinates are normalised to \[0, 1\] relative to image width and height.

**Segmentation** (Crack-Seg): polygon vertices in normalised coordinates.

---

## Quick Start (Ultralytics YOLO)

```bash
git clone https://github.com/gg123654/GC10-NET.git
cd GC10-NET

# GC10-DET detection
yolo detect train data=GC10-DET/dataset.yaml model=yolov8n.pt epochs=100 imgsz=640

# NEU-DET detection
yolo detect train data=NEU-DET/dataset.yaml model=yolov8n.pt epochs=100 imgsz=640

# Crack segmentation
yolo segment train data=Crack-Seg/dataset.yaml model=yolov8n-seg.pt epochs=100 imgsz=640
```

> **Note:** Update the `path` field in each `dataset.yaml` if you relocate the repository.

---

## Licence & Attribution

- **GC10-DET** images and annotations: original licence from [Lv et al. (2020)](https://github.com/lvxiaomin/GC10-DET).
- **NEU-DET** images: NEU surface-defect database (Song et al., 2013); detection annotations from [chaoxu650/NEU-DET](https://github.com/chaoxu650/NEU-DET).
- **Crack-Seg**: [Ultralytics Crack-Seg](https://docs.ultralytics.com/datasets/segment/crack-seg/) (AGPL-3.0).

This repository provides **processed splits and YOLO packaging only**. Please cite the original sources when using these datasets in publications.

---

## Contact

For questions regarding the MMA-Net manuscript experiments or split indices, please contact the corresponding author.
