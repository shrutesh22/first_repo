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
                bat 'dir'
            }
        }

        stage('Check kubectl') {
            steps {
                echo "Checking kubectl installation..."
                bat 'kubectl version --client'
            }
        }

        stage('Check Cluster Connection') {
            steps {
                echo "Checking Kubernetes cluster..."
                bat 'kubectl get nodes'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Applying YAML files..."
                bat 'kubectl apply -f .'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Checking pods..."
                bat 'kubectl get pods'

                echo "Checking services..."
                bat 'kubectl get svc'

                echo "Checking Istio resources..."
                bat 'kubectl get gateway'
                bat 'kubectl get virtualservice'
            }
        }
    }
}
