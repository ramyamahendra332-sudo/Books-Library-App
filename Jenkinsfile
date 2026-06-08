pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        NODE_OPTIONS = '--openssl-legacy-provider'
        SONAR_TOKEN = 'squ_905b99514fae1bcd273fac0c30089ab0a2944882'   // Your token added here
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install --legacy-peer-deps'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    
                    withSonarQubeEnv('SonarQube') {
                        bat """
                            ${scannerHome}\\bin\\sonar-scanner.bat ^
                            -Dsonar.projectKey=books-library-app ^
                            -Dsonar.projectName=Books-Library-App ^
                            -Dsonar.sources=src ^
                            -Dsonar.exclusions=**/node_modules/**,**/build/** ^
                            -Dsonar.token=%SONAR_TOKEN%
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and SonarQube scan completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check console output.'
        }
    }
}