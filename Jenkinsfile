pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mahendrashinde.azurecr.io/jenkins-demo'
        DOCKER_REGISTRY = 'mahendrashinde.azurecr.io'
        DOCKER_CREDENTIALS_ID = 'acr-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}").push()
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}").remove()
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}