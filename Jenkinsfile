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
                    deleteDir()
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                script {
                    echo 'Checking out SCM'
                    git credentialsId: 'github'
                    url: 'https://github.com/TonyMarzano/gitops-argocd-project.git'
                    branch: '*/main'
                }
            }
        }

        //stage('Build Docker Image') {
          //  steps {
            //    script {
              //      echo 'Building Docker Image'
                //    sh "docker build -t ${IMAGE_NAME} ."
                 //}
            //}
        //}


        stage('Push docker image') {
            steps {
                script {
                    echo 'Pushing Docker Image'
                    // docker.withRegistry('', REGISTRY_CREDS) {
                    //     docker_image.push("${BUILD_NUMBER}".toLowerCase())
                    //     docker_image.push('latest')
                    // }
                }
            }
        }
    }
}





// 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