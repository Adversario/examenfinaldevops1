pipeline {
    agent any

    tools {
        // Se asume que en Global Tool Configuration tienes configurado un JDK llamado "JDK" que apunta a OpenJDK 21
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
                sh 'echo JAVA_HOME=$JAVA_HOME'
                sh 'java -version'
            }
        }
        stage('Build') {
            steps {
                script {
                    // Forzar la configuración de JAVA_HOME con la herramienta JDK configurada
                    env.JAVA_HOME = tool 'JDK'
                    env.PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
                    sh 'echo JAVA_HOME=$JAVA_HOME'
                    sh 'java -version'
                    // Imprime la versión de Maven para confirmar el entorno que usa Maven
                    sh 'mvn -version'
                    // Forzar que Maven use el JAVA_HOME actual
                    sh 'JAVA_HOME=$JAVA_HOME mvn clean package'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'JAVA_HOME=$JAVA_HOME mvn test'
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
