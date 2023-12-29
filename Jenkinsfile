pipeline {
    agent any

    environment {
        // Define the Docker Hub credentials ID
        DOCKERHUB_CREDENTIALS = credentials('DOCKER_HUB_CREDENTIALS_ID')
        // Define the Docker image name and tag
        DOCKER_IMAGE = 'vvgadhave@gmail.com/testimage'
        // Define the Dockerfile path (if not in the project root)
        DOCKERFILE_PATH = '.' // Set to the directory containing your Dockerfile
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    docker.build(DOCKER_IMAGE, "-f ${/var/jenkins_home/workspace/Test/}/Dockerfile ${.}")
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
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials-id') {
                        sh "docker push ${DOCKER_IMAGE}"
                    }
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
