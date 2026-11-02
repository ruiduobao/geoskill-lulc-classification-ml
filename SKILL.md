---
name: geoskill-lulc-classification-ml
description: '多波段特征逐像元 RandomForest/梯度提升土地覆被分类，含精度评估、面积统计与众数滤波去盐噪'
---

# 机器学习土地覆被分类 | ML Land Cover Classification

Perform pixel-by-pixel supervised classification of multispectral imagery to produce a land use/land cover (LULC) class raster. Feature engineering is based on 6-band reflectance, from which NDVI and the local-variance texture of the near-infrared band are derived to form a per-pixel feature vector. The classifier supports Random Forest (`rf`) and Gradient Boosting (the `xgboost` option, implemented offline with scikit-learn's `GradientBoostingClassifier`).

Typical applications: regional land use/land cover mapping, rapid inventory of urban and cropland distribution, and pre-classification for change detection. After classification, an optional 3×3 majority filter removes salt-and-pepper noise, and overall accuracy (OA), Kappa and per-class producer/user accuracy are reported on a held-out validation set for quality control.

Synthetic mode automatically generates labeled multispectral scenes (water / vegetation / cropland / built-up / bare soil, each with a characteristic spectrum) fully offline, suitable for teaching, pipeline validation and unit testing.

## Dependencies / 依赖

```bash
pip install 'numpy' 'rasterio' 'scipy' 'scikit-learn'
```

## Usage / 使用方法

### Basic Usage

```bash
python geoskill-lulc-classification-ml.py --bbox 116.0 39.0 117.0 40.0 --n-classes 5 --method rf
```

### Example 1 (synthetic data, offline)

```bash
python geoskill-lulc-classification-ml.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2 (gradient boosting classifier)

```bash
python geoskill-lulc-classification-ml.py --bbox 121.0 31.0 122.0 32.0 --synthetic --method xgboost --output-dir ./out
```

### Example 3 (read a local multispectral image)

```bash
python geoskill-lulc-classification-ml.py --input scene_sr.tif --n-classes 5 --output-dir ./out
```

### Example 4 (disable majority filtering)

```bash
python geoskill-lulc-classification-ml.py --bbox 116 39 117 40 --synthetic --no-filter --output-dir ./out
```

### Example 5 (3 classes + custom validation fraction)

```bash
python geoskill-lulc-classification-ml.py --bbox 116 39 117 40 --synthetic --n-classes 3 --test-fraction 0.3 --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `lulc_classified.tif` | GeoTIFF (int32) | Per-pixel classification result (class index) |
| `accuracy.json` | JSON | OA, Kappa, confusion matrix, per-class accuracy |
| `area_stats.json` | JSON | Per-class pixel count, share, area (km²) |
| `output-manifest.json` | JSON | Run manifest |

Class indices: 0=water, 1=vegetation, 2=cropland, 3=built_up, 4=bare_soil.

## Data Source / 数据源 / Source

- Local multispectral GeoTIFF (surface reflectance, ≥4 bands);
- `--synthetic` generates a physically consistent scene offline (no network, no account required).

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is performed locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
