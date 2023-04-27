pipeline {
    agent any
    environment {
        IMG = "nand/devops-project"
        IMAGE_NAME = "nand/devops-project${BUILD_NUMBER}"
        GIT_REPO = "https://github.com/1599nand/GuessingGame.git"
        GIT_BRANCH = "main"
        CONTAINER_NAME = "GuessingGame"
    }
    stages {
        stage('Checkout Git') {
            steps {
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"
            }
        }
        stage('Build Image') {
            steps {
                script {
                    sh "sudo docker kill ${CONTAINER_NAME}|| true"
                    sh "sudo docker rm ${CONTAINER_NAME}|| true"
                    sh "sudo docker build . -t ${IMAGE_NAME}"
                }
            }
        }
        stage('Deploy Nginx') {
            steps {
                sh "sudo docker run -it -p 100:80 --name ${CONTAINER_NAME} -d ${IMAGE_NAME}"
            }
        }
        stage('Clear Image'){
            steps {
                sh "sudo docker rmi ${IMG}${BUILD_NUMBER.toInteger()-1}"
            }
        }
    }
}
