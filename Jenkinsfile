pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("my-python-app")
                }
            }
        }
        stage('Push to Artifactory') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'artifactory-credentials', usernameVariable: 'ARTIFACTORY_USER', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {
                    sh 'docker login -u $ARTIFACTORY_USER -p $ARTIFACTORY_PASSWORD <jfrog-artifactory-url>'
                    sh 'docker push <jfrog-artifactory-url>/my-python-app:latest'
                }
            }
        }
        stage('Deploy with ArgoCD') {
            steps {
                script {
                    sh 'argocd app sync my-python-app'
                }
            }
        }
    }
}
