pipeline {
    agent any
    environment {
        IMAGE_NAME = ''
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/iampsrv/jenkins-project/'
                sh "ls -ltr"
            }
        }
        stage('Setup') {
            steps {
                sh "pip install -r requirements.txt --break-system-packages"
            }
        }
        stage('Test') {
            steps {
                sh "pytest"
            }
        }
        stage('Login to Dockerhub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                sh 'docker login -u psrv3 -p ${dockerhubpwd}'}
                echo "Login Successfully"
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t flaskapp"
                echo "Docker image build successfully"
            }
        }
        stage('Push Docker Image') {
            steps {
                sh "docker push flaskapp"
                echo "Docker image push successfully"
            }
        }
    }
}

