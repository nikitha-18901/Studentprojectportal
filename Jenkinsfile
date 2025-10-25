pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nikitham01821/student-portal"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Pull code from GitHub
                git branch: 'main', url: 'https://github.com/nikitha-18901/studentprojectportal.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t student-portal:latest .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKERHUB_PASS')]) {
                    // Login and push image to Docker Hub
                    sh """
                    echo $DOCKERHUB_PASS | docker login -u yourdockerhubusername --password-stdin
                    docker push $DOCKER_IMAGE:latest
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Apply Kubernetes deployment and service YAMLs
                sh 'kubectl apply -f k8s/k8s-deployment.yaml'
                sh 'kubectl apply -f k8s/k8s-service.yaml'
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs!"
        }
    }
}
