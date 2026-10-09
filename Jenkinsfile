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
            steps { sh 'docker build -t $IMAGE .' }
        }
        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
                sh 'kubectl rollout restart deployment/portfolio-deployment'
                sh 'kubectl rollout status deployment/portfolio-deployment'
            }
        }
        stage('Verify') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc portfolio-service'
            }
        }
    }
}
