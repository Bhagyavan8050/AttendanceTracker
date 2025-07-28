pipeline {
    agent any

    stages {
        stage('Clone Repo') {
    steps {
        git branch: 'main', url: 'https://github.com/Bhagyavan8050/AttendanceTracker.git'
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
                withCredentials([usernamePassword(credentialsId: 'dockerhost299', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh 'docker tag attendance-tracker-app $DOCKER_USERNAME/attendance-tracker-app:latest'
                    sh 'docker push $DOCKER_USERNAME/attendance-tracker-app:latest'
                }
            }
        }
    }
}
