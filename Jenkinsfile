pipeline {
    agent any

    tools {
        sonarScanner 'SonarScanner'
    }

    environment {
        SONAR_HOST_URL = 'http://13.235.133.35/:9000'
        SONAR_AUTH_TOKEN = credentials('sonar-token') // ID from Jenkins credentials
    }

    stages {
        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner \
                        -Dsonar.projectKey=demo \
                        -Dsonar.sources=src \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_AUTH_TOKEN'
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
    }
}
