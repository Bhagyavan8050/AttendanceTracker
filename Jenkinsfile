pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Bhagyavan8050/AttendanceTracker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t attendance-tracker-app .'
            }
        }

        stage('Run Docker Compose') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Test API') {
            steps {
                sh 'curl http://localhost:5000/attendance'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhost299', variable: 'Bhagyavan8050')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u your-dockerhub-username --password-stdin'
                    sh 'docker tag attendance-tracker-app your-dockerhub-username/attendance-tracker-app:latest'
                    sh 'docker push your-dockerhub-username/attendance-tracker-app:latest'
                }
            }
        }
    }
}
