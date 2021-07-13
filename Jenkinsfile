pipeline {
  agent any
  stages {
    stage('Git Clone') {
      steps {
        git 'https://github.com/kiplink/cicd.git'
      }
    }
    stage('Build') {
      steps{
        script {
          dockerImage = docker.build asia.gcr.io/allcare-systems/cicd + ":$BRANCH_NAME-$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', 'allcare' )    {
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
