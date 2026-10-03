# 🏠 House Price Prediction System

A complete machine-learning project that predicts residential house prices.
**Backend and frontend live in one Python file** (`house_price_app.py`).

| Layer | Technology |
|-------|-----------|
| Backend (data + ML) | pandas, NumPy, scikit-learn, joblib |
| Frontend (web UI) | Streamlit, Matplotlib, Seaborn |

## Project structure

```
house_price_prediction/
├── house_price_app.py                  # backend + frontend (single file)
├── house_price_regression_dataset.csv  # dataset (1,000 houses)
├── requirements.txt                    # Python dependencies
├── README.md                           # this file
└── House_Price_Prediction_Report.docx  # full project report
```

`house_price_model.joblib` (the trained model) is created automatically the first time you run the app.

## Setup and run

Requires **Python 3.9+**.

```bash
# 1. (recommended) create a virtual environment
python -m venv venv
venv\Scripts\activate          # Windows
source venv/bin/activate       # macOS / Linux

# 2. install dependencies
pip install -r requirements.txt

# 3. start the app
streamlit run house_price_app.py
```

The app opens at <http://localhost:8501>. Keep the CSV in the same folder as the script
(or upload your own CSV from the sidebar).

## Features

- **Home** - dataset KPIs and an overview of the pipeline.
- **Data Explorer** - raw data, statistics, histograms, correlation heatmap, scatter/box plots.
- **Model Performance** - Linear, Ridge, Random Forest and Gradient Boosting compared using
  R², MAE, RMSE and 5-fold cross-validation; actual-vs-predicted, residual and feature-importance plots.
- **Predict Price** - enter house details to get an estimate with a 95% range, comparison with the
  market average, and price per sq.ft. Includes **batch prediction** from a CSV file.
- **Currency switch** - view prices in USD or in INR (lakh / crore); the exchange rate is editable.

## How it works

1. `load_data` / `clean_data` - read the CSV, validate columns, drop duplicates and missing values.
2. `add_features` - engineered features `House_Age`, `Total_Rooms`, `Sqft_per_Bedroom`
   (part of the sklearn `Pipeline`, so they are applied automatically at prediction time).
3. `train_and_evaluate` - 80/20 split, 5-fold CV, picks the model with the lowest CV RMSE.
4. `predict_price` / `predict_batch` - use the best model for new houses.

## Results (hold-out test set, 200 houses)

| Model | R² | MAE | RMSE |
|-------|----|-----|------|
| **Linear Regression (selected)** | **0.9984** | **8,160** | **10,052** |
| Ridge Regression | 0.9984 | 8,247 | 10,130 |
| Gradient Boosting | 0.9965 | 12,136 | 14,927 |
| Random Forest | 0.9941 | 15,902 | 19,498 |

Square footage is the dominant predictor (correlation with price ≈ 0.99).

## Dataset columns

`Square_Footage`, `Num_Bedrooms`, `Num_Bathrooms`, `Year_Built`, `Lot_Size`, `Garage_Size`,
`Neighborhood_Quality` (1-10) and the target `House_Price`.

## Limitations

- Only 1,000 records and no location data (city / ZIP code).
- Prices are treated as US dollars; the INR view is an approximate conversion.
- Estimates are statistical predictions, not professional valuations.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `streamlit: command not found` | `python -m streamlit run house_price_app.py` |
| "dataset was not found" | Put the CSV in the same folder as the script, or upload it in the sidebar |
| Port already in use | `streamlit run house_price_app.py --server.port 8502` |
