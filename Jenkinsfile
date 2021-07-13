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
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential )    {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy Dev') {
      // Developer Branches
      when {
        not { branch 'master' }
      }
      steps{
        echo 'Deployment started ...'
        step([$class: 'KubernetesEngineBuilder', projectId: 'allcare-systems', clusterName: 'allcare', location: 'asia-southeast2-a', manifestPattern: 'deployment.yaml', credentialsId: 'allcare', verifyDeployments: true])
        echo "Deployment Finished ..."
      }
    }
  }
}
