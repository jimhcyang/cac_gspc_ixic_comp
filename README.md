# SP500 / NASDAQ Rolling Forecast Comparison

Two aligned Jupyter notebooks for side-by-side deep learning comparison:
- `index_forecasting_tensorflow.ipynb`
- `index_forecasting_pytorch.ipynb`

Both notebooks are identical except for framework backend calls (TensorFlow vs PyTorch).

## What This Project Does

- Forecasts next-day index close using rolling `plus1` windows.
- Runs two separate pipelines:
  - `SP500` (features from SP500 technicals only)
  - `NASDAQ` (features from NASDAQ technicals only)
- Models:
  - `MLP`, `RNN`, `LSTM`, `GRU`, `TCN`, `Transformer`
- Compares two window setups:
  - `W=10`, `test_ratio=0.10`
  - `W=20`, `test_ratio=0.05`
- Hyperparameter selection default: **MAPE** (R2 reported as secondary metric).

## Repository Setup

```bash
cd /Users/jimyhc/Desktop/research/conference/root
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
```

Optional Jupyter kernel registration:

```bash
python -m ipykernel install --user --name conference-root --display-name "Python (conference-root)"
```

## Running the Notebooks

```bash
jupyter notebook
# or
jupyter lab
```

Recommended first run:
1. Open one notebook.
2. `Kernel -> Restart & Run All`.
3. Validate smoke test output first, then run full sweeps.

## Data Caching

Raw downloaded index data is cached locally:
- `data_cache/index_ohlcv/SP500_<start>_<end>.csv`
- `data_cache/index_ohlcv/NASDAQ_<start>_<end>.csv`

Behavior:
- Reads local cache first.
- If cache is missing/invalid, it redownloads and replaces.

## Output Layout

### Rolling logs (per framework/model/window/index)

`dl_logs_index/<framework>/<dataset>/w<window>/<MODEL>/<MODEL>_<dataset>.csv`

Examples:
- `dl_logs_index/tensorflow/SP500/w10/GRU/GRU_SP500.csv`
- `dl_logs_index/pytorch/NASDAQ/w20/TCN/TCN_NASDAQ.csv`

### Aggregated scoring outputs

- `results/index_config_scores_tensorflow.csv`
- `results/index_config_scores_pytorch.csv`
- `results/index_best_configs_tensorflow.csv`
- `results/index_best_configs_pytorch.csv`

### Figures for paper/report

- `results/figures_tensorflow/`
- `results/figures_pytorch/`

Includes heatmaps, grouped bars, tuning-vs-eval scatter, and frontier plots.

## Notes

- Date span: 2024-01-01 to 2025-12-31.
- Indicator computation uses a 6-month historical buffer before trimming to analysis range.
- `SMOKE_TEST` controls are defined near the top of each notebook for quick validation.
