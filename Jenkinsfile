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
                sh "docker push vvamzy/backend"
                sh "docker push vvamzy/frontend"
            }
        }               
        }
        stage('Run Test Containers') {      	
            steps{
                sh "docker run -d --name fe-test -p 3000:3000 vvamzy/frontend"
                sh "docker run -d --name be-test vvamzy/backend"
                sleep 60
                echo "terminating the test containers"
                sh "docker rm -f fe-test"
                sh "docker rm -f be-test"
        }               
        }
    }
}
