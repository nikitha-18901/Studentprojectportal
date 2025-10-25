pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "student-portal:latest"
        CONTAINER_NAME = "student-portal"
        HOST_PORT = "5000"
        CONTAINER_PORT = "5000"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the Student Project Portal repository...'
                git branch: 'main', url: 'https://github.com/nikitha-18901/studentprojectportal.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Student Project Portal...'
                bat 'echo Build stage completed.'
                // Optional: install dependencies
                // bat 'pip install -r requirements.txt'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image for Student Project Portal...'
                bat """
                    docker build -t %DOCKER_IMAGE% .
                """
            }
        }

        stage('Docker Run') {
            steps {
                echo 'Running Docker container for Student Project Portal...'
                bat """
                    docker stop %CONTAINER_NAME% || echo "No existing container to stop"
                    docker rm %CONTAINER_NAME% || echo "No existing container to remove"
                    docker run -d --name %CONTAINER_NAME% -p %HOST_PORT%:%CONTAINER_PORT% %DOCKER_IMAGE%
                """
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deployment successful!'
                echo "Open http://localhost:%HOST_PORT% in your browser to access the Student Project Portal."
                bat 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully ✅'
        }
        failure {
            echo 'Pipeline failed ❌ — please check the logs for details.'
        }
    }
}
