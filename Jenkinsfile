pipeline {
    agent any
    environment {
        DOCKER_REPO = 'perry2011.jfrog.io/docker-repo'
        JFROG_CREDENTIALS = 'jfrog-credentials-id'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/naperry2011/ci-cd-config.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_REPO}/my-python-app:latest")
                }
            }
        }
        stage('Push Docker Image to JFrog') {
            steps {
                script {
                    docker.withRegistry('https://perry2011.jfrog.io', JFROG_CREDENTIALS) {
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Deploy with ArgoCD') {
            steps {
                script {
                    sh '''
                    argocd app sync <your-argocd-app-name> \
                    --auth-token $(argocd account get-token --server <argocd-server-url>) \
                    --server <argocd-server-url>
                    '''
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
