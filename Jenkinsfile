pipeline {
    agent any
    environment {
        PATH = "C:/Program Files/Docker/Docker/resources/bin;" + env.PATH
    }
    tools {
        maven 'Maven3'
        jdk 'Java21'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/EdgarB1970/proyecto-micros.git', credentialsId: 'github-credentials'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t euraka .'
            }
        }

        stage('Docker Run') {
            steps {
                bat '''
                docker stop euraka || echo "No estaba corriendo"
                docker rm euraka || echo "No existía"
                '''
                bat 'docker run -d -p 8761:8761 --name euraka euraka'
            }
        }
    }
}
