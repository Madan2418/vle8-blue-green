pipeline {
    agent any

    environment {
        HOME = '/var/lib/jenkins'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Madan2418/vle8-blue-green.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'minikube image build -t vle8-app:v2 .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                    kubectl rollout status deployment/vle8-app
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods,svc'
            }
        }
    }
}
