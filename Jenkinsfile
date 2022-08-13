pipeline {
    agent any
    stages {
      stage('Docker Image build') {
        steps {
          script {
            sh 'docker image build -t gameoflife:1.1 .'
          }
        }
      }
      stage('Push Image to Jfrog'){
        steps {
          script {
            withCredentials([string(credentialsId: 'jfrog-id', variable: 'jfrogpwd')]) {
              sh 'docker login -u sivarani42.jfrog.io -p ${jfrogpwd}'
            }
             sh 'docker push sivarani42.jfrog.io/gameoflife/'
          }
        }
      }
    }
}