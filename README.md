
# 🏨 Hotel Reservation Prediction

## 📌 Project Overview

**Hotel Reservation Prediction** is a machine learning project aimed at forecasting hotel booking statuses. Leveraging a robust CI/CD pipeline, the project ensures seamless integration and deployment, utilizing tools like MLFlow, Jenkins, Docker, and Google Cloud Run.

---

## 🚀 Features

- **Automated CI/CD Pipeline**: Implemented using Jenkins for continuous integration and deployment.
- **Containerization**: Dockerized application for consistent environments across development and production.
- **Cloud Deployment**: Deployed on Google Cloud Run for scalable and serverless execution.
- **Machine Learning Model**: Utilizes LightGBM for efficient and accurate predictions.
- **MLflow Integration**: Tracks experiments, parameters, and metrics for reproducibility.

---

## 🛠️ Tech Stack

- **Programming Language**: Python
- **Machine Learning**: LightGBM, scikit-learn
- **CI/CD**: Jenkins
- **Containerization**: Docker
- **Cloud Platform**: Google Cloud Run
- **Experiment Tracking**: MLflow

---

## 🔧 Setup Instructions

### Prerequisites

- Python 3.8 or higher
- Docker
- Google Cloud SDK
- Jenkins

### Clone the Repository

```bash
git clone https://github.com/rasinmuhammed/hotel-reservation-prediction.git
cd hotel-reservation-prediction
```

### Set Up Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install --upgrade pip
pip install -e .
```

### Configure Google Cloud

Ensure you have a Google Cloud project set up and the SDK installed. Authenticate using:

```bash
gcloud auth login
gcloud config set project your-project-id
```

---

## 🧪 Model Training

The model training pipeline is defined in `pipeline/training_pipeline.py`. It performs the following steps:

1. **Data Loading**: Reads and preprocesses the dataset.
2. **Model Training**: Trains a LightGBM classifier with hyperparameter tuning.
3. **Evaluation**: Calculates accuracy, precision, recall, and F1-score.
4. **Model Saving**: Persists the trained model for future inference.
5. **MLflow Logging**: Records parameters and metrics for experiment tracking.

To initiate training:

```bash
python pipeline/training_pipeline.py
```

---

## 🐳 Dockerization

The application is containerized using Docker. The `Dockerfile` sets up the environment, installs dependencies, and defines the entry point.

### Build Docker Image

```bash
docker build -t hotel-reservation-prediction .
```

### Run Docker Container

```bash
docker run -p 5000:5000 hotel-reservation-prediction
```

---

## 🔄 CI/CD Pipeline with Jenkins

The Jenkins pipeline automates the build, test, and deployment processes. Below is an overview of the stages:

### Jenkinsfile

```groovy
pipeline {
    agent any
    environment {
        VENV_DIR = 'venv'
        GCP_PROJECT = 'your-gcp-project-id'
        GCLOUD_PATH = '/var/jenkins_home/google-cloud-sdk/bin'
    }
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        stage('Setup Environment') {
            steps {
                sh '''
                    python -m venv $VENV_DIR
                    source $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install -e .
                '''
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                        gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                        gcloud config set project $GCP_PROJECT
                        gcloud auth configure-docker
                        docker build -t gcr.io/$GCP_PROJECT/hotel-reservation-prediction:latest .
                        docker push gcr.io/$GCP_PROJECT/hotel-reservation-prediction:latest
                    '''
                }
            }
        }
        stage('Deploy to Cloud Run') {
            steps {
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                        gcloud run deploy hotel-reservation-prediction \
                        --image=gcr.io/$GCP_PROJECT/hotel-reservation-prediction:latest \
                        --platform=managed \
                        --region=us-central1 \
                        --allow-unauthenticated
                    '''
                }
            }
        }
    }
}
```

---

## ☁️ Deployment

After the CI/CD pipeline runs successfully, the application is deployed to Google Cloud Run. Access the deployed service via the URL provided by Cloud Run.

---

## 📈 MLflow Tracking

MLflow is integrated for experiment tracking. It logs:

- Parameters
- Metrics (accuracy, precision, recall, F1-score)
- Artifacts (trained models)

To view the MLflow UI:

```bash
mlflow ui
```

Access it at `http://localhost:5000`.

---

## 🤝 Contributing

Contributions are welcome! Please fork the repository and submit a pull request.

---


## 📬 Contact

For any inquiries or feedback, feel free to reach out via rasinbinabdulla@gmail.com
 
