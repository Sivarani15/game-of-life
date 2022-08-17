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
        // stage('Build') {
        //     steps {
        //         script {
        //           sh 'mvn clean install'
        //         }
        //     }
        // }
        stage('Docker Image build') {
            steps {
                script {
                    sh 'docker image build -t gameoflife:1.2 .'
                }
            }
        }
        stage('Arti-Configuration') {
            steps {
              rtServer (
                  id: 'JFROG_INSTANCE',
                  url: 'https://sivarani42.jfrog.io/gameoflife',
                  credentialsId: 'jfrog-id'
              )
            }
        }
        stage('Push image to Jfrog') {
          steps {
              rtDockerPush(
                  serverId: 'JFROG_INSTANCE',
                  image: 'gameoflife:1.2',
                  targetRepo: 'sivarani42.jfrog.io/gameoflife/gameoflife:1.2'
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