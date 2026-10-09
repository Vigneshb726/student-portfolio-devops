pipeline {
    agent any
    environment {
        IMAGE = 'vigneshb2006/student-portfolio:latest'
    }
    stages {
        stage('Checkout') {
            steps { git branch: 'main', url: 'https://github.com/Vigneshb726/student-portfolio-devops.git' }
        }
        stage('Build Image') {
            steps { bat 'docker build -t %IMAGE% .' }
        }
        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                        usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat 'docker login -u %USER% -p %PASS%'
                    bat 'docker push %IMAGE%'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/deployment.yaml'
                bat 'kubectl apply -f k8s/service.yaml'
                bat 'kubectl rollout restart deployment/portfolio-deployment'
            }
        }
        stage('Verify') {
            steps { bat 'kubectl get pods' }
        }
    }
}
