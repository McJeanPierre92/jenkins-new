pipeline {
    agent any

    tools {
        dockerTool "Docker" // Asegúrate de que este nombre coincida con el configurado en Jenkins
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                script {
                    def dockerBin = "${tool name: 'Docker', type: 'dockerTool'}/bin/docker"
                    sh "${dockerBin} run --rm -v \"${WORKSPACE}\":/usr/src/app -w /usr/src/app node:22-slim npm install"
                }
            }
        }

        stage('Ejecutar tests') {
            steps {
                script {
                    def dockerBin = "${tool name: 'Docker', type: 'dockerTool'}/bin/docker"
                    sh "${dockerBin} run --rm -v \"${WORKSPACE}\":/usr/src/app -w /usr/src/app node:22-slim npm test"
                }
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                script {
                    def dockerBin = "${tool name: 'Docker', type: 'dockerTool'}/bin/docker"
                    sh "${dockerBin} build -t hola-mundo-node:latest ."
                }
            }
        }

        stage('Ejecutar Contenedor Node.js') {
            steps {
                script {
                    def dockerBin = "${tool name: 'Docker', type: 'dockerTool'}/bin/docker"
                    sh """
                        ${dockerBin} stop hola-mundo-node || true
                        ${dockerBin} rm hola-mundo-node || true
                        ${dockerBin} run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                    """
                }
            }
        }
    }
}



