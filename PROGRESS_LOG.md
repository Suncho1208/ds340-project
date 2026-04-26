## Project Progress Log

### 2026-04-26 - Local RNN vs LSTM experiment

What we changed:
- Updated `02_LSTM_Models.ipynb` to run from local project paths (`data/` and `results/`) instead of Google Drive.
- Added/kept side-by-side sequence model comparison workflow (LSTM and vanilla RNN) under the same data split and training setup.
- Fixed notebook execution blockers for local runs:
  - Removed hard dependency on `xgboost` import for this notebook path.
  - Updated backtest helper to support both prediction key styles (`probabilities` and `probs`).
  - Made regime backtest robust when `regime_top_sectors.json` stores a single ticker string.

How we ran it:
- Executed full notebook locally: `02_LSTM_Models.ipynb` (CPU runtime on this machine).
- Output notebook generated: `02_LSTM_Models.executed.ipynb`.

Key findings from this run:
- Classification quality:
  - LSTM beats majority baseline in **2/11** sectors.
  - RNN beats majority baseline in **1/11** sectors.
  - Average sector accuracy:
    - LSTM: **0.477**
    - RNN: **0.518**
    - Average majority baseline: **0.663**
- Portfolio/backtest performance over test period (`results/lstm_backtest_results.csv`):
  - LSTM Direct: **Sharpe 0.533**, **Total Return 78.10%**
  - RNN Direct: **Sharpe 0.355**, **Total Return 59.67%**
  - SPY Benchmark: **Sharpe 0.669**, **Total Return 107.09%**
  - Macro Regime Rotation: **Sharpe 0.863**, **Total Return 220.21%**

Interpretation for presentation:
- On this cleaned local dataset and current setup, **RNN did not outperform LSTM on portfolio metrics**.
- Both sequence models underperform simple baselines in direction-classification terms relative to majority class baseline.
- The current direct-sequence modeling setup likely needs further feature/target engineering or architecture/hyperparameter changes before it can be a primary final model.

Artifacts produced/updated:
- `results/rnn_training_curves.png`
- `results/sequence_models_accuracy_comparison.png`
- `results/rnn_probabilities_over_time.png`
- `results/rnn_accuracy_by_regime.png`
- Updated LSTM comparison outputs and `results/lstm_backtest_results.csv`

Next experiment ideas:
- Try a feedforward neural network (MLP) on tabular lag features as a simpler baseline against LSTM/RNN.
- Add class-imbalance handling (class weights or focal loss).
- Reduce model complexity and test shorter windows (e.g., 3/6/9 months).
- Compare predicted probabilities with calibration metrics (Brier score) in addition to accuracy.
