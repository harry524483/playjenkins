pipeline {
  environment {
    DEPLOY = "${env.BRANCH_NAME == "master" || env.BRANCH_NAME == "develop" ? "true" : "false"}"
    VERSION = "${BUILD_NUMBER}"
    REGISTRY = 'harry1989/jenkins-web'
    REGISTRY_CREDENTIAL = 'dockerhub'
    GITHUB_CREDENTIAL = 'github-access'
  }

  agent {
    kubernetes {
      defaultContainer 'jnlp'
      yamlFile 'build.yaml'
    }
  }

  stages {
    stage("Checkout repos") {
      steps {
        dir("playjenkins") {
          git credentialsId: "${GITHUB_CREDENTIAL}",
            url: "https://github.com/harry524483/playjenkins.git"
        }

        dir("jenkins-helm-deployment") {
          git credentialsId: "${GITHUB_CREDENTIAL}", branch: 'main',
            url: 'https://github.com/harry524483/jenkins-helm-deployment.git'
        }
      }
    }

    stage('Docker Build') {
      when {
        environment name: 'DEPLOY', value: 'true'
      }
      steps {
        container('docker') {
          sh '''
            ls
            cd playjenkins
            ls
            docker build -t ${REGISTRY}:${VERSION} .
          '''
        }
      }
    }

    stage('Docker Publish') {
      when {
        environment name: 'DEPLOY', value: 'true'
      }
      steps {
        container('docker') {
          withDockerRegistry([credentialsId: "${REGISTRY_CREDENTIAL}", url: ""]) {
            sh "docker push ${REGISTRY}:${VERSION}"
          }
        }
      }
    }

    stage('Kubernetes Deploy') {
      when {
        environment name: 'DEPLOY', value: 'true'
      }
      steps {
        container('helm') {
          sh '''
            ls
            cd jenkins-helm-deployment
            ls
            helm upgrade --install --set web.tag=${VERSION} jenkins-web .
          '''
        }
      }
    }
  }
}