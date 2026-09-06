# Heart Disease Risk Predictor

**Author:** Zubair Ali  
🌐 **Live Demo:** https://heart-disease-project-14d0.onrender.com/  
> First request may take 30–60 seconds — hosted on Render free tier.

⚠️ **Note:** This is a learning project. Built to understand the MLOps workflow from training to deployment. Not production-grade.

---

## What is this?

A machine learning pipeline that takes a patient's clinical data (age, cholesterol, blood pressure, etc.) and predicts whether they are at risk of heart disease.

The goal wasn't just to train a model. The goal was to build the full pipeline — data, training, testing, API, Docker, CI, and cloud deployment — the way it's actually done in real ML engineering.

---

## How it was built

**1. Data Exploration**  
Loaded the UCI Heart Disease dataset, looked at feature distributions, found missing values, and understood what the data actually means before touching any model.

**2. Preprocessing**  
Cleaned the data, scaled numeric columns, applied KNN imputation, ran feature selection (Random Forest based), and created two new features (`age_chol_ratio` and `bp_age_interaction`) to give the model better signals.

**3. Training & Tuning**  
Trained Logistic Regression, Random Forest, LightGBM, and XGBoost. Used GridSearchCV with cross-validation and regularization to tune each. All four ended up close on recall — final pick came down to the tradeoffs below.

**4. Experiment Tracking**  
Every training run, metric, and parameter was logged with MLflow. This makes it easy to compare runs and know exactly why the final model was chosen.

**5. Testing**  
Wrote Pytest tests to check data integrity and model output. GitHub Actions runs these tests automatically on every push.

**6. API & Deployment**  
Built a FastAPI backend that loads the trained model and returns predictions. Wrapped it in Docker and deployed to Render.

---

## Model Performance (5-Fold CV)

| Model | Recall | ROC-AUC | F1-Score | Precision | Accuracy |
|-----------------------------|---------|---------|----------|-----------|----------|
| **Tuned LightGBM** | **0.8182** | 0.8406 | 0.8005 | 0.7845 | 0.7745 |
| Tuned XGBoost | 0.8131 | 0.8562 | 0.8010 | 0.7904 | 0.7772 |
| Tuned Random Forest | 0.8108 | 0.8422 | 0.7992 | 0.7885 | 0.7745 |
| Tuned Logistic Regression | 0.8107 | 0.8640 | 0.8047 | 0.7991 | 0.7826 |

**Why LightGBM was deployed:** it had the highest recall of the four models. In a clinical screening context, recall matters most — missing an at-risk patient (a false negative) is far costlier than a false alarm — so recall was the primary selection criterion, and LightGBM led on that metric.

---

## Tech Stack

| Area | Tools |
|---|---|
| ML & Data | Python, Scikit-Learn, Pandas, MLflow, Joblib, LightGBM, XGBoost |
| Backend | FastAPI, Uvicorn, SQLite |
| MLOps | Docker, Docker Compose, GitHub Actions, Pytest |
| Frontend | HTML, CSS, Vanilla JS |

---

## Project Structure


heart_disease_project/
├── .github/
│   └── workflows/
│       └── ci.yml                        # Runs tests on every push

├── api/
│   └── main.py                           # FastAPI backend

├── data/                                 # SQLite prediction logs

├── frontend/
│   ├── index.html
│   ├── script.js
│   └── style.css

├── logs/
│   └── logger.py

├── mlartifacts/                          # MLflow run history

├── models/
│   └── best_Tuned_LightGBM.pkl           # Final model

├── notebook/
│   └── eda.ipynb                         # Data exploration

├── preprocess_pipeline/
│   ├── __init__.py
│   └── pre_processing.py                 # Feature engineering

├── py_test/
│   ├── test_data.py                      # Data integrity tests
│   └── test_predict.py                   # Prediction output tests

├── training/
│   └── train.py                          # Training & tuning script

├── Dockerfile.api

├── docker-compose.yml

├── requirements.txt

└── setup.py
