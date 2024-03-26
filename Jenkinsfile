pipeline {
    agent any
    environment {     
    DOCKERHUB_CREDENTIALS= credentials('docker')     
    } 
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
	            sh 'echo $DOCKERHUB_CREDENTIALS | sudo docker login -u vvamzy --password-stdin'                		
	            echo 'Login Completed'      
            }           
        }               
    }
}
