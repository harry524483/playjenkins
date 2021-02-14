#!groovy

podTemplate(
  containers: [
    containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')
  ]
) {
  node(POD_LABEL) {
    // def myRepo = checkout scm
    // def gitCommit = myRepo.GIT_COMMIT
    // def gitBranch = myRepo.GIT_BRANCH
    // def shortGitCommit = "${gitCommit[0..10]}"
    // def previousGitCommit = sh(script: "git rev-parse ${gitCommit}~", returnStdout: true)

    dir('first-repo') {
      git credentialsId: 'github-access',
        url: "https://github.com/harry524483/playjenkins.git"
    }

    dir("second-repo") {
      git credentialsId: 'github-access',
        url: 'https://github.com/harry524483/jenkins-helm-deployment.git'
    }

    stage('Build') {
      container('docker') {
        withDockerRegistry([credentialsId: 'dockerhub', url: ""]){
          sh """
            pwd 
            ls
            docker build -t harry1989/jenkins-web:latest .
            docker push harry1989/jenkins-web:latest
          """
        }
      }
    }

    stage('Deploy') {
      container('helm') {
        sh '''
        #!/bin/bash

        cd /home/jenkins/agent/workspace/
        ls

        helm upgrade --install --force --set web.tag=latest jenkins-web .

        '''
      }
    }    
  }
}