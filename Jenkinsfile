pipeline {
    agent any
    stages {
        stage('Check Docker') {
            steps {
                // Verify that Docker is available
                sh 'docker --version'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build the Docker image using the Dockerfile in the root directory
                sh 'docker build -t my-python-app .'
            }
        }
        
        stage('Push to Artifactory') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'artifactory-credentials', usernameVariable: 'JFROG_USER', passwordVariable: 'JFROG_PASSWORD')]) {
                    // Log into JFrog Artifactory
                    sh 'docker login -u $JFROG_USER -p $JFROG_PASSWORD https://perry2011.jfrog.io/artifactory'
                    
                    // Tag and push the image to JFrog Artifactory
                    sh 'docker tag my-python-app:latest perry2011.jfrog.io/artifactory/docker-repo/my-python-app:latest'
                    sh 'docker push perry2011.jfrog.io/artifactory/docker-repo/my-python-app:latest'
                }
            }
        }
        
        stage('Deploy with ArgoCD') {
            steps {
                // Trigger an ArgoCD sync to deploy the updated image
                sh 'argocd app sync my-python-app'
            }
        }
    }
}
