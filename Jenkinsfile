pipeline {
    agent any

    environment {
        IMAGE = "jagpreeth/webapp:latest"   // Your Docker Hub image
    }

    stages {

        stage('Checkout') {
            steps {
                // Clone your GitHub repo
                git 'git@github.com:Jagpreeth/CICD-K8S-3Tier.git'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply K8s deployment YAML (make sure deployment uses IMAGE variable)
                    sh "kubectl apply -f k8s/deployment.yaml"
                    sh "kubectl apply -f k8s/service.yaml"
                    sh "kubectl apply -f k8s/ingress.yaml"
                }
            }
        }
    }

    post {
        success {
            echo "CI/CD pipeline completed successfully!"
        }
        failure {
            echo "CI/CD pipeline failed. Check logs."
        }
    }
}
