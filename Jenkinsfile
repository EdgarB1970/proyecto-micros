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
                docker ps -a --format "{{.Names}}" | findstr /I euraka
                IF %ERRORLEVEL%==0 (
                    docker stop euraka
                    docker rm euraka
                ) ELSE (
                    echo No existe contenedor euraka
                )
                docker run -d -p 8761:8761 --name euraka euraka
                '''
            }
        }
    }
}
