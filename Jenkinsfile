pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'docker-hub-creds'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarScanner') {
                    sh "${tool('SonarScanner')}/bin/sonar-scanner"
                }
            }
        }
    
        stage('Test Docker') {
            steps {
                sh 'docker version'
            }
        }
        
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
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

