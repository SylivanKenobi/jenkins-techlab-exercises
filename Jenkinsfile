@Library('jenkins-techlab-libraries') _

pipeline {
    agent any // with hosted env use agent { label env.JOB_NAME.split('/')[0] }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 10, unit: 'MINUTES')
        timestamps()  // Requires the "Timestamper Plugin"
    }
    tools {
        jdk 'jdk8'
        maven 'maven35'
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo hi'
                sh 'mvn -B -V -U -e clean verify -Dsurefire.useFile=false'
                archiveArtifacts 'target/*.?ar'
            }
            post {
              always {
                junit 'target/**/*.xml'  // Requires JUnit plugin
                }
            }
        }
    }
  post {
    always {
        logEnvVariables('jenkins-techlab')
    }
  }
}
