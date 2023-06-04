pipeline{

    agent any 
    
    environment {

        DOCKERHUB_USERNAME = "vikashashoke" 
        APP_NAME = "gitops-argo-app"  
        IMAGE_TAG="${BUILD_NUMBER}" 
        IMAGE_NAME="${DOCKERHUB_USERNAME}" 
        REGISTRY CREDS = 'dockerhub' 
    
    }

    stages { 
        stage('Clenup workspace') {
            steps{
                script {
                    cleanws()
                }
            }
        }
        stage('Checkout SCM') {
            script {
                git credentialsId : 'github',
                url : 'https://github.com/vikash-kumar01/gitops_argocd_project.git',
                branch : 'master'
            }
        }