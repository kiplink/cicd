pipeline {
  agent any
  environment {
    registry = "asia.gcr.io/allcare-systems/cicd-test"
    registryCredential = 'allcare'
  }
  stages {
    stage('Git Clone') {
      steps {
        git 'https://github.com/kiplink/cicd.git'
      }
    }
    stage('Build') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
  }
}
