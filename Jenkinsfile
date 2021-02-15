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
    dir("playjenkins") {
      git credentialsId: 'github-access',
        url: "https://github.com/harry524483/playjenkins.git"
    }

    dir("jenkins-helm-deployment") {
      git credentialsId: 'github-access', branch: 'main',
        url: 'https://github.com/harry524483/jenkins-helm-deployment.git'
    }
    
    stage('Build') {
      container('docker') {
        withDockerRegistry([credentialsId: 'dockerhub', url: ""]){
          sh '''
            cd playjenkins
            docker build -t harry1989/jenkins-web:latest .
            docker push harry1989/jenkins-web:latest
          '''
        }
      }
    }

    stage('Deploy') {
      container('helm') {
        sh '''
        ls
        cd jenkins-helm-deployment
        helm upgrade --install --set web.tag=latest jenkins-web .
        '''
      }
    }    
  }
}