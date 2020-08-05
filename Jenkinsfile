pipeline {
    agent any // with hosted env use agent { label env.JOB_NAME.split('/')[0] }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 10, unit: 'MINUTES')
        timestamps()  // Requires the "Timestamper Plugin"
    }
    environment {
        NVM_HOME = tool('nvm')
    }
    tools {
        jdk 'jdk8'
        maven 'maven35'
    }
    stages {
        stage('Build') {
            steps {
                sh """#!/bin/bash +x
                source \${HOME}/.nvm/nvm.sh
                nvm
                """

                sh 'java -version'

                sh 'javac -version'

                sh 'mvn --version'
            }
        }
    }
}
