pipeline {
    environment {
        DEPLOY = "${env.BRANCH_NAME == "master" || env.BRANCH_NAME == "develop" ? "true" : "false"}"
        VERSION = "${BUILD_NUMBER}"
        REGISTRY = 'harry1989/jenkins-web'
        REGISTRY_CREDENTIAL = 'dockerhub'
    }
    agent {
        kubernetes {
            defaultContainer 'jnlp'
            yamlFile 'build.yaml'
        }
    }
    stages {
      stage('Build') {
            steps {
                container('helm') {
                    sh 'helm ls'
                }
            }
      }
    }

 }