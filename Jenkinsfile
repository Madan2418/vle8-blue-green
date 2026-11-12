
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

        stage('Build BLUE and GREEN Images') {
            steps {
                sh '''
                    minikube image build -t vle8-app:v1 -f Dockerfile.blue .
                    minikube image build -t vle8-app:v2 -f Dockerfile .
                '''
            }
        }

        stage('Deploy BLUE') {
            steps {
                sh '''
                    kubectl apply -f blue-deployment.yaml
                    kubectl rollout status deployment/vle8-blue
                '''
            }
        }

        stage('Deploy GREEN') {
            steps {
                sh '''
                    kubectl apply -f green-deployment.yaml
                    kubectl rollout status deployment/vle8-green
                '''
            }
        }

        stage('Switch Traffic to BLUE') {
            steps {
                sh '''
                    kubectl patch service vle8-service \
                    -p '{"spec":{"selector":{"app":"vle8-app","version":"blue"}}}'
                    kubectl get endpoints vle8-service
                '''
            }
        }

        stage('Switch Traffic to GREEN') {
            steps {
                sh '''
                    kubectl patch service vle8-service \
                    -p '{"spec":{"selector":{"app":"vle8-app","version":"green"}}}'
                    kubectl get endpoints vle8-service
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    kubectl get deployments
                    kubectl get pods
                    kubectl get svc
                '''
            }
        }
    }
}
