pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                bat '''
                    npm install
                    npm audit fix --force
                '''
            }
        }
        
        stage('Lint'){
            steps{
                bat '''
                npm run lint
                '''
            }
        }

        stage('Build'){
            steps{
                bat '''
                npm run build
                '''
            }
        }



        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-token')
            }
            steps {
                bat '''
                sonar-scanner -Dsonar.projectKey=mern-frontend ^
                              -Dsonar.projectName=mern-frontend ^
                              -Dsonar.sources=. ^
                              -Dsonar.host.url=http://localhost:9000 ^
                              -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Build or analysis failed. Please check the logs.'
        }
    }
}
