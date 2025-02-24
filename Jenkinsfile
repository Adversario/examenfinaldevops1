pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Adversario/examenfinaldevops1.git', branch: 'master'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deployment') {
            steps {
                sh 'docker stop contenedor_vehiculos || true'
                sh 'docker rm contenedor_vehiculos || true'
                sh 'docker build -t imagen_vehiculos .'
                sh 'docker run -d -p 9090:8080 --name contenedor_vehiculos imagen_vehiculos'
            }
        }
        stage('Post-Deployment Verification') {
            steps {
                sh 'curl -s http://localhost:8080/endpoint_salud'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline ejecutado exitosamente.'
        }
        failure {
            echo 'Hubo un error en el pipeline.'
        }
    }
}
