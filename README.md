# GrapePBC

Source code accompanying **GrapePBC: Adaptive-Resolution Counting of Grape Berries per Bunch in Field Images**.

GrapePBC predicts berry locations at low resolution, selects regions for high-resolution processing, fuses their count estimates, and assigns the retained berry centers to individual bunches. Fragmented bunch detections are merged while preserving the whole-image count.

## Source archive

Download and extract `grapepbc-source.zip`. It contains the Python model and routing implementation, inference server, evaluation scripts, Android application source, and frozen calibration and merging settings.

| Directory | Contents |
| --- | --- |
| `src/geopriv` | Point and density models, data processing, routing, assignment, and metrics |
| `scripts` | Model training, calibration, and imported runtime dependencies |
| `server` | Image inference and the mobile-facing web service |
| `research/scripts` | Per-bunch evaluation and ablation evaluation |
| `configs` | Count calibration, count-bin fusion settings, and the final merging classifier |
| `mobile/android` | Android application and build files |
| `mobile/connection` | Computer pairing implementation |

Internal module names are preserved for training and inference dependency compatibility. Checkpoint files, cached predictions, and the ViViD-5K images are distributed separately from this source snapshot.

## Data

ViViD-5K is available from its original authors at https://huggingface.co/datasets/XZhi/ViViD-5k . The dataset provides the official training, validation, and test splits. Dataset licensing and attribution follow the original dataset record. The original reference implementation is https://github.com/tong-xz/GrapeSAM .

## Environment and execution

Install the Python dependencies with `python -m pip install -r requirements.txt`. Full adaptive-resolution inference requires a CUDA-compatible PyTorch installation. The original experiments used an RTX 4060 Laptop GPU with 8 GB of video memory.

The Python files use the release layout rooted at `D:/GrapePBC_release`. Internal module filenames are preserved for dependency compatibility. Before running the scripts on a different computer, configure the paths to the dataset, model checkpoints, calibration records, and output directories. The checkpoint locations are specified in `server/engine.py` and `server/full_runtime.py`. Training scripts under `scripts` define the corresponding architectures. The source archive has been checked for Python syntax and local module dependencies. End-to-end inference from a clean checkout requires the trained checkpoints and experiment records referenced by these modules.

The final merge classifier is `configs/merge_model_cv.json` and uses a probability threshold of 0.50. Its features and coefficients were fitted on training images. The four count-bin fusion settings are stored in `configs/ccf_train_fit.json`. Use the official dataset split files during evaluation.

## Manuscript

Lingfeng Hua, Yu Ding, Yi Xuan Gao, Chundi Yuan, and Zhaoyu Zhai. *GrapePBC: Adaptive-Resolution Counting of Grape Berries per Bunch in Field Images*.

Correspondence: zhaoyu.zhai@njau.edu.cn.
