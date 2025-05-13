````markdown
# 🛡️ ML Phish Detector

[![Build Status](https://github.com/your-org/mohd-afroz-ali-ml-phish-detector/actions/workflows/main.yml/badge.svg)](https://github.com/your-org/mohd-afroz-ali-ml-phish-detector/actions/workflows/main.yml)  
[![Docker Pulls](https://img.shields.io/docker/pulls/your-dockerhub/ml-phish-detector.svg)](https://hub.docker.com/r/your-dockerhub/ml-phish-detector)  
[![PyPI](https://img.shields.io/pypi/v/ml-phish-detector.svg)](https://pypi.org/project/ml-phish-detector/)  

An end-to-end Machine Learning pipeline to detect phishing URLs from network security data. This repository covers data ingestion, validation, transformation, model training, batch & real-time inference, containerization, and CI/CD automation.

---

## 🚀 Features

- **Schema-based Validation**  
  Validate raw CSVs against `schema.yaml` before ingesting to enforce data quality.

- **Modular Pipeline Components**  
  Clean, reusable steps for data ingestion, validation, transformation, training, and prediction.

- **Model & Preprocessor Serialization**  
  Persist trained `model.pkl` and `preprocessor.pkl` for easy production inference.

- **Interactive Flask App**  
  Real-time prediction via `app.py` and a simple HTML template (`templates/table.html`).

- **Batch Inference CLI**  
  Efficiently process large CSVs with `batch_prediction.py` to produce `prediction_output/output.csv`.

- **Cloud Sync & DB Integration**  
  Sync ingested data to AWS S3 (`cloud/s3_syncer.py`) and store in MongoDB (`push_data.py`, `test_mongodb.py`).

- **Robust Logging & Error Handling**  
  Custom logger and exception hierarchy for clear traceability and fail-safe operation.

- **Containerization & Deployment**  
  Dockerfile for building a reproducible image; environment-variable driven configuration.

- **CI/CD Automation**  
  GitHub Actions workflow (`.github/workflows/main.yml`) for linting, testing, and Docker builds on each push.

---

## ⚙️ Installation

1. **Clone the repository**  
   ```bash
   git clone https://github.com/your-org/mohd-afroz-ali-ml-phish-detector.git
   cd mohd-afroz-ali-ml-phish-detector
````

2. **Create & activate a virtual environment**

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install dependencies**

   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   python setup.py develop
   ```

4. **Set environment variables**

   ```bash
   export MONGO_URI="mongodb://localhost:27017/phish_db"
   export AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY"
   export AWS_SECRET_ACCESS_KEY="YOUR_SECRET_KEY"
   export S3_BUCKET="your-s3-bucket"
   ```

---

## ▶️ Usage

### 1. Data Ingestion & Validation

```bash
python main.py --step ingestion
```

* Reads raw CSV from `Network_Data/`
* Validates using `data_schema/schema.yaml`
* Stores to MongoDB & optionally syncs to S3

### 2. Model Training

```bash
python main.py --step training
```

* Performs data transformation
* Trains classification model
* Serializes artifacts to `final_model/`

### 3. Batch Prediction

```bash
python networksecurity/pipeline/batch_prediction.py \
  --input valid_data/test.csv \
  --output prediction_output/output.csv
```

### 4. Real-Time Web App

```bash
python app.py
```

* Navigate to `http://localhost:5000`
* Enter URL features to view phishing probability

### 5. Push Sample Data to MongoDB

```bash
python push_data.py --file valid_data/test.csv
```

### 6. Test MongoDB Connection

```bash
python test_mongodb.py
```

---

## 🐳 Docker

1. **Build the Docker image**

   ```bash
   docker build -t ml-phish-detector:latest .
   ```

2. **Run the container**

   ```bash
   docker run -d -p 5000:5000 \
     -e MONGO_URI \
     -e AWS_ACCESS_KEY_ID \
     -e AWS_SECRET_ACCESS_KEY \
     -e S3_BUCKET \
     ml-phish-detector:latest
   ```

3. **Access application**
   Open your browser at `http://localhost:5000`

---

## 🔄 CI/CD

**GitHub Actions** (`.github/workflows/main.yml`) are configured to:

* Lint and format code
* Run unit tests (including `test_mongodb.py`)
* Build and publish Docker image
* Report build status via badge

---

## 🤝 Contributing

1. Fork the repo
2. Create feature branch:

   ```bash
   git checkout -b feature/YourFeature
   ```
3. Commit changes with clear messages
4. Push to your fork and open a PR against `main`
5. Ensure all CI checks pass before review

Follow [PEP8](https://www.python.org/dev/peps/pep-0008/) and include unit tests for new functionality.

---

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

> “Building robust ML systems isn’t just about models—it’s about engineering reliable, maintainable pipelines.”
> — Mohd Afroz Ali

```
```
