
pipeline {
    agent any

    stages {
        stage('Validate Kubernetes') {
            steps {
                bat 'kubectl version --client'
            }
        }

        stage('Create ConfigMap') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl apply -f configmap.yaml'
                }
            }
        }

        stage('Deploy Application') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl apply -f deployment.yaml'
                    bat 'kubectl rollout status deployment/configmap-deployment --timeout=120s'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withCredentials([
                    file(credentialsId: 'kubeconfig-credentials',
                         variable: 'KUBECONFIG')
                ]) {
                    bat 'kubectl get configmap webapp-config'
                    bat 'kubectl get deployment configmap-deployment'
                    bat 'kubectl get pods -l app=configmap-app'
                }
            }
        }
    }
}
