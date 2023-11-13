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
    }
}



// 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