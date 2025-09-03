pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub-creds')  // Docker Hub creds stored in Jenkins
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        IMAGE_NAME = "ravimccullum/devops-build:dev-${BUILD_NUMBER}"
                    } else if (env.GIT_BRANCH == 'origin/master' || env.GIT_BRANCH == 'origin/main') {
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
