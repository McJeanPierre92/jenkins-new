pipeline {
    agent any

    tools {
        dockerTool "Docker"
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                // Ejecutamos npm install dentro de un contenedor para evitar problemas de librerías en el host
                sh 'docker run --rm -v "${WORKSPACE}":/usr/src/app -w /usr/src/app node:22-slim npm install'
            }
        }

        stage('Ejecutar tests') {
            steps {
                // Ejecutamos los tests dentro del contenedor
                sh 'docker run --rm -v "${WORKSPACE}":/usr/src/app -w /usr/src/app node:22-slim npm test'
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }

        stage('Ejecutar Contenedor Node.js') {
            steps {
                sh '''
                    docker stop hola-mundo-node || true
                    docker rm hola-mundo-node || true
                    docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                '''
            }
        }
    }
}


