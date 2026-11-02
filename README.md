# PR-MARMamba

CT metal artifact reduction with **MARMamba**, multi-scale residual refinement, and adaptive gating on **AAPM CT-MAR**. Includes MARViT, MARPVT, and MARformer comparison models.

## Setup

Requires an NVIDIA GPU and a compatible CUDA/Mamba environment.

```bash
cd MARMamba
pip install -r requirements.txt
pip install tqdm pandas
```

## Data

Prepare separate train/validation/test folders. Each contains anatomy folders with paired `Baseline/` and `Target/` images: headerless `float32` raw files named with `img<ID>` and `WxHx1`. Evaluation also uses `Mask/` files and metal-count metadata.

## Usage

Run from `MARMamba/`, replacing example paths:

```bash
# Train baseline
python train_step_multi_model.py --model marmamba -train_data_dir /data/train -val_data_dir /data/val -exp_name checkpoint/baseline -num_workers 0

# Train Gate refinement with a pretrained baseline
python train_multiscale_coarse_gate.py -init_baseline /path/to/baseline_ckpt -train_data_dir /data/train -val_data_dir /data/val -exp_name checkpoint/gate

# Evaluate Gate
python test/eval_aapm_multiscale_coarse_gate.py --test_data_dir /data/test --checkpoint checkpoint/gate/best_overall_psnr_ckpt --output_dir results/gate
```

Models: `model/` · Data and metrics: `utils/` · Evaluation: `test/`

Data and checkpoints are not included. See the [original MARMamba README](MARMamba/README.md) for the source paper and original instructions; current training scripts use AAPM.
