pipeline{
    agent any

    environment{

        DOCKERHUB_USERNAME = "SantinoTona"
        APP_NAME = "gitops-argocd-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
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
    }
}


// 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