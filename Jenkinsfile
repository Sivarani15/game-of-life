pipeline {
    agent {
        label 'docker'
    }
    stages {
        stage('Source Code') {
            steps {
                git url: "https://github.com/Sivarani15/game-of-life.git", branch: "docker"
            }    
        }
        stage('Docker Image build') {
            steps {
            script {
                sh 'docker image build -t gameoflife:1.1 .'
            }
            }
        }
        stage('Arti-Configuration') {
            steps {
              rtServer (
                  id: 'JFROG_INSTANCE',
                  url: 'https://sivarani42.jfrog.io',
                  credentialsId: 'jfrog-id'
              )
            }
        }
        stage('Push image to Jfrog') {
          steps {
              rtDockerPush(
                  serverId: 'JFROG_INSTANCE',
                  image: 'sivarani42.jfrog.io/gameoflife/gameoflife.war',
              )
          }
        }
        // stage('Push Image to Jfrog'){
        //   steps {
        //     script {
        //       withCredentials([string(credentialsId: 'jfrog-id', variable: 'jfrogpwd')]) {
        //         sh 'docker login -u sivarani42@gmail.com -p ${jfrogpwd}'
        //       }
        //       sh 'docker push sivarani42.jfrog.io/gameoflife/'
        //     }
        //   }
        // }
    }
}