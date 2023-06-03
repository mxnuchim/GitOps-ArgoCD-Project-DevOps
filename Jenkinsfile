pipeline{

    agent any

    environment{

        DOCKERHUB_USERNAME = "manuchim"
        APP_NAME = "gitops-argo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages{

        stage('Clean Workspace'){

            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){

            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/mxnuchim/GitOps-ArgoCD-Project-DevOps.git',
                    branch: 'main'
                }
            }
        }
    }
}