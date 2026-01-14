pipeline {
    agent any

    tools {
        dockerTool "Docker"
    }

    stages {
        stage('Instalar dependencias') {
            agent {
                docker {
                    image 'node:22-slim'
                    reuseNode true
                }
            }
            steps {
                sh 'npm install'
            }
        }

        stage('Ejecutar tests') {
            agent {
                docker {
                    image 'node:22-slim'
                    reuseNode true
                }
            }
            steps {
                sh 'npm test'
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

