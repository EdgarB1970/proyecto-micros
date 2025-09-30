pipeline {
    agent any
   environment {
        PATH = "C:/Program Files/Docker/Docker/resources/bin;"
    }
    tools {
        maven 'Maven3'    // Nombre EXACTO configurado en Jenkins -> Global Tool Configuration
        jdk 'Java21'      // O el que hayas configurado (ej: Java17)
    }

    stages {
        stage('Checkout') {
     steps {
                git branch: 'master', url: 'https://github.com/EdgarB1970/proyecto-micros.git', credentialsId: 'github-credentials'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t euraka .'
            }
        }

        stage('Docker Run') {
            steps {
                sh 'docker stop euraka || true && docker rm euraka || true'
                sh 'docker run -d -p 8761:8761 --name euraka euraka'
            }
        }
    }
}