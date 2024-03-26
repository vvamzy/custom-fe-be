pipeline {
    agent any

    stages {
        stage('Scan fs') {
            steps {
                sh 'trivy fs .'
            }
        }
        stage('Build Frontend Image') {
            steps {
                sh 'sudo docker build . -t frontend -f frontend/Dockerfile-fe'
            }
        }
        stage('Build Backend Image') {
            steps {
                sh 'sudo docker build . -t backend -f backend/Dockerfile-backend'
            }
        }
        stage('Scan fe Image') {
            steps {
                sh 'trivy image frontend:latest'
            }
        }
        stage('Scan be Image') {
            steps {
                sh 'trivy image backend:latest'
            }
        }
    }
}
