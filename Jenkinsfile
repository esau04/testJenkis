pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
                echo "Cloning the repository..."
                checkout scm
            }
        }
        stage("Run Tests") {
            steps {
                echo "Running test.js..."
                sh 'node test.js'
            }
        }
    }
}
