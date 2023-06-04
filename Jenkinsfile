pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'sulbiraj'
        APP_NAME = 'gitops-argo-app'
        IMAGE_TAG = "v.${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}"
        REGISTRY_CREDS = 'docker-cred'
    }

    stages {
        stage('Cleanup workspace') {
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM') {
            steps {
                script {
                    git credentialsId : 'github-token',
                    url : 'https://github.com/sulbiraj06/gitops_argocd_project.git',
                    branch : 'master'
                }
            }
        }
        stage('Buid Docker Image') {
            steps {
                script {
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
    }
}
