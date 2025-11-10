pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "jagpreeth/CI-Kubernetes-App"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'kubernetes', url: 'https://github.com/Jagpreeth/CICD-K8S-3Tier.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl set image deployment/backend-deploy backend=$IMAGE_NAME:$BUILD_NUMBER -n backend'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl rollout status deployment/backend-deploy -n backend'
            }
        }
    }

    post {
        failure {
            sh 'kubectl rollout undo deployment/backend-deploy -n backend'
            echo "❌ Deployment failed, rolled back to previous version"
        }
        success {
            echo "✅ Deployment successful! Version: $BUILD_NUMBER"
        }
    }
}
