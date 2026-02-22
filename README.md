# 🚚 Swiggy Delivery Time Prediction System  
### End-to-End MLOps Project (DVC + MLflow + DagsHub + AWS)

##  Project Overview
This project predicts **food delivery time** using Machine Learning and demonstrates a **complete end-to-end MLOps pipeline**.

The system covers:
- Data versioning with **DVC**
- Experiment tracking with **MLflow**
- Centralized repo & storage using **DagsHub**
- Model serving via **FastAPI**
- Containerization using **Docker**
- Cloud deployment on **AWS (ECR, EC2, CodeDeploy)**
- CI/CD automation using **GitHub Actions**

This project is designed to be **scalable, reproducible, and production-ready**.


##  Objective
To accurately predict food delivery time based on:
- Delivery partner attributes
- Order and vehicle details
- Traffic & weather conditions
- Distance and location features

---

##  Model Performance
- **R² Score / Accuracy:** ~87%
- **Evaluation Metrics:**
  - Mean Absolute Error (MAE)
  - R² Score
  - Cross-Validation MAE

---

##  Tech Stack

| Category | Tools |
|-------|------|
| Language | Python |
| ML | Scikit-learn |
| MLOps | DVC, MLflow |
| Experiment Tracking | MLflow (via DagsHub) |
| Data Versioning | DVC |
| API | FastAPI |
| Containerization | Docker |
| Cloud | AWS (ECR, EC2, CodeDeploy) |
| CI/CD | GitHub Actions |
| Storage | DagsHub S3 Backend |


## 📂 Project Structure

```

swiggy-delivery-time-prediction-system/
│
├── .dvc/                          # DVC metadata
├── .github/
│   └── workflows/
│       └── ci-cd.yml              # GitHub Actions pipeline
│
├── data/
│   ├── raw/                       # Raw dataset (DVC tracked)
│   ├── cleaned/                   # Cleaned data
│   ├── interim/                   # Train/Test split
│   └── processed/                 # Final transformed data
│
├── models/
│   ├── model.joblib
│   ├── preprocessor.joblib
│   ├── power_transformer.joblib
│   └── stacking_regressor.joblib
│
├── src/
│   ├── data/
│   │   ├── data_ingestion.py
│   │   ├── data_cleaning.py
│   │   └── data_preprocessing.py
│   │
│   ├── models/
│   │   ├── train.py
│   │   ├── model_evaluation.py
│   │   └── model_registry.py
│   │
│   └── utils/
│       └── common.py
│
├── scripts/
│   ├── data_clean_utils.py
│   └── sample_predictions.py
│
├── app.py                         # FastAPI inference service
├── dvc.yaml                       # DVC pipeline
├── params.yaml                    # Hyperparameters
├── Dockerfile                     # Docker image
├── requirements.txt
├── run_information.json           # MLflow run metadata
└── README.md

````

##  DVC Pipeline

### Run full pipeline
```bash
dvc repro
````

### Pipeline Stages

| Stage              | Description                       |
| ------------------ | --------------------------------- |
| data_ingestion     | Load raw dataset                  |
| data_cleaning      | Data cleaning & validation        |
| data_preprocessing | Feature engineering               |
| train              | Model training                    |
| evaluation         | Model evaluation & MLflow logging |
| model_registry     | Register model in MLflow          |


## MLflow Experiment Tracking

### MLflow Features Used

* Parameter logging
* Metric tracking
* Dataset tracking
* Model artifacts
* Model registry
* Model versioning
* Model staging

### Initialize DagsHub + MLflow

```python
import dagshub
import mlflow

dagshub.init(
    repo_owner="user-name",
    repo_name="rep-name",
    mlflow=True
)

mlflow.set_tracking_uri(
    "https://dagshub.com/quamrl-hoda/swiggy-delivery-time-prediction-system.mlflow"
)
```

### Logged to MLflow

* Model parameters
* Train & test metrics
* Cross-validation metrics
* Trained model
* Preprocessor & transformers
* Dataset inputs
* Model signature


##  FastAPI Inference Service

### Run locally

```bash
uvicorn app:app --host 0.0.0.0 --port 8000
```

### Swagger UI

```
http://localhost:8000/docs
```


### Sample API Request

```json
{
  "age": 32,
  "ratings": 4.3,
  "weather": "sunny",
  "traffic": "jam",
  "vehicle_condition": 3,
  "type_of_order": "snack",
  "type_of_vehicle": "motorcycle",
  "multiple_deliveries": 0,
  "festival": "No",
  "city_type": "Urban",
  "is_weekend": 0,
  "pickup_time_minutes": 15,
  "order_time_of_day": "Evening",
  "distance": 6.5,
  "distance_type": "short"
}
```


## Dockerization

### Build Docker image

```bash
docker build -t swiggy-delivery-time-pred .
```

### Run container

```bash
docker run -p 8000:8000 swiggy-delivery-time-pred
```


##  AWS Deployment (ECR + EC2 + CodeDeploy)

### Push Docker Image to Amazon ECR

```bash
aws ecr get-login-password --region ap-south-1 \
| docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com
```

```bash
docker tag swiggy-delivery-time-pred:latest <ECR_URI>
docker push <ECR_URI>
```


### EC2 Setup (Ubuntu – User Data Script)

```bash
#!/bin/bash
sudo apt update -y
sudo apt install ruby-full wget -y
cd /home/ubuntu
wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install
chmod +x install
sudo ./install auto
sudo systemctl start codedeploy-agent
```

---

### Required IAM Permissions

* AmazonEC2ContainerRegistryFullAccess
* AWSCodeDeployFullAccess
* AmazonS3FullAccess


##  CI/CD Pipeline

* GitHub Actions triggered on push
* Build Docker image
* Push image to Amazon ECR
* Deploy container to EC2 using CodeDeploy

---
##  Author

**Quamrul Hoda**
B.Tech (AIML) | MLOps | Cloud Deployment
