# Nifty50 Implied Volatility Surface Reconstruction

## Project Overview
This repository contains my submission for the Finance Club Open Project (PS1/PS2). The objective of this project was to reconstruct missing implied volatility (IV) values across a minute-by-minute Nifty50 options chain dataset.

## Performance
* **Final MSE Score:** 0.000163

## Quantitative Methodology
Rather than relying on basic machine learning or linear interpolation, this pipeline treats the options chain as a dynamic, two-dimensional financial surface. The core architecture includes:

1. **Moneyness Space Transformation (The Horizontal Sweep):** Absolute strike prices were mapped relative to the underlying Nifty50 index ($Strike / Underlying\ Price$). This stabilizes intraday price swings and ensures the volatility "smile" remains stationary before mathematical interpolation.
2. **Institutional Spatial Interpolation:** Missing internal strike gaps were interpolated cross-sectionally using a Natural Cubic Spline (`bc_type='natural'`). This forces the extreme out-of-the-money (OTM) tails into a linear momentum trajectory, capturing tail-risk premium without exponential overshooting.
3. **Spatio-Temporal Smoothing (The Vertical Sweep):** A Savitzky-Golay filter (`window_length=5, polyorder=2`) was applied vertically down the time axis for every specific strike. This successfully eliminated high-frequency microstructure noise and bid-ask bounce without lagging the underlying trend.

## Data Protection Strategy
To ensure zero degradation of the Mean Squared Error (MSE), the pipeline is structurally hardcoded to retain 100% of the true, unaltered market observations. Smoothing algorithms were strictly applied only to missing values.

## Files
* `notebook2d7afd3db0.ipynb`: The complete data processing, interpolation, and temporal smoothing pipeline.
* `submission.csv`: The final predicted values mapped back to the required submission format.
