pipeline {
    agent any

    tools {
        // Make sure Git is configured in Jenkins under Global Tools Configuration
        git 'Default'
    }

    environment {
        IMAGE_NAME = 'attendance-tracker-app'
        PORT = '5000'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Bhagyavan8050/AttendanceTracker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Run Docker Compose') {
            steps {
                bat "docker-compose up -d"
            }
        }

        stage('Test API') {
            steps {
                bat '''
                    echo Waiting for the app to start...
                    setlocal enabledelayedexpansion
                    for /l %%x in (1, 1, 30) do (
                        curl -s http://localhost:%PORT%/attendance >nul
                        if !errorlevel! == 0 (
                            echo App is running!
                            exit /b 0
                        )
                        timeout /t 2 >nul
                    )
                    echo Application did not respond in time
                    exit /b 1
                '''
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhost299', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        bat '''
                            echo Logging into DockerHub...
                            echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
                            docker tag attendance-tracker-app %DOCKER_USERNAME%/attendance-tracker-app:latest
                            docker push %DOCKER_USERNAME%/attendance-tracker-app:latest
                        '''
                    }
                }
            }
        }
    }
}
