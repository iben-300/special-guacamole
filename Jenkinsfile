pipeline {
    agent any

    environment {
        REPO_URL        = "https://github.com/iben-300/special-guacamole.git"
        DOCKER_IMAGE    = "book-app:latest"
        CONTAINER_NAME  = "book-app-container"
        APP_PORT        = "3000"
    }

    options {
        disableConcurrentBuilds()
        timestamps()
        timeout(time: 10, unit: 'MINUTES')
        retry(2)
    }

    stages {
        stage('Install Dependencies') {
            steps {
                bat "npm install"
            }
        }

        stage('Test') {
            steps {
                bat "npm test"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${env.DOCKER_IMAGE} ."
                }
            }
        }

        

        stage('Deploy Docker Image') {
            steps {
                script {
                    bat """
                    docker stop ${env.CONTAINER_NAME}  exit 0
                    docker rm ${env.CONTAINER_NAME}  exit 0
                    docker run -d --name ${env.CONTAINER_NAME} -p ${env.APP_PORT}:${env.APP_PORT} ${env.DOCKER_IMAGE}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded '
        }
        failure {
            echo 'Pipeline failed '
        }
        always {
            cleanWs()
        }
    }
}