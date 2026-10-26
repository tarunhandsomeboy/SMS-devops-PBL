
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

        bat '"C:\\Users\\Varun\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" version'

        bat '"C:\\Users\\Varun\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" ps'
    }
}

         }
  }

