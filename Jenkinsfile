pipeline {
  agent any
  stages {
    stage("Build") {
      steps {
        sh "docker build -t kiplink/cicd:1.0 ."
      }
    }
    stage('Push image') {
      docker.withRegistry('https://registry.hub.docker.com', 'DockerHub') {
        sh 'docker push kiplink/cicd-local:1.0'
      }
    }
  }
}
