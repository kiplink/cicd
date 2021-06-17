pipeline {
  agent any
  stages {
    stage("Build") {
      steps {
        sh "docker build -t kiplink/cicd:1.0 ."
      }
    }
  }
}
