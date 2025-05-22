# Calculator Microservice Deployment on Google Cloud Platform (GCP)
Dockerized Node.js calculator microservice published to a private Google Cloud Artifact Registry. Includes Dockerfile, containerization setup, tagging, authentication, push to GCP, and verification by running the microservice from the remote registry. Developed for SIT737 Task 5.2D.

---

## 🚀 Technologies Used

- Node.js (Express.js)
- Docker
- Google Cloud SDK (`gcloud`)
- Google Cloud Artifact Registry

---

## 🛠 Prerequisites

- Google Cloud project with billing enabled
- Docker and `gcloud` CLI installed and configured
- Docker image built from Task 6.2C

---

## 📦 Step-by-Step Instructions

### 1. Authenticate and Set GCP Project

gcloud auth login
gcloud config set project YOUR_PROJECT_ID

2. Enable Artifact Registry API (first time only)

gcloud services enable artifactregistry.googleapis.com

3. Create a Docker Repository (first time only)

gcloud artifacts repositories create microservices-repo \--repository-format=docker \--location=us-central1 \--description="Private container registry for calculator microservice"

## 🔐 Authenticate Docker with GCP


gcloud auth configure-docker us-central1-docker.pkg.dev
This allows Docker to securely push/pull images from GCP.

## 🏷️ Tag the Local Docker Image

### Find your image:

docker images

### Tag it for the registry:

docker tag username/new-calculator-microservice:v2 \us-central1-docker.pkg.dev/YOUR_PROJECT_ID/microservices-repo/new-calculator-microservice:v1

## ⬆️ Push Image to GCP Registry

docker push us-central1-docker.pkg.dev/YOUR_PROJECT_ID/microservices-repo/new-calculator-microservice:v1

You should see all layers uploaded to the registry.

## 🧪 Run the Image from the Cloud

docker run -p 3000:3000 \us-central1-docker.pkg.dev/YOUR_PROJECT_ID/microservices-repo/new-calculator-microservice:v1

### Open your browser:

http://localhost:3000/add?num1=10&num2=5

## ✅ If it works, your cloud-hosted microservice is running successfully.

## 🔍 Available Endpoints

Method	Endpoint	Description
GET	/add?num1=10&num2=5	Returns sum of numbers
GET	/subtract?num1=10&num2=5	Subtracts second from first
GET	/multiply?num1=5&num2=5	Returns product
GET	/divide?num1=10&num2=2	Returns quotient
GET	/power?num1=2&num2=3	Returns exponentiation
GET	/modulo?num1=10&num2=3	Returns remainder
GET	/sqrt?num1=16	Returns square root
GET	/percentage?num1=25&num2=100	Calculates percentage

## 🛡 Error Handling
Missing parameters: 400 Bad Request
Invalid numbers: 400 Bad Request
Division/Modulo by zero: 400 Bad Request
Square root of negative number: 400 Bad Request
Invalid endpoint: 404 Not Found