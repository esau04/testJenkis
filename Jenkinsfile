pipeline {
    agent any
    environment {
        // Usar las credenciales de Docker Hub
        DOCKERHUB_USER = credentials('dockerhub_credentials') // Para el nombre de usuario
DOCKERHUB_PASSWORD = credentials('dockerhub_credentials') // Para la contraseña

    }
    stages {
        stage("Build") {
            steps {
                echo 'Building docker images for deployment...'
                sh '''
                    docker-compose -f docker-compose-dev.yml down
                    docker-compose -f docker-compose-dev.yml build
                '''
            }
        }
        stage("PushBuilds") {
            steps {
                echo "Pushing docker images to DockerHub..."
                sh '''
                    echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USER --password-stdin
                    docker-compose -f docker-compose-dev.yml push
                '''
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying to production..."
                sh '''
                    echo "New deployment" >> deployments.txt
                    scp deployments.txt jenkins@${PUPPET_AGENT_URL_PROD}:${PUPPET_AGENT_HOME}/

                    scp docker-compose-prod.yml jenkins@${PUPPET_MASTER_URL}:${PUPPET_MASTER_HOME}/docker-compose.yml
                    scp site.pp jenkins@${PUPPET_MASTER_URL}:${PUPPET_MASTER_HOME}/
                    scp init.pp jenkins@${PUPPET_MASTER_URL}:${PUPPET_MASTER_HOME}/

                    ssh jenkins@${PUPPET_MASTER_URL} sudo mv ${PUPPET_MASTER_HOME}/docker-compose.yml ${PUPPET_MASTER_DEV_FILES_DIR}/
                    ssh jenkins@${PUPPET_MASTER_URL} sudo mv ${PUPPET_MASTER_HOME}/site.pp ${PUPPET_MASTER_MANIFEST_DIR}/
                    ssh jenkins@${PUPPET_MASTER_URL} sudo mv ${PUPPET_MASTER_HOME}/init.pp ${PUPPET_MASTER_MODULE_MANIFEST_DIR}/
                ''' 
            }
        }
    }
}
