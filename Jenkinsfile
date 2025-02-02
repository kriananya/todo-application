
pipeline {
    agent any 

    environment {
        DOCKER_IMAGE = "ananyak10/todo-application:latest"
        DOCKER_CREDENTIALS_ID = "ananyak10"

    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', credentialsID: "kriananya" , url: 'https://github.com/kriananya/todo-application.git'
            }
        }
        stage('Build Project with Maven (Skip Tests)') {
            steps {
                sh 'docker run --rm -v "$(pwd)":/app -w /app maven:3.9.9-eclipse-temurin-17 mvn clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                withDockerRegistry([credentialsID: DOCKER_CREDENTIALS_ID, url: '']) {
                sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose down || true'
                sh 'docker compose up -d'
            }
        }
    }
}
