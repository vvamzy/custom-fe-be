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
                sh 'sudo trivy image vvamzy/frontend:latest'
            }
        }
        stage('Scan be Image') {
            steps {
                sh 'sudo trivy image vvamzy/backend:latest'
            }
        }
        stage('Login to Docker Hub') {      	
            steps{
                withCredentials([string(credentialsId: 'vvamzy', variable: 'dockerpwd')]) {
                sh "docker login -u vvamzy -p ${dockerpwd}"
                sh "sudo docker push vvamzy/backend"
                sh "sudo docker push vvamzy/frontend"
            }
        }               
        }
        stage('Run Test Containers') {      	
            steps{
                sh "sudo docker run -d --name fe-test -p 3000:3000 vvamzy/frontend"
                sh "sudo docker run -d --name be-test vvamzy/backend"
                sleep 60
                echo "terminating the test containers"
                sh "sudo docker rm -f fe-test"
                sh "sudo docker rm -f be-test"
        }               
        }
    }
}
