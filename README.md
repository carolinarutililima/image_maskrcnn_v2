# image_maskrcnn_v2

This project trains and evaluates **Mask R-CNN** models on histology images to detect cancerous cells in whole-tissue slides.

**Dataset (Roboflow):**  
[https://universe.roboflow.com/carolina-rutili-de-lima/many_toget](https://universe.roboflow.com/carolina-rutili-de-lima/many_toget)

Export your copy in **COCO** format and place it under `datasets/` (see `datasets/README.md`).

---

## Requirements

- **Python 3.6–3.8** (TensorFlow 1.x is not supported on Python 3.11+).
- **TensorFlow 1.x** and **Keras 2.x** — see `requirements.txt`.
- **NVIDIA GPU + CUDA/cuDNN** recommended for training and inference at reasonable speed.
- A **C compiler** for `pycocotools` (e.g. Xcode CLT on macOS).

```bash
pip install -r requirements.txt
```

---

## Project layout

| Path | Purpose |
|------|---------|
| `mrcnn/` | Mask R-CNN library (Matterport) + custom wide-ResNet variants |
| `mrcnn/model.py` | Default backbone (used by `Evaluate.ipynb`, `Inference.ipynb`) |
| `mrcnn/model_203.py` | Wide-ResNet variant for training (`Train+Test_New.ipynb`) |
| `mrcnn/model_wide_ResNet.py`, `mrcnn/last_model.py` | ResNeXt + soft-NMS variant (`only_Test_New_v2.ipynb`) |
| `mrcnn/model_143.py` | Standard `model.py` copy for alternate experiments |
| `samples/coco/` | COCO helpers (`coco.py`; used by notebooks) |
| `Train+Test_New.ipynb` | Train + validate with `model_203` |
| `only_Test_New_v2.ipynb` | Test with `model_wide_ResNet` / `last_model` |
| `Evaluate.ipynb` | Metrics / COCO-style evaluation with `mrcnn.model` |
| `Inference.ipynb` | Run inference on images |

Run Jupyter **from this repository root** so `import mrcnn` and `from samples.coco import coco` resolve.

---

## Weights

Checkpoints are **not** committed (see `.gitignore`). Point each notebook to your `.h5` under `logs/` or train from COCO/ImageNet weights as in the Matterport workflow.

---

## License

Mask R-CNN core code is **MIT** (Matterport). See `LICENSE-Matterport-MIT.txt`. Custom model files follow the same upstream license terms.
