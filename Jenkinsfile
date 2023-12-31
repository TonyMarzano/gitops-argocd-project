pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "SantinoTona"
        APP_NAME = "gitops-argocd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/${APP_NAME}".toLowerCase()
        REGISTRY_CREDS = 'dockerhub'
    }

    stages {
        stage('Cleanup workspace') {
            steps {
                script {
                    echo 'Cleaning up workspace'
                    cleanWs()
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                script {
                    echo 'Checking out SCM'
                    git credentialsId: 'github', url: 'https://github.com/TonyMarzano/gitops-argocd-project.git', branch: 'main'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Build Docker Image Latest') {
            steps {
                echo 'Building Docker Image with latest tag'
                script {
                        sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }


        stage('Push docker image') {
            steps {
                script {
                    echo 'Pushing Docker Image'
                    echo "IMAGE_TAG: ${IMAGE_TAG}" // Add this line for debugging
                    docker.withRegistry('', REGISTRY_CREDS) {
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                        sh "docker push ${IMAGE_NAME}:latest"
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

        stage('Updating kubernetes deployment file'){
            steps{
                script{

                    sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                    """
                }
            }
        }

        stage('Push the changed deployment file to Git'){
            steps{
                script{
                    sh """
                     git config --global user.name "TonyMarzano"
                     git config --global user.email "marzanosantino@gmail.com"
                     git add deployment.yaml
                     git commit -m "Updated the deployment.yaml"
                     
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                        sh "git push 'https://github.com/TonyMarzano/gitops-argocd-project.git' main"
                    }
                }
            }
        }
    }
}