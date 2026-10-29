# AI Usage

This file documents how generative AI tools were used in this project.

## Claude Code

| Field | Entry |
|---|---|
| Tool | Claude Code (Anthropic), model Claude Opus 5.5 |
| Used by | Dương Nguyễn Tấn Đạt |
| Date | 28 October 2026 |
| Purpose | Reviewing the first version of the notebook; revising the data split, EDA, timing and error-analysis code |
| Affected files | `notebooks/01_linear_mlp_baselines.ipynb` |
| AI contribution | Identified missing EDA, test-set-based model selection, data-loading-bound timing and order-dependent seeding; proposed and implemented the fixes |
| How the output was verified | Re-ran the notebook, read the changed code and compared the PyTorch API usage with the official documentation |

| Field | Entry |
|---|---|
| Tool | Claude Code (Anthropic), model Claude Opus 5.5 |
| Used by | Dương Nguyễn Tấn Đạt |
| Date | 29 October 2026 |
| Purpose | Implementing the CNN, LSTM/GRU and Transformer models, the full comparison, error analysis and interpretability sections |
| Affected files | `notebooks/02_architecture_comparison.ipynb`, `README.md` |
| AI contribution | Wrote the model classes, experiment loop, analysis code and the discussion text; revised the discussion after the three-seed GPU run so that every claim matches the measured numbers |
| How the output was verified | Re-ran the full notebook on a Google Colab Tesla T4 GPU with three seeds and checked that it completed without errors; checked the parameter counts by hand; compared the PyTorch API usage with the official documentation |

## Other tools

None.
Claude Code with Claude Opus 5.5 was the only AI tool used in this project.
