pipeline {
    agent any
    environment {
    
      dockerImage = ""
      DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image') {
            steps {
                sh "docker build . -t tetris/myweb:${DOCKER_TAG}"
            }
        }
        stage('DokerHub Push') {
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u darrs08 -p ${dockerHubPwd}"
                    sh "docker push tetris/myweb:${DOCKER_TAG}"
                }
            }
        }
       stage('Deploying App to Kubernetes') {
           steps {
               script {
                   kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubernetes")
               }
           }
      }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
