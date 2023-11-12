pipeline{
    agent any

    environment{

        DOCKERHUB_USERNAME = "SantinoTona"
        APP_NAME = "gitops-argocd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}".toLowerCase() // Convertido a minúsculas
        REGISTRY_CREDS = 'dockerhub'
    }

    stages {
        stage('Cleanup workspace') {
            steps {
                script {
                    deleteDir()
                }
            }
        }
        stage('Checkout SCM'){
            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/TonyMarzano/gitops-argocd-project.git',
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
        stage('Push docker image'){
            steps{
                script{

                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("${BUILD_NUMBER}".toLowerCase()) // Convertido a minúsculas
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}


// 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