pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        timestamps()
    }

    environment {
        DOCKER = 'C:\\Users\\Varun\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
        IMAGE_NAME = 'student-management'
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
                bat '''
                    if not exist index.html exit /b 1
                    if not exist Dockerfile exit /b 1
                    if not exist students.html exit /b 1
                    echo Project files verified successfully.
                '''
            }
        }

        stage('Docker Connection Test') {
            steps {
                bat '''
                    "%DOCKER%" version
                    "%DOCKER%" ps
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                    "%DOCKER%" build -t student-management:jenkins-%BUILD_NUMBER% .
                '''
            }
        }

        stage('Test New Image') {
            steps {
                bat '''
                    "%DOCKER%" run -d --name sms-jenkins-check-%BUILD_NUMBER% -p 127.0.0.1:18084:80 student-management:jenkins-%BUILD_NUMBER%
                '''

                bat '''
                    powershell -NoProfile -Command "$ok=$false; for($i=0; $i -lt 15; $i++){ try { $r=Invoke-WebRequest -Uri 'http://localhost:18084' -TimeoutSec 3; if($r.StatusCode -eq 200){$ok=$true; break} } catch {}; Start-Sleep -Seconds 2 }; if(-not $ok){exit 1}; Write-Output 'Test website is responding.'"
                '''
            }

            post {
                always {
                    bat '''
                        "%DOCKER%" rm -f sms-jenkins-check-%BUILD_NUMBER%
                    '''
                }
            }
        }

        stage('Approve Production Deployment') {
            steps {
                input message: 'Tests passed. Deploy this version to production port 8080?', ok: 'Approve Deployment'
            }
        }

        stage('Deploy to Port 8080') {
            steps {
                echo 'Production deployment requires a protected container swap.'
                echo 'Do not replace the current production container until the deployment procedure is confirmed.'
            }
        }
    }
}