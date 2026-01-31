pipeline {
    agent any
    environment {
        // Use the GitHub credentials you added in Jenkins
        GITHUB_CREDENTIALS = credentials('github-creds')
        // Image name in GitHub Container Registry
        IMAGE_NAME = "ghcr.io/harsh5052311/python-app"
    }
    stages {
        stage('Checkout') {
            steps {
                // Pull code from your GitHub repo
                git branch: 'main', url: 'https://github.com/harsh5052311/python-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build Docker image from Dockerfile
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }
        stage('Push to GitHub Container Registry') {
            steps {
                // Login to GHCR using Jenkins credentials
                sh 'echo $GITHUB_CREDENTIALS_PSW | docker login ghcr.io -u $GITHUB_CREDENTIALS_USR --password-stdin'
                // Push the image
                sh 'docker push $IMAGE_NAME:latest'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                // Apply Kubernetes manifest to deploy app
                sh 'kubectl apply -f python-app.yaml'
            }
        }
    }
}

