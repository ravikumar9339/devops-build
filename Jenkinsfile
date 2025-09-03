pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub-creds')   // Your Docker Hub creds stored in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${env.BRANCH_NAME}",
                    credentialsId: 'github-creds',   // Your GitHub creds stored in Jenkins
                    url: 'https://github.com/ravikumar9339/devops-build.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        IMAGE_NAME = "ravimccullum/devops-build:dev-${BUILD_NUMBER}"
                    } else if (env.BRANCH_NAME == 'master') {
                        IMAGE_NAME = "ravimccullum/devops-build:prod-${BUILD_NUMBER}"
                    } else {
                        error("This pipeline only supports dev and master branches!")
                    }
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh "echo ${DOCKERHUB_PSW} | docker login -u ${DOCKERHUB_USR} --password-stdin"
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_NAME}"
            }
        }
    }
}
