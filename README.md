# Quantization and Pruning of Compact 1D-CNNs for Edge-Based Power Quality Disturbance Classification

Code, generator and results for the paper:

> Rabbuni Merugumala, "Quantization and Pruning of Compact 1D-CNNs for Edge-Based Power Quality Disturbance Classification: A Comparative Study under Noise", manuscript submitted to *Electrical Engineering* (Springer), 2026.

The study compares post-training quantization (PTQ), quantization-aware training (QAT), structured filter pruning, unstructured magnitude pruning and their combination on two compact 1D-CNNs: a plain 1D-CNN and 1D-CCANet-3. Every variant is evaluated as a TensorFlow Lite model at six noise levels and under grid-frequency deviation, with five random seeds.

## Repository contents

| Path | Content |
|---|---|
| `PQD_compression_full_experiment_v2.ipynb` | Full experiment: data generation, training, compression, TFLite evaluation, tables and figures |
| `results/` | All result files (CSV, JSON) used in the paper, including per-seed results and the PTQ diagnosis |
| `figures/` | Figures used in the paper (PNG, 600 dpi) |
| `models/` | Trained TFLite models for every variant and seed |

The dataset is not stored here. It is regenerated exactly from the generator code and the fixed seeds in the notebook.

## Dataset

- 16 disturbance classes. The mathematical models and parameter ranges are from Machlev et al., *Electric Power Systems Research* 195 (2021) 107152, based on IEEE Std 1159-2019.
- Signals: 50 Hz, 3.2 kHz sampling, 640 samples (10 cycles), random initial phase.
- Train / validation: 4000 / 500 signals per class. AWGN at 20–50 dB for 90% of signals, 10% clean.
- Test: 300 signals per class at clean, 50, 40, 30, 20 and 10 dB, plus 49.5–50.5 Hz at 30 dB.
- Data seeds: train 2026, validation 2027, test 2028+. Model seeds: 42–46.

## How to reproduce

1. Open the notebook on Kaggle (GPU T4 or P100, Internet on) or on a local machine with a GPU.
2. Run all cells. A full run takes about 3–4 hours on 2 × T4.
3. The outputs are written to `PQD_paper/results`, `PQD_paper/figures` and `PQD_paper/models`.

Tested with Python 3.13, TensorFlow 2.21, tf-keras 2.21 and TensorFlow Model Optimization 0.8.1.

## Main results (macro-F1, mean over 5 seeds)

| Model | Variant | Clean | 20 dB | TFLite size |
|---|---|---|---|---|
| 1D-CNN | FP32 | 99.06% | 91.24% | 43.05 KB |
| 1D-CNN | QAT-INT8 | 98.38% | 90.40% | 17.82 KB |
| 1D-CNN | SP-50 + QAT | 94.32% | 85.48% | 9.36 KB |
| 1D-CCANet-3 | FP32 | 95.97% | 85.29% | 78.65 KB |
| 1D-CCANet-3 | PTQ-INT8 | 42.50% | 38.85% | 26.66 KB |
| 1D-CCANet-3 | QAT-INT8 | 93.80% | 84.20% | 26.96 KB |

See the paper and `results/Table_main.csv` for all variants and metrics.

## Citation

If you use this code, please cite the paper (the full reference will be added after publication) and the generator of Machlev et al. (2021).

## License

MIT License. See `LICENSE`.

## Contact

Rabbuni Merugumala, Department of EEE, Seshadri Rao Gudlavalleru Engineering College, India. rabbuni144@gmail.com. ORCID 0009-0003-5671-7457
