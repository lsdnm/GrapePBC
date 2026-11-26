# GrapePBC

Implementation of **GrapePBC: Adaptive-Resolution Counting of Grape Berries per Bunch in Field Images**.

GrapePBC combines low-resolution point prediction, adaptive high-resolution processing, count-bin fusion, unique berry assignment, and bunch instance merging. It produces the whole-image count, per-bunch counts, berry center coordinates, and bunch locations.

## Files

Download `grapepbc-source.zip` and extract its `grapepbc` directory. The package contains the `grapepbc` Python module, inference configuration, an evaluation script, the Flask service, and Android source code. Model weights are provided in `grapepbc-weights.zip`. Extract its `checkpoints` directory into the project root.

```text
grapepbc/
  grapepbc/       Python implementation
  configs/        Count calibration, fusion settings, and merging classifier
  checkpoints/   Trained model weights
  server/         Flask service and web interface
  mobile/         Android client and pairing service
  evaluation.py   Bunch matching and counting metrics
```

## Installation

Use Python 3.10 or later and install a CUDA-compatible build of PyTorch. The reference environment uses PyTorch 2.6 and an RTX 4060 Laptop GPU with 8 GB of video memory.

```bash
python -m pip install -r requirements.txt
python -m grapepbc path/to/image.jpg
```

Full adaptive-resolution inference requires CUDA. Results are saved under `results`. Set `GRAPEPBC_CHECKPOINT_DIR`, `GRAPEPBC_CONFIG_DIR`, or `GRAPEPBC_RESULTS_DIR` to use different locations. Run the web service from the project root with `python -m server.app`. The service listens on port 7860. The Android source is in `mobile/android` and uses the pairing page provided by the service.

## Data

The official ViViD-5K images, berry points, bunch masks, and split files are available at https://huggingface.co/datasets/XZhi/ViViD-5k . Follow the dataset authors' licensing and citation requirements. The original GrapeSAM implementation is https://github.com/tong-xz/GrapeSAM .

## Evaluation

`evaluation.py` matches predicted and annotated boxes one-to-one at IoU 0.5 and reports matched-bunch MAE and total error per annotated bunch. Predictions are a JSON mapping from image IDs to inference result objects. Annotations are a JSON mapping from the same image IDs to lists of bunches, each containing a `bbox` array in `[x0,y0,x1,y1]` format and a `points` array of berry centers.

```bash
python evaluation.py predictions.json annotations.json
```

Count-bin fusion uses the initial whole-image LR count. The gains are 0.500, 0.500, 0.625, and 0.375 for the four count bins, with thresholds of 5, 5, 15, and 0 berries. Instance merging uses the fitted classifier in `configs/instance_merging.json` at probability threshold 0.50.

## Authors

Lingfeng Hua, Yu Ding, Yi Xuan Gao, Chundi Yuan, and Zhaoyu Zhai.

Correspondence: zhaoyu.zhai@njau.edu.cn.
