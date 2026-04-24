pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig')
    }

    options {
        timestamps()
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Cloning repository..."
                git branch: 'UAT', url: 'https://github.com/shrutesh22/first_repo.git'
            }
        }

        stage('List Files') {
            steps {
                echo "Workspace structure:"
                sh 'ls -R'
            }
        }

        stage('Verify kubectl') {
            steps {
                echo "Checking kubectl..."
                sh 'kubectl version --client'
            }
        }

        stage('Verify Cluster Connection') {
            steps {
                echo "Checking Kubernetes cluster..."
                sh 'kubectl get nodes'
            }
        }

        stage('Deploy Istio Configs (Auto)') {
            steps {
                echo "Deploying all Istio namespace configs..."

                sh '''
                set -e

                if [ ! -d "istio" ]; then
                    echo "ERROR: istio folder not found!"
                    exit 1
                fi

                for dir in istio/*; do
                    if [ -d "$dir" ]; then

                        ns=$(basename "$dir")

                        echo "======================================"
                        echo "Processing Namespace: $ns"
                        echo "======================================"

                        echo "Creating namespace if not exists..."
                        kubectl create namespace $ns --dry-run=client -o yaml | kubectl apply -f -

                        echo "Applying YAMLs from $dir..."
                        kubectl apply -f $dir

                    fi
                done
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo "Verifying Istio resources..."

                sh '''
                echo "========== Namespaces =========="
                kubectl get ns

                echo "========== VirtualServices =========="
                kubectl get virtualservice -A

                echo "========== EnvoyFilters =========="
                kubectl get envoyfilter -A
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment SUCCESSFUL 🚀"
        }
        failure {
            echo "Deployment FAILED ❌"
        }
    }
}
