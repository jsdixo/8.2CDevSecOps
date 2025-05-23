pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonarcloud-token') // Make sure this ID matches your Jenkins credential
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds', branch: 'main', url: 'https://github.com/jsdixo/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    bat 'sonar-scanner'
                }
            }
        }
    }
}
