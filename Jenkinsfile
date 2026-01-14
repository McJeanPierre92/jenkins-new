pipeline {
    agent any

    tools {
        nodejs "Node"
        dockerTool "Docker"
    }

    stages {
        stage('Instalar dependencias') {
            steps {
                sh 'npm install'
            }
        }

        stage('Ejecutar tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Construir Imagen Docker') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh 'docker build -t hola-mundo-node:latest .'
            }
        }

        stage('Ejecutar Contenedor Node.js') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh '''
                    # Forzar la eliminación del contenedor por nombre si existe
                    docker rm -f hola-mundo-node || true

                    # Identificar y eliminar CUALQUIER contenedor que esté usando el puerto 3000
                    CONTAINER_ID=$(docker ps -q --filter "publish=3000")
                    if [ ! -z "$CONTAINER_ID" ]; then
                        echo "Deteniendo contenedor existente en puerto 3000: $CONTAINER_ID"
                        docker rm -f $CONTAINER_ID
                    fi

                    # Ejecutar el nuevo contenedor
                    docker run -d --name hola-mundo-node -p 3000:3000 hola-mundo-node:latest
                '''
            }
        }
    }
}