pipeline {
    agent {label 'SPC'}
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage ('git checkout') {
            steps {
                git url: 'https://github.com/Ramana-Kandi/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('build and checkout') {
            steps {
                withCredentials([string(credentialsId: 'sonar_id',variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('sonar'){
                        sh '''mvn clean package sonar:sonar \
                              -Dsonar.projectKey=Ramana-Kandi_spring-petclinic \
                              -Dsonar.organization=Ramana \
                              -Dsonar.host.url=https://sonarcloud.io \
                              -Dsonar.login=$SONAR_TOKEN'''
                    }
                }
            }
        }
    }
}