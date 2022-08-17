pipeline {
    agent {
        label 'docker'
    }
    environment {
      imagename = "gameoflife:1.2"
      registryCredential = "jfrog-id"
      dockerImage = ''
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
              )
            }
        }
        // stage('Push image to Jfrog') {
        //   steps {
        //       rtDockerPush(
        //           serverId: 'JFROG_INSTANCE',
        //           image: 'gameoflife:1.2',
        //           targetRepo: 'sivarani42.jfrog.io/gameoflife/gameoflife:1.2'
        //       )
        //   }
        // }
        stage('Push Image to Jfrog'){
          steps {
            script {
              withCredentials([usernamePassword(credentialsId: 'jfrog-id', passwordVariable: 'jfrogpasswd', usernameVariable: 'jfrog-user')]) {
                  sh "docker login -u ${env.jfrog-user} -p ${env.jfrogpasswd}"
                  sh 'docker push sivarani42.jfrog.io/gameoflife/gameoflife:1.2'
              }
            }
          }
        }
    }
}