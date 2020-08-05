pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 10, unit: 'MINUTES')
        timestamps()  // Timestamper Plugin
        disableConcurrentBuilds()
    }
    environment {
      GREETINGS_TO = 'Jenkins Techlab'
        PATH+MAVEN = '/path/to/maven'
    }
    stages {
        stage('Greeting') {
            steps {
                echo 'normal:'
                echo "Hello ${env.GREETINGS_TO}!"
                echo 'shell:'
                sh 'echo "Hello, $GREETINGS_TO!"'
                echo "${env.PATH}"
            }
        }
    }
}
