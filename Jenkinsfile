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
                sh 'sudo docker build . -t vvamzy/frontend -f frontend/Dockerfile-fe'
            }
        }
        stage('Build Backend Image') {
            steps {
                sh 'sudo docker build . -t vvamzy/backend -f backend/Dockerfile-backend'
            }
        }
        stage('Scan fe Image') {
            steps {
                sh 'sudo trivy image frontend:latest'
            }
        }
        stage('Scan be Image') {
            steps {
                sh 'sudo trivy image backend:latest'
            }
        }
        stage('Login to Docker Hub') {      	
        steps{
            withCredentials([string(credentialsId: 'vvamzy', variable: 'dockerpwd')]) {
            sh "docker login -u vvamzy -p ${dockerpwd}"
            }
        }               
    }
}
}
