# Project Docker Jenkins EC2 Test

This project demonstrates a simple Flask application containerised with Docker and deployed to an AWS EC2 instance using Jenkins for CI/CD pipeline.

## Running Locally

1. Install dependencies:

    pip install -r requirements.txt

2. Run the application:

    python run.py

3. Open [http://localhost:5000](http://localhost:5000) in your browser.

## Running with Docker

1. Build the Docker image:

    docker build -t flask-app .

2. Run the Docker container:

    docker run -p 5000:5000 flask-app

3. Open [http://localhost:5000](http://localhost:5000) in your browser.

## Jenkins Pipeline

This project includes a `Jenkinsfile` for setting up a CI/CD pipeline in Jenkins. The pipeline performs the following steps:

1. Clones the repository.
2. Builds the Docker image.
3. Tests the Docker image.
4. Pushes the Docker image to Docker Hub.
5. Deploys the Docker container to an AWS EC2 instance.

## Summary

- **Application Code:** Flask app with a simple route and HTML template.
- **Tests:** Basic tests using pytest.
- **Dockerfile:** Docker configuration to containerise the Flask app.
- **Jenkinsfile:** Jenkins pipeline script for CI/CD.
