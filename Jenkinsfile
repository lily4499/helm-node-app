pipeline {
    agent any

    environment {
        MINIKUBE_IP = "192.168.49.2"  // Set Minikube IP
        APP_NAME = "node-app"
        NAMESPACE = "default"
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the Helm chart repository
                git url: 'https://github.com/your-repo/node-app.git', branch: 'main'
            }
        }

        stage('Helm Install') {
            steps {
                script {
                    // Set up kubectl and helm context for Minikube
                    sh 'kubectl config use-context minikube'
                    sh 'helm upgrade --install $APP_NAME ./node-app --namespace $NAMESPACE'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                // Check deployment status
                sh 'kubectl rollout status deployment/$APP_NAME-deployment -n $NAMESPACE'
                sh 'kubectl get all -n $NAMESPACE -l app=$APP_NAME'
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}

