pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('myapp-image').push('latest')
                }
            }
        }
        stage('Deploy with ArgoCD') {
            steps {
                script {
                    // Trigger ArgoCD sync here
                }
            }
        }
    }
}
