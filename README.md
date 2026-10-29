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
| `AI_USAGE.md` | How AI tools were used in this project |

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

To run locally instead, install Python 3.10+ with `torch`, `torchvision`, `numpy`, `pandas`, `matplotlib`, `seaborn` and `scikit-learn`, then open the notebook with Jupyter.

## Reproducibility

- **Data split:** stratified 50,000 / 10,000 train/validation split of the official training set (split seed 42), plus the official 10,000-image test set.
- **Seeds:** training seeds 42, 123 and 2026 for every model.
- **Environment:** software versions and hardware are recorded automatically in `results/environment.json`.

## Data

Fashion-MNIST is © Zalando SE and distributed under the MIT License.
