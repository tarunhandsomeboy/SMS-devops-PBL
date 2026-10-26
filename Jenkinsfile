
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out Student Management System code'
                checkout scm
            }
        }

        stage('Verify Project Files') {
            steps {
                bat 'dir'
                bat 'if not exist index.html exit /b 1'
                echo 'Project files verified successfully.'
            }
        }

        stage('Docker Connection Test') {
            steps {
                echo 'Testing Docker connection'
                bat 'where docker'
                bat 'docker version'
                bat 'docker ps'
            }
        }
    }
}
