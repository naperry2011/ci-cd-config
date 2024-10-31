pipeline {
    agent any
    stages {
        stage('Check Docker') {
            steps {
                // Check if Docker is accessible and print the Docker version
                sh 'docker --version'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build the Docker image and tag it as 'my-python-app'
                sh 'docker build -t my-python-app .'
            }
        }
        
        stage('Push to Artifactory') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'artifactory-credentials', usernameVariable: 'JFROG_USER', passwordVariable: 'JFROG_PASSWORD')]) {
                    // Login to JFrog Artifactory
                    sh 'docker login -u $JFROG_USER -p $JFROG_PASSWORD https://perry2011.jfrog.io/artifactory'
                    
                    // Tag the image with the full path in Artifactory
                    sh 'docker tag my-python-app:latest perry2011.jfrog.io/artifactory/docker-repo/my-python-app:latest'
                    
                    // Push the tagged image to JFrog Artifactory
                    sh 'docker push perry2011.jfrog.io/artifactory/docker-repo/my-python-app:latest'
                }
            }
        }
        
        stage('Deploy with ArgoCD') {
            steps {
                // Trigger an ArgoCD sync to deploy the new image
                sh 'argocd app sync my-python-app'
            }
        }
    }
}
