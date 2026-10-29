# Fashion-MNIST Architectures

How much does the way a network *sees* an image matter?
This project trains five architecture families from scratch on Fashion-MNIST under one shared data split, training protocol and evaluation, and compares them on accuracy, macro-F1, size, speed and failure modes.

| Family | Input representation |
|---|---|
| Linear / softmax | flattened pixels (784) |
| MLP | flattened pixels (784) |
| CNN | 2-D image, 1 × 28 × 28 |
| LSTM / GRU | sequence of 28 rows, 28 pixels each |
| Transformer | 49 patches of 4 × 4 pixels with positional encoding |

The project started as coursework for a deep learning course at Ho Chi Minh City University of Technology and grew into a self-contained study.

## Repository layout

| Path | Content |
|---|---|
| `notebooks/01_linear_mlp_baselines.ipynb` | EDA, data pipeline, training engine, linear and MLP baselines |
| `notebooks/02_architecture_comparison.ipynb` | Full study: all nine models, three seeds, error analysis and interpretability |
| `AI_USAGE.md` | How AI tools were used in this project |

## Results

Mean ± standard deviation over three seeds on the official test set, trained for 20 epochs on a Tesla T4.

| Model | Params | Test acc. (%) | Test macro-F1 | Train time (s) |
|---|---:|---:|---:|---:|
| CNN (2 stages, 32-64 channels) | 468,010 | 93.93 ± 0.16 | 0.9391 ± 0.0015 | 102.6 |
| GRU (rows, 2 × 128) | 161,034 | 91.01 ± 0.33 | 0.9098 ± 0.0034 | 88.7 |
| LSTM (rows, 2 × 128) | 214,282 | 90.39 ± 0.12 | 0.9038 ± 0.0011 | 89.8 |
| MLP-C (784-512-256-10) | 537,354 | 89.93 ± 0.08 | 0.8992 ± 0.0005 | 81.7 |
| MLP-B (784-256-10) | 204,042 | 89.27 ± 0.24 | 0.8925 ± 0.0023 | 74.1 |
| MLP-A (784-128-10) | 102,026 | 88.87 ± 0.09 | 0.8885 ± 0.0009 | 73.3 |
| Transformer (4 × 4 patches) | 139,018 | 88.88 ± 0.21 | 0.8885 ± 0.0023 | 220.9 |
| Transformer, no positional encoding | 135,818 | 85.44 ± 0.34 | 0.8541 ± 0.0034 | 219.6 |
| Linear | 7,850 | 82.60 ± 0.15 | 0.8244 ± 0.0016 | 66.3 |

Key findings:

- **Spatial structure beats parameter count.** With 87% of the parameters of MLP-C, the CNN is 3.99 points better in macro-F1.
- **Row sequences help moderately.** The LSTM and GRU beat every MLP with less than half of MLP-C's parameters, but stay well below the CNN.
- **Attention needs to be told where things are.** Removing positional encoding costs the Transformer 3.43 points; under a 20-epoch budget the Transformer only matches the smallest MLP.
- **The GRU is enough for 28 steps.** It is slightly more accurate than the LSTM with 75% of the parameters.
- **One error group dominates.** Confusion among T-shirt/top, Pullover, Coat and Shirt is the largest error group for every model; Shirt is the hardest class.

## Highlights

- **Leakage-aware data pipeline:** stratified, seeded train/validation split; normalization statistics from the training split only; exact-duplicate check across splits.
- **Honest comparison:** every model is trained with three seeds and reported as mean ± standard deviation; checkpoints are selected on validation macro-F1, and the test set is evaluated once per run.
- **Meaningful cost numbers:** a tensor-backed `Dataset` removes the PIL bottleneck, and inference latency is measured on device-resident inputs.
- **Error analysis:** errors are grouped and quantified; the dominant group (T-shirt / Pullover / Coat / Shirt confusion) is tracked across models.

## How to run

1. Open a notebook in Google Colab and select `Runtime -> Change runtime type -> T4 GPU`.
2. Run all cells.
   The dataset is downloaded automatically, and the split seed, training seeds and hyperparameters are fixed in the notebook.
3. The run writes `results/` (configuration, environment, split indices, per-run metrics, training histories and figures) and `checkpoints/` (best checkpoint per model and seed).

The results and all 27 checkpoints of the three-seed Tesla T4 run are available as [`a1_results.zip`](https://drive.google.com/file/d/150g5cS32hSYkwV9tk13GSz8qnhv4u55V/view?usp=sharing).

To run locally instead, install Python 3.10+ with `torch`, `torchvision`, `numpy`, `pandas`, `matplotlib`, `seaborn` and `scikit-learn`, then open the notebook with Jupyter.

## Reproducibility

- **Data split:** stratified 50,000 / 10,000 train/validation split of the official training set (split seed 42), plus the official 10,000-image test set.
- **Seeds:** training seeds 42, 123 and 2026 for every model.
- **Environment:** software versions and hardware are recorded automatically in `results/environment.json`.

## Data

Fashion-MNIST is © Zalando SE and distributed under the MIT License.
