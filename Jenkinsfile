pipeline {

  environment {
    registry = "tetris/myweb"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Darrs08/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      environment {
        registryCredential = 'dockerhub'
      }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com'', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
