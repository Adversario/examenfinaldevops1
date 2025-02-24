pipeline {
    agent any

    tools {
        // Usa el nombre exactamente como aparece en "Global Tool Configuration"
        jdk 'JDK'
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Adversario/examenfinaldevops1.git', branch: 'master'
            }
        }
        stage('Verify Java Version') {
            steps {
                // Verifica que se esté usando el JDK correcto
                sh 'java -version'
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
                sh 'curl -s http://localhost:9090/endpoint_salud'
            }
        }
    }
    post {
        success {
            echo 'Pipeline ejecutado exitosamente.'
        }
        failure {
            echo 'Error en la ejecución del pipeline.'
        }
    }
}
