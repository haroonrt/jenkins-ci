pipeline {
    agent any

    environment {
        // Replace with your actual Docker Hub credentials ID from Jenkins
        DOCKER_HUB_CREDENTIALS = 'docker-hub-creds'
    }

    tools {
        // Make sure this tool name matches your Jenkins Global Tool Configuration
        sonarScanner = 'SonarScanner'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv() {
                    sh "${tool('SonarScanner')}/bin/sonar-scanner"
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }

        stage('Build and Push Backend Image') {
            steps {
                sh '''
                    docker build -t haroon415/python:backend ./backend
                    docker push haroon415/python:backend
                '''
            }
        }

        stage('Build and Push DB Image') {
            steps {
                sh '''
                    docker build -t haroon415/python:db ./db
                    docker push haroon415/python:db
                '''
            }
        }
    }
}
