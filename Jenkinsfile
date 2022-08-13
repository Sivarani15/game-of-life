pipeline {
    agent any
    parameters {
        choice ( name: 'GOAL', choices: [ 'compile', 'test', 'package', 'clean package'])
    }
    stages {
        stage('Sourcecode') {
            steps {
                git url: "https://github.com/Sivarani15/game-of-life.git",  branch: "jenkins"
            }
        }
        stage('Build and SonarQubeAnalysis') {
            steps {
                withSonarQubeEnv('SONAR_LATEST')
                    sh script: "mvn ${params.GOAL} sonar:sonar"
            }
        }
        stage('Archiving test results') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            }
        }
        stage('Configuring Artifactory') {
            steps {
                rtMavenDeployer (
                    id: 'game-of-life',
                    serverId: 'JFROG_INSTANCE',
                    releaseRepo: 'gameoflife-release-local',
                    snapshotRepo: 'gameoflife-snapshot-local'
                )
            }
        }
        stage('Atrifact deployment') {
            steps {
                rtMavenRun (
                    // Tool name from Jenkins configuration.
                    tool: MAVEN_DEFAULT,
                    // Set to true if you'd like the build to use the Maven Wrapper.
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'game-of-life'
                )
            }
        }
    }
}