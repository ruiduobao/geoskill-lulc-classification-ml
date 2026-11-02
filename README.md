# Machine Learning Land Cover Classification (geoskill-lulc-classification-ml)

> Per-pixel land cover classification from multi-band features using Random Forest / gradient boosting, with accuracy assessment, area statistics, and majority filtering to remove salt-and-pepper noise

---

## 1. Overview

Performs per-pixel supervised classification of multispectral imagery, producing land cover (LULC) class rasters. Feature engineering builds on the 6-band reflectance base, deriving NDVI and the local-variance texture of the near-infrared band to form a per-pixel feature vector. Classifiers support random forest (`rf`) and gradient boosting (`xgboost` option, with an offline equivalent implemented via scikit-learn's `GradientBoostingClassifier`). Typical applications: regional land use / land cover mapping, rapid inventory of urban and agricultural distribution, and preliminary classification feeding change detection. After classification, an optional 3×3 majority filter removes salt-and-pepper noise, and overall accuracy (OA), Kappa, and per-class producer's / user's accuracy are reported on a held-out validation set for quality control. Synthetic mode automatically generates labeled multispectral scenes (water / vegetation / cropland / built-up / bare soil, each with characteristic spectra), fully offline and suitable for teaching, pipeline validation, and unit testing.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-lulc-classification-ml.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `lulc_classified.tif` | GeoTIFF (int32) | Per-pixel classification result (class indices) |
| `accuracy.json` | JSON | OA, Kappa, confusion matrix, per-class accuracy |
| `area_stats.json` | JSON | Per-class pixel count, proportion, area (km²) |
| `output-manifest.json` | JSON | Run manifest |

Class indices: 0=water, 1=vegetation, 2=cropland, 3=built_up, 4=bare_soil.


## 6. Technical Principle

(See SKILL.md for details.)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
