pipeline {
    agent any
    environment {
        IMAGE_NAME = 'psrv3/kodekloud_jenkins_flask_app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
        KUBECONFIG = credentials('kubeconfig-credentials-id')
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
                sh "docker build -t ${IMAGE_TAG} ."
                echo "Docker image build successfully"
            }
        }
        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_TAG}"
                echo "Docker image push successfully"
            }
        }
        stage('Deploy to EKS cluster') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                echo "Deploy successfully"
            }
        }
    }
}

