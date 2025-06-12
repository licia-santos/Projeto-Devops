pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // ID salvo no Jenkins
        IMAGE_NAME = "liciasantos/fastapi-hello"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/licia-santos/Projeto-Devops' // troque pelo seu repo
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
                        docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push()
                        docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push("latest")
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build e push realizados com sucesso!'
        }
        failure {
            echo 'Erro durante o build ou push da imagem.'
        }
    }
}


