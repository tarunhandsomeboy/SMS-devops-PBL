pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        timestamps()
    }

    environment {
        DOCKER = 'C:\\Users\\Varun\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
        IMAGE_NAME = 'student-management:jenkins'
        CONTAINER_NAME = 'sms-jenkins-test'
        APP_PORT = '8083'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out Student Management System code'
                checkout scm
            }
        }

        stage('Verify Project Files') {
            steps {
                bat 'if not exist index.html exit /b 1'
                bat 'if not exist Dockerfile exit /b 1'
                bat 'if not exist compose.yaml exit /b 1'

                echo 'Project files verified successfully.'
            }
        }

        stage('Docker Connection Test') {
            steps {
                echo 'Testing Docker connection'
                bat '"%DOCKER%" version'
                bat '"%DOCKER%" ps'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Student Management System image'
                bat '"%DOCKER%" build -t %IMAGE_NAME% .'
            }
        }

        stage('Deploy Test on Port 8083') {
            steps {
                echo 'Deploying Student Management System to port 8083'

                // Remove only the Jenkins test container.
                // The production container on port 8080 is untouched.
                bat '"%DOCKER%" rm -f %CONTAINER_NAME% 2>NUL || exit /b 0'

                // Start the newly built image on port 8083.
                bat '"%DOCKER%" run -d --name %CONTAINER_NAME% -p %APP_PORT%:80 --restart unless-stopped %IMAGE_NAME%'

                // Confirm that the test container is running.
                bat '"%DOCKER%" ps --filter "name=%CONTAINER_NAME%"'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Test deployment completed.'
            echo 'Open http://localhost:8083'
            echo 'Production port 8080 was not changed.'
        }

        failure {
            echo 'FAILED: Check Console Output.'
        }

        always {
            echo 'Jenkins pipeline finished.'
        }
    }
}