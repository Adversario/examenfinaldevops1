pipeline {
    agent any

    // Se asume que en "Global Tool Configuration" tienes configurado:
    // - JDK: se usará la versión compatible (por ejemplo, OpenJDK 17)
    // - Maven: se llama "Maven" (tal como lo has configurado)
    tools {
        jdk 'JDK17'  // Asegúrate de que el nombre coincida con el configurado en Global Tool Configuration
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
                // Verifica la versión de Java en el agente
                sh 'java -version'
            }
        }
        stage('Build') {
            steps {
                // Compila el proyecto utilizando Maven
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
                // Detener y eliminar contenedor previo (si existe)
                sh 'docker stop contenedor_vehiculos || true'
                sh 'docker rm contenedor_vehiculos || true'
                // Construye la imagen Docker a partir del Dockerfile
                sh 'docker build -t imagen_vehiculos .'
                // Ejecuta el contenedor: mapea el puerto 8080 del contenedor al 9090 del host
                sh 'docker run -d -p 9090:8080 --name contenedor_vehiculos imagen_vehiculos'
            }
        }
        stage('Post-Deployment Verification') {
            steps {
                // Verifica el endpoint de la aplicación (ajusta el endpoint si es necesario)
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
