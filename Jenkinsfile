pipeline {
    agent {label 'SPC'}
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage ('git checkout') {
            steps {
                git url: 'https://github.com/ManideepKesham/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('build and checkout') {
            steps {
                withCredentials([string(credentialsId: 'sonar_id',variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('sonar'){
                        sh '''mvn clean package sonar:sonar \
                              -Dsonar.projectKey=ManideepKesham_spring-petclinic \
                              -Dsonar.organization=manideepkesham \
                              -Dsonar.host.url=https://sonarcloud.io \
                              -Dsonar.login=$SONAR_TOKEN'''
                    }
                }
            }
        }
    }
}