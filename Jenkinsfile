podTemplate(
  containers: [
    containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.8', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')
  ]
) {
  node(POD_LABEL) {
    def myRepo = checkout scm
    def gitCommit = myRepo.GIT_COMMIT
    def gitBranch = myRepo.GIT_BRANCH
    def shortGitCommit = "${gitCommit[0..10]}"
    def previousGitCommit = sh(script: "git rev-parse ${gitCommit}~", returnStdout: true)
 
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

    stage('Run kubectl') {
      container('kubectl') {
        sh "kubectl get pods"
      }
    }

    stage('Run helm') {
      container('helm') {
        sh "helm list"
      }
    }
  }
}
