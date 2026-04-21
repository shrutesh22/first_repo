pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig')
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'UAT', url: 'https://github.com/shrutesh22/first_repo.git'
            }
        }

        stage('List Files') {
            steps {
                echo "Listing files in workspace..."
                sh 'ls -l'
            }
        }

        stage('Check kubectl') {
            steps {
                echo "Checking kubectl installation..."
                sh 'kubectl version --client'
            }
        }

        stage('Check Cluster Connection') {
            steps {
                echo "Checking Kubernetes cluster..."
                sh 'kubectl get nodes'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Applying YAML files..."
                sh 'kubectl apply -f .'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Checking pods..."
                sh 'kubectl get pods'

                echo "Checking services..."
                sh 'kubectl get svc'

                echo "Checking Istio resources..."
                sh 'kubectl get gateway'
                sh 'kubectl get virtualservice'
            }
        }
    }
}
