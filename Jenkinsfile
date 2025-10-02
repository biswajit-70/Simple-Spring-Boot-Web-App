pipeline {
    agent any

    tools {
        maven 'MAVEN'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/biswajit-70/Simple-Spring-Boot-Web-App.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Cleanup') {
            steps {
                sh 'docker rm -f mysql-db || true'
                sh 'docker rm -f springboot-login-app || true'
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
                sh 'sleep 20'
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
