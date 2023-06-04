pipeline{

    agent any 
    
    environment {

        DOCKERHUB_USERNAME = "sulbiraj" 
        APP_NAME = "gitops-argo-app"  
        IMAGE_TAG="${BUILD_NUMBER}" 
        IMAGE_NAME="${DOCKERHUB_USERNAME}" 
        REGISTRY_CREDS = 'docker-cred' 
    
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
            steps{
                script {
                    git credentialsId : 'github-token',
                    url : 'https://github.com/sulbiraj06/gitops_argocd_project.git',
                    branch : 'master'
                }
            }
        }
    }
}