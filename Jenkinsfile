pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning the repository...'
                git 'https://github.com/esau04/testJenkis.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building docker images...'
                sh 'docker-compose build'
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'docker-compose up --exit-code-from SumaTest SumaTest'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Aquí irían tus pasos de despliegue
            }
        }
    }
}
