pipeline {
    agent any

    // Esta sección indica a Jenkins que use la instalación de Maven configurada globalmente.
    tools {
        // "Maven3" debe coincidir con el nombre que configuraste en Global Tool Configuration.
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Adversario/examenfinaldevops1.git', branch: 'master'
            }
        }
        stage('Build') {
            steps {
                // Ahora, al haber configurado Maven, el comando "mvn" debería estar disponible.
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
