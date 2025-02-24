pipeline {
    agent any

    stages {
        // Etapa 1: Clonación del repositorio
        stage('Checkout') {
            steps {
                // Clona el repositorio desde GitHub
                git url: 'https://github.com/Adversario/examenfinaldevops1.git', branch: 'master'
            }
        }
        // Etapa 2: Compilación de la aplicación
        stage('Build') {
            steps {
                // Compila el proyecto utilizando Maven (asegúrate de que Maven esté instalado en el agente)
                sh 'mvn clean package'
            }
        }
        // Etapa 3: Ejecución de tests
        stage('Test') {
            steps {
                // Ejecuta las pruebas unitarias y de integración
                sh 'mvn test'
            }
        }
        // Etapa 4: Despliegue en Docker
        stage('Deployment') {
            steps {
                // Si existe un contenedor previo, deténlo y elimínalo
                sh 'docker stop contenedor_vehiculos || true'
                sh 'docker rm contenedor_vehiculos || true'
                // Construye la imagen Docker a partir del Dockerfile del proyecto
                sh 'docker build -t imagen_vehiculos .'
                // Ejecuta el contenedor; mapea el puerto 8080 del contenedor al 9090 del host
                sh 'docker run -d -p 9090:8080 --name contenedor_vehiculos imagen_vehiculos'
            }
        }
        // Etapa 5: Verificación Post Despliegue
        stage('Post-Deployment Verification') {
            steps {
                // Realiza una solicitud HTTP para verificar que el servicio responde correctamente
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
