pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('Jenkins')
  }
  stages {
    stage('Building Docker Image') {
      steps {
        sh 'docker build -t vvgadhave/ubuntu-gui:v1 .'
      }
    }
  }
}
