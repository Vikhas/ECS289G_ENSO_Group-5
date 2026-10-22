# ECS 289G Project 1 - Group 5 (CESM2)

ENSO forecasting. We train neural networks on CESM2 output only, predict the tropical Pacific SST anomaly map, and
compute the Niño 3.4 index 3 months ahead from the predicted map.

## Notebooks

| Notebook | Model |
|---|---|
| `notebooks/1_unet_3runs.ipynb` | U-Net trained on 3 CESM2 historical runs (r3, r4, r6) |
| `notebooks/2_resnet_23runs.ipynb` | ResNet trained on all 23 CESM2 runs |
| `notebooks/3_unet_23runs_final.ipynb` | U-Net trained on all 23 runs (final model), LIM baseline, forecast and model comparison |

Each notebook has two parts:

- Part A downloads CESM2 from Pangeo, trains 5 networks (different seeds) and scores them on the CESM2 test years.
  We ran it on Kaggle (T4 GPU).
- Part B scores the models on the real-world file `ecs289g_realworld_inputs_CESM2.nc` and makes the forecast.
  We ran it locally on CPU with the models saved in Part A.


## Data split

- Train: 1850-1955 of the 11 historical runs, plus 2027-2100 of the 12 SSP runs (r4, r10, r11)
- Validation: r4, 1961-1980
- Test: r4, 1986-2014

No simulated years from 2016-2026 are used for training or validation.

## Results at lead 3 (relative Niño 3.4, correlation / RMSE in °C)

| Model | CESM2 test 1986-2014 | Real-world data 2016-2026 |
|---|---|---|
| U-Net, 3 runs | 0.96 / 0.45 | 0.87 / 0.47 |
| U-Net, 23 runs (final) | 0.97 / 0.41 | 0.89 / 0.50 |
| ResNet, 23 runs | 0.97 / 0.42 | 0.87 / 0.50 |
| LIM | 0.94 / 0.52 | 0.88 / 0.48 |
| Persistence | 0.79 / 1.02 | 0.69 / 0.60 |

## Forecast

From data up to Sep 2026, the relative Niño 3.4 forecast for Dec 2026 is +3.4 ± 0.3 °C (spread of the 5 seeds).
Oct 2026: +3.4 °C, Nov 2026: +3.5 °C.

- `forecast/forecast_nino34_realworld.csv`: Niño 3.4 forecasts for the last 6 start months, leads 1-12
- `forecast/forecast_sst_fields_realworld.nc`: predicted SST anomaly maps from the Sep 2026 start, leads 1-12
