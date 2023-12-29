pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_credentials')
        DOCKER_IMAGE = 'vvgadhave/test1'
        DOCKERFILE_PATH = '.'  // Set to the directory containing your Dockerfile
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} -f ${DOCKERFILE_PATH}/Dockerfile ${DOCKERFILE_PATH}"
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Extract Docker Hub credentials
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        // Login to Docker Hub
                        sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push Docker image to Docker Hub
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! Docker image built and pushed to Docker Hub.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
