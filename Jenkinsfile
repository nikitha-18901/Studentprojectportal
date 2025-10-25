pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nikithan01821/student-portal"
        DOCKERHUB_USER = "nikithan01821"
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
                echo 'Building Docker image...'
                // Build image with 'latest' tag
                bat "docker build -t %DOCKER_IMAGE%:latest ."
            }
        }

       stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKERHUB_PASS')]) {
                    echo 'Logging in to Docker Hub and pushing image...'
                    bat """
                    docker logout
                    docker login -u %DOCKERHUB_USER% -p %DOCKERHUB_PASS%
                    docker push %DOCKER_IMAGE%:latest
                    """
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                echo 'Stopping any old container using the same image...'
                bat """
                for /f "tokens=*" %%i in ('docker ps -q -f ancestor=%DOCKER_IMAGE%') do docker stop %%i
                for /f "tokens=*" %%i in ('docker ps -aq -f ancestor=%DOCKER_IMAGE%') do docker rm %%i
                """
            }
        }

        stage('Run New Container') {
            steps {
                echo 'Running new container...'
                bat "docker run -d -p 5000:5000 %DOCKER_IMAGE%:latest"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes...'
                bat 'kubectl apply -f k8s\\k8s-deployment.yaml'
                bat 'kubectl apply -f k8s\\k8s-service.yaml'
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
