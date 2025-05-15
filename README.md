
# 🛡️ ML Phish Detector

[![Build Status](https://github.com/your-org/mohd-afroz-ali-ml-phish-detector/actions/workflows/main.yml/badge.svg)](https://github.com/your-org/mohd-afroz-ali-ml-phish-detector/actions/workflows/main.yml)  
[![Docker Pulls](https://img.shields.io/docker/pulls/your-dockerhub/ml-phish-detector.svg)](https://hub.docker.com/r/your-dockerhub/ml-phish-detector)  
[![PyPI](https://img.shields.io/pypi/v/ml-phish-detector.svg)](https://pypi.org/project/ml-phish-detector/)  

**Problem statement**  
Phishing websites trick users into revealing personal data. Manually detecting them at scale is impractical. This project builds an end-to-end machine learning pipeline that ingests URL features, validates and transforms data, trains multiple classifiers, monitors drift, and deploys a REST API—automating phishing detection in production.

---

## 🚀 Key Features

- **Modular Pipeline**: Ingestion, validation, transformation, training, evaluation & deployment each in its own component  
- **Schema-Driven Validation**: `schema.yaml` enforces column names & types before processing  
- **Drift Detection**: Kolmogorov–Smirnov tests guard against dataset distribution changes  
- **Hyperparameter Tuning**: Grid search over RandomForest, GradientBoosting, DecisionTree, LogisticRegression & AdaBoost  
- **Experiment Tracking**: MLflow logs metrics, parameters & models; DagsHub integration for visibility  
- **Real-Time & Batch Inference**: FastAPI for CSV uploads → HTML; CLI for bulk CSV predictions  
- **Cloud Deployment**: Docker‐containerized app on AWS ECR/ECS; artifacts & models versioned in S3  
- **CI/CD Automation**: GitHub Actions for linting, testing, Docker build & deploy

---

## 🛠️ Installation

```bash
git clone https://github.com/your-org/mohd-afroz-ali-ml-phish-detector.git
cd mohd-afroz-ali-ml-phish-detector
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
python setup.py develop
````

**Environment variables** (in `.env` or shell):

```bash
export MONGO_URI="mongodb://<user>:<pass>@host:27017/phish_db"
export AWS_ACCESS_KEY_ID="<your_access_key>"
export AWS_SECRET_ACCESS_KEY="<your_secret_key>"
export S3_BUCKET="your-phish-bucket"
```

---

## ▶️ Usage

### 1. Data Ingestion & Validation

```bash
python main.py
```

* **Extract** raw CSV → MongoDB
* **Validate** with `schema.yaml` & drift tests → `report.yaml`

### 2. Model Training

```bash
python main.py --step training
```

* **Transform**: KNN imputation & feature engineering
* **Train** multiple classifiers + hyperparameter search
* **Log** experiments to MLflow & DagsHub

### 3. Batch Prediction

```bash
python networksecurity/pipeline/batch_prediction.py \
  --input valid_data/test.csv \
  --output prediction_output/output.csv
```

### 4. Real-Time API

```bash
python app.py
```

* Visit `http://localhost:8000/docs` for Swagger UI
* Upload a CSV → view predictions as HTML table

---

## 🔁 Pipeline Flow

1. **Data Ingestion**

   * MongoDB → Pandas → raw CSV (`feature_store/`)
   * **Artifact**: raw, train.csv, test.csv

2. **Data Validation**

   * Column count & type checks via `schema.yaml`
   * Drift detection (KS test)
   * **Artifact**: `report.yaml`

3. **Data Transformation**

   * KNN Imputer → NumPy arrays
   * Persist preprocessor (`preprocessor.pkl`)
   * **Artifact**: train.npy, test.npy, preprocessor

4. **Model Training**

   * GridSearchCV over multiple classifiers
   * Best‐model selection & metrics logging
   * **Artifact**: `model.pkl`, network model wrapper

5. **Model Evaluation & Pusher**

   * Evaluate on hold-out set → if meets threshold → push to `final_model/`
   * **Artifact**: deployed model files

6. **Deployment & Serving**

   * Docker image → AWS ECR/ECS
   * S3 sync of artifacts & models

---

## 📦 Docker

```bash
docker build -t ml-phish-detector:latest .
docker run -d -p 8000:8000 \
  -e MONGO_URI -e AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY -e S3_BUCKET \
  ml-phish-detector:latest
```

---

## 📈 CI/CD

GitHub Actions workflow (`.github/workflows/main.yml`):

* **CI**: Lint, unit tests
* **CD**: Build & push Docker → Deploy on self-hosted runner (ECS)

---

## 📚 References

* **Dataset**: UCI Phishing Websites Data Set
* **MLflow**: [https://mlflow.org/](https://mlflow.org/)
* **DagsHub**: [https://dagshub.com/](https://dagshub.com/)
* **FastAPI**: [https://fastapi.tiangolo.com/](https://fastapi.tiangolo.com/)
* **Docker**: [https://www.docker.com/](https://www.docker.com/)
* **AWS**: S3, ECR, ECS

---

## 📝 License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.

```
```
