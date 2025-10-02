pipeline {
    agent any

    tools {
        maven 'MAVEN' // Adjust to match your Jenkins config
    }
    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/biswajit-70/Simple-Spring-Boot-Web-App.git' // Replace with your repo
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Compose Up') {
            steps {
                sh 'docker-compose down --volumes --remove-orphans || true'
                sh 'docker-compose up --build -d'
            }
        }

        stage('Verify App') {
            steps {
                sh 'sleep 20' // Wait for MySQL healthcheck
                sh 'curl -f http://localhost:8080/login || exit 1'
            }
        }
    }

    post {
        success {
            echo '✅ Spring Boot app deployed successfully with MySQL!'
        }
        failure {
            echo '❌ Deployment failed. Check logs and container status.'
        }
    }
}
