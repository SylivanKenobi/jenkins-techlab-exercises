pipeline {
    agent any // with hosted env use agent { label env.JOB_NAME.split('/')[0] }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timeout(time: 10, unit: 'MINUTES')
        timestamps()  // Requires the "Timestamper Plugin"
    }
    environment{
        M2_SETTINGS = credentials('m2_settings')
        KNOWN_HOSTS = credentials('known_hosts')
        ARTIFACTORY = credentials('jenkins-artifactory')
        ARTIFACT = "${env.JOB_NAME.split('/')[0]}-hello"
        REPO_URL = 'https://artifactory.puzzle.ch/artifactory/ext-release-local'
    }
    tools {
        jdk 'jdk8'
        maven 'maven35'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -V -U -e clean verify -Dsurefire.useFile=false'
                sh "mvn -s '${M2_SETTINGS}' -B deploy:deploy-file -DrepositoryId='puzzle-releases' -Durl='${REPO_URL}' -DgroupId='com.puzzleitc.jenkins-techlab' -DartifactId='${ARTIFACT}' -Dversion='1.0' -Dpackaging='jar' -Dfile=`echo target/*.jar`"
                sshagent(['testserver']) {  // SSH Agent Plugin
                    sh "ls -l target"
                    sh "ssh -o UserKnownHostsFile='${KNOWN_HOSTS}' -p 2222 richard@testserver.vcap.me 'curl -O -u \'${ARTIFACTORY}\' ${REPO_URL}/com/puzzleitc/jenkins-techlab/${ARTIFACT}/1.0/${ARTIFACT}-1.0.jar && ls -l'"
                }
                archiveArtifacts 'target/*.?ar'
                junit 'target/**/*.xml'  // Requires JUnit plugin
            }
        }
    }
}
