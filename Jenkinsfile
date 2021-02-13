podTemplate(
  label:"comic-relief-ui-agent",
  containers: [
    containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat')
  ],
  volumes: [
    hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')
  ]
) {
  node(POD_LABEL) {
    stage('Get playjenkins') {
      git "https://github.com/harry524483/playjenkins.git"
      container('docker') {
        stage('Run docker commands') {
          sh 'echo Hello $POD_CONTAINER'
          sh 'pwd'
          sh 'ls'
          sh 'docker build .'
        }
      }
    }
  }
}
