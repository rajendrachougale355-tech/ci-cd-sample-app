pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/ci-cd-sample-app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t sample-app .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm sample-app npm test'
            }
        }
        stage('Push to Registry') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u your-dockerhub-user --password-stdin'
                    sh 'docker tag sample-app your-dockerhub-user/sample-app:latest'
                    sh 'docker push your-dockerhub-user/sample-app:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
