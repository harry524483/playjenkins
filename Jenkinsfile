pipeline {

  environment {
    registry = "192.168.1.81:5000/justme/myweb"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/harry524483/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        sh 'echo "Build image"'
      }
    }

    stage('Push Image') {
      steps{
        sh 'echo "Push Image"'
      }
    }

    stage('Deploy App') {
      steps {
        sh 'echo "Deploy App"'
      }
    }

  }

}
