pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'sulbiraj'
        APP_NAME = 'gitops-argo-app'
        IMAGE_TAG = "v1.${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
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
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('', REGISTRY_CREDS) {
                        docker_image.push("v1.$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete docker image') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Update deployment file') {
            steps {
                script {
                    sh """
                        cat deployment.yaml
                        sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                        cat deployment.yaml
                    """
                }
            }
        }
        stage('Push the manifest changes back to GitHub') {
            steps {
                script {
                    sh """
                        git config --global user.name "sulbiraj06"
                        git config --global user.email "sulbiraj@gmail.com"
                        git add deployment.yml 
                        git commit -m "updated the deployment file to ${IMAGE_TAG} by Jenkins"
                    """
                    withCredentials([string(credentialsId: 'github-token', variable: 'github-creds')]) {
                        sh "git push https://github.com/sulbiraj06/gitops_argocd_project.git master"
                    }
                }
            }
        }
    }
}