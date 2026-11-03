# Student Performance Prediction

A machine-learning project that estimates a student's mathematics score using reading and writing scores together with selected demographic and educational attributes. The repository includes data ingestion, preprocessing, CatBoost regression, model evaluation, MLflow logging, and a Flask prediction interface.

## Features

- Uses reading and writing scores and categorical student attributes as input features.
- Handles missing numerical and categorical values using imputation.
- Scales numerical inputs and one-hot encodes categorical inputs.
- Trains a CatBoost regression model to estimate mathematics scores.
- Evaluates the model with RMSE, MAE, and R².
- Logs evaluation metrics and model information through MLflow.
- Provides a Flask interface for entering student details and viewing a prediction.

## Tech Stack

- **Python**
- **Pandas and NumPy** – data processing.
- **Scikit-learn** – preprocessing and evaluation.
- **CatBoost** – regression model.
- **Flask** – web application.
- **MLflow** – experiment tracking and model logging.
- **DVC metadata** – dataset/artifact workflow support.

## Project Structure

```text
student-performance/
├── notebook/
│   ├── data.csv                 # Dataset used by the ingestion pipeline
│   ├── eda.ipynb                # Exploratory data analysis
│   └── model.ipynb              # Model experiments
├── src/
│   ├── components/              # Ingestion, transformation, training, evaluation
│   ├── pipeline/                # Training and prediction pipelines
│   ├── exception.py             # Custom exception handling
│   ├── logger.py                # Logging configuration
│   └── utils.py                 # Model save/load helpers
├── templates/                   # Flask HTML templates
├── artifacts/                   # Generated model and preprocessing files (local)
├── app.py                       # Flask prediction application
├── requirements.txt
├── setup.py
└── README.md
```

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/IslamBilal/student-performance.git
cd student-performance
```

### 2. Create and activate a virtual environment

Windows:
```bash
python -m venv .venv
.venv\Scripts\activate
```

macOS/Linux:
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

The training code also imports CatBoost. Install it if it is not yet included in the dependency file:

```bash
pip install catboost
```

## Train the Model

From the repository root, run:

```bash
python -m src.pipeline.training_pipeline
```

The pipeline reads `notebook/data.csv`, splits the data, preprocesses the input columns, trains the regression model, and saves the preprocessor and model under `artifacts/`. It then evaluates the saved model and logs metrics through MLflow.

## Run the Prediction App

The Flask app expects these locally generated files:

- `artifacts/preprocessor.pkl`
- `artifacts/model.pkl`

After training, run:

```bash
python app.py
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser.

## Evaluation

The training/evaluation code calculates:

- **RMSE** – root mean squared prediction error.
- **MAE** – mean absolute prediction error.
- **R²** – proportion of target variance explained by the model.

Actual metric values should be taken from a successful run of the current training pipeline; no unverified performance score is claimed in this README.

## Notes and Limitations

- The target is the mathematics score (`math_score`); the model estimates a score rather than assigning a pass/fail label.
- Prediction results are estimates and should not be treated as definitive judgments about a student.
- The model and preprocessor are generated artifacts. Run training locally before starting the app if they are not present.
- MLflow tracking configuration may need adjustment for your local environment.
- Validate form inputs and disable Flask debug mode before any public deployment.

## Future Improvements

- Add automated tests for data processing and prediction.
- Add reproducible train/test splitting with a fixed random seed.
- Document the dataset source and licensing.
- Add a model-performance report and visualizations.
- Improve deployment configuration and input validation.
