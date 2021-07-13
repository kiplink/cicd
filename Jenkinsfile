pipeline {

  environment {
    PROJECT = "allcare-systems"
    APP_NAME = "cicd"
    FE_SVC_NAME = "${APP_NAME}-frontend"
    CLUSTER = "allcare"
    CLUSTER_ZONE = "asia-southeast2-a"
    IMAGE_TAG = "asia.gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    JENKINS_CRED = "allcare"
  }

  agent any
  stages {
    stage('Test') {
      steps {
        container('nginx') {
          sh """
          curl localhost
          """
        }
      }
    }
    stage('Build') {
      steps{
        script {
          dockerImage = docker.build env.IMAGE_TAG
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', env.JENKINS_CRED )    {
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
        step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT, clusterName: env.CLUSTER, location: env.CLUSTER_ZONE, manifestPattern: 'deployment.yaml', credentialsId: env.JENKINS_ID, verifyDeployments: true])
        echo "Deployment Finished ..."
      }
    }
  }
}
