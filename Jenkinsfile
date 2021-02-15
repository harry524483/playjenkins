pipeline {
    agent { docker { image 'lachlanevenson/k8s-helm:latest' } }
    stages {
        stage('deploy') {
            steps {
                sh 'helm ls'
            }
        }
    }
}