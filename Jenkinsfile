pipeline {
    agent any

    environment {
        IMAGE_NAME = "java-docker-app"
        CONTAINER_NAME = "java-docker-app-container"
        GIT_REPO = "https://github.com/shaibazkhan/java-docker-app.git"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8082:8080 --name $CONTAINER_NAME $IMAGE_NAME'
            }
        }

    }

    post {
        success {
            echo "Application deployed successfully!"
        }
        failure {
            echo "Pipeline execution failed!"
        }
    }
}
