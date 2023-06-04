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
        stage('Checkout Code From Github'){

            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/mxnuchim/GitOps-ArgoCD-Project-DevOps.git',
                    branch: 'main'
                }
            }
        }
        stage('Build Docker Image'){

            steps{
                script{

                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{

                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("${BUILD_NUMBER}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker Images'){

            steps{
                script{

                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Update Kubernetes Manifest File'){
            steps{
                script{
                    sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                    """
                }
            }
        }
        stage('Push the changed deployment manifest to Git'){
            steps{
                script{
                    sh """
                    git config --global user.name "mxnuchim"
                    git config --global user.email "manuchimoliver779@gmail.com"
                    git add deployment.yml
                    git commit -m "updated deployment file"
                    """

                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                        sh "git push https://github.com/mxnuchim/GitOps-ArgoCD-Project-DevOps.git main"
                    }
                }
            }
        }
    }
}