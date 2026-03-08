pipeline {
    agent { label 'SPC' }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Ramana-Kandi/spring-petclinic.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar_id', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SONAR') {
                        sh '''mvn sonar:sonar \
                              -Dsonar.projectKey=Ramana-Kandi_spring-petclinic \
                              -Dsonar.organization=ramana \
                              -Dsonar.host.url=https://sonarcloud.io \
                              -Dsonar.token=$SONAR_TOKEN'''
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('binary file store') {
            steps {
                rtupload (
                    serviceId: 'JFROG',
                    spec: '''{
                        "files": [
                            {
                                "pattern": "target/*.jar",
                                "target": "spcjava-spc"
                            }
                        ]
                    }'''
                )
                rtpublishbuildInfo(serverId: 'JFROG')
            }
        }
    }
    post {
        always { echo 'Pipeline finished' }
        failure { echo 'Pipeline failed' }
    }
}