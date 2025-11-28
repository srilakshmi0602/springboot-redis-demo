pipeline {

    agent any

    tools {
        maven 'maven'   // Name you added in Jenkins tools
    }

    environment {
        IMAGE_NAME = "sak_redis_app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/srilakshmi0602/springboot-redis-demo.git'
            }
        }

        stage('Build Maven Package') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Stop Old Containers') {
            steps {
                script {
                    sh """
                        echo "Stopping old containers..."
                        docker compose down || true
                    """
                }
            }
        }

        stage('Deploy Using Docker Compose') {
            steps {
                script {
                    sh """
                        echo "Starting new containers..."
                        docker compose up -d --build
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh "docker ps"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Spring Boot + Redis is running."
        }
        failure {
            echo "Deployment failed. Check logs."
        }
    }
}
